# Deployment, operasional dan pengujian

Kembali ke [indeks desain](../design.md). Semua konfigurasi berikut adalah target desain; Dockerfile, Compose, CI dan tests belum diimplementasikan.

## O1. Komponen Docker

| Service | Peran | Penyimpanan/akses |
| --- | --- | --- |
| `gateway` | Reverse proxy, TLS, header, batas upload/rate dasar | Port publik 443; sertifikat secret; tidak mengekspos DB/storage |
| `web` | Next.js production Node runtime, API dan private file gateway | Runtime non-root; DB role aplikasi; read-only filesystem kecuali temp/upload volume yang diperlukan |
| `worker` | TypeScript jobs/scheduler, domain services yang sama | DB role worker terbatas, quarantine/storage, resource cap renderer; outbound AI hanya saat aktif |
| `migrate` | One-shot migration sebelum web/worker versi baru | Credential DDL terpisah; tidak dipasang sebagai env runtime web |
| `db` | PostgreSQL | Durable volume, internal network, backup role terpisah |
| `scanner` | Malware scanning adapter (baseline kandidat ClamAV) | Internal service, signature update channel, quarantine read; fail-closed ketika unavailable |
| `backup` | Scheduled database + object manifest backup ke lokasi terpisah | Credential backup dan tujuan terenkripsi; bukan public endpoint |

Satu host Linux dengan Docker Compose menjadi baseline deployment internal, dengan downtime terencana dan single-host failure risk yang diketahui. Bukan arsitektur high availability. Scale-out berikutnya memerlukan shared private object store, shared queue tetap PostgreSQL, kapasitas connection pool, serta reverse proxy beberapa web instance. Tidak mengasumsikan volume lokal dapat dibagi antarhost.

Startup dependency: `db healthy → migrate completed successfully → web/worker`. Scanner health diperiksa untuk menyalakan scan handler, tetapi web dapat tetap menerima draft dengan attachment PendingScan. `service_healthy` dan `service_completed_successfully` memastikan urutan readiness; aplikasi tetap membutuhkan retry koneksi setelah restart dependency. Mekanisme ini mengikuti [Docker Compose startup order](https://docs.docker.com/compose/how-tos/startup-order/).

Build multi-stage: dependency install dari lockfile → Prisma generation sesuai versi → typecheck/build → runtime minimal. Satu immutable image tag/digest untuk web/worker release yang sama, command berbeda. Next.js self-hosting memakai reverse proxy dan proses Node produksi sesuai [panduan resmi](https://nextjs.org/docs/app/guides/self-hosting). Jangan expose dev server pada produksi. Versi patch, OS image dan compatibility matrix dicatat saat spike, kemudian dipin; tidak menggunakan tag `latest` pada deployment.

## O2. Konfigurasi dan secret

| Konfigurasi konseptual | Aturan |
| --- | --- |
| `APP_BASE_URL`, `COMPANY_TIMEZONE`, `DEFAULT_LOCALE` | Validasi saat startup; timezone DB session UTC, business timezone Asia/Jakarta default |
| `DATABASE_URL`, `MIGRATION_DATABASE_URL` | Secret terpisah; DDL hanya migrator; tidak memiliki prefix client-public |
| `AUTH_SECRET`, session TTL | Secret manager/mounted secret; rotasi dan revocation prosedural |
| `STORAGE_DRIVER`, private/quarantine paths | Di luar webroot; path absolut terkontrol; disk quota dan checksum |
| `SCANNER_ENDPOINT`, scan timeout | Internal endpoint; health dan signature age termonitor |
| `WORKER_CONCURRENCY`, lease/retry/batch limits | Awal rendah, dituning dari load test; job type AI/render dibatasi terpisah |
| `FEATURE_RELEASE`, `AI_ENABLED` | Server gating; R3 AI default false sampai provider/data policy siap |
| `AI_PROVIDER`, credentials/budget/timeout | Secret hanya worker; tidak masuk DB evidence atau log; model/template version tercatat |
| Backup destination/encryption reference | Tujuan di luar host aplikasi; akses read/write khusus backup operator |

Nama final env mengikuti library/tooling yang dipin. `.env.example` hanya placeholder, bukan credential. Bootstrap tidak menyediakan default password. Secret tidak dimasukkan ke image build layer, `NEXT_PUBLIC_*`, artefak test, atau log.

## O3. Pipeline dan migrasi rilis

1. Validasi link/traceability dan perubahan requirements/spec; lint/typecheck.
2. Domain tests untuk policy, state machine, decimal dan kalender; contract schema tests.
3. Integration tests pada PostgreSQL versi yang sama dengan target, migration dari kosong dan dari versi rilis sebelumnya; jangan memakai SQLite untuk membuktikan locking/partial indexes.
4. Build immutable Docker image, dependency/image scan, software inventory dan lockfile tersimpan.
5. Staging: migrate menggunakan role DDL, smoke/E2E untuk role/site berbeda, background job dan private file.
6. Sebelum produksi: backup terverifikasi, catatan perubahan, schema compatibility dan rollback plan. Jalankan satu migrator; jangan migrasi otomatis dari setiap replica web.
7. Jalankan web/worker baru, cek readiness, smoke scoped request, job heartbeat dan error rates.

Rollback aplikasi ke image sebelumnya hanya jika schema masih backward-compatible. Gunakan expand/contract agar ini memungkinkan. Data migration destructive tidak dibalik lewat reset otomatis; forward fix atau restore terencana dengan downtime dan potensi kehilangan data setelah checkpoint harus dicatat. Deployment pertama juga menguji pembuatan constraint SQL khusus dan privileges runtime.

## O4. Backup dan recovery

Target requirements: backup harian, RPO ≤24 jam, RTO ≤8 jam. Baseline satu host memakai maintenance window singkat untuk menghentikan write web/worker dan menunggu transaksi berjalan selesai, kemudian mengambil dump konsisten PostgreSQL, manifest seluruh object referenced beserta checksum, dan salinan file immutable. Resume write setelah checkpoint/manifest aman; copy ke remote storage dapat dilanjutkan dari checkpoint jika snapshot storage mendukungnya. Bila volume tidak mendukung snapshot, tulisannya tetap dihentikan sampai copy yang diperlukan konsisten.

Backup DB, object dan manifest memiliki satu backup ID; arsip tanpa seluruh bagian dianggap gagal. Simpan terenkripsi di luar host; alarm backup age >24 jam, checksum mismatch dan capacity rendah. Retensi final mengikuti Q-06; sebelum disepakati tidak menghapus bukti historis melalui aplikasi.

Restore drill ke lingkungan terisolasi: pulihkan database + files + konfigurasi nonsecret, sediakan secret baru, verifikasi FK/schema/record count/checksum, revision Effective, akses role/site, unduhan bukti, login dan job recovery. Reset lease yang tidak aktif melalui recovery command ter-audit; cegah side effect eksternal saat drill. Ukur waktu end-to-end hingga layanan siap, bukan hanya waktu restore SQL. Jika dataset tidak dapat memenuhi RPO/RTO dengan baseline ini, tingkatkan strategi backup sebelum produksi, bukan menurunkan target diam-diam.

## O5. Observability dan performance

`/health/live` memeriksa process hidup; `/health/ready` memeriksa DB connectivity/schema compatibility dan kemampuan storage utama. Endpoint publik hanya status generik. Scanner/worker/AI status lebih rinci hanya health dashboard internal; scanner down tidak membuat record Clean. Structured log memuat correlation ID, route/command, duration, error code dan actor ID yang diperlukan, tanpa password/token/binary/evidence body. Audit bisnis terpisah dari log teknis.

Monitor p95 request, failed commands, conflict/retry rate, DB pool wait, slow queries, queue depth/oldest age, lease churn, failed scan/signature age, disk, backup age, worker heartbeat, AI latency/cost. Alert pada audit-write failure dan blocked scheduler. Request timeout, DB statement/transaction timeout dan pool budget dikonfigurasi pada fondasi, diuji terhadap batas p95; worker retry tidak menutupi outage tanpa batas.

| Skenario | Target verifikasi |
| --- | --- |
| R1 50 concurrent users, 100.000 NCR | API list/detail p95 ≤2 detik, dashboard ≤3 detik, di luar transfer file |
| R2 grid 100 × 30, 20 operator | Buka/simpan grid p95 ≤3 detik dengan version/validation aktif |
| R3 matrix 200 × 50 | Pagination/virtualisasi, query tidak memuat seluruh personel tanpa scope |
| Dashboard | Refresh setelah mutation dan polling ≤60 detik saat aktif; stale ketika gagal |

Catat CPU/RAM/disk/network, versi software, distribusi role/site, dataset attachment/links, endpoint mix serta warm/cold runs. Target adalah acceptance baseline, bukan klaim performa yang sudah dicapai. Gunakan explain plans, select DTO minimal dan indeks yang direncanakan sebelum menambah cache shared atau service baru.

## O6. Strategi tests

Kandidat tool implementasi: unit runner TypeScript, Playwright untuk E2E, PostgreSQL container untuk integration, load runner, dan accessibility scanner. Versi/library akhir dipin pada fondasi; yang normatif adalah bukti perilaku di bawah.

| Suite | Bukti wajib | AC/NFR |
| --- | --- | --- |
| `access-policy` | Role × site × ownership × assignment × independence; read/list/count/search/export/file/job; revoke next request | AC-03/05/17/22/34/38, NFR-SEC-001 |
| `quality-workflow` | Happy path dan setiap invalid transition; Critical CAPA, effectiveness fail, graph reopen, auditor conflict | AC-01/02/04/06/07/16/43 |
| `document-versioning` | Immutable submissions, effective date, concurrent publish, withdrawal vs scheduler race | AC-08/09/19/25/42/43 |
| `task-notification` | Workflow projection tidak dapat close manual, standalone reopen, retry dedupe, timezone boundaries | AC-13/20/21/41 |
| `analytics` | Empty cohort, monthly zeros, Pareto ties, reopened NCR, costs corrections, aging, denominator per unit | AC-10/11/12/23/24/29/37 |
| `transaction-faults` | Force audit insert failure, stale version, duplicate request concurrent, child edit vs parent close | AC-14/15/19, NFR-DAT-001/002 |
| `files` | Oversize stream, spoof MIME, malicious PDF/image, scanner outage, private derivative/range fetch | AC-18/22, NFR-SEC-002 |
| `inspection-calibration` | Decimal boundary, unexpected cell, invalid gage, submit Fail, amendment, Fail scan impact and hold races | AC-25/26/27/28/29/30/42, NFR-DAT-003 |
| `maintenance-supplier` | Anchor Jan 31, leap year, cancelled occurrence retry, incomplete checklist, weights and independent approval | AC-31/32/33/34, NFR-JOB-001 |
| `training-compliance-ai` | Pending vs Qualified, expiry/retraining, N/A approval, no-source AI, injection/cross-site, timeout/cancel late result | AC-35/36/37/38/39/43, NFR-AI-001 |
| `ui-i18n` | Keyboard, focus/error/empty/conflict, chart table, 360/768/1440, id/en/ja and locale decimal | AC-40, NFR-UX-001, NFR-COMP-001, NFR-I18N-001 |
| `operations` | Process kill/lease reclaim, repeated migration, fresh+upgrade schema, backup/restore, load | NFR-REL-001, NFR-OBS-001, NFR-PER-001/002, NFR-JOB-001 |

Security tests menggunakan dua site, tiga role, author versus reviewer, session aktif kemudian direvoke, dokumen multisite, dan linked record dengan ACL berbeda. Concurrency tests memakai koneksi/transaksi sungguhan dengan barrier tersinkron, bukan mock Prisma. Clock dependency diinjeksi untuk deadline, leap year, locale dan effective date. Seed synthetic demo tidak digunakan pada produksi.

## O7. Exit gate dan status verifikasi desain

Sebelum merge implementasi: setiap requirement Must fitur mempunyai test/AC mapping, contract tests lulus, migration/recovery impact direview, UI error states ada, serta tidak ada bypass audit/policy. Sebelum rilis: semua Must rilis, NFR relevan, compatibility browser saat rilis, scan file, restore dan load evidence lulus; Should yang ditunda dicatat.

Pada tahap desain, validasi yang dapat dilakukan hanya struktur dokumen, ID/link/traceability, dan review konsistensi. Tidak ada hasil build, database migration, UI render, performance atau security test runtime yang diklaim sudah lulus.

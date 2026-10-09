# Desain teknis e-QMS

| Atribut | Nilai |
| --- | --- |
| Versi desain | 0.1.0 |
| Tanggal | 9 Oktober 2026 |
| Baseline kebutuhan | [Requirements v0.2.0](requirements.md) yang ada di working tree saat desain disusun |
| Stack wajib | Next.js, TypeScript, PostgreSQL, Prisma, Docker |
| Status | Desain untuk review dan dasar implementasi; belum ada aplikasi, migrasi, atau pengujian runtime |
| Cakupan | R1, R2, R3; rincian implementasi mengikuti dependensi tiap rilis |

Desain memakai **modular monolith** dengan Next.js App Router sebagai web dan API, domain service TypeScript, PostgreSQL sebagai penyimpanan transaksi, Prisma sebagai akses data/migrasi, dan worker TypeScript terpisah dalam Docker. Web dan worker berbagi kode domain serta database, sehingga aturan bisnis tidak diduplikasi. File disimpan privat di luar database; metadata dan jejak aksesnya tetap di PostgreSQL.

## 1. Cara membaca dan prosedur spec-driven development

Urutan artefak: requirements → spec sistem → plan/desain → tasks → implementasi → verifikasi terhadap acceptance criteria. Permintaan ini menyelesaikan artefak sampai desain dan rencana pekerjaan, tanpa menyatakan fitur sudah berjalan.

| Artefak | Isi |
| --- | --- |
| [Spesifikasi sistem](../specs/001-system-design/spec.md) | User journey, batas modul, invariant dan perilaku kegagalan yang harus dipenuhi |
| [Plan sistem](../specs/001-system-design/plan.md) | Pemetaan spesifikasi ke desain teknis, dependensi, dan keputusan yang perlu ditutup |
| [Tasks](../specs/001-system-design/tasks.md) | Urutan implementasi dengan ID requirement, bukti hasil dan pengujian yang ditargetkan |
| [Arsitektur dan keamanan](design/architecture.md) | Next.js, domain service, transaksi, policy akses, file, jobs, cache, AI |
| [Model data](design/data-model.md) | ERD, katalog entitas, tipe kolom, FK, indeks, constraint, snapshot dan migrasi |
| [API dan workflow](design/api-workflows.md) | Kontrak request/response, katalog endpoint dan command, state transition, race condition |
| [UI/UX](design/ui-ux.md) | Sitemap, layout/wireframe, komponen, interaksi per modul, responsivitas dan i18n |
| [Operasional dan pengujian](design/operations-testing.md) | Docker, konfigurasi, CI/CD, recovery, observability, test matrix dan target performa |
| [Traceability](design/traceability.md) | Pemetaan seluruh FR/NFR dan AC dari baseline ke desain dan pekerjaan |
| [ADR](adr/README.md) | Alasan pemilihan arsitektur, akses, workflow, data/file, serta strategi rilis |

Paket `001-system-design` adalah spesifikasi lintas sistem. Sebelum coding satu modul, turunkan bagian terkait menjadi paket fitur `specs/002-foundation`, `003-ncr-capa`, dan seterusnya sebagaimana [tasks](../specs/001-system-design/tasks.md). Paket fitur tersebut belum dibuat atau dianggap approved. Invariant dan kontrak dalam desain ini menjadi batas teknis yang harus dibawa ke paket fitur, bukan diganti dengan asumsi baru.

## 2. Keputusan desain utama

| Area | Keputusan |
| --- | --- |
| Bentuk aplikasi | Satu repository TypeScript; modul domain terpisah, dua proses aplikasi: web dan worker |
| UI | Next.js App Router; Server Components untuk initial read; Client Components untuk form, grid, grafik dan drawing |
| API | Route Handlers `/api/v1`; domain command eksplisit untuk transisi; tidak menerima edit status generik |
| Data | PostgreSQL; Prisma repository; SQL migration terkontrol untuk constraint/indeks yang diperlukan |
| Otorisasi | Database session dan policy server berdasarkan role + site + assignment + independensi + izin modul |
| Konsistensi | Mutasi, audit event, keputusan, task projection dan outbox ditulis dalam satu transaksi |
| Pekerjaan latar | Antrean PostgreSQL dengan lease, retry dan deduplikasi; tidak bergantung pada timer proses web |
| Dokumen | Revisi immutable setelah submit; working version baru saat returned; tepat satu versi Effective bila ada revisi berlaku |
| File | Adapter private storage, quarantine dan scan sebelum tersedia; unduhan lewat gateway terautorisasi |
| Deployment | Docker Compose untuk development dan baseline satu host internal; produksi memerlukan backup di luar host dan TLS |
| AI | Adapter provider terpisah, job eksplisit, bukti terpilih dan review manusia; default nonaktif sampai Q-19 selesai |

## 3. Cakupan rilis

| Rilis | Modul | Exit gate |
| --- | --- | --- |
| R1 | Auth/role/site, master, lampiran/log, NCR/CAPA, audit, dokumen, tasks, dashboard, pencarian, tiga bahasa | Seluruh Must R1, AC terkait, security/concurrency/load/restore memenuhi baseline |
| R2 | Quality plans, inspeksi, kalibrasi, maintenance, supplier quality | R1 tetap lulus; keterlacakan plan–alat–hasil–NCR serta scheduler terverifikasi |
| R3 | Training, compliance, AI auditor | Kualifikasi/assessment tervalidasi; sumber AI berizin, human review dan pengujian negatif lulus |

FR-DOC-010 dan FR-ANA-008 tetap Should. Desainnya disediakan, tetapi penundaan memerlukan catatan rilis. Hal yang dikecualikan requirements, seperti SPC, portal pemasok, offline, dan keputusan AI otomatis, tidak ditambahkan oleh desain.

## 4. Keputusan yang masih terbuka

Stack yang diminta sudah ditetapkan. Keputusan lain di bawah tidak menghalangi desain ini, tetapi menjadi gate sebelum implementasi atau produksi terkait.

| Gate | Keputusan | Baseline desain |
| --- | --- | --- |
| Sebelum fondasi | Versi paket, Node.js, Prisma API/migration CLI dan auth library | Pilih rilis stabil yang kompatibel, pin lockfile dan digest image; buktikan login, transaksi, migrasi dan build container pada spike. Tidak mengasumsikan versi `latest`. |
| Sebelum R1 production | Host/domain/TLS, lokasi data, retensi, storage, scan, backup, akun approver | Satu host Linux Docker, volume privat + scanner adapter; backup terenkripsi di host/lokasi berbeda; workflow tanpa approver tetap tertahan. Q-02/03/06/09. |
| Sebelum laporan biaya | Mata uang dan format nomor | IDR dan nomor `{TYPE}-{SITE}-{YEAR}-{SEQUENCE}`; nomor dokumen global; nomor yang sudah dialokasikan tidak dipakai ulang. Q-08/20. |
| Sebelum R2 operasional | Presisi, toleransi, sampling/release lot, kalibrasi dan interval maintenance | Numerik sederhana, input manual, calendar recurrence; detail Q-11–15 wajib tertulis di feature spec. |
| Sebelum R3 operasional | Kompetensi, konten standar berlisensi, provider AI dan hak pemrosesan | Q-17–19; compliance manual tetap berfungsi ketika AI nonaktif. |
| Sebelum setiap rilis | Glossary dan reviewer tiga bahasa | id/en/ja, status internal stabil, perusahaan Asia/Jakarta; Q-21. |

Usulan teknis seperti antrean PostgreSQL, storage adapter dan layout tidak mengubah requirement bisnis. Jika gate menghasilkan perubahan perilaku, revisi requirement dan traceability dalam perubahan yang sama.

## 5. Referensi teknis

Dokumentasi resmi diperiksa saat desain disusun. Rekomendasi proyek tetap dinyatakan sebagai keputusan desain; tautan ini mendasari kemampuan stack, bukan bukti implementasi e-QMS.

- Batas Server/Client Components mengikuti [Next.js Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components).
- Otorisasi dekat akses data dan validasi setiap entry point mengikuti [Next.js authentication guide](https://nextjs.org/docs/app/guides/authentication).
- Hosting proses Next.js dan reverse proxy mengacu pada [Next.js self-hosting](https://nextjs.org/docs/app/guides/self-hosting).
- Batas transaksi dan penanganan konflik diverifikasi terhadap [Prisma transactions](https://www.prisma.io/docs/orm/fundamentals/transactions); API persis bergantung versi yang dikunci.
- Perubahan SQL khusus dan deploy migration mengacu pada [Prisma editing migrations](https://www.prisma.io/docs/orm/migrations/editing-a-migration) dan [applying migrations](https://www.prisma.io/docs/orm/migrations/applying-a-migration).
- Keunikan bersyarat dan row locks mengacu pada [PostgreSQL partial indexes](https://www.postgresql.org/docs/current/indexes-partial.html) dan [explicit locking](https://www.postgresql.org/docs/current/explicit-locking.html).
- Readiness dependency Compose mengacu pada [Docker startup order](https://docs.docker.com/compose/how-tos/startup-order/); container running belum berarti aplikasi siap.

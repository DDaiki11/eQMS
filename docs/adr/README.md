# Architecture Decision Records

Tanggal: 9 Oktober 2026. Status semua ADR: **diusulkan sebagai baseline desain**, kecuali pilihan lima teknologi inti yang sudah diminta pengguna. Lihat [desain](../design.md) dan [requirements](../requirements.md).

## ADR-001 — Modular monolith dengan web dan worker

**Konteks:** banyak modul saling bergantung pada closure/approval dan audit atomik; baseline satu perusahaan serta tim/infrastruktur belum ditentukan.

**Keputusan:** satu codebase TypeScript, Next.js App Router untuk UI/API, application/domain module boundaries, PostgreSQL dan Prisma, proses worker terpisah dalam Docker. Transaksi lintas modul tetap satu database. Gunakan Route Handlers untuk kontrak HTTP; server-rendered read memanggil query service langsung.

**Alternatif:** microservices memerlukan konsistensi lintas database; backend terpisah menduplikasi transport/deployment; keduanya belum diperlukan baseline. Serverless web-only tidak cocok untuk job panjang yang dibutuhkan.

**Konsekuensi:** deployment lebih sederhana tetapi isolasi modul perlu ditegakkan melalui import boundaries dan review. Worker scale terpisah dari web; database tetap failure domain bersama. Evaluasi pemisahan service hanya dengan bukti bottleneck/domain ownership.

Requirement: FR-LOG-003, FR-CAPA-007, NFR-DAT-001, NFR-JOB-001.

## ADR-002 — Session database dan policy server

**Konteks:** perubahan akses harus berlaku request berikutnya; akses juga bergantung assignment, independensi dan revisi dokumen.

**Keputusan:** library auth yang diverifikasi pada fondasi, database sessions dapat direvoke, user/membership/grant dibaca setiap request, centralized policy dan scoped repositories. Runtime DB role berprivilege minimum. RLS belum baseline; tidak ada shared permission cache lintas request.

**Alternatif:** JWT berisi role/site dengan TTL panjang menunda revoke. UI-only guards tidak melindungi API/files. RLS sendiri tidak menyelesaikan independent reviewer/linked evidence tanpa context tambahan.

**Konsekuensi:** tambahan query session/policy yang perlu indeks; test semua entry point wajib. Bisa menambah RLS sebagai defense tambahan setelah pooling dan worker context dibuktikan, bukan menggantikan policy domain.

Requirement: FR-ACC-001–005, FR-DOC-007, FR-COM-007, FR-AI-004.

## ADR-003 — State machine, snapshots dan transaksi audit

**Konteks:** approve versi usang, edit hasil final, publikasi ganda dan reopen parsial merusak keterlacakan.

**Keputusan:** explicit command/state machine, aggregate version + parent locks, immutable submission/approval subject, unique effective revision, receipt idempotensi dan business audit dalam transaksi. Outbox untuk side effect asinkron saja.

**Alternatif:** edit status generik, audit lewat async logger, atau status tugas menjadi sumber workflow akan membiarkan bypass/kehilangan event.

**Konsekuensi:** migration/SQL constraints dan concurrency tests diperlukan; payload historis bertambah. Snapshot immutable tidak berarti event sourcing penuh: current domain tables tetap sumber state, audit adalah histori perubahan.

Requirement: FR-LOG-001–006, FR-DOC-003–008, FR-QPL-004–005, NFR-DAT-001–003.

## ADR-004 — PostgreSQL queue dan private storage adapter

**Konteks:** reminder, publication, scanner, PDF dan AI memerlukan pekerjaan durable; file perlu scan, private ACL dan backup.

**Keputusan:** antrean PostgreSQL dengan lease/fencing/dedupe/retry, worker TypeScript, private file adapter dengan quarantine/scan/gateway. Baseline single-host volume durable; object store dipakai bila scale-out diperlukan.

**Alternatif:** in-process cron/queue hilang saat restart; public static upload melanggar ACL. Redis/S3 service tertentu belum menjadi kebutuhan wajib, sehingga tidak mengikat deployment provider pada desain awal.

**Konsekuensi:** queue menggunakan kapasitas DB dan perlu indeks/cleanup operasional; file dan DB perlu rekonsiliasi backup. External side effect hanya at-least-once dengan provider reconciliation, bukan klaim exactly-once universal.

Requirement: FR-COM-002–003/009, FR-MNT-002, FR-AI-001, NFR-SEC-002, NFR-JOB-001, NFR-REL-001.

## ADR-005 — Kontrak desimal, versi stack dan human-reviewed AI

**Konteks:** batas toleransi/biaya tidak aman bila terkonversi float; dokumentasi API framework/ORM berubah; AI tidak boleh mengambil keputusan kualitas.

**Keputusan:** PostgreSQL numeric dan decimal string pada API; locale conversion di boundary, comparison sebelum rounding. Pin versi stack setelah compatibility spike, adapter mengisolasi transaksi ORM. AI provider adapter tanpa mutation tools, manifest evidence dan schema/citation checks, acceptance manusia menghasilkan draft.

**Alternatif:** memilih `latest` tanpa spike menambah risiko migration/API mismatch; memakai model output sebagai skor/final decision melanggar requirement.

**Konsekuensi:** perlu fixture desimal/i18n, integration spike sebelum scaffolding, dan evaluation AI termasuk access/injection/citation. Provider AI tetap gate terpisah R3, tidak memblokir R1.

Requirement: NFR-DAT-003, FR-INS-003, FR-SUP-003, FR-CMP-004, FR-AI-001–004, NFR-AI-001.

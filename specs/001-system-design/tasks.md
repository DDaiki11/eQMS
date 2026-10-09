# TASKS-001 — Rencana pekerjaan implementasi

Semua checklist di bawah **belum dikerjakan**. Artefak yang selesai pada permintaan ini adalah [desain](../../docs/design.md), [spec](spec.md), [plan](plan.md) dan traceability. Nomor Txx menjadi referensi pemetaan; test intent bukan hasil test.

## T01 — Fondasi stack, akses dan delivery (R1)

- [ ] Buat `specs/002-foundation/{spec,plan,tasks}.md`; tutup versi Next/Node/TypeScript/PostgreSQL/Prisma, auth library dan image melalui spike login, transaction rollback, decimal, custom index, migration upgrade, build/start Docker.
- [ ] Scaffold struktur modular, environment validation, private DTO, session revocation, role/site/grant policy, master data, locale id/en/ja dan bootstrap tanpa default credential.
- [ ] Buat schema/migration shared registry, number allocation, command receipt, outbox/job, append-only audit, file quarantine/scan/gateway dan task assignment primitives.
- [ ] Buat Compose web/worker/db/migrator/scanner, health endpoints, structured log, CI unit/integration/build dan backup/restore baseline.

Requirement: FR-ACC-001–005, FR-COM-001–010, FR-LOG-001–006, seluruh NFR fondasi. Bukti: AC-03/14/15/17/18/22/40, rollback transaction, revoke next request, startup fresh/upgrade, privileged-role separation.

## T02 — NCR dan CAPA (R1)

- [ ] Buat `specs/003-ncr-capa` dengan seluruh status/guard/field validation, link graph dan snapshot cycle.
- [ ] Implementasi pelaporan/triage/containment, plan review/approval, actions, effectiveness, closure/reopen coordinator dan cost corrections.
- [ ] Hubungkan UI detail/form, task projection, notification, attachment, audit dan export query scope.

Requirement: FR-NCR-001–008, FR-CAPA-001–007. Bukti: AC-01/02/04/05/06/14/15/16/23; race child-action edit versus close dan graph link edit versus reopen.

## T03 — Dokumen terkendali (R1)

- [ ] Buat `specs/004-documents`; working version versus business revision, classifications/site scope dan effective-date policy.
- [ ] Implementasi folder/preview, revision content immutable, review/approval, manual/scheduled publish, withdraw/obsolete, linked records dan acknowledgment (Should).
- [ ] Uji publikasi paralel, scheduler setelah withdraw, ACL preview/range, revisi superseded dan file scan pending.

Requirement: FR-DOC-001–013. Bukti: AC-05/08/09/19/22/43; tepat satu Effective setelah publish, tanpa menghilangkan history.

## T04 — Audit dan temuan (R1; supplier extension R2)

- [ ] Buat `specs/005-audits`; checklist templates/snapshot, responsibility mapping, finding links dan report/closure terpisah.
- [ ] Implementasi scheduler, checklist evidence, findings, independent verification, report approval/return/cancel serta reopen propagation.

Requirement: FR-AUD-001–010. Bukti: AC-07/16/43, auditor area conflict, source belum closed dan report tanpa temuan.

## T05 — Tasks, analytics, search dan release R1

- [ ] Buat `specs/006-tasks-analytics`; kontrak population/filter/DTO dan standalone versus source tasks.
- [ ] Implementasi list tasks, notifications, scope-safe search, dashboard, Pareto/trend/aging/costs, CSV dan previous-period comparison (Should).
- [ ] Uji tiga bahasa, keyboard/viewport, 50 pengguna/100.000 NCR, security dan restore drill; tutup gate R1.

Requirement: FR-TSK-001–005, FR-ANA-001–010, FR-COM-003–004/007–008. Bukti: AC-10/11/12/13/20/21/22/23/24/40; NFR-PER-001, NFR-UX-001, NFR-REL-001.

## T06 — Quality plans, register alat dan kalibrasi (R2)

- [ ] Buat `specs/007-plans-calibration`; tutup Q-11/13, precision dan approval plan yang terpisah secara keputusan.
- [ ] Implementasi part/unit/equipment, drawing balloon manual, characteristics/snapshot, plan publication dan exports.
- [ ] Implementasi calibration record/verification, hold/release, due date, status history dan impact review; register supplier minimal sebelum inspeksi incoming.

Requirement: FR-QPL-001–006, FR-CAL-001–006, FR-COM-009–010, FR-SUP-001 (identity). Bukti: AC-25/27/30/41/42; unit and coordinate boundaries, failed calibration block atomic.

## T07 — Inspeksi (R2)

- [ ] Buat `specs/008-inspections`; expected sample pairs, quantity scope, timestamp alat, kebijakan lot Q-12 dan N/A rules.
- [ ] Implementasi grid/decimal evaluation, measurement references, submit/review/finalize, NCR link, disposition dan amendment/PDF.
- [ ] Implementasi defect rate unit-sample dan final revision population; uji 100 × 30 pada 20 operator.

Requirement: FR-INS-001–007, FR-ANA-011–012, NFR-DAT-003, NFR-PER-002. Bukti: AC-25/26/27/28/29/30; measurement versus calibration hold race.

## T08 — Maintenance (R2)

- [ ] Buat `specs/009-maintenance`; recurrence/anchor/overdue, schedule revision dan asset retirement.
- [ ] Implementasi schedule generation, work orders/checklist/verifier, postpone/cancel, audit/notifications dan link NCR/CAPA.

Requirement: FR-MNT-001–005, FR-COM-009, FR-ANA-011, NFR-JOB-001. Bukti: AC-31/32/41; restart worker, duplicate occurrence dan leap-year dates.

## T09 — Supplier quality dan release R2

- [ ] Buat `specs/010-suppliers`; criteria/weights, independent decisions, certificate verification dan self-assessment provenance.
- [ ] Implementasi SupplierSite scope, certificates/reminders, evaluation/approval/status, assessment dan incoming/audit/NCR links.
- [ ] Uji regression R1, operasional R2, decimal/clock/queue races, security, load dan restore upgrade R1→R2.

Requirement: FR-SUP-001–006, FR-AUD-010 supplier extension, FR-ANA-011. Bukti: AC-33/34/41 dan seluruh Must R2.

## T10 — Training (R3)

- [ ] Buat `specs/011-training`; tutup kompetensi/level/masa berlaku/kriteria lulus Q-17.
- [ ] Implementasi kursus/revisi, requirements jabatan, assignment/attempt/verification, matrix/gap/expiry, retraining decisions dan person-level ACL.

Requirement: FR-TRN-001–006, FR-ANA-011, FR-COM-009. Bukti: AC-35/36/40/41; matrix 200 × 50 dan confidentiality personel.

## T11 — Compliance manual (R3)

- [ ] Buat `specs/012-compliance`; standard content license/version Q-18, applicability, N/A decision dan evidence change rules.
- [ ] Implementasi catalog/snapshot, clause assessment/evidence/gap, independent approval, deterministic readiness dan needs-review events.

Requirement: FR-CMP-001–005, FR-ANA-011. Bukti: AC-37/43, all-N/A, provisional/approved split dan source revision change.

## T12 — AI auditor dan release R3

- [ ] Buat `specs/013-ai-auditor`; tutup provider/data-retention/consent/budget/evaluation Q-19 sebelum mengaktifkan integration.
- [ ] Implementasi provider adapter, evidence manifest, job lifecycle, source verification, permission recheck dan human accept/reject to draft.
- [ ] Uji injection/cross-site/no-source/timeout/cancel/retry biaya, model/template audit; regression seluruh R1–R3 dan restore/load/UX release gate.

Requirement: FR-AI-001–004, NFR-AI-001, NFR-JOB-001. Bukti: AC-38/39/43; penilaian manual tetap tersedia tanpa provider.

## T13 — Gate lintas rilis

- [ ] Pada setiap T01–T12, perbarui traceability requirement→feature spec→task→test→hasil; status test hanya lulus bila eksekusi dan buktinya tersedia.
- [ ] Catat Should yang ditunda, accepted ADR, open business questions yang sudah ditutup, migration compatibility dan operational runbook.
- [ ] Jangan menandai rilis lengkap hanya karena UI tersedia; policy, audit, recovery, scan, NFR dan acceptance harus terverifikasi.

Requirement: NFR-TEST-001 dan seluruh NFR. Bukti: test report per rilis, migration diff, restore timings, load profile, security/UX review serta daftar known limitations.

# Traceability requirements → desain → implementasi → verifikasi

Baseline: [requirements v0.2.0](../requirements.md). Lihat [spec sistem](../../specs/001-system-design/spec.md), [plan](../../specs/001-system-design/plan.md), [tasks](../../specs/001-system-design/tasks.md), dan [suite pengujian](operations-testing.md).

Seluruh **136 requirement FR/NFR dan 43 acceptance criteria** dipetakan di bawah. Semua baris berstatus **dirancang; belum diimplementasikan/diuji runtime**. Kolom AC memetakan skenario yang sudah tertulis pada baseline; tanda “uji turunan” berarti feature spec harus menambahkan kasus khusus, bukan bahwa requirement boleh dilewati. Kode S/A/D/P/U/O merujuk bagian bernomor pada file tertaut. Txx adalah task dalam paket desain sistem.

## 1. Requirement fungsional dan nonfungsional

| ID | Rilis / prioritas | Spec | Desain | Task | Suite / AC baseline |
| --- | --- | --- | --- | --- | --- |
| FR-ACC-001 | R1; Must | S1 | [A4](architecture.md) | T01 | access-policy; Uji turunan per field/guard requirement ini |
| FR-ACC-002 | R1; Must | S1 | [A4](architecture.md) | T01 | access-policy; AC-03 |
| FR-ACC-003 | R1; Must | S1 | [A4](architecture.md) | T01 | access-policy; AC-03 |
| FR-ACC-004 | R1; Must | S1 | [A4](architecture.md) | T01 | access-policy; AC-03, AC-17 |
| FR-ACC-005 | R1; Must | S1 | [A4](architecture.md) | T01 | access-policy; AC-03, AC-17 |
| FR-NCR-001 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; AC-01, AC-02 |
| FR-NCR-002 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; Uji turunan per field/guard requirement ini |
| FR-NCR-003 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; Uji turunan per field/guard requirement ini |
| FR-NCR-004 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; Uji turunan per field/guard requirement ini |
| FR-NCR-005 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; Uji turunan per field/guard requirement ini |
| FR-NCR-006 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; Uji turunan per field/guard requirement ini |
| FR-NCR-007 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; Uji turunan per field/guard requirement ini |
| FR-NCR-008 | R1; tautan operasional R2; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / analytics; AC-23 |
| FR-CAPA-001 | R1; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / transaction-faults; Uji turunan per field/guard requirement ini |
| FR-CAPA-002 | R1; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / transaction-faults; Uji turunan per field/guard requirement ini |
| FR-CAPA-003 | R1; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / transaction-faults; Uji turunan per field/guard requirement ini |
| FR-CAPA-004 | R1; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / transaction-faults; AC-04, AC-05 |
| FR-CAPA-005 | R1; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / transaction-faults; AC-04, AC-05, AC-06 |
| FR-CAPA-006 | R1; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / transaction-faults; AC-04 |
| FR-CAPA-007 | R1; Must | S2 | [P4](api-workflows.md) | T02 | quality-workflow / transaction-faults; AC-16 |
| FR-AUD-001 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; Uji turunan per field/guard requirement ini |
| FR-AUD-002 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; Uji turunan per field/guard requirement ini |
| FR-AUD-003 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; Uji turunan per field/guard requirement ini |
| FR-AUD-004 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; Uji turunan per field/guard requirement ini |
| FR-AUD-005 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; Uji turunan per field/guard requirement ini |
| FR-AUD-006 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; AC-07 |
| FR-AUD-007 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; AC-07 |
| FR-AUD-008 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; AC-07, AC-16 |
| FR-AUD-009 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; Uji turunan per field/guard requirement ini |
| FR-AUD-010 | R1; supplier R2; Must | S2 | [P4](api-workflows.md) | T04 | quality-workflow; AC-43 |
| FR-DOC-001 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; Uji turunan per field/guard requirement ini |
| FR-DOC-002 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; Uji turunan per field/guard requirement ini |
| FR-DOC-003 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-09 |
| FR-DOC-004 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-05 |
| FR-DOC-005 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-08 |
| FR-DOC-006 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-08, AC-19 |
| FR-DOC-007 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-09 |
| FR-DOC-008 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; Uji turunan per field/guard requirement ini |
| FR-DOC-009 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; Uji turunan per field/guard requirement ini |
| FR-DOC-010 | R1; related records R2/R3; Should | S2 | [D3](data-model.md) | T03 | document-versioning / files; Uji turunan per field/guard requirement ini |
| FR-DOC-011 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-22 |
| FR-DOC-012 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-22 |
| FR-DOC-013 | R1; related records R2/R3; Must | S2 | [D3](data-model.md) | T03 | document-versioning / files; AC-22 |
| FR-ANA-001 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; AC-11 |
| FR-ANA-002 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; AC-11 |
| FR-ANA-003 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; AC-10, AC-11, AC-12 |
| FR-ANA-004 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; AC-10, AC-11 |
| FR-ANA-005 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; AC-13 |
| FR-ANA-006 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; Uji turunan per field/guard requirement ini |
| FR-ANA-007 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; Uji turunan per field/guard requirement ini |
| FR-ANA-008 | R1; Should | S5 | [P7](api-workflows.md) | T05 | analytics; Uji turunan per field/guard requirement ini |
| FR-ANA-009 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; AC-23, AC-24 |
| FR-ANA-010 | R1; Must | S5 | [P7](api-workflows.md) | T05 | analytics; AC-24 |
| FR-ANA-011 | R2/R3; Must | S5 | [P7](api-workflows.md) | T07–T12 | analytics; AC-41 |
| FR-ANA-012 | R2; Must | S5 | [P7](api-workflows.md) | T07 | analytics; AC-29 |
| FR-LOG-001 | Semua; Must | S5 | [A5](architecture.md) | T01 | transaction-faults / access-policy; AC-01 |
| FR-LOG-002 | Semua; Must | S5 | [A5](architecture.md) | T01 | transaction-faults / access-policy; AC-14 |
| FR-LOG-003 | Semua; Must | S5 | [A5](architecture.md) | T01 | transaction-faults / access-policy; AC-14 |
| FR-LOG-004 | Semua; Must | S5 | [A5](architecture.md) | T01 | transaction-faults / access-policy; AC-14 |
| FR-LOG-005 | Semua; Must | S5 | [A5](architecture.md) | T01 | transaction-faults / access-policy; Uji turunan per field/guard requirement ini |
| FR-LOG-006 | Semua; Must | S5 | [A5](architecture.md) | T01 | transaction-faults / access-policy; Uji turunan per field/guard requirement ini |
| FR-COM-001 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A4](architecture.md) | T01 | access-policy / files / ui-i18n; Uji turunan per field/guard requirement ini |
| FR-COM-002 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A7](architecture.md) | T01 | access-policy / files / ui-i18n; AC-18 |
| FR-COM-003 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A4](architecture.md) | T01 | access-policy / files / ui-i18n; AC-13 |
| FR-COM-004 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A4](architecture.md) | T01 | access-policy / files / ui-i18n; Uji turunan per field/guard requirement ini |
| FR-COM-005 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A4](architecture.md) | T01 | access-policy / files / ui-i18n; Uji turunan per field/guard requirement ini |
| FR-COM-006 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A4](architecture.md) | T01 | access-policy / files / ui-i18n; Uji turunan per field/guard requirement ini |
| FR-COM-007 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A4](architecture.md) | T01 | access-policy / files / ui-i18n; AC-22 |
| FR-COM-008 | R1; master/reminder modul R2/R3; Must | S1/S5 | [U7](ui-ux.md) | T01 | access-policy / files / ui-i18n; AC-40 |
| FR-COM-009 | R1; master/reminder modul R2/R3; Must | S1/S5 | [A6](architecture.md) | T06/T08/T09/T10 | access-policy / files / ui-i18n; AC-41 |
| FR-COM-010 | R1; master/reminder modul R2/R3; Must | S1/S5 | [D3](data-model.md) | T01/T06/T09/T10/T11 | access-policy / files / ui-i18n; Uji turunan per field/guard requirement ini |
| FR-TSK-001 | R1; sumber mengikuti modul; Must | S2 | [A6](architecture.md) | T05 | task-notification; AC-21 |
| FR-TSK-002 | R1; sumber mengikuti modul; Must | S2 | [A6](architecture.md) | T05 | task-notification; AC-20 |
| FR-TSK-003 | R1; sumber mengikuti modul; Must | S2 | [A6](architecture.md) | T05 | task-notification; AC-20, AC-21 |
| FR-TSK-004 | R1; sumber mengikuti modul; Must | S2 | [A6](architecture.md) | T05 | task-notification; AC-20 |
| FR-TSK-005 | R1; sumber mengikuti modul; Must | S2 | [A6](architecture.md) | T05 | task-notification; AC-21 |
| FR-QPL-001 | R2; Must | S3 | [D4](data-model.md) | T06 | document-versioning / inspection-calibration; Uji turunan per field/guard requirement ini |
| FR-QPL-002 | R2; Must | S3 | [D4](data-model.md) | T06 | document-versioning / inspection-calibration; AC-25 |
| FR-QPL-003 | R2; Must | S3 | [D4](data-model.md) | T06 | document-versioning / inspection-calibration; AC-25 |
| FR-QPL-004 | R2; Must | S3 | [D4](data-model.md) | T06 | document-versioning / inspection-calibration; AC-25, AC-42 |
| FR-QPL-005 | R2; Must | S3 | [D4](data-model.md) | T06 | document-versioning / inspection-calibration; AC-25, AC-42 |
| FR-QPL-006 | R2; Must | S3 | [D4](data-model.md) | T06 | document-versioning / inspection-calibration; AC-42 |
| FR-INS-001 | R2; Must | S3 | [P6](api-workflows.md) | T07 | inspection-calibration; Uji turunan per field/guard requirement ini |
| FR-INS-002 | R2; Must | S3 | [P6](api-workflows.md) | T07 | inspection-calibration; AC-26 |
| FR-INS-003 | R2; Must | S3 | [P6](api-workflows.md) | T07 | inspection-calibration; AC-26 |
| FR-INS-004 | R2; Must | S3 | [P6](api-workflows.md) | T07 | inspection-calibration; AC-27 |
| FR-INS-005 | R2; Must | S3 | [P6](api-workflows.md) | T07 | inspection-calibration; AC-28 |
| FR-INS-006 | R2; Must | S3 | [P6](api-workflows.md) | T07 | inspection-calibration; AC-28 |
| FR-INS-007 | R2; Must | S3 | [P6](api-workflows.md) | T07 | inspection-calibration; AC-28 |
| FR-CAL-001 | R2; Must | S3 | [P5](api-workflows.md) | T06 | inspection-calibration; Uji turunan per field/guard requirement ini |
| FR-CAL-002 | R2; Must | S3 | [P5](api-workflows.md) | T06 | inspection-calibration; AC-30 |
| FR-CAL-003 | R2; Must | S3 | [P5](api-workflows.md) | T06 | inspection-calibration; AC-30 |
| FR-CAL-004 | R2; Must | S3 | [P5](api-workflows.md) | T06 | inspection-calibration; AC-27, AC-30 |
| FR-CAL-005 | R2; Must | S3 | [P5](api-workflows.md) | T06 | inspection-calibration; AC-30 |
| FR-CAL-006 | R2; Must | S3 | [P5](api-workflows.md) | T06 | inspection-calibration; AC-30 |
| FR-MNT-001 | R2; Must | S3 | [P6](api-workflows.md) | T08 | maintenance-supplier; Uji turunan per field/guard requirement ini |
| FR-MNT-002 | R2; Must | S3 | [P6](api-workflows.md) | T08 | maintenance-supplier; AC-31 |
| FR-MNT-003 | R2; Must | S3 | [P6](api-workflows.md) | T08 | maintenance-supplier; AC-31, AC-32 |
| FR-MNT-004 | R2; Must | S3 | [P6](api-workflows.md) | T08 | maintenance-supplier; AC-31, AC-32 |
| FR-MNT-005 | R2; Must | S3 | [P6](api-workflows.md) | T08 | maintenance-supplier; AC-32 |
| FR-SUP-001 | R2; Must | S3 | [D4](data-model.md) | T09 | maintenance-supplier / access-policy; AC-34 |
| FR-SUP-002 | R2; Must | S3 | [D4](data-model.md) | T09 | maintenance-supplier / access-policy; AC-34 |
| FR-SUP-003 | R2; Must | S3 | [D4](data-model.md) | T09 | maintenance-supplier / access-policy; AC-33 |
| FR-SUP-004 | R2; Must | S3 | [D4](data-model.md) | T09 | maintenance-supplier / access-policy; AC-33 |
| FR-SUP-005 | R2; Must | S3 | [D4](data-model.md) | T09 | maintenance-supplier / access-policy; AC-34 |
| FR-SUP-006 | R2; Must | S3 | [D4](data-model.md) | T09 | maintenance-supplier / access-policy; AC-34 |
| FR-TRN-001 | R3; Must | S4 | [P5](api-workflows.md) | T10 | training-compliance-ai; Uji turunan per field/guard requirement ini |
| FR-TRN-002 | R3; Must | S4 | [P5](api-workflows.md) | T10 | training-compliance-ai; AC-35 |
| FR-TRN-003 | R3; Must | S4 | [P5](api-workflows.md) | T10 | training-compliance-ai; AC-35 |
| FR-TRN-004 | R3; Must | S4 | [P5](api-workflows.md) | T10 | training-compliance-ai; AC-35 |
| FR-TRN-005 | R3; Must | S4 | [P5](api-workflows.md) | T10 | training-compliance-ai; AC-35 |
| FR-TRN-006 | R3; Must | S4 | [P5](api-workflows.md) | T10 | training-compliance-ai; AC-36 |
| FR-CMP-001 | R3; Must | S4 | [P6](api-workflows.md) | T11 | training-compliance-ai; Uji turunan per field/guard requirement ini |
| FR-CMP-002 | R3; Must | S4 | [P6](api-workflows.md) | T11 | training-compliance-ai; AC-43 |
| FR-CMP-003 | R3; Must | S4 | [P6](api-workflows.md) | T11 | training-compliance-ai; AC-37 |
| FR-CMP-004 | R3; Must | S4 | [P6](api-workflows.md) | T11 | training-compliance-ai; AC-37 |
| FR-CMP-005 | R3; Must | S4 | [P6](api-workflows.md) | T11 | training-compliance-ai; AC-37, AC-43 |
| FR-AI-001 | R3; Must | S4 | [A8](architecture.md) | T12 | training-compliance-ai; AC-38, AC-39 |
| FR-AI-002 | R3; Must | S4 | [A8](architecture.md) | T12 | training-compliance-ai; AC-38, AC-39 |
| FR-AI-003 | R3; Must | S4 | [A8](architecture.md) | T12 | training-compliance-ai; AC-38 |
| FR-AI-004 | R3; Must | S4 | [A8](architecture.md) | T12 | training-compliance-ai; AC-38 |
| NFR-SEC-001 | Semua; NFR wajib diverifikasi | S1/S5 | [A4](architecture.md) | T01 | access-policy / files; Uji turunan per field/guard requirement ini |
| NFR-SEC-002 | Semua; NFR wajib diverifikasi | S1/S5 | [A7](architecture.md) | T01 | access-policy / files; AC-18 |
| NFR-DAT-001 | Semua; NFR wajib diverifikasi | S5 | [D6](data-model.md) | T01 | transaction-faults / inspection-calibration; AC-15 |
| NFR-DAT-002 | Semua; NFR wajib diverifikasi | S5 | [D6](data-model.md) | T01 | transaction-faults / inspection-calibration; AC-19 |
| NFR-PER-001 | Rilis terkait; NFR wajib diverifikasi | S5/S7 | [O5](operations-testing.md) | T13 | load; Uji turunan per field/guard requirement ini |
| NFR-REL-001 | Semua; NFR wajib diverifikasi | S6 | [O4](operations-testing.md) | T13 | operations / recovery; Uji turunan per field/guard requirement ini |
| NFR-OBS-001 | Semua; NFR wajib diverifikasi | S6 | [O5](operations-testing.md) | T01 | operations; Uji turunan per field/guard requirement ini |
| NFR-UX-001 | Semua; NFR wajib diverifikasi | S1 | [U3](ui-ux.md) | T13 | ui-i18n; Uji turunan per field/guard requirement ini |
| NFR-COMP-001 | Semua; NFR wajib diverifikasi | S1 | [U7](ui-ux.md) | T13 | ui-i18n; Uji turunan per field/guard requirement ini |
| NFR-TEST-001 | Semua; NFR wajib diverifikasi | S7 | [O6](operations-testing.md) | T13 | Seluruh suite; Uji turunan per field/guard requirement ini |
| NFR-DAT-003 | Semua; NFR wajib diverifikasi | S5 | [D1](data-model.md) | T06/T07/T09 | transaction-faults / inspection-calibration; AC-26 |
| NFR-JOB-001 | Semua; NFR wajib diverifikasi | S6 | [A6](architecture.md) | T01 | operations / task-notification; AC-31 |
| NFR-PER-002 | Rilis terkait; NFR wajib diverifikasi | S5/S7 | [O5](operations-testing.md) | T13 | load; Uji turunan per field/guard requirement ini |
| NFR-AI-001 | R3; NFR wajib diverifikasi | S4/S6 | [A8](architecture.md) | T12 | training-compliance-ai; AC-38 |
| NFR-I18N-001 | Semua; NFR wajib diverifikasi | S1 | [U7](ui-ux.md) | T01 | ui-i18n; Uji turunan per field/guard requirement ini |

## 2. Acceptance criteria dan test intent

Setiap baris memerlukan fixture happy path dan kasus negatif pada sumber requirements. Implementasi test memakai ID AC pada nama/metadata agar bukti dapat ditemukan.

| AC | Requirement sumber | Task terkait | Test intent |
| --- | --- | --- | --- |
| AC-01 | FR-NCR-001, FR-LOG-001 | T02 | Submit valid menghasilkan nomor/status/event atomik |
| AC-02 | FR-NCR-001 | T02 | Required-field failure tidak menghasilkan partial submission |
| AC-03 | FR-ACC-002–005 | T01 | Isolasi role/site pada semua entry point |
| AC-04 | FR-CAPA-004–006 | T02 | Closure ditolak bila tindakan/efektivitas belum lengkap |
| AC-05 | FR-CAPA-004–005, FR-DOC-004 | T02/T03 | Independensi author/pelaksana tetap berlaku bagi Manager |
| AC-06 | FR-CAPA-005 | T02 | Efektivitas gagal kembali Draft dengan histori |
| AC-07 | FR-AUD-006–008 | T04 | Report approval terpisah dari close audit |
| AC-08 | FR-DOC-005–006 | T03 | Tanggal efektif dan pergantian revisi atomik |
| AC-09 | FR-DOC-003, FR-DOC-007 | T03 | Submission immutable dan pembaca mendapat Effective |
| AC-10 | FR-ANA-003–004 | T05 | Pareto 5/3/2 dan kumulatif 50/80/100 |
| AC-11 | FR-ANA-001–004 | T05 | Data kosong tanpa divide-by-zero |
| AC-12 | FR-ANA-003 | T05 | Reopen tidak menambah first-submitted cohort |
| AC-13 | FR-COM-003, FR-ANA-005 | T01/T05 | Overdue kalender perusahaan dan dedupe notification |
| AC-14 | FR-LOG-002–004 | T01/T02 | Audit insert failure membatalkan business write |
| AC-15 | NFR-DAT-001 | T01 | Edit versi usang menghasilkan conflict |
| AC-16 | FR-CAPA-007, FR-AUD-008 | T02/T04 | Reopen propagasi graph satu transaksi |
| AC-17 | FR-ACC-004–005 | T01 | Nonaktif/cabut membership berlaku request berikutnya |
| AC-18 | FR-COM-002, NFR-SEC-002 | T01 | Upload oversize/spoof/malware tidak tersedia |
| AC-19 | FR-DOC-006, NFR-DAT-002 | T03 | Dua publish/retry tetap satu Effective |
| AC-20 | FR-TSK-002–004 | T05 | Task proyeksi tidak dapat melewati approval |
| AC-21 | FR-TSK-001, FR-TSK-003, FR-TSK-005 | T05 | Standalone completion/reopen serta assignment scope |
| AC-22 | FR-DOC-011–013, FR-COM-007 | T01/T03/T05 | Folder/search/preview tetap tunduk ACL |
| AC-23 | FR-NCR-008, FR-ANA-009 | T02/T05 | Biaya correction chain tidak double count |
| AC-24 | FR-ANA-009–010 | T05 | Bucket aging tepat dan refresh/stale |
| AC-25 | FR-QPL-002–005 | T06/T07 | Plan revisi baru tidak mengubah inspeksi lama |
| AC-26 | FR-INS-002–003, NFR-DAT-003 | T07 | Batas inklusif, precision, null dan unit |
| AC-27 | FR-INS-004, FR-CAL-004 | T06/T07 | Alat tidak valid ditolak UI dan API |
| AC-28 | FR-INS-005–007 | T07 | Completeness/NCR/finalize/amend/disposition guards |
| AC-29 | FR-ANA-012 | T07 | Unit gagal unik dan denominator nol |
| AC-30 | FR-CAL-002–006 | T06/T07 | Calibration Fail hold serta impact review |
| AC-31 | FR-MNT-002–004, NFR-JOB-001 | T08 | Jan 31 recurrence/retry/anchor |
| AC-32 | FR-MNT-003–005 | T08 | Independent work-order verification, tidak release alat |
| AC-33 | FR-SUP-003–004 | T09 | Weighted score/total bobot/independent approval |
| AC-34 | FR-SUP-001–002, FR-SUP-005–006 | T09 | Expiry/provenance/site isolation pemasok |
| AC-35 | FR-TRN-002–005 | T10 | Pending versus Qualified, expiry dan renewal dedupe |
| AC-36 | FR-TRN-006 | T10 | SOP revision dan explicit retraining decision |
| AC-37 | FR-CMP-003–005 | T11 | Applicability/readiness 62.5 persen dan all-N/A |
| AC-38 | FR-AI-001–004, NFR-AI-001 | T12 | AI injection/site/citation dan draft-only accept |
| AC-39 | FR-AI-001–002 | T12 | AI timeout/insufficient evidence, manual tetap tersedia |
| AC-40 | FR-COM-008, NFR-I18N-001 | T01/T05 | Locale preference, Unicode, numeric/timezone invariance |
| AC-41 | FR-COM-009, FR-ANA-011 | T01/T06/T08/T09/T10 | Reminder thresholds/cycles/widget filter labels |
| AC-42 | FR-QPL-004–006 | T06 | Concurrent plan publication dan withdrawn/obsolete |
| AC-43 | FR-AUD-010, FR-CMP-002, FR-CMP-005 | T03/T04/T11 | Template/source update menjaga approved snapshot |

## 3. Bukti implementasi yang harus ditambahkan

Saat task dikerjakan, tambahkan path feature spec, nama test yang benar-benar dibuat, command/run ID dan hasil. Jangan mengganti status desain menjadi lulus hanya berdasarkan keberadaan kode atau mock UI. Perubahan requirement wajib memperbarui baris terkait, fixture, API contract dan ADR bila keputusan arsitektur berubah.

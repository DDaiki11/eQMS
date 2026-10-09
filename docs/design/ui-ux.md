# Rancangan UI/UX

Kembali ke [indeks desain](../design.md). Dokumen ini adalah rancangan layar dan perilaku; bukan screenshot aplikasi yang telah dibuat. Referensi: FR-COM-004/007/008, FR-ANA, modul R1–R3, NFR-UX-001, NFR-COMP-001, NFR-I18N-001.

## U1. Navigasi dan layout

Route UI memakai `/{locale}/...` dengan `locale=id|en|ja`; default preferensi user atau id. Header menampilkan search, selected site, notifications, language dan profile. Sidebar dapat diciutkan dan hanya menampilkan modul yang dirilis serta berizin. Deep link tetap diotorisasi server.

| Route setelah locale | Layar | Rilis |
| --- | --- | --- |
| `/dashboard`, `/analytics` | Ringkasan kualitas, tugas, tren, Pareto, biaya, aging dan KPI modul | R1–R3 |
| `/tasks` | My Tasks / tugas site sesuai izin; detail drawer | R1 |
| `/ncr`, `/ncr/[id]`, `/capa/[id]` | List, create wizard singkat, detail workflow, action dan effectiveness | R1 |
| `/audits`, `/audits/[id]`, `/findings/[id]` | Calendar/list, checklist, laporan dan follow-up | R1 |
| `/documents`, `/documents/[id]/revisions/[revision]` | Folder/table, preview, revisions, review, approval | R1 |
| `/audit-trail`, `/settings` | Event read-only; user/site/master/configuration sesuai izin | R1 |
| `/quality-plans`, `/quality-plans/[id]/revisions/[revision]` | Plan list dan drawing editor | R2 |
| `/inspections`, `/inspections/[id]` | Grid sample, review, amendment dan disposition | R2 |
| `/calibration`, `/equipment/[id]` | Register, due dates, calibration records dan impact review | R2 |
| `/maintenance`, `/assets/[id]`, `/work-orders/[id]` | Schedule, checklist work order dan histori | R2 |
| `/suppliers`, `/suppliers/[id]` | Profil per site, sertifikat, evaluasi/assessment dan linked records | R2 |
| `/training`, `/training/matrix`, `/training/records/[id]` | My courses, catalog, assignments, matrix dan verification | R3 |
| `/compliance`, `/compliance/assessments/[id]` | Standard readiness, clause/evidence, gap dan AI drawer | R3 |

## U2. Wireframe dashboard desktop

```text
┌───────────────┬────────────────────────────────────────────────────────┐
│ e-QMS         │ Cari nomor/judul...   Site ▾   Notifikasi   ID ▾ Profil│
│ Dashboard     ├────────────────────────────────────────────────────────┤
│ Tugas         │ Dashboard       Periode ▾ Kategori ▾      + Buat NCR  │
│ NCR & CAPA    │ Diperbarui 10:35 WIB                         Refresh    │
│ Audit         ├───────────────────────────┬────────────────────────────┤
│ Dokumen       │ Ringkasan kualitas        │ Tugas saya                 │
│ Analisis      │ Backlog / overdue         │ Review, approval, tindakan │
│ ...           ├───────────────────────────┼────────────────────────────┤
│               │ Tren NCR 12 bulan         │ Pareto defect              │
│               │ Grafik + tabel data       │ Bar + kumulatif + tabel    │
│               ├─────────────┬─────────────┼─────────────┬──────────────┤
│               │ NCR terbuka │ CAPA overdue│ Temuan      │ Doc approval │
│               ├─────────────┴─────────────┼─────────────┴──────────────┤
│               │ Biaya scrap/rework        │ Aging NCR / per site       │
└───────────────┴───────────────────────────┴────────────────────────────┘
```

Semua kartu mengarah ke daftar terfilter. Ringkasan berupa angka faktual, bukan paragraf AI atau skor buatan. Widget biaya menyebut mata uang dan cakupan biaya tercatat. Tugas menampilkan label “Semua tugas aktif; tidak dibatasi periode”. R2/R3 menambahkan tab operasional/kompetensi/compliance agar dashboard awal tidak terlalu padat. Perubahan scope mereset selection serta memuat ulang seluruh widget; request lama dibatalkan/diabaikan agar data scope lama tidak tercampur.

## U3. Komponen visual dan aksesibilitas

| Komponen | Baseline |
| --- | --- |
| Warna | Background `#F8FAFC`, surface putih, teks `#0F172A`, primary `#1D4ED8`; semantik warning/danger/success disertai teks/icon |
| Tipografi | Sans-serif sistem dengan fallback font Jepang; body 14–16 px, heading 20–28 px; tabular numbers untuk grid |
| Spacing | Skala 4/8/12/16/24/32 px; radius 8 px; shadow ringan pada overlay saja |
| DataTable | Search, filter chips, sort, column chooser, cursor pagination, visible result scope; row menu berlabel |
| StatusBadge | Label terjemahan dari kode stabil; warna tidak menjadi satu-satunya pembeda |
| EntityHeader | Number, title, state, version, site, owner/due, action utama sesuai allowedActions |
| Tabs | Ringkasan, bukti, terkait, keputusan dan timeline; tab domain khusus jika dibutuhkan |
| WorkflowActionDialog | Ringkasan snapshot/versi, reason/evidence, prasyarat belum terpenuhi, submit disabled saat request |
| FileUploader | Size/type, upload progress, Pending scan/Clean/Rejected; file pending tidak memberi kesan sudah siap approval |
| AuditTimeline | Actor/time/aksi/reason; diff fields berizin; event read-only |
| ConflictPanel | Informasi versi berubah, input lokal dipertahankan, aksi muat versi baru/bandingkan; tidak overwrite otomatis |

Palette merupakan token usulan; setiap kombinasi teks/background tetap harus diuji contrast sebelum dinyatakan memenuhi WCAG 2.2 AA. Focus ring, skip link, semantic heading, error summary, aria status dan keyboard navigation wajib. Field required ditandai teks serta atribut, bukan warna saja. Form tidak menghapus input ketika server error.

## U4. Alur layar per domain

| Modul | Rancangan layar/interaksi |
| --- | --- |
| NCR | Form identitas/site/area → masalah/severity/kategori → quantity/source/bukti → save draft atau submit. Detail menampilkan containment, CAPA links, costs, closure prerequisites dan timeline. Severity/owner/due correction meminta reason. |
| CAPA | Tab problem/root cause, plan/action list, approval, effectiveness dan closure. Tampilkan status tiap action dengan PIC/due. Tombol close memperlihatkan action/bukti/verifikasi yang belum selesai. |
| Audit | List/calendar; scheduler mengecualikan auditor dengan area conflict. Checklist memperlihatkan question/reference, selected evidence revision dan answer berdampingan. Laporan approval dan close audit dua aksi terpisah. |
| Dokumen | Folder breadcrumb + table; drawer preview/details/revisions/approval. Default Effective; label Draft/Superseded/Obsolete pada preview dan file. Picker linked document memilih revision eksplisit. |
| Tasks | View “Saya” dan “Site” bila berizin. Task workflow menampilkan tombol “Buka sumber”/aksi workflow; tidak memiliki generic Done. Standalone mempunyai status, PIC, due, comment dan evidence. |
| Calibration | Tabel alat dengan operational state dan calibration validity terpisah. Form record hasil/sertifikat, verification drawer, impact list berisi inspeksi terkait dan keputusan. |
| Maintenance | Tab assets/schedules/work orders. Work order checklist dengan start/end/downtime/evidence; verify terpisah dari complete implementation. Kalender memperlihatkan occurrence tetap serta postponed due. |
| Supplier | Tabs profile/site status, certificates, evaluation, assessment, issues/audits. Skor/bobot dan rumus terlihat; status approval pemasok diubah melalui aksi tersendiri. |

## U5. Quality plan dan inspeksi

```text
Quality Plan / PN-001 / Revision 2                  In Review   Actions ▾
┌──────────────────────┬──────────────────────────────┬──────────────────┐
│ Karakteristik        │ Drawing: page 1/2   - 100% + │ Detail terpilih  │
│ 1  Diameter          │                              │ Tipe / unit      │
│ 2  Panjang           │       (1)          (2)       │ Nominal / limits │
│ 3  Visual            │                              │ Metode / alat    │
│ + Tambah             │ Balloon dapat dipilih        │ Sampel / wajib   │
└──────────────────────┴──────────────────────────────┴──────────────────┘
Inspeksi / LOT-001 / Plan revision 2                  Draft  Simpan Submit
┌─────┬───────────────────────┬─────────────┬────────┬────────┬───────────┐
│ No. │ Karakteristik / unit  │ LSL—USL     │ Unit 1 │ Unit 2 │ Alat      │
│ 1   │ Diameter mm           │ 9.50—10.50  │ 10.00✓ │ 10.51✗ │ EQ-001    │
│ 2   │ Visual                │ Kriteria...│ Pass   │ Belum  │ N/A sah   │
└─────┴───────────────────────┴─────────────┴────────┴────────┴───────────┘
```

Balloon menyimpan page dan koordinat ternormalisasi, bukan piksel viewport. Pilih balloon menyorot baris karakteristik; keyboard/table alternative memungkinkan edit tanpa drag. Drawing read-only ketika submitted; “revisi kerja baru” adalah aksi eksplisit.

Grid mendukung Tab/Shift-Tab/arrows yang tidak menjebak fokus, sticky header, unit/toleransi selalu terlihat, serta validasi numerik server. Paste block hanya ke expected cells, menampilkan ringkasan validasi sebelum simpan. Nilai Pass/Fail provisional di client diberi status belum tersimpan sampai respons server. Simpan menampilkan saved version/time; leave page dengan unsaved edits memunculkan konfirmasi. Finalize read-only; “Buat amendment” membuat versi baru tanpa mengosongkan histori final.

## U6. Training dan compliance

Training matrix: baris personel, kolom kompetensi/kursus, filters site/jabatan/status, legend dan list alternative. Klik cell membuka record dengan course revision, required/achieved level, attempts, evidence, verifier dan expiry. Tidak ada dropdown bebas untuk mengubah Qualified tanpa verifikasi. Admin training dapat membuka retraining decision dari revisi SOP terkait.

Compliance: selector standard/edition/assessment; header menampilkan Approved/provisional, assessed date, readiness formula serta gap. Bagian klausul menampilkan status, applicability reason, evidence revision/location dan owner/due. Drawer AI menampilkan manifest sumber, progres/error, suggestion dengan citation dan pilihan accept/reject beralasan. Tombol accept berlabel “Buat draft dari usulan”; approval assessment tetap aksi terpisah. Tidak ada badge “Certified” dari readiness.

## U7. Responsivitas, state dan bahasa

- Uji lebar 360/768/1440 px. Sidebar menjadi drawer pada layar sempit; panel detail menjadi halaman penuh; dashboard satu kolom. Tabel/grid boleh horizontal scroll dengan row identity tetap terlihat dan aksi tidak terpotong.
- Semua halaman memiliki loading skeleton, no records, no match, permission denied/not found, dependency error/retry, conflict dan unsaved state. Empty state tidak diisi data demo produksi.
- Grafik mempunyai tabel alternatif; progres job menggunakan text state, bukan spinner tanpa batas. Polling berhenti saat tab tidak aktif dan refresh saat kembali.
- Locale route dan user preference diselaraskan; pesan error memakai key terjemahan dari server. Input tanggal memakai date bisnis, decimal dinormalisasi sebelum API; timezone perusahaan dicantumkan pada due date/laporan yang ambigu.
- Nomor bisnis, status API, nama input pengguna dan dokumen asli tidak diterjemahkan otomatis. Japanese layout diuji untuk wrap, font coverage dan panjang label; CSV/PDF memakai Unicode serta font Jepang yang berlisensi tersedia.

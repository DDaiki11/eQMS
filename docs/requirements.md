# Requirements — Enterprise Quality Management System (e-QMS)

| Atribut | Nilai |
| --- | --- |
| Versi | 0.2.0 |
| Status | Draft kebutuhan diperluas |
| Tanggal | 9 Oktober 2026 |
| Pendekatan | Spec-driven development |
| Target rilis | Aplikasi web internal; R1 fondasi/MVP, R2 operasional kualitas, R3 kompetensi dan compliance |

Dokumen ini menjadi dasar spesifikasi fitur, desain, implementasi, dan acceptance test. Detail yang diberi label **asumsi** merupakan usulan awal, bukan keputusan bisnis yang sudah disetujui. Nama modul dan interaksi visual di bagian 2.4.

**Status implementasi:** pada saat revisi ini, repository hanya memiliki dokumen requirements, belum kode aplikasi. Penambahan di sini merupakan spesifikasi fitur untuk implementasi berikutnya, bukan fitur yang sudah berjalan. Catatan observasi tersedia di [referensi video](video-reference.md).

## 1. Tujuan produk

Menyediakan satu sistem untuk mencatat ketidaksesuaian, menyelesaikan tindakan perbaikan, menjalankan audit, mengendalikan dokumen, merencanakan dan mencatat inspeksi, mengelola alat/aset, mengevaluasi pemasok, memantau kompetensi, serta menilai kesiapan compliance. Setiap keputusan dan perubahan harus dapat ditelusuri, dengan akses sesuai tanggung jawab pengguna.

Hasil yang diharapkan:

- Inspector dapat mencatat masalah beserta bukti secara konsisten.
- Supervisor dapat menilai, menugaskan, dan memantau penyelesaian masalah.
- Manager dapat menyetujui keputusan penting dan melihat kondisi kualitas lintas site yang menjadi kewenangannya.
- Tim dapat menelusuri hubungan antara NCR, CAPA, temuan audit, dan revisi dokumen.
- Metrik dashboard dapat direkonsiliasi dengan data sumber melalui drill-down.
- Hasil inspeksi terhubung ke revisi quality plan, gambar, alat ukur, serta NCR bila terjadi kegagalan.
- Kalibrasi, maintenance, sertifikat pemasok, dan kompetensi yang jatuh tempo dapat ditindaklanjuti melalui tugas terpusat.
- Kesiapan compliance ditelusuri sampai klausul dan bukti; bantuan AI tetap memerlukan penilaian manusia.

## 2. Ruang lingkup dan asumsi

### 2.1 Cakupan produk dan rilis

**R1 — MVP/fondasi**, melanjutkan cakupan awal:

1. Nonconformity Report (NCR) dan Corrective and Preventive Action (CAPA).
2. Audit internal, checklist, temuan, dan tindak lanjut.
3. Dokumen terkendali, revisi, review, approval, dan publikasi.
4. Dashboard, analisis defect, Pareto chart, dan tren kualitas.
5. Role-based access control untuk Inspector, Supervisor, dan Manager.
6. Audit trail atas perubahan data dan keputusan workflow.
7. Task management terpusat, folder/preview dokumen, pencarian global, pilihan bahasa, dan analitik biaya/aging dasar.

**R2 — Operasional kualitas**, ditambahkan berdasarkan video:

1. Quality plans, revisi drawing, ballooning manual, karakteristik, dan toleransi.
2. Inspeksi incoming/in-process/final berbasis sampel dan keterlacakan hasil.
3. Register alat ukur, kalibrasi, sertifikat, dan pemantauan jatuh tempo.
4. Preventive maintenance, jadwal berulang, dan work order.
5. Supplier quality: profil, sertifikasi, evaluasi, assessment, dan tindakan terkait.

**R3 — Kompetensi dan compliance**, ditambahkan berdasarkan video:

1. Katalog training, penugasan, bukti hasil, dan matriks kompetensi.
2. Compliance dashboard berbasis klausul, bukti, gap, dan penilaian manusia.
3. AI auditor sebagai bantuan analisis dengan sumber yang dapat diperiksa.

R2/R3 termasuk cakupan produk yang diminta; pemisahan rilis merupakan urutan delivery usulan. R1 selesai tidak berarti seluruh cakupan video selesai.

### 2.2 Asumsi awal

| ID | Asumsi kerja |
| --- | --- |
| ASM-01 | Satu perusahaan, dengan satu atau beberapa site/pabrik; SaaS multi-tenant di luar MVP. |
| ASM-02 | Pengguna memiliki satu role aktif serta daftar site yang boleh diakses. Data operasional dimiliki satu site; dokumen dapat berlaku pada beberapa site. |
| ASM-03 | Antarmuka awal berbahasa Indonesia, ada opsi untuk mengganti bahasa menjadi bahasa inggris dan bahasa jepang; istilah NCR, CAPA, dan nama status teknis boleh dipertahankan. |
| ASM-04 | Timestamp disimpan dalam UTC. Tampilan dan batas periode laporan menggunakan zona waktu perusahaan, default Asia/Jakarta. |
| ASM-05 | Manager mengelola pengguna, role, site, dan master data melalui izin administratif yang terpisah dari izin approval. Bootstrap Manager dilakukan melalui prosedur deployment. |
| ASM-06 | Approval MVP adalah persetujuan internal terautentikasi, belum merupakan tanda tangan elektronik tersertifikasi. |
| ASM-07 | Preventive action menggunakan alur CAPA yang sama, dengan jenis tindakan dan sumber risiko dicatat secara eksplisit. |
| ASM-08 | Inspector/Supervisor/Manager tetap menjadi role dasar. Pelaksana kalibrasi, teknisi, trainer, dan evaluator pemasok memakai penugasan serta izin modul, bukan otomatis role baru. |
| ASM-09 | Input inspeksi dan hasil kalibrasi awal dilakukan manual; nominal, batas, dan unit harus eksplisit. Tidak mengasumsikan pembacaan mesin atau interpretasi GD&T otomatis. |
| ASM-10 | Satu mata uang perusahaan digunakan untuk analitik biaya, baseline IDR. Nilai kosong berarti belum dicatat, bukan nol. |
| ASM-11 | Evaluasi pemasok dan register alat/aset dimiliki satu site; pemasok bersama memakai identitas pusat dengan catatan evaluasi terpisah per site. |
| ASM-12 | Compliance menggunakan katalog standar/klausul berversi yang disediakan secara sah. Formula kesiapan internal bukan hasil sertifikasi. |

### 2.3 Di luar cakupan baseline R1–R3

- Skor kesehatan QMS generatif tanpa formula/bukti, keputusan approval otomatis oleh AI, dan klaim sertifikasi otomatis.
- SPC/control chart, OCR/ballooning otomatis, interpretasi GD&T otomatis, dan penerimaan lot otomatis berdasarkan standar sampling.
- Akuisisi data inspeksi massal otomatis, integrasi mesin/IoT, ERP, MES, serta impor historis. Inspeksi sampel manual masuk R2.
- Aplikasi mobile native, mode offline, SSO, dan notifikasi email/WhatsApp.
- Klaim sertifikasi ISO atau kepatuhan regulasi tertentu tanpa analisis kebutuhan tambahan.
- Portal/login pemasok eksternal, e-learning SCORM, pengadaan dan persediaan suku cadang penuh.

### 2.4 Pemetaan video ke fitur e-QMS

Timestamp bersifat perkiraan; video merupakan overview singkat, bukan demonstrasi semua aturan bisnis.

| Waktu | Yang terlihat di video | Requirement e-QMS | Rilis |
| --- | --- | --- | --- |
| 00:08–00:10 | Real Time Analytics; tren, Pareto, cost of quality, NCR aging, issues by factory | FR-ANA-001–012 | R1; KPI modul baru mengikuti rilis modul |
| 00:10–00:12 | Task Management; daftar dan form task | FR-TSK-001–005 | R1 |
| 00:12–00:18 | Compliance Dashboard, progres klausul, panel AI Auditor | FR-CMP-001–005, FR-AI-001–004 | R3 |
| 00:18–00:22 | Quality Plans; drawing dengan balloon, nominal, toleransi dan sampling | FR-QPL-001–006 | R2 |
| 00:22–00:29 | Document Control; folder, detail, revisi/riwayat | FR-DOC-001–013 | R1 |
| 00:29–00:36 | Corrective and Preventive Actions; issue database, laporan, containment, foto | FR-NCR-001–008, FR-CAPA-001–007 | R1 |
| 00:36–00:42 | Inspections; grid hasil pengukuran, alat ukur, unduh PDF | FR-INS-001–007 | R2 |
| 00:42–00:50 | Calibration; register, form hasil, detail alat | FR-CAL-001–006 | R2 |
| 00:50–00:54 | Preventive Maintenance; daftar dan detail work order/checklist | FR-MNT-001–005 | R2 |
| 00:54–01:00 | Supplier Quality; sertifikat, evaluasi, self-assessment | FR-SUP-001–006 | R2 |
| 01:00–01:04 | Audits; daftar dan checklist pembandingan bukti terhadap requirement | FR-AUD-001–010 | R1 |
| 01:04–01:08 | Training Management; courses, training matrix, verifikasi record | FR-TRN-001–006 | R3 |

Fitur pengayaan R1 pada bagian 4.1–4.6 mengikuti penanda di sana; bagian 4.7–4.14 menyatakan rilis masing-masing. Kecuali disebut berbeda, requirement awal tetap R1.

## 3. Aktor dan otorisasi

### 3.1 Matriks akses awal

Semua izin berikut dibatasi oleh site yang diberikan. Izin membaca catatan tidak otomatis memberikan izin mengubah atau menyetujuinya.

| Aktivitas | Inspector | Supervisor | Manager |
| --- | --- | --- | --- |
| Membuat NCR | Ya | Ya | Ya |
| Membaca NCR/temuan | Milik sendiri atau ditugaskan | Seluruh site berizin | Seluruh site berizin |
| Mengubah draft NCR | Draft sendiri | Draft sendiri | Draft sendiri |
| Triage NCR, menetapkan owner dan tenggat | Tidak | Ya | Ya |
| Menyusun/melaksanakan CAPA | Jika ditugaskan | Jika ditugaskan | Jika ditugaskan |
| Review rencana CAPA | Tidak | Ya, independen | Ya, independen |
| Menyetujui rencana dan penutupan CAPA/NCR | Tidak | Tidak | Ya, independen |
| Membuat jadwal audit dan menetapkan auditor | Tidak | Ya | Ya |
| Mengisi checklist dan mencatat temuan | Jika auditor yang ditugaskan | Jika ditugaskan | Jika ditugaskan |
| Menyetujui laporan/penutupan audit | Tidak | Tidak | Ya, independen |
| Membaca dokumen efektif | Sesuai cakupan dokumen | Sesuai cakupan dokumen | Sesuai cakupan dokumen |
| Membuat revisi dokumen | Jika ditetapkan sebagai author | Ya | Ya |
| Review dokumen | Tidak | Ya, independen | Ya, independen |
| Approval dan publikasi dokumen | Tidak | Tidak | Ya, independen |
| Melihat dashboard dan ekspor | Data yang boleh dibaca | Seluruh site berizin | Seluruh site berizin |
| Melihat audit trail | Catatan yang boleh dibaca | Catatan site berizin | Catatan site berizin |
| Mengelola pengguna dan master data | Tidak | Tidak | Jika diberi izin administratif |

Matriks tambahan berikut menggunakan batas site dan independensi yang sama:

| Aktivitas | Inspector | Supervisor | Manager |
| --- | --- | --- | --- |
| Membuat/mengerjakan tugas mandiri | Milik sendiri atau ditugaskan | Ya, dalam site berizin | Ya, dalam site berizin |
| Membuat quality plan/revisi | Jika ditetapkan author | Ya | Ya |
| Review / approval quality plan | Tidak | Review independen | Review / approval independen |
| Mengisi inspeksi | Jika ditugaskan | Jika ditugaskan | Jika ditugaskan |
| Review hasil / disposisi lot inspeksi | Tidak | Review independen | Review / disposisi independen |
| Mengelola register alat/aset dan jadwal | Tidak | Dengan izin modul | Dengan izin modul |
| Mencatat kalibrasi / mengerjakan work order | Jika ditugaskan dan berizin modul | Jika ditugaskan dan berizin modul | Jika ditugaskan dan berizin modul |
| Memverifikasi kalibrasi / work order | Tidak | Independen, berizin modul | Independen, berizin modul |
| Mengelola profil/evaluasi pemasok | Bukti tugas sendiri | Dengan izin modul | Dengan izin modul |
| Menyetujui evaluasi/status pemasok | Tidak | Tidak | Independen, berizin modul |
| Mengikuti training / membaca record | Milik sendiri | Milik sendiri; tim jika berizin training | Site berizin training |
| Mengelola kursus, matriks, dan penugasan | Tidak | Dengan izin training | Dengan izin training |
| Memverifikasi kompetensi | Tidak | Trainer/verifier independen yang ditugaskan | Trainer/verifier independen yang ditugaskan |
| Mengisi assessment compliance / menjalankan AI | Bukti yang ditugaskan; tanpa AI | Dengan izin compliance | Dengan izin compliance |
| Menyetujui assessment compliance | Tidak | Tidak | Independen, berizin compliance |

Author quality plan tidak boleh mereview/menyetujui revisinya sendiri. Pengisi inspeksi, pelaksana kalibrasi/maintenance, peserta training, serta penyusun evaluasi pemasok/compliance tidak boleh memverifikasi atau menyetujui hasilnya sendiri. Izin modul dikelola melalui FR-ACC-004–005. Inspector tidak memperoleh akses register pemasok, catatan personel lain, atau bukti compliance lain hanya karena ditugaskan satu tugas.

**Independen:** author tidak boleh mereview atau menyetujui revisi dokumennya sendiri. Penyusun/pelaksana CAPA tidak boleh menjadi reviewer, verifier efektivitas, atau approver penutupannya sendiri. Pelapor NCR tidak boleh menyetujui penutupan NCR sendiri. Lead auditor tidak boleh menyetujui laporan atau penutupan auditnya sendiri. Jika pengguna yang memenuhi syarat belum tersedia, workflow tetap tertahan; sistem tidak melewati kontrol ini secara otomatis.

### 3.2 Requirements akses

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-ACC-001 | Sistem menyediakan login, logout, penonaktifan akun, dan pengelolaan sesi yang aman. | Must |
| FR-ACC-002 | Sistem memeriksa role, site, kepemilikan, dan penugasan di server pada setiap operasi baca/tulis, unduhan, ekspor, dan agregasi. | Must |
| FR-ACC-003 | Akses langsung melalui URL/API tidak boleh melewati pembatasan UI atau memperlihatkan keberadaan data site lain. | Must |
| FR-ACC-004 | Perubahan role/site hanya dapat dilakukan pengguna berizin administratif dan selalu tercatat. Akun nonaktif tidak dapat memakai sesi lama. | Must |
| FR-ACC-005 | Pengguna tidak boleh menaikkan hak aksesnya sendiri. Perubahan izin berlaku pada request berikutnya, termasuk untuk sesi yang sedang aktif. | Must |

## 4. Requirements fungsional

**Must** wajib untuk rilis yang ditetapkan (R1, R2, atau R3), bukan semuanya wajib pada MVP R1. **Should** dapat ditunda dengan alasan yang dicatat dalam rencana rilis. Semua modul tambahan mewarisi akses server, audit trail, lampiran privat, notifikasi, concurrency, dan idempotensi pada requirement bersama.

### 4.1 NCR dan CAPA

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-NCR-001 | Pengguna dapat menyimpan draft NCR dengan nomor unik yang dibuat sistem. Submit mensyaratkan judul, deskripsi, site, proses/area, waktu kejadian, kategori defect utama, severity, dan pelapor. | Must |
| FR-NCR-002 | NCR mendukung referensi produk, batch/lot, jumlah diperiksa, jumlah defect, sumber temuan, serta lampiran foto/PDF. Kolom yang tidak relevan boleh kosong dengan keterangan. | Must |
| FR-NCR-003 | Supervisor/Manager melakukan triage: memvalidasi masalah, mencatat containment/disposisi, menunjuk owner, menetapkan tenggat, dan menentukan kebutuhan CAPA. | Must |
| FR-NCR-004 | Penolakan/pengembalian, perubahan severity, perubahan owner/tenggat, dan keputusan tanpa CAPA wajib memiliki alasan. NCR Critical wajib memiliki CAPA sebelum dapat ditutup. | Must |
| FR-NCR-005 | Data NCR setelah submit diubah melalui tindakan koreksi terkontrol oleh Supervisor/Manager dengan alasan dan audit trail; pelapor tidak dapat menimpa laporan yang sudah diajukan. | Must |
| FR-NCR-006 | Penutupan NCR mensyaratkan bukti penyelesaian, seluruh CAPA terkait ditutup, dan persetujuan Manager independen. Tanpa CAPA, dasar keputusan dan bukti koreksi tetap wajib. | Must |
| FR-NCR-007 | Daftar NCR mendukung pencarian, filter status/severity/site/owner/periode, pagination, dan indikator overdue. | Must |
| FR-NCR-008 | NCR memiliki catatan biaya scrap/rework: jenis, jumlah biaya nonnegatif, mata uang perusahaan, tanggal biaya, keterangan, pencatat, dan bukti. Koreksi memakai revisi/reversal tercatat, bukan menimpa histori. R2 menambahkan tautan inspeksi, pemasok, alat, atau aset sumber. | Must |
| FR-CAPA-001 | CAPA dapat berasal dari NCR, temuan audit, atau risiko preventif mandiri. Sumber dan hubungan antarcatatan dapat ditelusuri dua arah. | Must |
| FR-CAPA-002 | CAPA memuat problem statement, analisis akar penyebab, metode analisis seperti 5 Why, jenis tindakan, owner, dan target penyelesaian. CAPA preventif menjelaskan risiko/potensi penyebab. | Must |
| FR-CAPA-003 | Rencana CAPA terdiri dari satu atau lebih action item, masing-masing memiliki PIC, tenggat, status, dan bukti implementasi. | Must |
| FR-CAPA-004 | Rencana harus direview Supervisor/Manager independen lalu disetujui Manager independen sebelum implementasi dinyatakan dimulai. Perubahan substansial setelah approval memerlukan approval ulang. | Must |
| FR-CAPA-005 | Verifikasi efektivitas mencatat kriteria keberhasilan, waktu evaluasi, verifier independen, hasil, dan bukti. Hasil gagal mengembalikan CAPA ke perencanaan dengan riwayat tetap utuh. | Must |
| FR-CAPA-006 | Manager hanya dapat menutup CAPA setelah semua action item selesai dan verifikasi efektivitas lulus. | Must |
| FR-CAPA-007 | CAPA/NCR tertutup dapat dibuka kembali oleh Manager dengan alasan, owner, dan tenggat baru. Relasi sumber yang perlu dibuka kembali diperiksa dalam transaksi yang sama. | Must |

**Alur NCR:** `Draft → Submitted → Under Review → In Progress → Pending Closure → Closed`.

- `Submitted/Under Review → Draft`: pengembalian dengan alasan; pengajuan sebelumnya tetap tersimpan.
- `Under Review → Rejected`: laporan duplikat/tidak valid, dengan alasan dan referensi bila duplikat.
- `Pending Closure → In Progress`: penutupan ditolak dengan alasan.
- `Closed → In Progress`: reopen oleh Manager; siklus penyelesaian baru tercatat.
- `Rejected` terminal; koreksi diajukan sebagai laporan baru yang menautkan laporan lama.

**Alur CAPA:** `Draft → Pending Review → Pending Approval → In Progress → Effectiveness Check → Pending Closure → Closed`.

- Review/approval ditolak: kembali ke `Draft`, dengan komentar wajib.
- Evaluasi efektivitas gagal atau rencana berubah substansial: kembali ke `Draft` untuk review/approval ulang.
- `Pending Closure → Effectiveness Check`: bukti verifikasi belum memadai.
- `Closed → Draft`: reopen; rencana baru melalui review/approval kembali.

Containment darurat pada NCR dapat dilakukan sebelum approval rencana CAPA dan harus dicatat. Status workflow hanya berubah lewat transisi yang diizinkan server, bukan edit bebas.

### 4.2 Audit internal dan temuan

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-AUD-001 | Supervisor/Manager membuat audit dengan nomor unik, judul, site, area/proses, ruang lingkup, acuan/klausul, jadwal, lead auditor, auditor, dan auditee. | Must |
| FR-AUD-002 | Checklist mendukung pertanyaan, acuan, jawaban sesuai/tidak sesuai/tidak berlaku, catatan, dan bukti. Checklist dibekukan saat audit dimulai agar perubahan template tidak mengubah audit berjalan. | Must |
| FR-AUD-003 | Auditor mencatat temuan dengan klasifikasi major/minor/observation, deskripsi, bukti objektif, acuan, owner, dan tenggat. | Must |
| FR-AUD-004 | Temuan major/minor wajib ditautkan ke NCR atau CAPA sebelum masuk tindak lanjut. Observation dapat memakai tindakan langsung dengan alasan tercatat. | Must |
| FR-AUD-005 | Sistem menyediakan monitoring temuan terbuka, jatuh tempo, overdue, dan selesai per audit/site/owner. | Must |
| FR-AUD-006 | Laporan audit diajukan setelah checklist lengkap dan setiap temuan memiliki owner/tenggat; Manager independen menyetujui laporan meskipun tindak lanjut masih berjalan. | Must |
| FR-AUD-007 | Penutupan audit terpisah dari persetujuan laporan. Audit hanya dapat ditutup setelah semua temuan closed dan Manager independen menyetujui penutupannya. | Must |
| FR-AUD-008 | Auditor/verifier independen dari pelaksana tindakan memverifikasi bukti temuan. Temuan tidak boleh closed selama NCR/CAPA terkait masih terbuka. | Must |
| FR-AUD-009 | Sistem menolak penugasan auditor pada area yang tercatat sebagai tanggung jawab operasionalnya; pemetaan tanggung jawab dikelola pada master data. | Must |
| FR-AUD-010 | Template checklist berversi dapat digunakan ulang untuk audit internal, proses, atau pemasok (R2). Tiap jawaban dapat menautkan revisi dokumen/bukti dan ditampilkan berdampingan dengan pertanyaan/acuan. Perubahan template tidak mengubah snapshot audit. | Must |

**Alur audit:** `Draft → Scheduled → In Progress → Report Review → Follow-up → Closed`.

- Laporan ditolak: `Report Review → In Progress` dengan komentar.
- Audit tanpa temuan tetap melalui persetujuan laporan, lalu dapat diajukan untuk penutupan.
- `Draft/Scheduled → Cancelled` oleh Supervisor/Manager dengan alasan; audit yang sudah berjalan tidak dihapus.
- Reopen temuan pada audit tertutup mengembalikan audit ke `Follow-up` oleh Manager dengan alasan.

**Alur temuan:** `Open → In Progress → Pending Verification → Closed`. Verifikasi ditolak kembali ke `In Progress`. Reopen sumber NCR/CAPA pada temuan tertutup membuka temuan dan audit terkait kembali secara konsisten.

### 4.3 Manajemen dokumen

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-DOC-001 | Sistem menyimpan identitas dokumen: nomor unik, judul, jenis (SOP/WI/form/policy), owner, site berlaku, dan klasifikasi akses. | Must |
| FR-DOC-002 | Setiap revisi menyimpan nomor revisi, file, ringkasan perubahan, author, reviewer, approver, tanggal efektif, dan tanggal review berikutnya bila ada. | Must |
| FR-DOC-003 | File atau metadata revisi yang sudah diajukan tidak dapat ditimpa. Pengembalian memungkinkan author membuat versi kerja berikutnya dengan riwayat pengajuan terjaga. | Must |
| FR-DOC-004 | Supervisor/Manager independen melakukan review; Manager independen melakukan approval. Penolakan wajib disertai komentar. | Must |
| FR-DOC-005 | Approval tidak otomatis membuat revisi berlaku sebelum tanggal efektif. Publikasi hanya boleh terjadi setelah approval dan tanggal efektif tercapai. | Must |
| FR-DOC-006 | Tepat satu revisi dapat berstatus Effective per dokumen. Aktivasi revisi baru dan perubahan revisi sebelumnya menjadi Superseded harus atomik. | Must |
| FR-DOC-007 | Pembaca secara default melihat revisi Effective. Revisi lama tersedia bagi pengguna berizin dengan label Superseded/Obsolete yang jelas. Draft hanya terlihat bagi author dan pihak workflow berizin. | Must |
| FR-DOC-008 | Revisi Superseded/Obsolete tidak dapat diubah atau digunakan sebagai dokumen aktif. Penarikan dokumen dilakukan Manager dengan alasan, tanpa menghapus histori. | Must |
| FR-DOC-009 | Pencarian mendukung nomor, judul, jenis, site, status, dan owner. Hasil menunjukkan revisi serta tanggal efektif. | Must |
| FR-DOC-010 | Pengguna dapat mencatat acknowledgment telah membaca revisi tertentu; acknowledgment lama tidak dianggap berlaku untuk revisi baru. | Should |
| FR-DOC-011 | Dokumen dapat ditata dalam folder/subfolder dengan breadcrumb, pencarian, dan filter metadata. Pemindahan folder tidak mengubah nomor, revisi, atau otorisasi dokumen. | Must |
| FR-DOC-012 | Panel detail menampilkan metadata, preview PDF/gambar, revisi, keputusan approval, dan timeline. Preview serta thumbnail memerlukan hak baca yang sama dengan unduhan. | Must |
| FR-DOC-013 | Detail menampilkan catatan terkait yang boleh dibaca. Pada R2/R3 dokumen dapat ditautkan ke quality plan, training, dan klausul compliance menggunakan revisi tertentu; revisi baru tidak mengganti tautan bukti historis secara diam-diam. | Must |

**Alur revisi:** `Draft → In Review → Pending Approval → Approved → Effective → Superseded`.

- Penolakan review/approval: kembali ke `Draft`, dengan riwayat dan komentar terjaga.
- Publikasi dapat dijalankan terjadwal atau oleh Manager setelah tanggal efektif; keduanya memakai validasi yang sama.
- Manager dapat membatalkan revisi `Approved` sebelum efektif menjadi `Withdrawn`, dengan alasan; revisi tersebut tidak boleh terbit otomatis.
- Penarikan revisi efektif: `Effective → Obsolete`. Sistem menampilkan bahwa dokumen tidak lagi memiliki revisi berlaku.
- Revisi baru dibuat dari identitas dokumen yang sama; nomor revisi yang pernah digunakan tidak dipakai ulang.

### 4.4 Dashboard dan analisis kualitas

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-ANA-001 | Dashboard menampilkan filter site, periode, dan kategori defect; semua kartu, grafik, drill-down, dan ekspor memakai filter serta izin yang konsisten. | Must |
| FR-ANA-002 | Kartu KPI mencakup NCR terbuka, CAPA overdue, temuan audit terbuka, dan dokumen menunggu approval. Setiap kartu membuka daftar sumbernya. | Must |
| FR-ANA-003 | Grafik tren menampilkan jumlah NCR submitted per bulan, default 12 bulan terakhir, dengan nilai bulan kosong tetap nol. | Must |
| FR-ANA-004 | Pareto menampilkan kategori defect utama secara menurun, jumlah NCR per kategori, garis persentase kumulatif 0–100%, dan tooltip. | Must |
| FR-ANA-005 | My Tasks menggabungkan tugas tindakan, review, approval, dan verifikasi milik pengguna; setiap item memuat sumber, prioritas, tenggat, status, dan tautan detail. | Must |
| FR-ANA-006 | Pengguna dapat mengekspor daftar dan agregat yang diizinkan ke CSV dengan informasi filter, zona waktu, dan waktu ekspor. | Must |
| FR-ANA-007 | Ringkasan kondisi kualitas menggunakan hitungan faktual: backlog, overdue, dan penyelesaian. R3 menambahkan kesiapan compliance terukur serta panel AI terpisah; AI tidak mengganti metrik faktual. | Must |
| FR-ANA-008 | Perbandingan dengan periode sebelumnya ditampilkan hanya untuk periode sebanding; jika pembanding nol, tampilkan N/A, bukan persentase menyesatkan. | Should |
| FR-ANA-009 | Dashboard menampilkan aging NCR terbuka, tren biaya scrap/rework tercatat, serta jumlah NCR per site berizin; setiap agregat dapat ditelusuri ke sumber dan periode/mata uangnya. Biaya hanya mencakup biaya yang dicatat, bukan seluruh cost of quality perusahaan. | Must |
| FR-ANA-010 | Data diperbarui setelah operasi pengguna berhasil serta polling maksimal 60 detik saat dashboard aktif. Tampilkan waktu pembaruan terakhir, tombol refresh, dan indikator stale/gagal; tidak mengklaim streaming real-time. | Must |
| FR-ANA-011 | R2 menambahkan ringkasan inspeksi gagal, alat kalibrasi jatuh tempo, work order overdue, serta sertifikat pemasok kedaluwarsa. R3 menambahkan gap training dan readiness compliance. Filter tak relevan diberi label tidak berlaku pada widget tersebut. | Must |
| FR-ANA-012 | Defect rate sampel (R2) hanya dihitung dari inspeksi final berdenominator valid dan unik per unit sampel; tampilkan unit gagal/unit diperiksa serta cakupan sampel. Jangan menyebutnya defect rate seluruh produksi atau PPM produksi. | Must |

**Definisi metrik MVP:**

| Metrik | Aturan perhitungan |
| --- | --- |
| NCR terbuka | NCR berstatus Submitted, Under Review, In Progress, atau Pending Closure pada saat laporan dijalankan. Filter periode memilih kohort berdasarkan first_submitted_at. |
| Tren NCR | Jumlah NCR unik berdasarkan first_submitted_at; Draft dan Rejected dikecualikan. Reopen tidak dihitung sebagai NCR baru. |
| Pareto defect | Satu NCR eligible dihitung satu kali pada kategori defect utamanya. Menggunakan populasi dan periode yang sama dengan tren. Bukan jumlah unit defect. |
| Kumulatif Pareto | Running sum jumlah NCR dibagi total NCR eligible × 100%. Nilai kategori seri diurutkan berdasarkan kode kategori. Kategori tanpa klasifikasi ditampilkan sebagai Uncategorized. |
| Overdue | Item belum selesai dan tenggat telah lewat; tenggat berbentuk tanggal berakhir pukul 23:59:59 zona waktu perusahaan. Item selesai tidak termasuk. |
| CAPA overdue | Jumlah CAPA unik yang belum closed dan target penyelesaian CAPA sudah lewat; action item overdue ditampilkan sebagai hitungan terpisah. |
| Temuan audit terbuka | Temuan belum Closed; periode menggunakan tanggal temuan dibuat. |
| Dokumen menunggu approval | Revisi Pending Approval; periode menggunakan tanggal pengajuan approval. |
| My Tasks | Seluruh penugasan aktif pengguna dalam site terpilih, termasuk dari sebelum periode dashboard. Pengecualian filter periode ini harus diberi label. |

Dashboard menyajikan **status saat ini**, bukan rekonstruksi status pada tanggal historis. Perubahan status/kategori dapat memperbarui laporan periode lama; audit trail menyimpan riwayat perubahan. Saat tidak ada data, tampilkan empty state dan tidak menggambar persentase kumulatif fiktif. Defect rate sampel baru tersedia pada R2; PPM produksi belum tersedia karena denominator seluruh produksi belum masuk cakupan.

**Definisi metrik tambahan (usulan e-QMS):**

| Metrik | Aturan perhitungan |
| --- | --- |
| Aging NCR | Umur hari kalender dari first_submitted_at sampai tanggal perusahaan saat ini untuk NCR terbuka; bucket tanpa tumpang tindih 0–7, 8–14, 15–30, 31–60, dan >60 hari. Reopen mempertahankan umur awal. |
| Biaya scrap/rework | Jumlah nilai entri biaya aktif setelah koreksi, dikelompokkan menurut tanggal biaya; dapat mencakup NCR yang sudah closed. Mata uang berbeda tidak dijumlahkan. Kelengkapan pencatatan biaya ditampilkan; kosong bukan nol. |
| NCR per site | Populasi NCR sama dengan tren, dikelompokkan per site yang berizin; tidak menampilkan nama/jumlah site di luar akses. |
| Inspeksi gagal | Jumlah laporan final unik yang memiliki minimal satu karakteristik sampel Fail, berdasarkan tanggal finalisasi; bukan jumlah sel gagal. Disposisi tidak menghapus hasil gagal. |
| Defect rate sampel | 100 × jumlah unit sampel unik dengan minimal satu Fail / jumlah unit sampel yang seluruh karakteristik wajibnya sudah dievaluasi, dari laporan final dan filter terpilih. Dua karakteristik gagal pada unit sama tetap satu unit gagal. Penyebut nol ditampilkan N/A. |
| Kalibrasi jatuh tempo | Alat aktif dengan due date dalam 30 hari kalender ke depan termasuk hari ini; overdue adalah tanggal sebelum hari ini. Alat tanpa riwayat valid dilaporkan sebagai belum terkalibrasi, bukan dianggap valid. |
| Work order overdue | Work order selain Completed/Cancelled yang due date-nya sudah lewat. Periode memakai due date. |
| Sertifikat pemasok kedaluwarsa | Sertifikat aktif yang tanggal kedaluwarsanya sebelum tanggal hari ini; tanpa tanggal ditandai belum lengkap. Periode memakai tanggal kedaluwarsa. |
| Gap training | Pasangan personel–kompetensi wajib yang belum Qualified atau masa berlakunya habis; kursus opsional dan personel nonaktif dikecualikan. Snapshot saat ini; filter periode dashboard tidak berlaku. |
| Readiness compliance | 100 × klausul applicable yang berstatus Met / seluruh klausul applicable pada satu assessment approved. Partial/Not Met/Not Assessed bernilai nol; Not Applicable dikecualikan hanya jika disetujui dengan alasan. Penyebut nol N/A. Draft ditampilkan terpisah sebagai provisional. |

Filter kategori defect hanya berlaku pada widget berbasis NCR/hasil defect yang memiliki klasifikasi tersebut. Widget alat, maintenance, sertifikat, training, dan compliance tetap memakai filter site serta aturan periode di atas, dengan label eksplisit agar total tidak dianggap berasal dari populasi yang sama.

### 4.5 Audit trail

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-LOG-001 | Sistem mencatat create, perubahan field, transisi status, assignment, perubahan tenggat, approval/rejection, reopen, revisi/publikasi dokumen, serta perubahan hak akses. | Must |
| FR-LOG-002 | Event memuat ID unik, waktu server UTC, aktor atau identitas sistem, jenis/ID entitas, aksi, field sebelum/sesudah, alasan bila wajib, dan correlation ID. | Must |
| FR-LOG-003 | Penyimpanan perubahan bisnis dan audit event harus atomik: jika pencatatan event gagal, perubahan bisnis juga gagal. | Must |
| FR-LOG-004 | Audit trail bersifat append-only melalui aplikasi; tidak ada role aplikasi yang dapat mengubah atau menghapusnya. Koreksi menghasilkan event tambahan. | Must |
| FR-LOG-005 | Pembacaan/ekspor trail mengikuti hak baca entitas. Password, token, session secret, serta isi biner lampiran tidak boleh tersalin ke log. | Must |
| FR-LOG-006 | Log dapat difilter berdasarkan entitas, aktor, aksi, dan waktu. Unduhan dokumen serta ekspor data dicatat sebagai event akses terpisah dari perubahan data. | Must |

Append-only pada aplikasi tidak sama dengan perlindungan terhadap administrator database. Kebutuhan tamper evidence kriptografis/WORM harus dianalisis jika diwajibkan regulasi. Sampai kebijakan retensi disetujui, MVP tidak menyediakan penghapusan permanen catatan kualitas atau audit trail melalui aplikasi.

### 4.6 Kebutuhan bersama

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-COM-001 | Master data mencakup site, area/proses, kategori defect, severity (Low/Medium/High/Critical), klasifikasi temuan, dan jenis dokumen. Data yang sudah dirujuk hanya dapat dinonaktifkan. | Must |
| FR-COM-002 | Lampiran awal mendukung JPEG, PNG, dan PDF dengan batas usulan 10 MB/file. Validasi dilakukan berdasarkan konten dan tipe file; download membutuhkan otorisasi. | Must |
| FR-COM-003 | Penugasan, pengembalian, permintaan approval, dan overdue menghasilkan notifikasi dalam aplikasi tanpa duplikasi pada retry. | Must |
| FR-COM-004 | Semua daftar utama menyediakan pagination, pencarian, filter, sorting, serta empty/loading/error state. | Must |
| FR-COM-005 | Catatan yang sudah diajukan tidak dihapus permanen. Draft dapat dibuang secara logis dengan event audit; catatan lain memakai transisi penolakan, penarikan, atau penutupan yang sesuai. | Must |
| FR-COM-006 | Nomor bisnis unik dan tidak digunakan ulang. Jumlah defect tidak boleh negatif atau melebihi jumlah diperiksa jika kedua nilai tersedia. | Must |
| FR-COM-007 | Pencarian global mencakup nomor/judul/metadata entitas yang telah dirilis. Server memfilter hasil, suggestion, jumlah hasil, dan snippet berdasarkan izin sebelum dikirim. | Must |
| FR-COM-008 | Pengguna dapat memilih Indonesia, Inggris, atau Jepang; preferensi tersimpan. Label, validasi, status tampilan, serta tanggal/angka mengikuti locale; kode status/API tetap stabil dan zona waktu mengikuti perusahaan. Input pengguna tidak diterjemahkan otomatis. | Must |
| FR-COM-009 | Reminder jatuh tempo berlaku untuk kalibrasi, maintenance, sertifikat, dan training pada rilis modul terkait. Baseline reminder 30/7/1 hari sebelum tenggat dan satu saat overdue; kunci deduplikasi menyertakan entitas, siklus, penerima, serta ambang. | Must |
| FR-COM-010 | R1 menyediakan mata uang perusahaan untuk biaya NCR. R2 menambah master part/revisi, satuan ukur, alat/aset, pemasok, dan interval jadwal; R3 menambah jabatan, kompetensi, dan katalog standar/klausul berversi. Penonaktifan tidak merusak referensi historis. | Must |

### 4.7 Task management (R1)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-TSK-001 | Pengguna membuat tugas mandiri berisi judul, deskripsi, site, PIC aktif, prioritas, tenggat, dan lampiran. Inspector hanya dapat menugaskan diri sendiri; Supervisor/Manager dapat menugaskan pengguna site berizin. | Must |
| FR-TSK-002 | Daftar tugas menggabungkan tugas mandiri dan proyeksi tugas dari workflow, dengan filter sumber/PIC/status/prioritas/due date dan ringkasan overdue. Satu penugasan sumber muncul satu kali. | Must |
| FR-TSK-003 | Tugas mandiri memakai Open → In Progress → Done; Blocked dapat dipakai dari Open/In Progress dengan alasan dan kembali ke In Progress setelah kendala selesai. PIC memberi bukti/catatan penyelesaian. Pembuat atau Supervisor/Manager dapat mengubah tugas aktif menjadi Cancelled, atau reopen Done/Cancelled ke Open dengan alasan dan tenggat baru. | Must |
| FR-TSK-004 | Tugas dari CAPA, audit, approval, inspeksi, kalibrasi, maintenance, atau training berubah melalui workflow sumbernya; menandai tugas selesai tidak boleh menutup/melompati approval sumber. Sumber reopened memperbarui tugas sesuai siklus baru. | Must |
| FR-TSK-005 | Komentar, lampiran, assignment, dan perubahan tenggat tercatat pada timeline; notifikasi hanya ke pengguna yang boleh membaca sumber. Penugasan ke akun nonaktif/site tak berizin ditolak. | Must |

### 4.8 Quality plans dan ballooning (R2)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-QPL-001 | Quality plan memuat nomor unik, part/revisi, site, proses, jenis inspeksi, author, revisi plan, serta drawing PDF/gambar yang diunggah atau ditautkan ke revisi dokumen terkendali. | Must |
| FR-QPL-002 | Pengguna menambah/memindah balloon bernomor pada halaman drawing, menautkannya ke karakteristik inspeksi, serta mengatur urutan. Koordinat tersimpan relatif terhadap halaman sehingga tetap sesuai saat zoom. Nomor karakteristik unik dalam revisi. | Must |
| FR-QPL-003 | Karakteristik memuat deskripsi, tipe numerik/atribut/catatan, unit, nominal serta LSL/USL bila numerik, metode, jenis alat, jumlah sampel bilangan bulat positif, dan status wajib. Validasi LSL ≤ nominal ≤ USL; karakteristik atribut mempunyai kriteria Pass/Fail eksplisit. | Must |
| FR-QPL-004 | Revisi plan melalui Draft → In Review → Approved → Effective → Superseded. Reviewer Supervisor/Manager dan approver Manager harus independen dari author. Penolakan kembali Draft dengan alasan. Tanggal efektif serta aktivasi satu revisi aktif per identitas plan diterapkan atomik. | Must |
| FR-QPL-005 | Inspeksi menggunakan snapshot revisi Effective; perubahan plan/drawing memerlukan revisi baru dan tidak mengubah inspeksi berjalan atau historis. Revisi Approved dapat Withdrawn sebelum efektif; Effective dapat Obsolete oleh Manager dengan alasan, mencegah inspeksi baru. | Must |
| FR-QPL-006 | Plan dapat ditinjau sebagai drawing dan tabel karakteristik; ekspor PDF menyertakan identitas part, revisi, unit, toleransi, metode, sampling, dan status. GD&T kompleks boleh direkam sebagai referensi dengan keputusan manual, tanpa klaim evaluasi otomatis. | Must |

### 4.9 Inspeksi dan hasil pengukuran (R2)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-INS-001 | Laporan inspeksi memuat nomor, site, jenis incoming/in-process/final, part/revisi, batch/lot, referensi PO/WO bila ada, pemasok/customer bila relevan, PIC, waktu, jumlah lot, serta snapshot quality plan. | Must |
| FR-INS-002 | Grid karakteristik × unit sampel mendukung input numerik/atribut, simpan draft, catatan, dan bukti. ID unit sampel stabil dalam laporan; kosong adalah Not Measured, bukan nol/Pass. Satuan berbeda ditolak kecuali konversi eksplisit yang tervalidasi tersedia. | Must |
| FR-INS-003 | Untuk karakteristik numerik sederhana, Pass jika LSL ≤ nilai ≤ USL; nilai di luar batas Fail. Evaluasi memakai presisi tersimpan sebelum pembulatan tampilan. Atribut memakai keputusan serta bukti; catatan/GD&T manual tidak dihitung numerik otomatis. | Must |
| FR-INS-004 | Tiap pengukuran yang memerlukan alat merekam alat, waktu ukur, dan referensi kalibrasi valid pada waktu tersebut. Alat overdue, tidak terkalibrasi, atau Out of Service tidak boleh dipakai untuk pengukuran baru; pemeriksaan dilakukan server. | Must |
| FR-INS-005 | Alur Draft → In Progress → Pending Review → Finalized; submit mensyaratkan seluruh sampel/karakteristik wajib lengkap, atau N/A yang diizinkan plan dan beralasan. Reviewer independen dapat mengembalikan ke In Progress. Hasil Fail wajib menautkan NCR sebelum finalisasi. | Must |
| FR-INS-006 | Finalized mengunci hasil. Koreksi membuat amendment beralasan yang mempertahankan versi lama serta review ulang. Laporan gagal berdisposisi Hold sampai Manager independen mencatat Accepted under concession/Rework/Rejected berikut alasan dan bukti; keputusan ini tidak mengubah Fail menjadi Pass atau menutup NCR otomatis. | Must |
| FR-INS-007 | Detail/ekspor PDF mencakup snapshot plan, hasil tiap sampel, keputusan, alat, reviewer, waktu, revisi laporan, dan NCR terkait. Ekspor draft berlabel Draft; hitungan dashboard hanya memakai versi final terkini per laporan. | Must |

Penerimaan sampel tidak otomatis menyatakan seluruh lot diterima. Kebijakan release lot dan metode sampling statistik merupakan keputusan terpisah (Q-12).

### 4.10 Kalibrasi alat ukur (R2)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-CAL-001 | Register alat memuat ID unik, nama, serial, tipe, range, unit, resolusi, lokasi/site, owner, interval kalibrasi, due date, serta status operasional Active/Out of Service/Retired. | Must |
| FR-CAL-002 | Record kalibrasi memuat waktu pelaksanaan, pelaksana/laboratorium, internal/eksternal, metode/referensi standar, titik uji dan hasil, keputusan Pass/Fail, sertifikat/lampiran, serta usulan next due date. Kelengkapan bukti diwajibkan sebelum submit. | Must |
| FR-CAL-003 | Record melalui Draft → Pending Verification → Verified; verifier independen memeriksa hasil dan bukti, atau mengembalikan Draft dengan alasan. Hanya record Verified Pass yang dapat membentuk periode valid kalibrasi. Bukti gagal tidak dihapus oleh record berikutnya. | Must |
| FR-CAL-004 | Validitas memerlukan Active, record Verified Pass yang berlaku pada waktu ukur, dan due date belum lewat. Hasil Fail yang diajukan segera menahan penggunaan alat; alat tetap diblokir hingga ada kalibrasi ulang terverifikasi dan pelepasan oleh verifier independen. Tidak ada perpanjangan otomatis melalui edit due date. | Must |
| FR-CAL-005 | Daftar menampilkan Valid/Due Soon/Overdue/Not Calibrated serta status operasional, histori, dan reminder. Perubahan interval/due date butuh alasan dan verifikasi; tidak mengubah validitas pengukuran historis diam-diam. | Must |
| FR-CAL-006 | Bila alat ditemukan gagal, buat penilaian dampak terhadap inspeksi yang memakai alat sejak kalibrasi lulus terakhir atau awal pemakaian bila belum ada. Tampilkan daftar terpengaruh, PIC, keputusan, dan NCR terkait; jangan otomatis mengubah semua hasil historis menjadi Fail. | Must |

### 4.11 Preventive maintenance (R2)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-MNT-001 | Register aset memuat ID, site/lokasi, nama, jenis, serial, owner, serta status operasional. Jadwal maintenance memuat interval hari/minggu/bulan, tanggal acuan, checklist berversi, PIC, dan target durasi. | Must |
| FR-MNT-002 | Scheduler membuat satu work order per aset/jadwal/tanggal occurrence secara idempoten. Baseline jadwal mengikuti tanggal acuan, bukan tanggal selesai; tanggal bulanan yang tidak tersedia memakai hari terakhir bulan. Jadwal nonaktif tidak menghasilkan order baru. | Must |
| FR-MNT-003 | Work order melalui Open → Assigned → In Progress → Pending Verification → Completed; pelaksana merekam checklist, waktu mulai/selesai, downtime, catatan, suku cadang teks, dan bukti. Verifier independen dapat mengembalikan ke In Progress. | Must |
| FR-MNT-004 | Penundaan atau Cancel dilakukan Supervisor/Manager berizin dengan alasan; occurrence lama tetap tercatat dan tidak dibuat ulang saat retry. Tugas overdue ditampilkan tanpa mengubah jadwal berikutnya diam-diam. Aset Retired menghentikan jadwal; order terbuka harus diselesaikan atau dibatalkan eksplisit. | Must |
| FR-MNT-005 | Temuan maintenance dapat ditautkan ke NCR/CAPA. Penyelesaian maintenance tidak otomatis menutup CAPA, mengesahkan kalibrasi, atau mengaktifkan alat yang masih diblokir. Histori per aset dapat difilter dan diekspor. | Must |

### 4.12 Supplier quality (R2)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-SUP-001 | Profil pemasok memuat kode unik, nama, kontak, produk/jasa, site berlaku, owner, dan status per site: Pending/Approved/Conditional/Suspended/Inactive. Perubahan status hanya oleh Manager berizin dengan dasar keputusan dan alasan. | Must |
| FR-SUP-002 | Sertifikat mencatat jenis/standar, nomor, penerbit, tanggal terbit/kedaluwarsa, file, dan verifikasi. Daftar membedakan valid, expired, dan belum diverifikasi serta menghasilkan reminder. Expiry memicu review, bukan mengganti Approved secara diam-diam. | Must |
| FR-SUP-003 | Evaluasi berkala memuat periode, evaluator, kriteria quality/delivery/responsiveness/compliance/cost, skor 1–5, bobot, bukti, rekomendasi, dan next review date. Bobot berjumlah 100%; skor keseluruhan = Σ(skor × bobot)/100, dibulatkan dua desimal hanya pada tampilan. | Must |
| FR-SUP-004 | Evaluasi melalui Draft → Pending Approval → Approved; Manager independen menyetujui atau mengembalikan Draft dengan alasan. Kriteria/bobot dibekukan per evaluasi; skor saja tidak mengubah status pemasok tanpa keputusan eksplisit. | Must |
| FR-SUP-005 | Self-assessment memakai template berversi, jawaban, bukti, identitas responden eksternal, dan review internal. Baseline input oleh petugas internal dari jawaban pemasok; asal jawaban dicatat terpisah dari penginput. Tidak mengasumsikan portal eksternal atau pengiriman email. | Must |
| FR-SUP-006 | Detail pemasok menautkan inspeksi incoming, NCR/CAPA, audit pemasok, sertifikat, evaluasi, dan assessment yang boleh dibaca. Tindakan korektif pemasok memakai workflow CAPA dengan PIC internal dan kontak pemasok, tanpa akses lintas site otomatis. | Must |

### 4.13 Training dan matriks kompetensi (R3)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-TRN-001 | Katalog kursus berversi memuat kode, judul, kategori, tujuan kompetensi, trainer, metode classroom/online/OJT, materi/revisi dokumen, kriteria lulus, masa berlaku, dan status Draft/Published/Archived. Pengelola training mempublikasikan materi lengkap. | Must |
| FR-TRN-002 | Supervisor/Manager berizin menetapkan kompetensi wajib menurut jabatan/personel/site, kursus yang memenuhi, PIC trainer, dan tenggat. Penugasan menghasilkan tugas pribadi tanpa duplikasi. Perubahan kewajiban berversi dan tidak menghapus histori. | Must |
| FR-TRN-003 | Training record menyimpan peserta, snapshot kursus, progres, kehadiran, nilai bila relevan, bukti/OJT, trainer, verifier, waktu selesai dan expiry. Peserta tidak dapat memverifikasi dirinya sendiri; membuka materi tidak otomatis berarti kompeten. | Must |
| FR-TRN-004 | Matriks personel × kompetensi menampilkan Not Started/In Progress/Pending Verification/Qualified/Not Qualified/Expired, due date dan legenda teks. Awareness adalah tingkat kompetensi terpisah; pemenuhan kewajiban ditentukan tingkat yang dipersyaratkan, bukan warna sel. | Must |
| FR-TRN-005 | Penyelesaian diajukan ke Pending Verification; verifier independen memberi Qualified atau Not Qualified berdasarkan bukti/kriteria. Kegagalan menghasilkan percobaan ulang dengan histori tetap utuh. Masa berlaku habis menampilkan Expired dan tugas pembaruan; koreksi keputusan wajib beralasan serta diaudit. | Must |
| FR-TRN-006 | Revisi kursus/dokumen tidak otomatis membatalkan semua kualifikasi. Pengelola mencatat penilaian dampak, siapa perlu retraining, revisi tujuan, dan tenggat. Matriks, daftar gap, dan ekspor dibatasi izin personel/site. | Must |

### 4.14 Compliance dashboard dan AI auditor (R3)

| ID | Requirement | Prioritas |
| --- | --- | --- |
| FR-CMP-001 | Katalog standar berversi berisi judul/edisi, klausul, deskripsi requirement yang berhak digunakan, dan applicability per assessment/site. Katalog dapat dikonfigurasi; tidak mengasumsikan teks standar berlisensi tersedia dalam aplikasi. | Must |
| FR-CMP-002 | Assessment memuat standar/edisi, site, ruang lingkup, periode, owner, evaluator, snapshot klausul, serta bukti yang ditautkan ke revisi dokumen, audit, training, atau record kualitas tertentu. | Must |
| FR-CMP-003 | Tiap klausul dinilai Not Assessed/Met/Partial/Not Met/Not Applicable. Met memerlukan bukti dan alasan evaluator; N/A memerlukan alasan serta persetujuan Manager independen. Gap mempunyai owner, due date, tugas, dan NCR/CAPA bila diperlukan. | Must |
| FR-CMP-004 | Dashboard menampilkan readiness per standar/bagian, jumlah gap dan bukti, waktu assessment, dan drill-down. Formula pada bagian 4.4 dihitung deterministik; teks menjelaskan bahwa ini kesiapan internal, bukan sertifikat atau jaminan kepatuhan. | Must |
| FR-CMP-005 | Assessment melalui Draft → In Review → Approved; Manager independen dapat mengembalikan ke Draft dengan alasan. Snapshot Approved tidak ditimpa; perubahan bukti/sumber memicu penanda perlu review dan assessment versi berikutnya tanpa mengubah histori. | Must |
| FR-AI-001 | Pengguna berizin dapat memulai analisis AI atas assessment dan bukti terpilih yang boleh dibacanya. Job menampilkan Queued/Running/Succeeded/Failed/Cancelled, progres, waktu, serta pesan kegagalan/retry; penilaian manual tetap tersedia saat layanan AI tidak aktif. | Must |
| FR-AI-002 | Hasil AI berupa usulan gap, ringkasan dan pertanyaan tindak lanjut per klausul dengan referensi record/revisi/halaman atau lokasi bukti. Klaim tanpa bukti diberi label bukti tidak cukup; hasil selalu berlabel Usulan AI, belum diverifikasi. | Must |
| FR-AI-003 | Pengguna menerima/menolak usulan dengan alasan; penerimaan hanya membuat draft penilaian/tugas. AI tidak boleh mengubah status Approved, menutup NCR/CAPA, memberi kualifikasi, atau menerbitkan dokumen. Keputusan akhir tetap melalui workflow manusia. | Must |
| FR-AI-004 | Audit job merekam aktor, ruang lingkup, versi sumber/model/template, waktu, hasil, dan keputusan manusia tanpa secret. Terapkan ulang izin saat membaca hasil. Konten dokumen diperlakukan sebagai data, bukan instruksi; provider, retensi, biaya/batas penggunaan, serta izin pemrosesan data ditetapkan sebelum aktivasi. | Must |

AI auditor sengaja ditempatkan setelah bukti dan workflow dasar tersedia. Keluaran tidak boleh menampilkan nilai readiness hasil generasi bebas; FR-CMP-004 tetap menjadi sumber perhitungan.

## 5. Arah UI/UX

### 5.1 Struktur navigasi

R1: Dashboard · Tugas · NCR & CAPA · Audit · Dokumen · Analisis Kualitas · Audit Trail · Pengaturan.

R2 menambahkan Quality Plans · Inspeksi · Kalibrasi · Maintenance · Supplier Quality. R3 menambahkan Training · Compliance. Menu hanya muncul setelah modul tersedia dan mengikuti izin pengguna. Detail entitas menyediakan ringkasan, status, owner/tenggat, bukti, catatan terkait, dan timeline perubahan. Sidebar dapat diciutkan; header menyediakan pencarian global, pemilih site, notifikasi, bahasa, dan profil.

### 5.2 Dashboard berdasarkan screenshot

- Header berisi judul Dashboard, filter site, filter periode, dan tombol **Buat NCR**.
- Bagian atas terdiri dari kartu **Ringkasan Kualitas** dan **Tugas Saya**.
- Bagian tengah menampilkan **Tren NCR** dan **Pareto Defect** berdampingan pada desktop.
- Bagian bawah menampilkan KPI NCR terbuka, CAPA overdue, temuan audit terbuka, serta dokumen menunggu approval.
- Gunakan latar netral terang, kartu putih, border halus, radius sudut konsisten, dan hierarki angka yang jelas.
- Warna biru untuk data utama, amber untuk perhatian, merah untuk risiko/overdue, dan hijau untuk selesai; selalu sertakan teks atau ikon agar makna tidak bergantung pada warna.
- Pada layar sempit, kartu tersusun satu kolom; tabel mendukung scroll horizontal tanpa menyembunyikan aksi penting.
- Grafik menyediakan tooltip dan tabel data alternatif. Navigasi keyboard, focus indicator, label form, dan pesan validasi harus tersedia.
- Data contoh hanya digunakan pada lingkungan demo dan diberi label; produksi tidak boleh menampilkan angka atau insight buatan.

UI mengambil inspirasi struktur informasi screenshot dan video. Branding, aset, dan komponen dirancang untuk proyek e-QMS ini.

### 5.3 Pola interaksi tambahan dari video

- Tabel utama memiliki filter, sorting, pagination, pilihan kolom, status teks/warna, serta aksi yang sesuai izin. Panel detail di sisi kanan mempertahankan konteks daftar; pada mobile memakai layar penuh.
- Quality plan menampilkan drawing dengan zoom/nomor halaman, balloon, dan editor karakteristik; tabel alternatif memungkinkan navigasi tanpa bergantung pada gambar.
- Inspeksi menampilkan grid karakteristik × sampel dengan navigasi keyboard, unit/toleransi selalu terlihat, penanda belum diukur/Pass/Fail, serta peringatan perubahan belum disimpan. Warna tidak menentukan hasil perhitungan.
- Kalibrasi, maintenance, dan supplier memakai pola daftar → detail/tab histori → formulir record, dengan due date dan status mudah dibaca.
- Training matrix menyediakan filter site/personel/jabatan/kursus, legenda status, panel record, dan tampilan daftar alternatif pada mobile.
- Compliance menampilkan standar, readiness per bagian, gap dan bukti; panel AI menunjukkan ruang lingkup, progres, sumber, dan aksi menerima/menolak usulan.
- Semua panel menjaga focus keyboard, dapat ditutup dengan kontrol berlabel, dan mengembalikan fokus ke pemicunya. Form panjang menampilkan ringkasan error serta status penyimpanan.

## 6. Model domain awal

| Entitas | Isi/relasi utama |
| --- | --- |
| User, Role, SiteMembership | Identitas, status aktif, role, izin administratif, akses site. |
| Site, ProcessArea, DefectCategory | Master operasional dan klasifikasi; area mencatat penanggung jawab. |
| NCR | Identitas, site, pelapor, first_submitted_at, kategori utama, severity, status, owner, tenggat. |
| CAPA, ActionItem, EffectivenessCheck | Analisis, rencana, pelaksanaan, verifier, kriteria dan hasil efektivitas. |
| QualityLink | Hubungan NCR/CAPA/temuan, dengan tipe relasi dan aturan sumber yang jelas. |
| Audit, ChecklistSnapshot, ChecklistResponse, AuditFinding | Jadwal, auditor/auditee, acuan, jawaban, temuan, dan tindak lanjut. |
| Document, DocumentRevision | Identitas dokumen stabil dan revisi dengan file/status/tanggal efektif masing-masing. |
| ApprovalDecision | Objek dan versi yang disetujui, reviewer/approver, keputusan, komentar, waktu. |
| Attachment | Metadata, checksum, lokasi penyimpanan, uploader, dan entitas pemilik. |
| Notification | Penerima, sumber tugas, jenis event, status dibaca, kunci deduplikasi. |
| AuditEvent | Perubahan append-only dan metadata penelusuran. |
| Task, TaskComment | Tugas mandiri; proyeksi tugas sumber memakai source type/ID/assignment/siklus untuk deduplikasi. |
| DocumentFolder, QualityCostEntry | Folder dokumen dan entri biaya NCR dengan koreksi tercatat. |
| Part, PartRevision, QualityPlan, QualityPlanRevision | Identitas part, drawing, rencana, versi dan tanggal efektif. |
| InspectionCharacteristic, DrawingBalloon | Kriteria, batas, unit, sampling, posisi halaman dan nomor balloon pada revisi plan. |
| Inspection, InspectionRevision, SampleUnit, Measurement, LotDisposition | Snapshot plan, hasil unit/karakteristik, alat/kalibrasi, keputusan serta amendment. |
| MeasuringEquipment, CalibrationRecord, CalibrationImpactReview | Register alat, hasil/verifikasi/sertifikat, periode valid dan inspeksi terdampak. |
| Asset, MaintenanceSchedule, WorkOrder, WorkOrderResponse | Register aset, recurrence, snapshot checklist, pelaksanaan dan verifikasi. |
| Supplier, SupplierSite, SupplierCertificate, SupplierEvaluation, SupplierAssessment | Identitas pemasok, status per site, bukti, penilaian dan template/snapshot assessment. |
| Course, CourseRevision, CompetencyRequirement, TrainingAssignment, TrainingRecord | Materi berversi, jabatan/kompetensi wajib, penugasan, percobaan, verifikasi dan masa berlaku. |
| StandardVersion, Clause, ComplianceAssessment, ClauseAssessment, EvidenceLink | Acuan berversi, applicability, penilaian manusia serta tautan versi bukti. |
| AIAnalysisJob, AISuggestion, AIReviewDecision | Snapshot ruang lingkup, versi model/template, progres, usulan bersumber serta keputusan manusia. |

Aturan relasi: satu NCR dapat memiliki beberapa CAPA; satu CAPA dapat menangani beberapa sumber dalam site yang sama. Penutupan sumber mensyaratkan seluruh tindakan terkait selesai. Reopen tindakan yang menopang penutupan sumber harus mengevaluasi ulang dan membuka sumber terkait. Hubungan lintas site ditunda dari MVP. Dokumen yang dirujuk sebagai bukti ditautkan ke revisi tertentu, bukan hanya identitas dokumen terbaru.

Aturan tambahan: satu revisi plan memiliki banyak karakteristik/balloon dan menjadi snapshot banyak inspeksi. Satu hasil ukur terkait satu unit sampel, satu karakteristik snapshot, serta alat/record kalibrasi jika diwajibkan. Satu laporan dapat memiliki banyak revisi tetapi hanya satu versi final terkini untuk agregasi. Satu alat/aset dapat dihubungkan secara eksplisit bila objek fisiknya sama; maintenance dan kalibrasi tetap mempunyai status berbeda. SupplierSite mencegah evaluasi/status satu site bocor ke site lain. CourseRevision dan CompetencyRequirement dibekukan dalam assignment; matriks merupakan proyeksi record sah terbaru, bukan tabel status yang dapat diedit bebas. EvidenceLink memeriksa akses sumber dan target; agregat compliance tidak memperlihatkan bukti yang tak berizin. Dokumen multisite hanya dapat ditautkan bila berlaku dan boleh dibaca pada site target.

## 7. Requirements nonfungsional

Target numerik di bawah merupakan baseline usulan untuk diuji dan disesuaikan sebelum rilis.

| ID | Requirement dan cara verifikasi |
| --- | --- |
| NFR-SEC-001 | TLS pada deployment, password di-hash memakai algoritma yang sesuai standar saat implementasi, rate limit login, cookie sesi aman, proteksi CSRF jika memakai autentikasi cookie, dan validasi input di server. Verifikasi lewat integration/security tests. |
| NFR-SEC-002 | File tersimpan privat, tanpa public bucket atau URL permanen terbuka; unduhan memeriksa izin. Upload dipindai sebelum tersedia untuk pengguna lain. Provider pemindaian ditentukan pada spesifikasi teknis. |
| NFR-DAT-001 | Transaksi mencegah status parsial; optimistic concurrency menolak update dari versi data usang tanpa kehilangan perubahan pengguna lain. Approval hanya berlaku bagi versi yang diperiksa. |
| NFR-DAT-002 | Retry submit/approval/publikasi tidak menghasilkan duplikasi catatan, keputusan, atau notifikasi. Kunci idempotensi dan constraint unik ditentukan pada desain API. |
| NFR-PER-001 | Target p95 respons API daftar/detail ≤2 detik dan dashboard ≤3 detik, di luar transfer file, pada 50 pengguna serentak dan 100.000 NCR. Profil dataset, beban, dan infrastruktur harus dicatat saat load test. |
| NFR-REL-001 | Target backup harian, RPO ≤24 jam, RTO ≤8 jam. Restore database dan lampiran diuji bersama sebelum produksi. |
| NFR-OBS-001 | Error log dan health check tersedia, memakai correlation ID, tanpa membocorkan kredensial atau isi dokumen sensitif. |
| NFR-UX-001 | Alur inti dapat dipakai dengan keyboard; target WCAG 2.2 AA untuk form, navigasi, warna, dan pesan error. Verifikasi otomatis dan pemeriksaan manual. |
| NFR-COMP-001 | Mendukung dua versi mayor terbaru Chrome dan Edge saat rilis; layout diuji pada lebar 360, 768, dan 1440 piksel. |
| NFR-TEST-001 | Workflow, izin akses, transaksi audit trail, dan kalkulasi metrik memiliki automated tests yang ditautkan ke ID requirement. |
| NFR-DAT-003 | Nilai ukur, batas, bobot, dan biaya memakai representasi desimal yang menjaga presisi. Uji nilai tepat pada batas, di luar batas sekecil presisi tersimpan, angka negatif, separator locale, dan pembulatan tampilan. |
| NFR-JOB-001 | Scheduler, reminder, dan AI job mempunyai status, retry terbatas, deduplikasi, dan pemulihan setelah restart. Uji eksekusi paralel dan retry agar tidak membuat work order/notifikasi ganda. |
| NFR-PER-002 | R2 menargetkan buka/simpan grid 100 karakteristik × 30 sampel p95 ≤3 detik pada 20 operator serentak; R3 matriks 200 personel × 50 kompetensi memakai pagination/virtualisasi. Profil beban, ukuran payload, dan jaringan dicatat. |
| NFR-AI-001 | Sebelum AI diaktifkan, uji isolasi site, sumber tanpa izin, instruksi berbahaya dalam dokumen, kutipan salah, bukti kosong, timeout, dan batas biaya. Seluruh skenario akses negatif harus lulus; jawaban tanpa sumber tidak boleh menjadi keputusan final. |
| NFR-I18N-001 | Alur login, daftar, formulir, approval, ekspor, dan pesan error diuji dalam Indonesia/Inggris/Jepang; tanggal perusahaan tetap konsisten dan Unicode tidak rusak. Terjemahan yang hilang memakai fallback berlabel/tercatat, tanpa menampilkan key internal. |

## 8. Acceptance criteria utama

Skenario berikut adalah dasar acceptance test, bukan bukti bahwa fitur telah diimplementasikan atau diuji.

| ID | Requirement | Given / When / Then |
| --- | --- | --- |
| AC-01 | FR-NCR-001, FR-LOG-001 | Given Inspector memiliki akses site A; when submit NCR valid; then nomor unik dibuat, status Submitted, dan event audit merekam aktor serta waktu. |
| AC-02 | FR-NCR-001 | Given field wajib belum lengkap; when submit; then validasi menjelaskan field yang kurang dan tidak menghasilkan NCR submitted parsial. |
| AC-03 | FR-ACC-002–005 | Given pengguna hanya berizin site A; when membaca, mengubah, mengunduh, mengekspor, atau meminta agregat site B lewat API; then tidak ada data site B yang bocor. |
| AC-04 | FR-CAPA-004–006 | Given CAPA memiliki action item belum selesai atau efektivitas belum lulus; when diminta close; then server menolak dan menjelaskan prasyarat. |
| AC-05 | FR-CAPA-004–005, FR-DOC-004 | Given aktor adalah penyusun/pelaksana yang tidak independen; when melakukan review, verifikasi, atau approval terlarang; then server menolak meskipun aktor memiliki role Manager. |
| AC-06 | FR-CAPA-005 | Given CAPA sedang Effectiveness Check; when verifier menyatakan gagal dengan bukti; then CAPA kembali Draft dan approval/hasil sebelumnya tetap terbaca. |
| AC-07 | FR-AUD-006–008 | Given laporan audit sudah approved tetapi satu temuan masih terbuka; when close audit; then ditolak. Setelah semua temuan diverifikasi dan closed, Manager independen dapat menutup audit. |
| AC-08 | FR-DOC-005–006 | Given revisi 1 Effective dan revisi 2 Approved bertanggal masa depan; when belum mencapai tanggal efektif; then revisi 1 tetap berlaku. Saat publikasi sah dijalankan, hanya revisi 2 Effective. |
| AC-09 | FR-DOC-003, FR-DOC-007 | Given revisi telah diajukan; when author mencoba menimpa file langsung; then ditolak. Pembaca biasa tetap mendapat revisi Effective sesuai aksesnya. |
| AC-10 | FR-ANA-003–004 | Given 10 NCR eligible dengan kategori A=5, B=3, C=2; when Pareto dibuka; then bar bernilai 5,3,2 dan kumulatif 50%,80%,100%, dengan total drill-down 10. |
| AC-11 | FR-ANA-001–004 | Given filter menghasilkan nol NCR; when dashboard dibuka; then KPI/tren menampilkan nol dan Pareto menampilkan empty state tanpa pembagian nol. |
| AC-12 | FR-ANA-003 | Given NCR submitted Januari lalu reopened Maret; when tren Januari–Maret dibuka; then NCR dihitung satu kali pada Januari. |
| AC-13 | FR-COM-003, FR-ANA-005 | Given tugas belum selesai melewati akhir tanggal tenggat perusahaan; when dashboard dibuka; then tugas ditandai overdue dan notifikasi tidak berulang akibat retry proses. |
| AC-14 | FR-LOG-002–004 | Given perubahan tenggat beralasan; when disimpan; then nilai sebelum/sesudah dan aktor tercatat. Jika penyimpanan audit gagal, tenggat tidak berubah. |
| AC-15 | NFR-DAT-001 | Given dua pengguna mengedit versi yang sama; when pengguna kedua menyimpan setelah pengguna pertama; then konflik ditampilkan dan perubahan pertama tidak tertimpa. |
| AC-16 | FR-CAPA-007, FR-AUD-008 | Given CAPA menopang temuan dan audit yang sudah closed; when Manager membuka kembali CAPA; then temuan, NCR sumber bila ada, dan audit terkait dibuka kembali dengan event audit yang konsisten. |
| AC-17 | FR-ACC-004–005 | Given pengguna mempunyai sesi aktif; when akun dinonaktifkan atau izin site dicabut; then request berikutnya ditolak sesuai perubahan tersebut. |
| AC-18 | FR-COM-002, NFR-SEC-002 | Given upload melebihi batas, tipe tersamar, atau hasil scan berbahaya; when diproses; then file ditolak/diisolasi dan tidak dapat diunduh pengguna lain. |
| AC-19 | FR-DOC-006, NFR-DAT-002 | Given dua permintaan publikasi/retry bersamaan; when diproses; then tetap hanya satu revisi Effective dan satu keputusan publikasi untuk operasi tersebut. |
| AC-20 | FR-TSK-002–004 | Given tugas berasal dari approval CAPA; when pengguna mencoba menandainya Done melalui API tugas; then ditolak dan status hanya berubah setelah keputusan workflow sah; retry tidak menggandakan tugas. |
| AC-21 | FR-TSK-001, FR-TSK-003, FR-TSK-005 | Given tugas mandiri ditugaskan kepada PIC aktif; when PIC menyelesaikan dengan bukti lalu pembuat reopen beralasan; then histori Done tetap utuh, tugas kembali Open, dan notifikasi dideduplikasi. Assignment lintas site ditolak. |
| AC-22 | FR-DOC-011–013, FR-COM-007 | Given dokumen terbatas dipindahkan folder; when pengguna tanpa izin memakai search, preview, thumbnail atau direct URL; then judul, isi, jumlah hasil, dan akses tetap terlindungi. |
| AC-23 | FR-NCR-008, FR-ANA-009 | Given biaya scrap 100000 dan rework 50000 IDR pada bulan yang sama; when dashboard dibuka; then total tercatat 150000 IDR. NCR tanpa biaya ditandai belum tercatat; koreksi scrap menjadi 80000 menghasilkan total 130000 dengan histori asli tetap ada. |
| AC-24 | FR-ANA-009–010 | Given NCR terbuka berumur 7, 8, 14, 15, 30, 31, 60, 61 hari; when aging ditampilkan; then bucket bernilai 1,2,2,2,1. Setelah perubahan sah, dashboard aktif diperbarui dalam 60 detik atau menampilkan stale bila refresh gagal. |
| AC-25 | FR-QPL-002–005 | Given plan rev 1 dipakai inspeksi berjalan; when balloon/toleransi diubah dan rev 2 diaktifkan; then inspeksi lama tetap rev 1, inspeksi baru memakai rev 2, dan author tidak dapat menyetujui revisinya sendiri. |
| AC-26 | FR-INS-002–003, NFR-DAT-003 | Given LSL=9.50 dan USL=10.50; when nilai 9.50/10.50/10.501/kosong diisi; then hasil Pass/Pass/Fail/Not Measured sebelum pembulatan. Input dengan unit yang tidak sesuai ditolak. |
| AC-27 | FR-INS-004, FR-CAL-004 | Given alat overdue atau Out of Service; when alat dipilih lewat UI maupun API untuk pengukuran baru; then ditolak. Hasil lama tetap menautkan kalibrasi yang berlaku pada waktu ukur. |
| AC-28 | FR-INS-005–007 | Given hasil Fail tanpa NCR atau karakteristik wajib kosong; when submit/finalize; then ditolak. Dengan data lengkap dan NCR, reviewer independen dapat finalize; amendment mempertahankan versi lama, dan disposisi tidak mengubah Fail menjadi Pass. |
| AC-29 | FR-ANA-012 | Given 10 unit sampel lengkap dan dua karakteristik gagal pada unit yang sama; when rate dihitung; then 1/10=10%, bukan 2/10; laporan draft dan unit tidak lengkap tidak masuk denominator. Nol unit eligible menghasilkan N/A. |
| AC-30 | FR-CAL-002–006 | Given alat pernah dipakai pada tiga inspeksi sejak kalibrasi lulus terakhir; when record kalibrasi Fail diajukan; then alat diblokir dan tiga inspeksi muncul pada penilaian dampak tanpa ditulis ulang. Record Pass baru hanya memulihkan validitas setelah verifikasi independen. |
| AC-31 | FR-MNT-002–004, NFR-JOB-001 | Given jadwal bulanan bertanggal acuan 31 Januari; when scheduler Februari dijalankan dua kali; then satu order untuk hari terakhir Februari dibuat. Penyelesaian terlambat tidak menggeser jadwal 31 Maret. |
| AC-32 | FR-MNT-003–005 | Given checklist work order belum lengkap atau verifier adalah pelaksana; when complete diminta; then ditolak. Penyelesaian sah tidak membuka blokir kalibrasi atau menutup CAPA terkait. |
| AC-33 | FR-SUP-003–004 | Given skor 4,3,2,5,1 dengan bobot masing-masing 20%; when evaluasi disubmit; then skor 3.00 dan status Pending Approval. Bobot total 90% ditolak; approval oleh evaluator sendiri ditolak. |
| AC-34 | FR-SUP-001–002, FR-SUP-005–006 | Given sertifikat pemasok site A kedaluwarsa dan self-assessment dicatat petugas internal; when detail dibuka; then expiry, sumber responden, penginput, dan tugas review terlihat; status Approved tidak berubah otomatis dan pengguna site B tidak melihat evaluasi A. |
| AC-35 | FR-TRN-002–005 | Given kursus wajib telah dituntaskan peserta tetapi belum diverifikasi; when matriks dibuka; then Pending Verification, bukan Qualified. Verifikasi sendiri ditolak; setelah verifier sah menyetujui, status Qualified hingga expiry lalu Expired dan tugas pembaruan dibuat sekali. |
| AC-36 | FR-TRN-006 | Given peserta Qualified pada dokumen rev 1; when rev 2 diterbitkan; then histori rev 1 tetap utuh dan keputusan retraining/target peserta harus dicatat sebelum assignment baru dibuat. |
| AC-37 | FR-CMP-003–005 | Given 10 klausul dengan dua N/A disetujui, lima Met, dua Partial, satu Not Met; when readiness dihitung; then 5/8=62.5%. Semua N/A menghasilkan N/A; N/A belum disetujui tetap masuk penyebut dan bukti berubah menandai perlu review. |
| AC-38 | FR-AI-001–004, NFR-AI-001 | Given bukti berisi instruksi untuk membaca site lain/menyetujui dokumen; when AI dijalankan; then tidak ada akses site lain atau mutasi workflow. Usulan memuat sumber berizin; penerimaan hanya membuat draft yang tetap membutuhkan review manusia. |
| AC-39 | FR-AI-001–002 | Given provider timeout atau bukti tidak cukup; when analisis dijalankan; then status/pesan kegagalan atau label bukti tidak cukup tampil; readiness faktual dan assessment manual tetap dapat digunakan. |
| AC-40 | FR-COM-008, NFR-I18N-001 | Given bahasa diganti ke Jepang lalu Inggris; when form angka/tanggal disimpan dan sesi dibuka ulang; then preferensi bertahan, nilai tetap sama, teks Unicode terjaga, dan batas overdue tetap Asia/Jakarta. |
| AC-41 | FR-COM-009, FR-ANA-011 | Given due date kalibrasi/maintenance/sertifikat/training dan scheduler retry; when ambang reminder terlewati; then satu notifikasi per penerima/siklus/ambang dibuat, widget sesuai sumber, dan filter defect yang tak berlaku diberi label. |
| AC-42 | FR-QPL-004–006 | Given dua aktivasi plan bersamaan; when transaksi selesai; then hanya satu revisi Effective dan ekspor menyebut revisi yang benar. Plan Withdrawn/Obsolete tidak dapat digunakan untuk inspeksi baru. |
| AC-43 | FR-AUD-010, FR-CMP-002, FR-CMP-005 | Given checklist/bukti snapshot telah disetujui; when template atau revisi sumber diperbarui; then snapshot audit/assessment lama tetap utuh dan perubahan memerlukan review/versi berikutnya, tanpa approval otomatis. |

## 9. Alur spec-driven development

1. **Validasi requirements:** tinjau asumsi, matriks role, workflow, dan pertanyaan bisnis. Catat perubahan beserta dampaknya pada ID requirement.
2. **Tulis spesifikasi per fitur:** gunakan struktur yang diusulkan `specs/<nomor>-<fitur>/spec.md`, berisi user story, aturan bisnis, state transition, edge case, dan acceptance criteria.
3. **Susun desain teknis:** `plan.md` mendefinisikan model data, API, komponen UI, layanan pendukung, strategi keamanan, serta migrasi. Catat keputusan arsitektur penting sebagai ADR.
4. **Pecah pekerjaan:** `tasks.md` menautkan setiap task ke requirement dan acceptance criteria, termasuk pengujian negatif untuk akses dan workflow.
5. **Implementasi dan verifikasi:** jalankan pengujian yang merealisasikan spesifikasi; perubahan kebutuhan harus memperbarui spesifikasi dan traceability pada perubahan yang sama.

Urutan delivery yang disarankan:

| Tahap | Hasil |
| --- | --- |
| 1 — Fondasi | Autentikasi, role/site, master data, audit event, penyimpanan lampiran privat, layout dasar. |
| 2 — NCR & CAPA | Pelaporan sampai verifikasi efektivitas, closure, dan reopen. |
| 3 — Audit | Penjadwalan, checklist, temuan, laporan, tindak lanjut, dan penutupan. |
| 4 — Dokumen | Revisi, review, approval, publikasi efektif, dan histori. |
| 5 — Analisis & kesiapan rilis | Dashboard, Pareto, tren, ekspor, load test, security checks, dan restore drill. |

Tahap 1–5 menghasilkan R1 dan juga mencakup task management, folder/preview, biaya/aging, pencarian global, serta tiga bahasa. Kelanjutan cakupan video:

| Rilis/tahap | Hasil dan dependensi |
| --- | --- |
| R2 / 6 — Plan dan alat | Master part/unit/aset, quality plan/revisi/ballooning serta kalibrasi; mendahului penggunaan alat dalam inspeksi. |
| R2 / 7 — Inspeksi | Grid sampel, validasi toleransi/alat, review, disposisi, amendment, PDF serta hubungan NCR. |
| R2 / 8 — Maintenance dan pemasok | Scheduler/checklist work order, sertifikat, evaluasi/assessment pemasok serta audit pemasok; KPI operasional dan reminder. |
| R3 / 9 — Training | Katalog kursus, kewajiban jabatan, verifikasi kompetensi, matriks dan retraining. |
| R3 / 10 — Compliance | Katalog klausul, evidence mapping, assessment dan readiness deterministik; memakai bukti modul sebelumnya. |
| R3 / 11 — AI auditor dan verifikasi akhir | Job analisis bersumber, review manusia, evaluasi akses/ketepatan sumber, uji beban dan recovery semua modul. |

**Definition of Ready per fitur:** kebutuhan dan cakupan jelas; role serta state transition terdefinisi; data wajib, kegagalan, dan acceptance criteria tersedia; keputusan yang menghalangi implementasi telah diselesaikan.

**Definition of Done per fitur:** requirement Must terkait diimplementasikan; pengujian positif/negatif lulus; izin diperiksa di server; perubahan tercatat di audit trail; UI memiliki state lengkap; spesifikasi, dokumentasi operasional, dan traceability diperbarui. R1 selesai setelah seluruh Must R1 dan target nonfungsionalnya tervalidasi. Cakupan perluasan video selesai setelah Must R2/R3 beserta dependensi, acceptance criteria, dan target nonfungsional terkait tervalidasi.

Contoh traceability: `FR-CAPA-006 → specs/002-ncr-capa/spec.md → task penutupan CAPA → AC-04 → integration/E2E test penutupan`. Struktur tersebut adalah usulan file berikutnya; dokumen ini belum menyatakan file-file itu sudah tersedia.

## 10. Keputusan yang perlu divalidasi

Pertanyaan ini tidak menghalangi penyusunan draft, tetapi jawaban yang relevan harus diselesaikan sebelum spesifikasi fitur terkait difinalkan.

| ID | Pertanyaan | Baseline sementara |
| --- | --- | --- |
| Q-01 | Industri, standar, dan regulasi apa yang dituju: ISO 9001, IATF 16949, ISO 13485, atau lainnya? | QMS generik; tanpa klaim kepatuhan khusus. |
| Q-02 | Berapa site, jumlah pengguna, dan perkiraan volume NCR/lampiran? | Satu perusahaan multi-site; target performa awal pada bagian 7. |
| Q-03 | Apakah approver independen tersedia untuk tiap site, dan apakah perlu delegasi saat cuti? | Tidak ada bypass; workflow tertahan bila personel belum tersedia. |
| Q-04 | Apa definisi severity, SLA per severity, dan jalur eskalasi overdue? | Severity empat tingkat; tenggat ditetapkan saat triage; notifikasi dalam aplikasi. |
| Q-05 | Apakah Pareto utama memakai jumlah NCR, unit defect, biaya, atau nilai scrap? | Jumlah NCR berdasarkan satu kategori utama. |
| Q-06 | Berapa masa retensi dokumen, catatan kualitas, lampiran, dan audit trail? | Tidak ada penghapusan permanen melalui aplikasi. Kebijakan final wajib sebelum produksi. |
| Q-07 | Apakah diperlukan SSO, e-signature, integrasi ERP/MES, atau multi-tenant? | Ditunda sampai kebutuhan dikonfirmasi. |
| Q-08 | Apa konvensi nomor NCR/CAPA/audit/dokumen serta nomor revisi? | Unik dan tidak dipakai ulang; format ditentukan pada spesifikasi. |
| Q-09 | Bagaimana deployment, lokasi data, pemindaian malware, dan backup disediakan? | Ditentukan pada desain teknis; tidak mengasumsikan layanan vendor tertentu. |
| Q-10 | Apakah pembaca boleh mengakses revisi historis dan dokumen lintas site? | Berdasarkan klasifikasi akses serta site berlaku yang eksplisit. |
| Q-11 | Jenis drawing, toleransi, GD&T, unit, dan ukuran file plan apa yang diperlukan? | PDF/gambar, balloon manual, batas numerik sederhana dan atribut; batas upload 10 MB mengikuti FR-COM-002. |
| Q-12 | Metode sampling dan siapa yang berwenang melepas lot? | Sampel manual dengan jumlah eksplisit; approval hasil tidak otomatis melepas seluruh lot. Kebijakan release lot harus difinalkan sebelum penggunaan operasional. |
| Q-13 | Interval, laboratorium, metode, toleransi kalibrasi, serta cakupan dampak alat gagal? | Interval per alat; bukti dan verifier independen; review sejak kalibrasi lulus terakhir. |
| Q-14 | Jadwal maintenance mengikuti kalender, jam operasi, atau counter mesin? | Kalender tetap; jam operasi/counter dan integrasi mesin ditunda. |
| Q-15 | Kriteria/bobot evaluasi pemasok dan aturan Approved/Conditional/Suspended? | Lima kriteria berbobot sama sampai dikonfigurasi; keputusan Manager independen tetap wajib. |
| Q-16 | Apakah diperlukan portal pemasok atau self-assessment cukup melalui petugas internal? | Petugas internal merekam jawaban dan asal bukti; portal di luar baseline. |
| Q-17 | Tingkat kompetensi, masa berlaku, kriteria lulus, serta dampak revisi SOP pada training? | Ditetapkan per kursus/jabatan; review independen dan keputusan retraining eksplisit. |
| Q-18 | Standar/edisi apa untuk compliance dan siapa menyediakan konten klausul berlisensi? | Katalog dikonfigurasi dari konten yang berhak digunakan; tanpa klaim sertifikasi. |
| Q-19 | Provider/model AI, lokasi/retensi data, biaya, batas penggunaan, dan tolok ukur evaluasi? | Belum ditetapkan; wajib diselesaikan sebelum AI diaktifkan; alur manual tetap tersedia. |
| Q-20 | Apakah biaya kualitas memerlukan multi-currency, appraisal/prevention cost, atau integrasi keuangan? | Scrap/rework tercatat dalam IDR; perluasan biaya dan konversi kurs di luar baseline. |
| Q-21 | Siapa reviewer terjemahan Indonesia/Inggris/Jepang dan istilah kualitas resmi? | Kode internal stabil; glossary dan validasi terjemahan harus tersedia sebelum rilis R1. |

## 11. Riwayat perubahan

| Versi | Tanggal | Perubahan |
| --- | --- | --- |
| 0.1.0 | 9 Oktober 2026 | Draft requirements awal untuk enam modul inti dan fondasi spec-driven development. |
| 0.2.0 | 9 Oktober 2026 | Memetakan 12 kelompok fitur dari video lokal; menambahkan task management, quality plans, inspeksi, kalibrasi, maintenance, supplier quality, training, compliance dan AI auditor; memperkaya analitik/dokumen/audit, role, workflow, model data, NFR, AC-20–43, dan roadmap R1–R3. Mempertahankan kebutuhan tiga bahasa yang sudah ada dan memformalkannya sebagai FR-COM-008. Belum ada implementasi aplikasi. |

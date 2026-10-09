# Requirements Awal — Enterprise Quality Management System (e-QMS)

| Atribut | Nilai |
| --- | --- |
| Versi | 0.1.0 |
| Status | Draft awal untuk validasi kebutuhan |
| Tanggal | 9 Oktober 2026 |
| Pendekatan | Spec-driven development |
| Target rilis | MVP aplikasi web internal |
| Referensi visual | Screenshot dashboard yang diberikan pengguna; https://factoryqa.com/ |

Dokumen ini menjadi dasar spesifikasi fitur, desain, implementasi, dan acceptance test. Detail yang diberi label **asumsi** merupakan usulan awal, bukan keputusan bisnis yang sudah disetujui. Referensi visual mengacu pada screenshot yang diberikan, bukan verifikasi seluruh kapabilitas FactoryQA.

## 1. Tujuan produk

Menyediakan satu sistem untuk mencatat ketidaksesuaian, menyelesaikan tindakan perbaikan, menjalankan audit internal, mengendalikan dokumen, dan memantau kualitas. Setiap keputusan dan perubahan harus dapat ditelusuri, dengan akses sesuai tanggung jawab pengguna.

Hasil yang diharapkan:

- Inspector dapat mencatat masalah beserta bukti secara konsisten.
- Supervisor dapat menilai, menugaskan, dan memantau penyelesaian masalah.
- Manager dapat menyetujui keputusan penting dan melihat kondisi kualitas lintas site yang menjadi kewenangannya.
- Tim dapat menelusuri hubungan antara NCR, CAPA, temuan audit, dan revisi dokumen.
- Metrik dashboard dapat direkonsiliasi dengan data sumber melalui drill-down.

## 2. Ruang lingkup dan asumsi

### 2.1 Cakupan MVP

1. Nonconformity Report (NCR) dan Corrective and Preventive Action (CAPA).
2. Audit internal, checklist, temuan, dan tindak lanjut.
3. Dokumen terkendali, revisi, review, approval, dan publikasi.
4. Dashboard, analisis defect, Pareto chart, dan tren kualitas.
5. Role-based access control untuk Inspector, Supervisor, dan Manager.
6. Audit trail atas perubahan data dan keputusan workflow.

### 2.2 Asumsi awal

| ID | Asumsi kerja |
| --- | --- |
| ASM-01 | Satu perusahaan, dengan satu atau beberapa site/pabrik; SaaS multi-tenant di luar MVP. |
| ASM-02 | Pengguna memiliki satu role aktif serta daftar site yang boleh diakses. Data operasional dimiliki satu site; dokumen dapat berlaku pada beberapa site. |
| ASM-03 | Antarmuka awal berbahasa Indonesia; istilah NCR, CAPA, dan nama status teknis boleh dipertahankan. |
| ASM-04 | Timestamp disimpan dalam UTC. Tampilan dan batas periode laporan menggunakan zona waktu perusahaan, default Asia/Jakarta. |
| ASM-05 | Manager mengelola pengguna, role, site, dan master data melalui izin administratif yang terpisah dari izin approval. Bootstrap Manager dilakukan melalui prosedur deployment. |
| ASM-06 | Approval MVP adalah persetujuan internal terautentikasi, belum merupakan tanda tangan elektronik tersertifikasi. |
| ASM-07 | Preventive action menggunakan alur CAPA yang sama, dengan jenis tindakan dan sumber risiko dicatat secara eksplisit. |

### 2.3 Di luar MVP

- AI health analysis, rekomendasi generatif, dan skor kesehatan QMS otomatis.
- Supplier quality, kalibrasi alat, training management, dan SPC/control chart.
- Eksekusi inspeksi produksi massal, integrasi mesin/IoT, ERP, MES, serta impor historis.
- Aplikasi mobile native, mode offline, SSO, dan notifikasi email/WhatsApp.
- Klaim sertifikasi ISO atau kepatuhan regulasi tertentu tanpa analisis kebutuhan tambahan.

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

**Must** wajib untuk MVP. **Should** dapat ditunda dengan alasan yang dicatat dalam rencana rilis.

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
| FR-ANA-007 | Ringkasan kondisi kualitas menggunakan hitungan faktual: backlog, overdue, dan penyelesaian; skor kesehatan dan rekomendasi AI di luar MVP. | Must |
| FR-ANA-008 | Perbandingan dengan periode sebelumnya ditampilkan hanya untuk periode sebanding; jika pembanding nol, tampilkan N/A, bukan persentase menyesatkan. | Should |

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

Dashboard menyajikan **status saat ini**, bukan rekonstruksi status pada tanggal historis. Perubahan status/kategori dapat memperbarui laporan periode lama; audit trail menyimpan riwayat perubahan. Saat tidak ada data, tampilkan empty state dan tidak menggambar persentase kumulatif fiktif. Defect rate/PPM belum tersedia karena denominator produksi yang andal belum masuk cakupan MVP.

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

## 5. Arah UI/UX

### 5.1 Struktur navigasi

Dashboard · NCR & CAPA · Audit Internal · Dokumen · Analisis Kualitas · Audit Trail · Pengaturan. Menu dan aksi mengikuti izin pengguna. Detail entitas menyediakan ringkasan, status, owner/tenggat, bukti, catatan terkait, dan timeline perubahan.

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

UI mengambil inspirasi struktur informasi dan nuansa visual screenshot. Branding, aset, dan komponen dirancang untuk proyek e-QMS ini.

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

Aturan relasi: satu NCR dapat memiliki beberapa CAPA; satu CAPA dapat menangani beberapa sumber dalam site yang sama. Penutupan sumber mensyaratkan seluruh tindakan terkait selesai. Reopen tindakan yang menopang penutupan sumber harus mengevaluasi ulang dan membuka sumber terkait. Hubungan lintas site ditunda dari MVP. Dokumen yang dirujuk sebagai bukti ditautkan ke revisi tertentu, bukan hanya identitas dokumen terbaru.

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

**Definition of Ready per fitur:** kebutuhan dan cakupan jelas; role serta state transition terdefinisi; data wajib, kegagalan, dan acceptance criteria tersedia; keputusan yang menghalangi implementasi telah diselesaikan.

**Definition of Done per fitur:** requirement Must terkait diimplementasikan; pengujian positif/negatif lulus; izin diperiksa di server; perubahan tercatat di audit trail; UI memiliki state lengkap; spesifikasi, dokumentasi operasional, dan traceability diperbarui. MVP selesai setelah seluruh Must lintas fitur dan target nonfungsional yang disepakati tervalidasi.

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

## 11. Riwayat perubahan

| Versi | Tanggal | Perubahan |
| --- | --- | --- |
| 0.1.0 | 9 Oktober 2026 | Draft requirements awal untuk enam modul inti dan fondasi spec-driven development. |

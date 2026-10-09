# SPEC-001 — Spesifikasi lintas sistem e-QMS

Status: draft design baseline, 9 Oktober 2026. Sumber: [requirements v0.2.0](../../docs/requirements.md). Semua ID FR/NFR dan AC di sana tetap normatif. Dokumen ini menyatukan interaksi lintas modul; tidak mengganti aturan detail requirements.

## S1. Pengguna dan batas akses

Sebagai Inspector saya melihat data milik/penugasan yang berizin; sebagai Supervisor saya mengatur pekerjaan dalam site berizin; sebagai Manager saya memberi keputusan independen sesuai izin modul. Izin administratif terpisah dari approval. Semua pengguna dapat memilih bahasa Indonesia, Inggris, atau Jepang.

- Membership site adalah syarat, bukan jaminan boleh membaca setiap record; klasifikasi dokumen, assignment, dan izin personel tetap diperiksa.
- Akun nonaktif atau membership dicabut langsung memengaruhi request berikutnya, termasuk unduhan, hasil pencarian, ekspor dan AI.
- Data tak berizin tidak bocor melalui judul, jumlah, grafik, snippet, notifikasi atau linked record.
- Ketiadaan approver sah memblokir keputusan; tidak ada auto-approval atau eskalasi izin implisit.
- Cakupan: FR-ACC-001–005, FR-COM-007–008; bukti utama AC-03, AC-05, AC-17, AC-22, AC-40.

## S2. Alur inti kualitas R1

Sebagai pelapor, saya menyimpan draft, melampirkan bukti dan mengajukan NCR; Supervisor melakukan triage, containment, assignment serta kebutuhan CAPA. Rencana CAPA menjalani review dan approval, action item diimplementasikan, efektivitas diverifikasi, lalu Manager independen menutup CAPA dan NCR.

- Critical memerlukan CAPA; penutupan sumber menunggu seluruh tindakan tertaut selesai.
- Reopen tindakan membuka kembali NCR/temuan/audit yang penutupannya bergantung padanya, dalam transaksi yang sama.
- Audit memisahkan approval laporan dari penutupan tindak lanjut. Auditor tidak ditugaskan pada area tanggung jawab operasionalnya.
- Dokumen memiliki revisi/working version yang jelas; approval sebelum effective date tidak mengganti revisi yang sedang berlaku.
- Tugas dari workflow adalah proyeksi kewajiban; menyelesaikannya tidak dapat melewati workflow sumber. Tugas mandiri memiliki workflow sendiri.
- Cakupan: FR-NCR-001–008, FR-CAPA-001–007, FR-AUD-001–010, FR-DOC-001–013, FR-TSK-001–005; AC-01–09, AC-13–16, AC-19–23, AC-43.

## S3. Operasional kualitas R2

Sebagai inspector, saya menjalankan inspeksi memakai quality plan Effective, mengisi hasil per unit sampel, dan memilih alat dengan kalibrasi sah. Hasil gagal menautkan NCR dan tetap gagal meskipun Manager memberikan disposisi khusus.

- Plan dan drawing di-snapshot saat inspeksi dibuat; perubahan revisi tidak memodifikasi hasil historis.
- Kosong bukan nol; numerik dibandingkan sebelum pembulatan; unit dan presisi eksplisit.
- Finalisasi memerlukan hasil wajib lengkap, reviewer independen dan NCR untuk kegagalan; amendment tidak menimpa versi final lama.
- Kalibrasi Fail yang diajukan segera menahan alat. Pelepasan membutuhkan hasil lulus terverifikasi dan keputusan independen; hasil inspeksi terdampak dinilai, tidak ditulis ulang otomatis.
- Maintenance memakai kalender tetap, occurrence unik dan verifikasi; menyelesaikan work order tidak meluluskan kalibrasi.
- Pemasok memiliki status per site, sertifikat dan evaluasi berversi; skor/expiry tidak mengubah status approval tanpa keputusan.
- Cakupan: FR-QPL-001–006, FR-INS-001–007, FR-CAL-001–006, FR-MNT-001–005, FR-SUP-001–006; AC-25–34, AC-41–42.

## S4. Kompetensi dan compliance R3

Sebagai pengelola training, saya menetapkan kompetensi, menugaskan kursus dan meminta verifikasi bukti; sebagai evaluator compliance saya menautkan bukti ke klausul dan mengajukan assessment. AI dapat mengusulkan gap yang harus diperiksa manusia.

- Membuka materi atau mencapai progres 100% belum Qualified. Expiry dan tingkat kompetensi menentukan pemenuhan kewajiban.
- Revisi SOP tidak langsung menghapus kualifikasi; keputusan dampak/retraining dicatat.
- Readiness dihitung dari klausul snapshot applicable dan keputusan manusia; N/A tanpa persetujuan tetap masuk penyebut.
- AI hanya menerima bukti yang diseleksi dan berizin, memberi sumber/lokasi, serta menghasilkan usulan. Menerima usulan membuat draft; tidak memberi keputusan final.
- Saat provider gagal, assessment manual dan metrik faktual tetap tersedia.
- Cakupan: FR-TRN-001–006, FR-CMP-001–005, FR-AI-001–004; AC-35–39, AC-43.

## S5. Informasi, bukti, dan konsistensi

Sebagai pengguna saya dapat menelusuri nilai dashboard ke daftar sumber berizin, melihat waktu pembaruan, serta membuka bukti dan histori keputusan. Pencarian, ekspor, dan data grafik menggunakan policy yang sama dengan detail record.

- Business write dan audit event berhasil atau gagal bersama; event tidak dapat ditimpa role aplikasi.
- Conflict versi tidak boleh menghilangkan perubahan pengguna lain; retry tidak menggandakan nomor, keputusan atau notifikasi.
- Lampiran quarantine tidak dapat dipakai sebagai bukti siap review; file berbahaya tidak dipublikasikan.
- Waktu kejadian disimpan UTC, due date adalah tanggal perusahaan; locale tidak mengubah zona waktu.
- Formula metrik mengikuti bagian 4.4 requirements, termasuk populasi NCR, jumlah unit sampel, biaya tercatat, aging dan pengecualian filter.
- Cakupan: FR-ANA-001–012, FR-LOG-001–006, FR-COM-001–010 dan seluruh NFR; AC-10–12, AC-14–19, AC-23–24, AC-29, AC-40–41.

## S6. Perilaku kegagalan dan batas produk

| Keadaan | Perilaku wajib |
| --- | --- |
| Database gagal / audit event gagal | Tidak ada mutasi bisnis parsial; tampilkan error dengan correlation ID |
| File scanner gagal | Lampiran tetap quarantine/pending; record draft boleh disimpan, keputusan yang membutuhkan bukti tertahan |
| Dua pengguna mengedit | Request versi lama gagal conflict; UI mempertahankan input untuk dibandingkan |
| Worker mati setelah side effect | Lease/retry pulih; efek DB idempoten; efek eksternal direkonsiliasi sebelum ulang |
| Hak baca dicabut saat job antre | Job memeriksa ulang akses, batal/gagal terkontrol; hasil tidak dapat diunduh |
| Data kosong / penyebut nol | Empty state atau N/A sesuai metrik; tidak membuat angka fiktif |
| AI tidak dikonfigurasi | Aksi AI dinonaktifkan dengan alasan; alur manual tetap aktif |

Stack ditentukan pengguna. Multi-tenant, SPC, offline, integrasi mesin/ERP, SSO, portal pemasok dan e-signature tersertifikasi tetap di luar baseline. Standar/regulasi spesifik dan konten berlisensi mengikuti keputusan bisnis terbuka.

## S7. Acceptance dan traceability

[Traceability](../../docs/design/traceability.md) menghubungkan seluruh requirement dan AC ke desain, test intent serta task. Acceptance criteria adalah kontrak uji yang direncanakan; tidak ada hasil runtime lulus pada tahap dokumentasi ini. Uji concurrency, isolasi site, invalid transition, batas numerik, kegagalan worker, dan restore menjadi syarat rilis, bukan hanya uji happy path.

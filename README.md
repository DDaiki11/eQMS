# e-QMS

Enterprise Quality Management System berbasis web untuk mengelola kualitas, audit, dokumen, dan operasional pabrik dengan pendekatan spec-driven development serta Next.js, TypeScript, PostgreSQL, Prisma, dan Docker.

**Status: tahap spesifikasi dan desain.** Repository saat ini berisi requirements, desain teknis, dan rencana implementasi. Kode aplikasi, migrasi database, konfigurasi Docker, serta pengujian runtime belum tersedia.

## Tentang proyek

e-QMS dirancang untuk menyatukan pelaporan masalah kualitas, tindakan perbaikan, pengendalian dokumen, dan kegiatan operasional dalam satu aplikasi web internal. Setiap perubahan dan keputusan dapat ditelusuri melalui audit trail, dengan akses sesuai role, site, dan penugasan pengguna.

Target penggunaan adalah satu perusahaan dengan satu atau beberapa site/pabrik. Role dasar meliputi **Inspector**, **Supervisor**, dan **Manager**. Antarmuka direncanakan mendukung bahasa **Indonesia, Inggris, dan Jepang**.

## Cakupan fitur dan roadmap

Seluruh fitur berikut merupakan cakupan yang direncanakan; belum diimplementasikan.

| Rilis | Fokus | Fitur |
| --- | --- | --- |
| **R1 — Fondasi dan MVP** | Alur inti manajemen kualitas | Autentikasi dan hak akses, NCR/CAPA, audit dan temuan, dokumen terkendali, task management, dashboard, Pareto, tren, biaya kualitas, pencarian, notifikasi dalam aplikasi, dan audit trail |
| **R2 — Operasional kualitas** | Pengendalian kualitas di pabrik | Quality plans dan drawing ballooning manual, inspeksi berbasis sampel, kalibrasi alat, preventive maintenance, serta supplier quality |
| **R3 — Kompetensi dan compliance** | Kompetensi dan kesiapan pemenuhan persyaratan | Training dan matriks kompetensi, assessment compliance berbasis bukti, serta AI auditor dengan review manusia |

NCR (*Nonconformity Report*) digunakan untuk mencatat ketidaksesuaian. CAPA (*Corrective and Preventive Action*) mengelola tindakan perbaikan/pencegahan sampai verifikasi efektivitas dan penutupan.

Rancangan menekankan approval independen, riwayat revisi yang terjaga, keterlacakan bukti, dan metrik yang dapat ditelusuri ke data sumber. AI auditor direncanakan sebagai bantuan analisis; keputusan final tetap mengikuti workflow manusia.

## Tech stack

| Teknologi | Peran dalam desain |
| --- | --- |
| **Next.js** | Aplikasi web dengan App Router, Server/Client Components, dan API Route Handlers |
| **TypeScript** | Bahasa utama untuk UI, kontrak API, aturan bisnis, dan worker |
| **PostgreSQL** | Database transaksi, relasi data, audit trail, dan antrean pekerjaan |
| **Prisma** | Akses data dan pengelolaan migrasi database |
| **Docker** | Container aplikasi dan layanan pendukung; Docker Compose untuk lingkungan pengembangan dan baseline deployment internal |

Arsitektur yang dipilih adalah **modular monolith**: modul bisnis berada dalam satu codebase, dengan proses web dan worker terpisah. Worker menangani pekerjaan seperti reminder, publikasi terjadwal, pemindaian lampiran, ekspor, dan analisis AI. Versi paket akan dikunci setelah uji kompatibilitas pada tahap fondasi.

## Dokumentasi

Mulai dari requirements untuk memahami kebutuhan produk, lalu baca desain dan spesifikasi sebelum mengerjakan task implementasi.

| Dokumen | Isi |
| --- | --- |
| [Requirements](docs/requirements.md) | Ruang lingkup, role, aturan bisnis, workflow, kebutuhan nonfungsional, dan acceptance criteria |
| [Desain teknis](docs/design.md) | Ringkasan arsitektur, keputusan teknis, dependensi, dan cakupan rilis |
| [Arsitektur dan keamanan](docs/design/architecture.md) | Batas modul, otorisasi, transaksi, file privat, jobs, dan AI |
| [Model data](docs/design/data-model.md) | ERD, entitas, relasi, constraint, indeks, dan strategi revisi |
| [API dan workflow](docs/design/api-workflows.md) | Kontrak HTTP, endpoint, transisi status, serta aturan konsistensi |
| [UI/UX](docs/design/ui-ux.md) | Navigasi, wireframe, komponen, interaksi, aksesibilitas, dan bahasa |
| [Operasional dan pengujian](docs/design/operations-testing.md) | Deployment, migrasi, backup/restore, observability, dan strategi pengujian |
| [Traceability](docs/design/traceability.md) | Pemetaan requirements dan acceptance criteria ke desain serta task |
| [Architecture Decision Records](docs/adr/README.md) | Alasan dan konsekuensi keputusan arsitektur |
| [Spesifikasi sistem](specs/001-system-design/spec.md) | Perilaku lintas modul, user journey, dan invariant |
| [Rencana implementasi](specs/001-system-design/plan.md) | Pemetaan spesifikasi ke komponen dan urutan dependensi |
| [Daftar task](specs/001-system-design/tasks.md) | Pekerjaan implementasi bertahap beserta requirement dan bukti pengujiannya |

## Pendekatan pengembangan

Proyek mengikuti **spec-driven development** dengan urutan:

1. Tetapkan kebutuhan dan acceptance criteria dalam requirements.
2. Susun `spec.md` per fitur untuk perilaku, aturan bisnis, dan kasus kegagalan.
3. Susun `plan.md` untuk desain data, API, UI, keamanan, dan pengujian.
4. Pecah implementasi dalam `tasks.md` yang ditautkan ke ID requirement.
5. Implementasikan fitur, verifikasi acceptance criteria, dan perbarui traceability.

Paket yang sudah tersedia adalah `specs/001-system-design`. Paket spesifikasi fitur berikutnya akan dibuat sesuai daftar task. Perubahan kebutuhan harus memperbarui spesifikasi, desain, dan pengujian terkait dalam perubahan yang sama.

## Struktur repository

```text
.
├── README.md
├── LICENSE
├── docs/
│   ├── requirements.md
│   ├── design.md
│   ├── video-reference.md
│   ├── design/
│   └── adr/
└── specs/
    └── 001-system-design/
        ├── spec.md
        ├── plan.md
        └── tasks.md
```

## Memulai

Untuk saat ini, gunakan [daftar task](specs/001-system-design/tasks.md) sebagai titik awal pengembangan. Tahap pertama adalah memverifikasi kompatibilitas stack, membuat fondasi aplikasi, serta menyiapkan autentikasi, database, audit trail, dan lingkungan Docker.

Perintah instalasi, menjalankan aplikasi, migrasi, dan pengujian akan ditambahkan setelah scaffolding tersedia. Belum ada aplikasi yang dapat dijalankan dari repository ini.

## Kontribusi

Sebelum mengubah fitur, baca requirements dan desain terkait. Sertakan ID requirement, perubahan perilaku, pembaruan dokumentasi, dan hasil pengujian yang relevan dalam pull request. Gunakan data sintetis untuk contoh atau fixture, dan jangan menyertakan credential maupun data operasional sensitif.

## Lisensi

Proyek ini menggunakan [MIT License](LICENSE).

Copyright (c) 2026 Dimas Hidayatulhaq.

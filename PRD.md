Product Requirement Document (PRD): Toko Sayur 2.0 (Mini-ERP & Commerce Platform)
1. Ikhtisar Proyek (Project Overview)
Nama Produk: Toko Sayur 2.0 (Platform E-Commerce & Manajemen Toko Sederhana)

Tujuan Utama: Mengubah sistem tugas kuliah konvensional (PHP/XAMPP) menjadi aplikasi web modern berbasis cloud yang berfungsi ganda: sebagai etalase penjualan online berorientasi konversi ke WhatsApp, sekaligus sistem internal (admin dashboard) untuk mengelola inventaris, produk, dan arus kas harian.

Target Pengguna:

Pelanggan: Ibu rumah tangga / pembeli retail yang ingin belanja sayur segar dengan cepat.

Admin / Pemilik Toko: Pengelola toko yang butuh sistem praktis untuk ganti foto produk, cek stok, dan memantau keuangan tanpa ribet.

Tech Stack: Next.js (App Router), Tailwind CSS, Supabase (Database PostgreSQL, Auth, & Storage), Vercel (Hosting).

2. Batasan Proyek (Scope & Out of Scope)
Dalam Cakupan (In-Scope):

Katalog produk publik dengan fitur filter kategori dan pencarian.

Keranjang belanja (shopping cart) interaktif.

Checkout otomatis yang mengonversi pesanan menjadi format teks rapi via WhatsApp API.

Autentikasi admin (Login/Logout aman).

Panel Admin CRUD untuk Produk (Tambah, Edit, Hapus, Upload Foto ke Supabase Storage).

Pencatatan Arus Kas (Cash Flow) sederhana untuk laba-rugi harian/bulanan.

Diluar Cakupan (Out-of-Scope untuk Versi 2.0):

Payment gateway otomatis (Midtrans/Xendit) — pembayaran murni via transfer manual atau COD yang dikoordinasikan lewat WhatsApp.

Sistem pengiriman kurir pihak ketiga (JNE/GoSend otomatis) — pengaturan ongkir manual atau flat rate.

3. Fitur Utama & Kebutuhan Fungsional (Functional Requirements)
A. Sisi Publik (Storefront & Katalog Pelanggan)
Halaman Beranda & Katalog (/):

Menampilkan Hero Section dengan banner promosi toko.

Daftar produk sayur lengkap dengan foto, nama, harga per satuan (kg/ikat), dan status ketersediaan (Stok Habis / Ready).

Fitur filter kategori (Sayur Daun, Buah, Bumbu Dapur) dan search bar pencarian produk instan.

Keranjang Belanja (Cart Modal/Page):

Pelanggan dapat menambah/mengurangi jumlah barang yang ingin dibeli.

Sistem menghitung total harga otomatis secara real-time.

Checkout WhatsApp:

Tombol "Pesan Sekarang" yang membuka tautan WhatsApp (wa.me) dengan format pesan yang sudah terisi otomatis (Rincian pesanan, total harga, nama pembeli, dan alamat kirim).

B. Sisi Internal (Admin Dashboard)
Autentikasi Admin (/admin/login):

Halaman login khusus menggunakan email dan password yang dilindungi oleh Supabase Auth dan Route Guard (tidak bisa diakses publik secara sembarangan).

Manajemen Produk & Stok (/admin/products):

Create: Menambah produk baru (Nama, Kategori, Harga, Stok awal, Satuan, dan Upload Gambar). Gambar diunggah langsung ke Supabase Storage.

Read: Tabel daftar produk dengan informasi stok terkini.

Update: Mengubah harga, stok, atau mengganti foto produk kapan saja tanpa perlu redeploy kode.

Delete: Menghapus produk dari katalog.

Pencatatan Arus Kas / Keuangan (/admin/finance):

Formulir pencatatan kas masuk (hasil penjualan) dan kas keluar (modal kulakan sayur ke pasar).

Rekapitulasi sederhana untuk melihat total pemasukan, pengeluaran, dan estimasi keuntungan bersih (net profit).

4. Perancangan Struktur Database (Supabase PostgreSQL)
Berikut adalah skema tabel yang perlu kamu buat di Supabase:

1. Tabel categories (Kategori Produk)
id (UUID / Serial, PK)

name (Text: e.g., "Sayur Daun", "Bumbu Dapur")

created_at (Timestamp)

2. Tabel products (Katalog Produk & Stok)
id (UUID / Serial, PK)

category_id (Integer/UUID, Foreign Key ke categories.id)

name (Text)

price (Integer)

stock (Integer)

unit (Text: e.g., "kg", "ikat", "pack")

image_url (Text - link asset dari Supabase Storage)

created_at (Timestamp)

3. Tabel cash_flow (Pencatatan Keuangan)
id (UUID / Serial, PK)

type (Text: 'IN' untuk pemasukan / 'OUT' untuk pengeluaran)

amount (Integer)

description (Text: e.g., "Penjualan sayur harian" atau "Belanja stok di pasar")

created_at (Timestamp)

5. Alur Kerja Pengguna (User Flow)
Plaintext
[Pelanggan Buka Website] 
       │
       ├──> Cari & Filter Sayur 
       ├──> Masukkan ke Keranjang
       └──> Klik "Checkout via WhatsApp" ──> Otomatis Redirect ke Chat Admin (Pesan Terformat)

[Admin Toko Login ke /admin/login]
       │
       ├──> Tambah/Edit/Hapus Produk & Update Stok (CRUD ke Supabase)
       └──> Catat Modal Belanja & Hasil Penjualan di Modul Cash Flow
6. Kriteria Keberhasilan (Acceptance Criteria)
Website berhasil dimuat di Vercel dengan performa cepat (Server-Side Rendering Next.js).

Admin dapat mengunggah foto sayur baru melalui dashboard, dan foto tersebut langsung tampil secara real-time di halaman depan katalog publik.

Tombol checkout sukses membawa teks pesanan yang akurat ke aplikasi WhatsApp.

Seluruh data tersimpan aman di cloud Supabase menggantikan database lokal XAMPP lama.
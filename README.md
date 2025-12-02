# Tokokita

Aplikasi manajemen produk sederhana berbasis Flutter yang terhubung dengan konsep REST API (Simulasi Backend CodeIgniter 4). Aplikasi ini memungkinkan pengguna untuk melakukan Registrasi, Login, melihat Daftar Produk, melihat Detail, serta Menambah/Mengubah/Menghapus produk.

## Identitas
Nama : Sarah Shiba Huwaidah<br>
NIM : H1D023044<br>
Shift : C<br>
Shift KRS : A<br>

## Dokumentasi & Alur Aplikasi

Berikut adalah penjelasan langkah demi langkah penggunaan aplikasi beserta tampilan antarmukanya.

### 1. Proses Registrasi
Pengguna baru harus mendaftarkan akun terlebih dahulu agar data tersimpan di database.

* **Langkah 1 (Isi Form):** Menginputkan Nama, Email, Password, dan Konfirmasi Password. Validasi memastikan email memiliki format yang benar (`@`) dan password minimal 6 karakter.
* **Langkah 2 (Sukses):** Jika data valid, sistem akan menampilkan *Popup Sukses*.

| Form Registrasi (Validasi) | Registrasi Sukses |
| :---: | :---: |
| <img width="200" src="https://github.com/user-attachments/assets/3f63f0d1-5977-4c84-89de-d6967d8aad82" />| <img width="200" src="https://github.com/user-attachments/assets/0ab06b02-13b5-47d1-a992-5a787a62bdd6" />|

> **Kode Logic (`RegistrasiBloc`):** Mengirim data via `POST` ke endpoint `/registrasi`. Jika respon sukses (200), menampilkan dialog sukses.

### 2. Proses Login
Setelah registrasi berhasil, pengguna masuk menggunakan email dan password yang terdaftar.

* **Langkah 1 (Isi Form):** Input Email dan Password.
* **Langkah 2 (Validasi):** Jika password salah atau email tidak ditemukan, muncul *Popup Gagal*.
* **Langkah 3 (Sukses):** Jika benar, token disimpan (Auto Login) dan masuk ke dashboard.

| Form Login | Login Gagal |
| :---: | :---: |
|  <img width="200" src="https://github.com/user-attachments/assets/714a210b-c72f-4b64-a08d-375abb56d665" />|  <img width="200" src="https://github.com/user-attachments/assets/9ae969f3-84c4-4221-874b-c001aa32515f" />|

> **Kode Logic (`LoginBloc`):** Melakukan verifikasi ke API. Jika sukses, token dan UserID disimpan ke penyimpanan lokal HP menggunakan `UserInfo().setToken()`.

### 3. Dashboard (List Produk) - READ
Halaman utama yang menampilkan seluruh data produk yang diambil dari database server.

* **Tampilan:** Menggunakan `FutureBuilder` untuk mengambil data dari API dan menampilkannya dalam list.
* **Fitur:** Tombol Tambah (+) dan Menu Drawer untuk Logout.

| List Produk | Menu Logout |
| :---: | :---: |
|  <img width="200" src="https://github.com/user-attachments/assets/528f15b6-6d98-49f2-b369-f2bbb67b1c94" />|<img width="200" src="https://github.com/user-attachments/assets/20be5c01-8896-42d2-8d94-c565a467a41b" />|

> **Kode Logic (`ProdukBloc.getProduks`):** Melakukan request `GET` ke API. Data JSON diubah menjadi List Objek dan ditampilkan.

### 4. Tambah Produk - CREATE
Menambahkan data barang jualan baru ke toko.

* **Proses:** Klik tombol (+) -> Isi Kode, Nama, Harga -> Klik SIMPAN.

| Form Tambah Produk |
| :---: |
|  <img width="200" src="https://github.com/user-attachments/assets/b8d41942-c324-44ec-84c4-9d54530428a9" />|

> **Kode Logic (`ProdukForm`):** Memanggil `ProdukBloc.addProduk()`. Form ini bersifat *reusable* (digunakan untuk tambah & edit).

### 5. Detail & Ubah Produk - UPDATE
Melihat rincian produk dan melakukan revisi data.

* **Detail:** Klik salah satu item di List Produk.
* **Ubah:** Klik tombol "EDIT". Form akan terisi data lama secara otomatis.
* **Proses:** Ubah data (misal harga) -> Klik "UBAH".

| Halaman Detail | Form Ubah Produk |
| :---: | :---: |
|  <img width="200" src="https://github.com/user-attachments/assets/d2cd57a5-89eb-493f-b893-d647af2ac2bd" />|<img width="200" src="https://github.com/user-attachments/assets/b8792a98-79c4-4c5c-81d0-20df7a048978" />|

> **Kode Logic (`ProdukBloc.updateProduk`):** Mengirim data revisi beserta **ID Produk** ke API menggunakan method `PUT` agar data lama diperbarui.

### 6. Hapus Produk - DELETE
Menghapus data produk yang sudah tidak diperlukan.

* **Proses:** Klik tombol "DELETE" di halaman detail -> Muncul Konfirmasi -> Klik "Ya".

| Konfirmasi Hapus |
| :---: |
|  <img width="200" src="https://github.com/user-attachments/assets/95552526-92aa-46e1-98db-18f2fea8939c" />|

> **Kode Logic:** Menggunakan `AlertDialog` untuk konfirmasi, kemudian memanggil `ProdukBloc.deleteProduk()` untuk menghapus data di server.

---

## Penjelasan Struktur Kode

Berikut adalah fungsi utama dari setiap file dalam folder `lib/ui` dan `lib/bloc`:

### 1. `main.dart`
- **Fungsi:** Pintu gerbang aplikasi.
- **Logika:** Mengecek sesi login (`UserInfo().getToken()`). Jika ada token -> Buka `ProdukPage`, jika tidak -> Buka `LoginPage`.

### 2. `login_page.dart`
- **Fungsi:** Halaman autentikasi.
- **Validasi:** Memastikan input tidak kosong.
- **Navigasi:** Menggunakan `pushReplacement` saat login berhasil agar pengguna tidak bisa kembali ke halaman login.

### 3. `registrasi_page.dart`
- **Fungsi:** Pendaftaran pengguna.
- **Validasi:** Menggunakan logika sederhana `!value.contains('@')` untuk validasi email guna mencegah error pada inputan keyboard HP tertentu.

### 4. `produk_page.dart` (Dashboard)
- **Fungsi:** Menampilkan daftar produk.
- **Teknis:** Menggunakan `ListView.builder` yang dibungkus `FutureBuilder` untuk menangani proses *loading* data dari internet (Asynchronous).

### 5. `produk_detail.dart`
- **Fungsi:** Menampilkan detail satu produk spesifik.
- **Aksi:** Menyediakan tombol navigasi ke halaman Edit dan tombol eksekusi Hapus.

### 6. `produk_form.dart`
- **Fungsi:** Form *reusable* (bisa dipakai ulang) untuk Tambah dan Edit.
- **Logika:**
    - Jika `widget.produk == null`: Judul "TAMBAH", tombol "SIMPAN" (Create).
    - Jika `widget.produk != null`: Judul "UBAH", tombol "UBAH", kolom terisi otomatis (Update).

### 7. `produk_bloc.dart` (Logic)
- **Fungsi:** Jembatan antara UI (Tampilan) dan API (Server).

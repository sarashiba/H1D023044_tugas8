# Tokokita

Aplikasi manajemen produk sederhana berbasis Flutter yang terhubung dengan konsep REST API (Simulasi). Aplikasi ini memungkinkan pengguna untuk melakukan Registrasi, Login, melihat Daftar Produk, melihat Detail, serta Menambah/Mengubah/Menghapus produk.

## Tampilan Aplikasi (Screenshots)

<img width="270" height="602" alt="image" src="https://github.com/user-attachments/assets/0350f2f4-976a-4b6e-81a5-c558f736fe6f" />
<img width="270" height="602" alt="image" src="https://github.com/user-attachments/assets/a9f36815-9071-4796-bd67-90da334def16" />
<img width="270" height="602" alt="image" src="https://github.com/user-attachments/assets/e4b34927-3476-409a-9077-39a2edb9f4eb" />
<img width="270" height="602" alt="image" src="https://github.com/user-attachments/assets/1374507b-b471-47c9-9389-310c9d7a4ac4" />
<img width="270" height="602" alt="image" src="https://github.com/user-attachments/assets/2e9932dc-0b75-44d5-be34-5345a4b69316" />
<img width="270" height="602" alt="image" src="https://github.com/user-attachments/assets/187612f6-98d1-43ee-8d85-aa211e25eaff" />


## Penjelasan Kode

### 1. `main.dart`
- **Fungsi:** Mengatur tema awal dan menentukan halaman pertama yang dibuka.
- **Logika:** `home:` diset ke `LoginPage()` agar pengguna wajib login terlebih dahulu sebelum masuk ke menu utama.

### 2. `login_page.dart`
Halaman untuk autentikasi pengguna.
- **Validasi:** Memastikan format email benar dan password tidak kosong.
- **Navigasi:**
  - Jika tombol **Login** ditekan (dan validasi sukses), aplikasi menggunakan `Navigator.pushReplacement` untuk pindah ke `ProdukPage`
  - Teks **Registrasi** mengarahkan pengguna ke `RegistrasiPage`.

### 3. `registrasi_page.dart`
Halaman pendaftaran pengguna baru.
- **Form:** Berisi input Nama, Email, Password, dan Konfirmasi Password.
- **Validasi:** Memastikan password minimal 6 karakter dan konfirmasi password cocok dengan password utama.

### 4. `produk_page.dart`
Halaman utama setelah login berhasil.
- **Tampilan:** Menggunakan `ListView` untuk menampilkan daftar produk.
- **Fitur:**
  - **Drawer:** Menu samping yang berisi tombol Logout (kembali ke Login).
  - **Tombol Tambah (+):** Di pojok kanan atas AppBar untuk membuka `ProdukForm` mode tambah.
  - **Klik Item:** Jika salah satu produk diklik, akan membuka `ProdukDetail`.

### 5. `produk_detail.dart`
Halaman untuk melihat info lengkap produk.
- **Data:** Menerima objek `Produk` dari halaman sebelumnya dan menampilkannya.
- **Tombol Aksi:**
  - **Edit (Hijau):** Membuka `ProdukForm` dengan membawa data produk yang sedang dilihat untuk diedit.
  - **Delete (Merah):** Memunculkan *Dialog Konfirmasi* sebelum menghapus data.

### 6. `produk_form.dart`
Halaman fleksibel yang berfungsi ganda (Tambah & Ubah).
- **Logika:**
  - Cek `widget.produk != null`:
    - Jika **Ada Data**: Judul menjadi "UBAH PRODUK" dan kolom terisi otomatis (Mode Edit).
    - Jika **Kosong**: Judul menjadi "TAMBAH PRODUK" dan kolom kosong (Mode Tambah).

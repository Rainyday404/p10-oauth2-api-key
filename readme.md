# PRAKTIKUM #10: SIMULASI API KEY & OAUTH 2.0 (WEB SERVICE ENGINEERING)

Proyek ini adalah implementasi simulasi keamanan API menggunakan **Node.js**, **Express**, dan **MongoDB**. Proyek ini mendemonstrasikan dua lapisan keamanan: **API Key** untuk akses publik (Read-Only) dan **OAuth 2.0 (JWT)** untuk akses privat (CRUD) dengan otorisasi berbasis peran (Admin).

## SKENARIO APLIKASI
Sistem Manajemen Produk dengan dua level akses:
1.  **Akses Publik (API Key):** Klien hanya dapat melihat daftar produk.
2.  **Akses Privat (OAuth 2.0/JWT):** Pengguna terotentikasi dengan peran Admin dapat menambahkan, mengedit, dan menghapus produk.

## TOOLS ATAU ALAT
* **Backend:** Node.js & Express.js
* **Database:** MongoDB Atlas (Mongoose ODM)
* **Autentikasi:** JSON Web Tokens (JWT) & API Key
* **Keamanan:** bcryptjs untuk hashing password
* **Tools:** Postman/Insomnia untuk pengujian

## STRUKTUR PROYEK
```text
p10-oauth2-api-key-nimanda/
├── controllers/      # Logika handler (Auth & Product)
├── middleware/       # Validasi API Key & Token JWT
├── models/           # Skema Mongoose (User, Product, ApiKey)
├── routes/           # Definisi endpoint API
├── seeders/          # Script inisialisasi data awal
├── utils/            # Fungsi utilitas (Generate Token)
├── evidence/         # Bukti screenshot pengujian
├── .env              # Variabel lingkungan (Credentials)
├── package.json      # Dependencies
└── server.js         # Entry point aplikasi
````

## CARA INSTALASI & MENJALANKAN

### 1\. Prasyarat

Pastikan Anda telah menginstal Node.js dan memiliki akun MongoDB Atlas.

### 2\. Instalasi Dependencies

Jalankan perintah berikut di terminal root proyek:

```bash
npm init -y
npm install express mongoose dotenv bcryptjs jsonwebtoken
```

### 3\. Konfigurasi Lingkungan (.env)

Buat file `.env` di root folder dan isi dengan konfigurasi berikut (gunakan kredensial database Anda sendiri):

```env
PORT=3000
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/<dbname>?retryWrites=true&w=majority
JWT_SECRET=rahasia-super-aman-simulasi-jwt
```

### 4\. Database Seeding (Data Awal)

Sebelum menjalankan server, isi database dengan data awal (User, Produk, dan API Key) menggunakan script seeder:

```bash
node seeders/seed.js
```

*Pastikan output menunjukkan "Proses Seeding Database Berhasil\!".*

### 5\. Menjalankan Server

Jalankan server aplikasi:

```bash
node server.js
```

Server akan berjalan di `http://localhost:3000`

-----

## KREDENSIAL PENGUJIAN (SEEDER DATA)

Gunakan data berikut untuk pengujian di Postman:

**1. User Admin**

  * Username: `admin`
  * Password: `password123`
  * Role: `admin`
  * Keterangan: Bisa melakukan CRUD (POST, PUT, DELETE)

**2. User Biasa**

  * Username: `userbiasa`
  * Password: `userpass`
  * Role: `user`
  * Keterangan: Hanya bisa Login, tidak bisa CRUD

**3. API Key**

  * Key: `PRACTICUM_API_KEY_A_1234567890`
  * Keterangan: Masukkan di Header `x-api-key`

-----

## DOKUMENTASI HASIL PENGUJIAN API

Berikut adalah rangkuman hasil pengujian API untuk Praktikum 10 Web Service Engineering dalam format tabel.

### 1. Skenario Otentikasi (Authentication)

| No | Skenario Pengujian | Endpoint | Hasil Status | Analisis Singkat |
| :-- | :--- | :--- | :--- | :--- |
| 1 | **Login Berhasil (Admin)** | `POST /api/v1/auth/token` | `200 OK` | Server memvalidasi kredensial admin dan mengembalikan *access token* dengan role admin. |
| 2 | **Login Berhasil (User)** | `POST /api/v1/auth/token` | `200 OK` | Server memvalidasi kredensial user biasa dan mengembalikan *access token* dengan role user. |
| 3 | **Login Gagal** | `POST /api/v1/auth/token` | `401 Unauthorized` | Sistem keamanan berhasil menolak permintaan login karena password/username salah. |

---

### 2. Skenario Create Data (POST)

| No | Skenario Pengujian | Auth / Role | Hasil Status | Analisis Singkat |
| :-- | :--- | :--- | :--- | :--- |
| 1 | **Admin Membuat Produk** | Bearer Token (Admin) | `201 Created` | Data produk baru berhasil ditambahkan ke database karena user memiliki hak akses Admin. |
| 2 | **User Membuat Produk** | Bearer Token (User) | `403 Forbidden` | Permintaan ditolak. User biasa tidak memiliki izin (*permission*) untuk menambahkan data. |
| 3 | **Akses Tanpa Token** | Tidak Ada (No Auth) | `403 Forbidden` | Endpoint private terlindungi dengan baik; permintaan tanpa token valid otomatis ditolak. |

---

### 3. Skenario Update Data (PUT)

| No | Skenario Pengujian | Auth / Role | Hasil Status | Analisis Singkat |
| :-- | :--- | :--- | :--- | :--- |
| 1 | **Admin Mengupdate Produk** | Bearer Token (Admin) | `200 OK` | Perubahan data (harga/deskripsi) berhasil disimpan ke database oleh Admin. |
| 2 | **User Mengupdate Produk** | Bearer Token (User) | `403 Forbidden` | Middleware berhasil mencegah user dengan role "user" untuk mengubah data produk. |

---

### 4. Skenario Delete Data (DELETE)

| No | Skenario Pengujian | Auth / Role | Hasil Status | Analisis Singkat |
| :-- | :--- | :--- | :--- | :--- |
| 1 | **Admin Menghapus Produk** | Bearer Token (Admin) | `200 OK` | Data produk berhasil dihapus sepenuhnya dari database oleh Admin. |
| 2 | **User Menghapus Produk** | Bearer Token (User) | `403 Forbidden` | Validasi role berfungsi; user biasa tidak diizinkan melakukan operasi penghapusan data. |
-----

**Dosen Pengampu:** Muhayat, M.IT

````

---
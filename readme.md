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
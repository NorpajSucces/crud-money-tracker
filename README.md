# 💰 Money Tracker API

REST API backend untuk aplikasi pencatatan keuangan pribadi. Dibangun dengan **Express.js** dan **MongoDB** menggunakan arsitektur **MVC** (Model-View-Controller). Ini adalah project based learning saja.

## ✨ Fitur

- 🔐 **Autentikasi** — Register & Login menggunakan JWT (JSON Web Token)
- 🔒 **Keamanan Data** — Password di-hash dengan bcrypt, data transaksi terisolasi per user
- 📝 **CRUD Transaksi** — Tambah, lihat, edit, dan hapus transaksi (income/expense)
- 📊 **Ringkasan Saldo** — Endpoint khusus untuk melihat total saldo
- ⚠️ **Error Handling** — Pesan error yang informatif dan terstruktur

## 📁 Struktur Folder

```
crud-backend-money-tracker/
├── controllers/
│   ├── authController.js          # Logic register & login
│   └── transactionsController.js  # Logic CRUD transaksi & summary
├── helpers/
│   └── createError.js             # Helper untuk membuat custom error
├── middlewares/
│   ├── authentication.js          # Middleware verifikasi JWT token
│   └── errorHandler.js            # Middleware penanganan error terpusat
├── models/
│   ├── user.js                    # Schema user (username, password)
│   └── Transaction.js             # Schema transaksi (title, amount, type, userId)
├── routes/
│   ├── index.js                   # Router utama (menggabungkan semua route)
│   ├── auth.js                    # Route autentikasi (register, login)
│   └── transactions.js            # Route transaksi (CRUD + summary)
├── .env.example                   # Template environment variables
├── .gitignore                     # File yang diabaikan Git
├── index.js                       # Entry point aplikasi
├── package.json                   # Konfigurasi project & dependencies
└── README.md                      # Dokumentasi (file ini)
```

## 🛠️ Teknologi

| Teknologi    | Fungsi                          |
| ------------ | ------------------------------- |
| Express.js   | Web framework untuk Node.js     |
| MongoDB      | Database NoSQL                  |
| Mongoose     | ODM (Object Data Modeling)      |
| bcryptjs     | Hashing password                |
| jsonwebtoken | Autentikasi berbasis JWT        |
| dotenv       | Manajemen environment variables |
| nodemon      | Auto-restart server saat dev    |

## 🚀 Cara Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/username-kamu/crud-backend-money-tracker.git
cd crud-backend-money-tracker
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Setup Environment Variables

Salin file `.env.example` menjadi `.env`, lalu isi dengan konfigurasi milikmu:

```bash
cp .env.example .env
```

Isi file `.env`:

```env
PORT=3000
MONGO_URI=mongodb://127.0.0.1:27017/money-tracker
JWT_SECRET=kunci_rahasia_jwt_kamu
```

### 4. Jalankan Server

```bash
# Mode development (auto-restart)
npm run dev

# Mode production
npm start
```

Server akan berjalan di `http://localhost:3000`

## 📖 API Endpoints

### 🔓 Public (Tanpa Token)

| Method | Endpoint         | Deskripsi              | Body                            |
| ------ | ---------------- | ---------------------- | ------------------------------- |
| POST   | `/api/register`  | Mendaftar user baru    | `{ username, password }`        |
| POST   | `/api/login`     | Login & dapatkan token | `{ username, password }`        |

### 🔒 Protected (Wajib Token di Header)

Semua endpoint di bawah ini **wajib** menyertakan header:

```
Authorization: Bearer <access_token>
```

| Method | Endpoint                      | Deskripsi                      | Body                              |
| ------ | ----------------------------- | ------------------------------ | --------------------------------- |
| GET    | `/api/transactions`           | Ambil semua transaksi milik user | —                               |
| GET    | `/api/transactions/summary`   | Lihat total saldo user          | —                               |
| GET    | `/api/transactions/:id`       | Ambil transaksi berdasarkan ID  | —                               |
| POST   | `/api/transactions`           | Buat transaksi baru             | `{ title, amount, type }`       |
| PUT    | `/api/transactions/:id`       | Update transaksi                | `{ title, amount, type }` (opsional) |
| DELETE | `/api/transactions/:id`       | Hapus transaksi                 | —                               |

### Contoh Body Transaksi

```json
{
  "title": "Gaji Bulanan",
  "amount": 5000000,
  "type": "income"
}
```

> **Catatan:** Field `type` hanya boleh diisi `"income"` atau `"expense"`.

## 🧪 Contoh Penggunaan di Postman

### 1. Register
```
POST http://localhost:3000/api/register
Body (JSON): { "username": "budi", "password": "budi123" }
```

### 2. Login
```
POST http://localhost:3000/api/login
Body (JSON): { "username": "budi", "password": "budi123" }
→ Response: { "access_token": "eyJhbG..." }
```

### 3. Buat Transaksi (dengan Token)
```
POST http://localhost:3000/api/transactions
Headers: Authorization: Bearer eyJhbG...
Body (JSON): { "title": "Beli Kopi", "amount": 25000, "type": "expense" }
```

### 4. Cek Saldo
```
GET http://localhost:3000/api/transactions/summary
Headers: Authorization: Bearer eyJhbG...
```

## 📝 Catatan

- Pastikan MongoDB sudah berjalan sebelum menjalankan server.
- Jangan pernah meng-commit file `.env` ke repository (sudah diatur di `.gitignore`).
- File `.env.example` disediakan sebagai template bagi developer lain.

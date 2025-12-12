# Portal Berita - REST API & Flutter App

Aplikasi Portal Berita dengan REST API menggunakan Laravel Passport dan tampilan menarik menggunakan Flutter.

## 📁 Struktur Project

```
portal-berita/
├── api/                 # Laravel REST API
└── flutter_app/         # Flutter Mobile App
```

## 🚀 Fitur

### API (Laravel Passport)
- ✅ Authentication (Register, Login, Logout)
- ✅ CRUD Berita (Create, Read, Update, Delete)
- ✅ CRUD Komentar
- ✅ Filter berita berdasarkan kategori
- ✅ Search berita
- ✅ Protected routes dengan OAuth2

### Flutter App
- ✅ Splash Screen dengan animasi
- ✅ Home Screen dengan list berita
- ✅ Filter kategori berita
- ✅ Search berita
- ✅ Detail berita dengan komentar
- ✅ Login & Register
- ✅ Tambah komentar (pengguna login)
- ✅ Tulis berita baru (pengguna login)
- ✅ Profile pengguna
- ✅ UI/UX modern dan responsif

## 📊 Database Tables

1. **users** (Pengguna)
   - id, name, email, password, timestamps

2. **berita** (Berita)
   - id, user_id, judul, konten, gambar, kategori, status, timestamps

3. **komentar** (Komentar)
   - id, user_id, berita_id, isi, timestamps

## 🛠️ Instalasi

### 1. Setup Laravel API

```bash
cd portal-berita/api

# Install dependencies
composer install

# Copy environment file
cp .env.example .env

# Generate app key
php artisan key:generate

# Setup database (SQLite already configured)
php artisan migrate

# Seed sample data
php artisan db:seed

# Install Passport
php artisan passport:install

# Start server
php artisan serve
```

### 2. Setup Flutter App

```bash
cd portal-berita/flutter_app

# Get dependencies
flutter pub get

# Run app (pastikan API server sudah berjalan)
flutter run
```

## 📱 Konfigurasi API URL

Edit file `lib/services/api_service.dart`:

```dart
// Untuk emulator Android
static const String baseUrl = 'http://10.0.2.2:8000/api';

// Untuk device fisik (ganti dengan IP komputer Anda)
static const String baseUrl = 'http://192.168.x.x:8000/api';

// Untuk iOS Simulator
static const String baseUrl = 'http://localhost:8000/api';
```

## 🔐 API Endpoints

### Public Routes
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/register` | Register user baru |
| POST | `/api/login` | Login user |
| GET | `/api/berita` | Get semua berita |
| GET | `/api/berita/{id}` | Get detail berita |
| GET | `/api/berita/kategori/{kategori}` | Get berita by kategori |
| GET | `/api/search?q={query}` | Search berita |
| GET | `/api/berita/{id}/komentar` | Get komentar berita |

### Protected Routes (Require Auth)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/profile` | Get user profile |
| POST | `/api/logout` | Logout user |
| POST | `/api/berita` | Create berita baru |
| PUT | `/api/berita/{id}` | Update berita |
| DELETE | `/api/berita/{id}` | Delete berita |
| POST | `/api/berita/{id}/komentar` | Add komentar |
| PUT | `/api/berita/{beritaId}/komentar/{id}` | Update komentar |
| DELETE | `/api/berita/{beritaId}/komentar/{id}` | Delete komentar |

## 👤 Akun Demo

| Email | Password |
|-------|----------|
| admin@portal.com | password123 |
| john@example.com | password123 |
| jane@example.com | password123 |

## 📸 Screenshots

Flutter App Features:
- Modern gradient design
- Category filter chips
- Beautiful news cards with gradient colors
- Smooth animations
- Comment system
- User authentication flow

## 🛡️ Technology Stack

### Backend
- Laravel 11
- Laravel Passport (OAuth2)
- SQLite Database

### Frontend
- Flutter 3.x
- Provider (State Management)
- HTTP package
- Shared Preferences
- Shimmer loading effect
- Google Fonts
- Intl (Date formatting)

## 📝 License

MIT License
# 7.5-Tugas-Praktik
# 7.5-Tugas-Praktik

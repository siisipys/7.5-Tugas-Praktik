# 📱 Panduan Build APK - Portal Berita

## 🔧 Konfigurasi Sebelum Build

### 1. Pilih Mode & Platform

Edit file `lib/services/api_service.dart`:

```dart
// Untuk testing di HP (development)
static const String mode = 'dev';
static const String devPlatform = 'device';  // gunakan IP laptop

// Untuk rilis ke public (production)
static const String mode = 'prod';
static const String prodUrl = 'https://your-api.railway.app/api';
```

### 2. Testing dengan Real Device (Development)

**Langkah:**
1. Pastikan HP dan Laptop di WiFi yang sama
2. Jalankan Laravel server dengan:
   ```bash
   cd portal-berita/api
   php artisan serve --host=0.0.0.0 --port=8000
   ```
3. Ubah `mode = 'dev'` dan `devPlatform = 'device'`
4. Build APK:
   ```bash
   cd portal-berita/flutter_app
   flutter build apk
   ```
5. Install APK di HP (file ada di `build/app/outputs/flutter-apk/app-release.apk`)

**Catatan:** Backend harus tetap running di laptop selama testing.

---

## 🚀 Production (Rilis ke Public)

### Opsi 1: Deploy Backend ke Server Online

#### A. Deploy ke Railway.app (Gratis & Mudah)

1. **Daftar di Railway.app**
   - Buka https://railway.app
   - Login dengan GitHub

2. **Deploy Laravel**
   ```bash
   # Install Railway CLI
   npm i -g @railway/cli
   
   # Login
   railway login
   
   # Deploy
   cd portal-berita/api
   railway init
   railway up
   ```

3. **Setup Database**
   - Di Railway dashboard, tambah MySQL/PostgreSQL
   - Update environment variables

4. **Get Public URL**
   - Railway akan provide URL seperti: `https://your-app.railway.app`
   - Copy URL ini

5. **Update Flutter Config**
   ```dart
   static const String mode = 'prod';
   static const String prodUrl = 'https://your-app.railway.app/api';
   ```

6. **Build APK Production**
   ```bash
   flutter build apk --release
   ```

#### B. Deploy ke Render.com

1. Buka https://render.com
2. Create New > Web Service
3. Connect GitHub repo
4. Setup environment: PHP
5. Build Command: `composer install`
6. Start Command: `php artisan serve --host=0.0.0.0 --port=$PORT`

#### C. Deploy ke InfinityFree (Hosting Gratis)

1. Daftar di https://infinityfree.net
2. Upload Laravel files via FTP
3. Setup database MySQL
4. Update .env dengan database credentials

---

### Opsi 2: Migrasi ke Firebase (Tanpa Backend Server)

**Keuntungan Firebase:**
- ✅ Gratis (quota tinggi)
- ✅ Tidak perlu server/hosting
- ✅ Real-time updates
- ✅ Scalable otomatis
- ✅ APK langsung bisa jalan

**Anda sudah punya project `Portal-Berita-firebase` yang siap pakai!**

Untuk menggunakan versi Firebase:

```bash
cd Portal-Berita-firebase
flutter pub get

# Setup Firebase
flutterfire configure

# Build APK
flutter build apk --release
```

APK ini langsung bisa diinstall di HP manapun tanpa perlu backend server!

---

## 📊 Perbandingan

| Aspek | Laravel API | Firebase |
|-------|-------------|----------|
| **Biaya** | Perlu hosting (~$5/bulan) | Gratis (quota besar) |
| **Setup** | Butuh deploy & maintenance | Sekali setup, selesai |
| **Kecepatan** | Tergantung server | Sangat cepat |
| **Real-time** | Perlu setup manual | Built-in |
| **Scalability** | Manual upgrade | Auto-scale |
| **Control** | Full control | Terbatas pada Firebase |

---

## 🎯 Rekomendasi

### Untuk Tugas Kuliah / Portfolio:
→ **Gunakan Firebase** (Portal-Berita-firebase)
- Mudah demo ke dosen
- APK bisa langsung dibagikan
- Tidak perlu maintain server

### Untuk Project Profesional:
→ **Deploy Laravel ke Railway/Render**
- Full control backend
- Custom business logic
- Integrasi dengan sistem lain

---

## 🛠️ Troubleshooting

### APK tidak bisa connect ke API

**Cek:**
1. API URL di `api_service.dart` sudah benar?
2. Server online bisa diakses dari browser?
3. CORS sudah dikonfigurasi di Laravel?
4. Firewall tidak blocking port?

### Error saat build APK

```bash
# Clean build cache
flutter clean
flutter pub get
flutter build apk --release
```

---

## 📝 Checklist Sebelum Build APK

- [ ] API URL sudah di-set ke production
- [ ] Backend server sudah online & accessible
- [ ] CORS sudah dikonfigurasi
- [ ] Database sudah ada data sample
- [ ] Testing di browser dulu (flutter run -d chrome)
- [ ] App icon & name sudah disesuaikan
- [ ] Version number sudah di-update di pubspec.yaml

---

**Good luck dengan project Anda! 🚀**


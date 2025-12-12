# 🚀 Quick Start - Deploy ke Railway

Panduan cepat deploy Laravel API ke Railway dalam 10 menit!

## ⚡ Langkah Singkat

### 1️⃣ Persiapan (5 menit)

```bash
# Masuk ke folder API
cd portal-berita/api

# Jalankan setup script (Windows)
powershell -ExecutionPolicy Bypass -File railway-setup.ps1

# ATAU untuk Linux/Mac
chmod +x railway-setup.sh
./railway-setup.sh
```

**Catat APP_KEY** yang muncul! Anda akan butuh ini nanti.

### 2️⃣ Push ke GitHub (2 menit)

```bash
# Init git jika belum
git init

# Tambahkan remote (ganti dengan repo Anda)
git remote add origin https://github.com/username/portal-berita-api.git

# Commit & push
git add .
git commit -m "Deploy to Railway"
git branch -M main
git push -u origin main
```

### 3️⃣ Deploy di Railway (3 menit)

1. **Login:** https://railway.app → Login with GitHub
2. **New Project** → Deploy from GitHub repo
3. **Pilih repo:** `portal-berita-api`
4. **Add Database:** New → Database → MySQL
5. **Set Variables:**
   - Klik service API → Variables tab
   - Add Reference dari MySQL (DB_HOST, DB_PORT, dll)
   - Add manual: `APP_KEY`, `APP_ENV=production`, `APP_DEBUG=false`
6. **Generate Domain:** Settings → Networking → Generate Domain
7. **Run Commands:**
   ```bash
   railway run php artisan migrate --force
   railway run php artisan db:seed --force
   railway run php artisan passport:install --force
   ```

### 4️⃣ Update Flutter (1 menit)

Edit `flutter_app/lib/services/api_service.dart`:

```dart
static const String prodUrl = 'https://portal-berita-api-production.up.railway.app/api';
static const String mode = 'prod';
```

### 5️⃣ Test!

```bash
# Test di browser
curl https://your-app.railway.app/api/berita

# Build APK
cd ../flutter_app
flutter build apk
```

---

## 🆘 Troubleshooting Cepat

| Masalah | Solusi |
|---------|--------|
| Build failed | Cek Logs di Railway, pastikan `composer.json` valid |
| Database error | Pastikan MySQL variables sudah di-reference |
| 500 Error | Pastikan APP_KEY sudah di-set |
| CORS error | Sudah oke di config/cors.php, restart deployment |
| No data | Run `railway run php artisan db:seed --force` |

---

## 📖 Butuh Detail Lengkap?

Baca: **DEPLOY_RAILWAY_TUTORIAL.md** - Tutorial step-by-step dengan screenshot

---

## ✅ Checklist

- [ ] Script setup sudah dijalankan
- [ ] APP_KEY sudah dicatat
- [ ] Code sudah di push ke GitHub
- [ ] Project Railway sudah dibuat
- [ ] MySQL database sudah ditambahkan
- [ ] Environment variables sudah di-set
- [ ] Domain sudah di-generate
- [ ] Migration & seeder sudah dijalankan
- [ ] API sudah bisa diakses
- [ ] Flutter app sudah update prodUrl

---

**Total waktu:** ~10-15 menit ⚡

**Selamat! API Anda sudah online! 🎉**


# 🚂 Tutorial Deploy Laravel ke Railway.app

Railway.app adalah platform hosting modern yang mudah digunakan dan menawarkan free tier yang cukup untuk project kecil-menengah.

## 📋 Prasyarat

- [x] Akun GitHub (untuk login Railway)
- [x] Project Laravel sudah berfungsi di local
- [x] Git sudah terinstall

---

## 🎯 Step 1: Persiapan Project Laravel

### 1.1 Buat file `Procfile` di root folder API

File ini memberitahu Railway cara menjalankan aplikasi Laravel.

```bash
cd portal-berita/api
```

Buat file bernama `Procfile` (tanpa ekstensi) dengan isi:

```
web: php artisan serve --host=0.0.0.0 --port=$PORT
```

### 1.2 Buat file `nixpacks.toml` (opsional tapi recommended)

File ini mengkonfigurasi build process di Railway.

Buat file `nixpacks.toml` di folder `api/` dengan isi:

```toml
[phases.setup]
nixPkgs = ['php82', 'php82Packages.composer']

[phases.install]
cmds = ['composer install --no-dev --optimize-autoloader']

[phases.build]
cmds = [
    'php artisan config:cache',
    'php artisan route:cache',
    'php artisan view:cache'
]

[start]
cmd = 'php artisan migrate --force && php artisan serve --host=0.0.0.0 --port=$PORT'
```

### 1.3 Update `.env` untuk production

Pastikan file `.env.example` sudah ada dan up to date:

```bash
cp .env .env.example
```

### 1.4 Commit ke Git Repository

```bash
# Di folder api/
git init
git add .
git commit -m "Initial commit for Railway deployment"
```

### 1.5 Push ke GitHub

```bash
# Buat repository baru di GitHub (https://github.com/new)
# Kemudian:
git remote add origin https://github.com/username/portal-berita-api.git
git branch -M main
git push -u origin main
```

---

## 🚀 Step 2: Deploy ke Railway

### 2.1 Login ke Railway

1. Buka https://railway.app
2. Klik **"Start a New Project"** atau **"Login"**
3. Login dengan **GitHub**
4. Authorize Railway untuk mengakses repositories Anda

### 2.2 Create New Project

1. Di Railway dashboard, klik **"New Project"**
2. Pilih **"Deploy from GitHub repo"**
3. Pilih repository `portal-berita-api` yang sudah Anda push
4. Railway akan otomatis detect Laravel dan mulai build

### 2.3 Tunggu Deployment

- Railway akan install dependencies dan deploy aplikasi
- Proses ini memakan waktu 2-5 menit
- Anda bisa lihat logs di tab **"Deployments"**

---

## 🗄️ Step 3: Setup Database MySQL

Laravel memerlukan database. Railway menyediakan MySQL gratis.

### 3.1 Add MySQL Database

1. Di project Railway, klik **"+ New"**
2. Pilih **"Database"**
3. Pilih **"Add MySQL"**
4. Railway akan create MySQL instance otomatis

### 3.2 Connect Database ke Laravel App

1. Klik service **"portal-berita-api"** (bukan database)
2. Buka tab **"Variables"**
3. Klik **"+ New Variable"** dan tambahkan reference ke MySQL:

Railway akan otomatis provide variable `DATABASE_URL`, tapi kita perlu variable terpisah:

Klik **"Add Reference"** dan pilih MySQL service, lalu add:
- `DB_HOST` → MySQL's `MYSQLHOST`
- `DB_PORT` → MySQL's `MYSQLPORT`
- `DB_DATABASE` → MySQL's `MYSQLDATABASE`
- `DB_USERNAME` → MySQL's `MYSQLUSER`
- `DB_PASSWORD` → MySQL's `MYSQLPASSWORD`

### 3.3 Tambah Environment Variables Lain

Masih di tab Variables, tambahkan:

```
APP_NAME=PortalBerita
APP_ENV=production
APP_KEY=  # akan di-generate otomatis nanti
APP_DEBUG=false
APP_URL=https://your-app.railway.app

DB_CONNECTION=mysql

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=error

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120
```

### 3.4 Generate APP_KEY

Di tab Variables, klik **"+ New Variable"**:
- Name: `APP_KEY`
- Value: (generate dengan command berikut di local, lalu copy hasilnya)

```bash
php artisan key:generate --show
```

Copy output (contoh: `base64:xxxxxxxxxxxxx`) dan paste ke Railway.

---

## 🔐 Step 4: Setup Laravel Passport

Passport memerlukan encryption keys.

### 4.1 Generate Keys di Local

```bash
cd portal-berita/api
php artisan passport:keys
```

Ini akan generate 2 files:
- `storage/oauth-private.key`
- `storage/oauth-public.key`

### 4.2 Upload Keys ke Railway

**Cara 1: Via Git (Recommended untuk development)**

```bash
# Edit .gitignore, comment baris storage/*.key
git add storage/oauth-*.key
git commit -m "Add passport keys"
git push
```

Railway akan auto-deploy dengan keys.

**Cara 2: Via Environment Variables (Recommended untuk production)**

```bash
# Di local terminal
cat storage/oauth-private.key | base64
cat storage/oauth-public.key | base64
```

Di Railway Variables, tambahkan:
```
PASSPORT_PRIVATE_KEY=[base64 dari oauth-private.key]
PASSPORT_PUBLIC_KEY=[base64 dari oauth-public.key]
```

Lalu update Laravel code untuk read dari env (optional, untuk production security).

---

## 🌐 Step 5: Get Public URL

### 5.1 Generate Domain

1. Di Railway project, klik service **"portal-berita-api"**
2. Buka tab **"Settings"**
3. Scroll ke **"Networking"**
4. Klik **"Generate Domain"**
5. Railway akan provide URL seperti: `https://portal-berita-api-production-xxxx.up.railway.app`

### 5.2 Test API

Buka browser dan test:
```
https://your-app.up.railway.app/api/berita
```

Jika berhasil, akan muncul response JSON dengan data berita.

---

## 🗃️ Step 6: Run Migrations & Seeder

### 6.1 Via Railway CLI

Install Railway CLI:
```bash
npm install -g @railway/cli
```

Login:
```bash
railway login
```

Link ke project:
```bash
cd portal-berita/api
railway link
```

Run commands:
```bash
railway run php artisan migrate --force
railway run php artisan db:seed --force
railway run php artisan passport:install --force
```

### 6.2 Via Railway Dashboard (Alternatif)

1. Di Railway, buka tab **"Deployments"**
2. Klik deployment terbaru
3. Klik **"View Logs"**
4. Cari tombol **"Run Command"** atau **"Console"**
5. Run commands:
   ```bash
   php artisan migrate --force
   php artisan db:seed --force
   php artisan passport:install --force
   ```

---

## ✅ Step 7: Update Flutter App

### 7.1 Update API URL

Edit `portal-berita/flutter_app/lib/services/api_service.dart`:

```dart
static const String prodUrl = 'https://your-app.up.railway.app/api';
static const String mode = 'prod';  // ubah dari 'dev' ke 'prod'
```

### 7.2 Test dari Flutter

```bash
cd portal-berita/flutter_app
flutter run -d chrome
```

Atau build APK:
```bash
flutter build apk --release
```

---

## 🐛 Troubleshooting

### Error: "Application key not set"

**Solusi:** Generate APP_KEY dan add ke Railway Variables
```bash
php artisan key:generate --show
```

### Error: "SQLSTATE[HY000] [2002] Connection refused"

**Solusi:** Cek database variables sudah benar dan MySQL service running.

### Error: "419 Session expired" atau CSRF token mismatch

**Solusi:** Update CORS settings dan add trusted hosts.

Di Railway Variables, tambahkan:
```
SESSION_DOMAIN=.railway.app
SANCTUM_STATEFUL_DOMAINS=your-app.railway.app
```

### Error: "Storage not writable"

**Solusi:** Railway filesystem bersifat ephemeral. Untuk production, gunakan AWS S3 atau Cloudinary untuk file uploads.

Di `config/filesystems.php`, set default disk ke `s3` atau storage online lainnya.

### API works but no data

**Solusi:** Run seeder lagi
```bash
railway run php artisan db:seed --force
```

---

## 💰 Railway Free Tier Limits

Railway free tier memberikan:
- ✅ **$5 credit per bulan** (cukup untuk ~500 jam runtime)
- ✅ **1GB RAM** per service
- ✅ **Unlimited deployments**
- ✅ **Custom domains**
- ❌ No credit card required untuk start

**Tips menghemat credit:**
- Deploy hanya saat development selesai
- Sleep services yang tidak digunakan (Railway auto-sleep after 1 hour inactive)
- Monitor usage di dashboard

---

## 🎉 Selesai!

Aplikasi Laravel Anda sudah online dan bisa diakses dari mana saja!

**URL API Anda:** `https://your-app.railway.app/api`

**Next Steps:**
1. Test semua endpoints (register, login, berita, komentar)
2. Build Flutter APK dengan prodUrl yang baru
3. Deploy Flutter Web ke Vercel/Netlify (optional)
4. Setup monitoring & analytics
5. Configure custom domain (optional)

---

## 📚 Resources

- Railway Docs: https://docs.railway.app
- Laravel Deployment: https://laravel.com/docs/deployment
- Railway Discord: https://discord.gg/railway

**Selamat! Backend Anda sudah production-ready! 🚀**


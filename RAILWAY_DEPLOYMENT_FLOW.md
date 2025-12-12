# 🔄 Railway Deployment Flow

Diagram alur deployment Laravel + Flutter ke Railway.

## 📊 Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    PRODUCTION ARCHITECTURE                   │
└─────────────────────────────────────────────────────────────┘

┌──────────────┐         HTTPS           ┌──────────────────┐
│              │    ───────────────►      │                  │
│  Flutter App │                          │  Railway Server  │
│   (Mobile)   │    ◄───────────────      │   Laravel API    │
│              │      JSON Response       │                  │
└──────────────┘                          └────────┬─────────┘
                                                   │
                                                   │ SQL
                                                   ▼
                                          ┌─────────────────┐
                                          │  MySQL Database │
                                          │    (Railway)    │
                                          └─────────────────┘
```

---

## 🔄 Deployment Process Flow

### Step 1: Local Development

```
┌─────────────────────────────────────────────────────────────┐
│                   LOCAL DEVELOPMENT                          │
└─────────────────────────────────────────────────────────────┘

Developer Machine
├── portal-berita/
│   ├── api/ (Laravel Backend)
│   │   ├── app/
│   │   ├── routes/
│   │   ├── database/
│   │   ├── .env
│   │   └── composer.json
│   │
│   └── flutter_app/ (Frontend)
│       ├── lib/
│       └── pubspec.yaml

        ↓
    
   📝 Development & Testing
   ├── php artisan serve (localhost:8000)
   ├── flutter run -d chrome
   └── Test API endpoints
```

### Step 2: Preparation

```
┌─────────────────────────────────────────────────────────────┐
│                     PREPARATION                              │
└─────────────────────────────────────────────────────────────┘

Run Setup Script
├── railway-setup.ps1 (Windows)
└── railway-setup.sh (Linux/Mac)

        ↓

Creates/Checks:
├── ✅ Procfile
├── ✅ nixpacks.toml  
├── ✅ .env.example
├── ✅ Passport keys (storage/oauth-*.key)
└── ✅ APP_KEY

        ↓

Git Repository
├── git init
├── git add .
├── git commit
└── git push origin main
```

### Step 3: Railway Deployment

```
┌─────────────────────────────────────────────────────────────┐
│                  RAILWAY DEPLOYMENT                          │
└─────────────────────────────────────────────────────────────┘

GitHub Repository
        ↓
        
Railway Platform
├── 1. Create Project
├── 2. Connect GitHub
├── 3. Auto-detect Laravel
└── 4. Start Build

        ↓

Build Process (nixpacks.toml)
├── Phase 1: Setup
│   └── Install PHP 8.2 + Composer
│
├── Phase 2: Install
│   └── composer install --no-dev
│
├── Phase 3: Build
│   ├── php artisan config:cache
│   ├── php artisan route:cache
│   └── php artisan view:cache
│
└── Phase 4: Start
    └── php artisan serve --host=0.0.0.0

        ↓

Deployment Success! 🎉
└── URL: https://portal-berita-api.up.railway.app
```

### Step 4: Database Setup

```
┌─────────────────────────────────────────────────────────────┐
│                    DATABASE SETUP                            │
└─────────────────────────────────────────────────────────────┘

Railway Dashboard
└── Add New Service
    └── Database → MySQL

        ↓

MySQL Instance Created
├── Host: mysql.railway.internal
├── Port: 3306
├── Database: railway
├── User: root
└── Password: [auto-generated]

        ↓

Link to Laravel App
└── Variables → Reference MySQL vars
    ├── DB_HOST → MYSQLHOST
    ├── DB_PORT → MYSQLPORT
    ├── DB_DATABASE → MYSQLDATABASE
    ├── DB_USERNAME → MYSQLUSER
    └── DB_PASSWORD → MYSQLPASSWORD

        ↓

Run Migrations
├── railway run php artisan migrate --force
├── railway run php artisan db:seed --force
└── railway run php artisan passport:install --force
```

### Step 5: Flutter Integration

```
┌─────────────────────────────────────────────────────────────┐
│                  FLUTTER INTEGRATION                         │
└─────────────────────────────────────────────────────────────┘

Update api_service.dart
├── static const String mode = 'prod';
└── static const String prodUrl = 'https://your-app.railway.app/api';

        ↓

Test Connection
└── flutter run -d chrome
    ├── Test Login
    ├── Test Get Berita
    ├── Test Create Berita
    └── Test Komentar

        ↓

Build Production APK
└── flutter build apk --release
    └── Output: build/app/outputs/flutter-apk/app-release.apk

        ↓

Distribute APK 📱
├── Install di HP Android
├── Share via Google Drive
├── Upload ke Play Store
└── Share link download
```

---

## 🔄 CI/CD Flow (Auto-Deploy)

```
┌─────────────────────────────────────────────────────────────┐
│              CONTINUOUS DEPLOYMENT FLOW                      │
└─────────────────────────────────────────────────────────────┘

Developer Makes Changes
        ↓
Git Commit & Push
        ↓
GitHub Repository Updated
        ↓
Railway Auto-Detects Changes
        ↓
Automatic Rebuild & Deploy
        ↓
Health Check
        ↓
Production Live! ✅

⏱️ Total Time: 2-5 minutes
```

---

## 🌐 API Request Flow (Production)

```
┌─────────────────────────────────────────────────────────────┐
│                    API REQUEST FLOW                          │
└─────────────────────────────────────────────────────────────┘

Mobile App (Flutter)
        ↓
    HTTP Request
    GET https://your-app.railway.app/api/berita
        ↓
Railway Load Balancer
        ↓
Laravel Application
├── Middleware (CORS, Auth)
├── Router (routes/api.php)
├── Controller (BeritaController)
├── Model (Berita)
└── Database Query (MySQL)
        ↓
    Database Result
        ↓
JSON Response
        ↓
Flutter App Displays Data
```

---

## 📊 Environment Variables Flow

```
┌─────────────────────────────────────────────────────────────┐
│                ENVIRONMENT VARIABLES                         │
└─────────────────────────────────────────────────────────────┘

Railway Dashboard Variables
├── APP_NAME
├── APP_KEY
├── APP_ENV=production
├── DB_HOST (from MySQL service)
├── DB_PORT (from MySQL service)
├── DB_DATABASE (from MySQL service)
├── DB_USERNAME (from MySQL service)
└── DB_PASSWORD (from MySQL service)

        ↓

Railway Injects at Runtime
        ↓

Laravel Reads via env()
└── config/database.php
    └── config/app.php
```

---

## 🔐 Security Flow

```
┌─────────────────────────────────────────────────────────────┐
│                     SECURITY FLOW                            │
└─────────────────────────────────────────────────────────────┘

User Login Request
        ↓
Laravel Passport OAuth2
├── Validate Credentials
├── Generate Access Token
└── Return Token to Client
        ↓
Client Stores Token
        ↓
Subsequent Requests
└── Header: Authorization: Bearer {token}
        ↓
Middleware Validates Token
├── Valid → Allow Request
└── Invalid → 401 Unauthorized
```

---

## 💰 Railway Free Tier Usage

```
┌─────────────────────────────────────────────────────────────┐
│                  RESOURCE MONITORING                         │
└─────────────────────────────────────────────────────────────┘

Monthly Credit: $5
        ↓
    Usage Tracking
├── Runtime Hours
├── Memory Usage (1GB max)
├── Network Egress
└── Build Minutes
        ↓
Auto-Sleep (after 1h inactive)
└── Saves Credits 💰
        ↓
Wake on Request
└── ~5s cold start
```

---

## 🎯 Success Checklist

```
Development Ready
├── [✓] Laravel API working locally
├── [✓] Flutter app working locally
└── [✓] Database seeded with data

Deployment Ready
├── [✓] GitHub repo created
├── [✓] Code pushed to main branch
├── [✓] Procfile created
└── [✓] nixpacks.toml created

Production Live
├── [✓] Railway project created
├── [✓] MySQL database added
├── [✓] Environment variables set
├── [✓] Migrations run
├── [✓] Domain generated
└── [✓] API accessible online

App Distribution
├── [✓] Flutter app updated with prod URL
├── [✓] APK built successfully
├── [✓] Tested on real device
└── [✓] Ready to distribute! 🎉
```

---

## 📈 Scaling Path (Future)

```
Current: Free Tier
        ↓
Growing Users
        ↓
Upgrade Options:
├── Railway Pro ($20/month)
│   ├── More resources
│   ├── No sleep timeout
│   └── Priority support
│
├── Add Redis Cache
│   └── Faster API responses
│
├── CDN for Assets
│   └── CloudFlare / AWS CloudFront
│
└── Multiple Regions
    └── Better global latency
```

---

**Ready to deploy? Follow QUICK_START_RAILWAY.md! 🚀**


# 🔐 ContainerCalc dengan Firebase Authentication

Aplikasi calculator container kamu sekarang sudah dilengkapi dengan sistem login/signup menggunakan Firebase!

## 📝 Panduan Setup

### Step 1: Setup Firebase Project
1. Buka [Firebase Console](https://console.firebase.google.com/)
2. Buat project baru
3. Enable **Authentication** → pilih **Email/Password**
4. Buat **Firestore Database** (mode development untuk testing)
5. Copy Firebase Config kamu

### Step 2: Masukkan Firebase Config
Edit file `index.html`, `app.html`, dan `admin.html`. Cari bagian ini dan ganti dengan config kamu:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "your-project.firebaseapp.com",
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "your-messaging-id",
    appId: "your-app-id"
};
```

### Step 3: Setup Firestore Rules (Penting!)
Di Firebase Console → Firestore → Rules, ganti dengan:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users collection - hanya user sendiri + admin yg bisa baca
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId || 
                           get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    
    // Activity logs - hanya admin yg bisa baca
    match /activity_logs/{document=**} {
      allow read: if get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
      allow create: if request.auth != null;
    }
  }
}
```

## 🎯 Fitur yang Tersedia

### 1. **Login & Signup (index.html)**
- Form login dengan email & password
- Form signup dengan validasi
- User baru harus disetujui admin sebelum bisa akses app

### 2. **Dashboard (app.html)**
- Profile user dengan nama, email, role
- Menu untuk akses calculator
- Link ke admin panel (hanya admin)
- Logout

### 3. **Admin Panel (admin.html)** ⚙️
- **📊 Dashboard**: Statistik user, admin count, pending approvals
- **👥 Kelola User**: List semua user, bisa promosi ke admin
- **✅ Persetujuan**: Approve atau tolak user baru
- **📝 Activity Log**: Lihat semua aksi admin

## 📂 File yang Dibuat

- `index.html` - Login/Signup page
- `app.html` - Dashboard untuk user
- `admin.html` - Admin control panel
- `CONTAINER_ADI_v4.html` - Calculator original (di-rename jadi `calc.html`)

## 🔄 User Flow

```
1. Orang baru → index.html (Login/Signup)
   ↓
2. Klik "Daftar" → Isi form → Akun pending
   ↓
3. Admin login → admin.html → Approve/Tolak user
   ↓
4. Jika approved → User bisa login & akses calculator
   ↓
5. User → app.html (Dashboard) → Klik "Akses Calculator"
```

## 🚀 Cara Pakai

### Pertama Kali Setup
1. Buat akun kamu di app
2. Langsung go to Firebase Console
3. Edit document di Firestore:
   - Collection: `users`
   - Document: `[UID kamu]`
   - Update `role` menjadi `"admin"`
   - Update `approved` menjadi `true`

### Sebagai Admin
- Login dengan akun yang sudah approved & role admin
- Buka admin.html (dari profile menu)
- Manage user, approve pendaftar, lihat activity

### Sebagai User Biasa
- Signup & tunggu approval admin
- Setelah approved, bisa login & akses calculator
- Role default: `"user"`, approved: `false`

## 🔒 Keamanan

- Password dienkripsi Firebase
- User hanya bisa akses data mereka sendiri
- Admin bisa akses semua data user
- Activity log mencatat semua aksi admin
- Validasi di Firestore Rules

## ⚡ Tips

- Sidebar bisa di-customize sesuai kebutuhan
- Untuk testing, bisa langsung approve user di Firestore Console
- Admin panel bisa di-akses dari `/admin.html`
- Semua user data tersimpan di Firebase Firestore

## 📞 Kontrol User

Sebagai admin kamu bisa:
- ✅ Approve user baru
- ❌ Reject/hapus user
- 👑 Promote user jadi admin
- 📝 Lihat activity log semua aksi

Semoga membantu! 🎉
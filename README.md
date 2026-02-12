# Aplikasi RT - Web Mobile Friendly

Sistem Manajemen RT untuk mengelola data warga, dokumen, dan pembayaran IPL.

## 🎯 Fitur Utama

### Untuk Warga
- ✅ Login dengan email & password
- ✅ Lihat data rumah & profil
- ✅ Upload dokumen (KK & KTP)
- ✅ Lihat tagihan IPL per bulan
- ✅ Upload bukti transfer IPL
- ✅ Lihat status verifikasi dokumen & pembayaran

### Untuk Admin/Ketua RT
- ✅ Kelola data rumah & warga
- ✅ Lihat dokumen masuk
- ✅ Approve/Reject dokumen
- ✅ Lihat pembayaran IPL masuk
- ✅ Verifikasi pembayaran
- ✅ Rekap pembayaran per bulan

## 🚀 Cara Install & Jalankan

### Prerequisites
- Node.js 18+ (https://nodejs.org/)
- npm atau yarn

### Langkah Install

1. **Extract file dan masuk ke folder**
```bash
cd rt-app
```

2. **Install dependencies**
```bash
npm install
```

3. **Jalankan development server**
```bash
npm run dev
```

4. **Buka browser**
```
http://localhost:3000
```

## 👤 Akun Demo

### Admin
- Email: `admin@rt.com`
- Password: `admin123`

### Warga
- Email: `warga1@gmail.com`
- Password: `warga123`

## 📱 Teknologi

- **Framework**: Next.js 14 (React)
- **Styling**: TailwindCSS
- **Auth**: JWT (JSON Web Token)
- **Database**: In-memory (MVP) - siap upgrade ke PostgreSQL/MySQL
- **File Upload**: Local storage (MVP) - siap upgrade ke S3/R2

## 📂 Struktur Folder

```
rt-app/
├── app/
│   ├── api/           # Backend API routes
│   ├── dashboard/     # Halaman warga
│   ├── admin/         # Halaman admin
│   ├── login/         # Halaman login
│   └── globals.css    # Global styles
├── lib/
│   ├── db.ts          # In-memory database
│   └── auth.ts        # JWT utilities
├── types/
│   └── index.ts       # TypeScript types
└── public/
    └── uploads/       # Uploaded files
```

## 🔄 Alur Kerja

### Upload Dokumen
1. Warga login → Dashboard → Dokumen
2. Upload KK/KTP
3. Status: Pending
4. Admin review → Approve/Reject
5. Warga lihat status

### Pembayaran IPL
1. Warga cek tagihan IPL
2. Transfer ke rekening bendahara
3. Upload bukti transfer
4. Status: Menunggu Verifikasi
5. Admin/Bendahara verifikasi
6. Status: Lunas/Ditolak

## 🎨 Fitur UI

- ✅ Responsive (Mobile & Desktop)
- ✅ Bottom Navigation (Mobile)
- ✅ Sidebar Navigation (Desktop)
- ✅ Card-based Layout
- ✅ Status Badges
- ✅ Form Validation

## 🔒 Keamanan

- ✅ Password hashing (bcrypt)
- ✅ JWT authentication
- ✅ HTTP-only cookies
- ✅ Role-based access control
- ✅ Server-side validation

## 📦 File yang Sudah Dibuat (MVP v1)

### Core Files
- ✅ Authentication (Login/Logout)
- ✅ Database setup
- ✅ TypeScript types
- ✅ Navigation components

### Warga Pages
- ✅ Dashboard
- ⏳ Upload Dokumen (perlu dilengkapi)
- ⏳ Tagihan IPL (perlu dilengkapi)

### Admin Pages
- ⏳ Dashboard Admin (perlu dilengkapi)
- ⏳ Data Rumah (perlu dilengkapi)
- ⏳ Review Dokumen (perlu dilengkapi)
- ⏳ Verifikasi IPL (perlu dilengkapi)

## 🚧 Todo - Versi Selanjutnya

### API Routes yang Perlu Dibuat
- [ ] `/api/dokumen` - Upload & manage documents
- [ ] `/api/ipl` - Manage IPL payments
- [ ] `/api/rekening` - Manage bank accounts

### Halaman yang Perlu Dibuat
- [ ] `/dashboard/dokumen` - Upload dokumen page
- [ ] `/dashboard/ipl` - IPL payment page
- [ ] `/admin/*` - All admin pages
- [ ] `/admin/rumah` - Manage houses
- [ ] `/admin/dokumen` - Review documents
- [ ] `/admin/ipl` - Verify payments

### Fitur Tambahan
- [ ] Upload multiple files
- [ ] Image preview
- [ ] Excel export
- [ ] Search & filter
- [ ] Pagination
- [ ] Notifications

## 🔧 Development Tips

### Menambah Data Dummy
Edit file `lib/db.ts` untuk menambah data:
```typescript
// Tambah rumah baru
this.rumah.push({
  id: 'rumah-2',
  nomorRumah: 'A-02',
  namaKepalaKeluarga: 'Andi',
  // ...
});
```

### Menambah User Baru
```typescript
const password = await bcrypt.hash('password123', 10);
this.users.push({
  id: 'user-2',
  email: 'user@example.com',
  password: password,
  role: 'warga',
  rumahId: 'rumah-2',
  createdAt: new Date(),
});
```

## 📝 Environment Variables (Opsional)

Buat file `.env.local`:
```
JWT_SECRET=your-secret-key-here
NODE_ENV=development
```

## 🆙 Upgrade ke Production

### Database
Ganti in-memory database dengan:
- PostgreSQL (Vercel Postgres, Supabase)
- MySQL (PlanetScale, Railway)

### File Storage
Ganti local storage dengan:
- AWS S3
- Cloudflare R2
- Vercel Blob

### Deployment
Deploy ke:
- Vercel (recommended)
- Netlify
- Railway
- VPS

## 📞 Support

Untuk pertanyaan atau bug report, silakan buat issue atau hubungi developer.

## 📄 License

MIT License - Bebas digunakan dan dimodifikasi.

---

**Catatan**: Ini adalah MVP (Minimum Viable Product) versi 1. File masih perlu dilengkapi untuk fitur upload dokumen dan IPL. Struktur sudah siap, tinggal implementasi detail.

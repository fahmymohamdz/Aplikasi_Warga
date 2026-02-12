# 📋 Dokumentasi Teknis - Aplikasi RT

## ✅ Status Development

### Yang Sudah Selesai (100%)

#### 1. **Setup Project & Konfigurasi**
- ✅ Next.js 14 dengan TypeScript
- ✅ TailwindCSS untuk styling
- ✅ Konfigurasi lengkap (tsconfig, tailwind, postcss)
- ✅ Struktur folder yang terorganisir

#### 2. **Authentication System**
- ✅ JWT-based authentication
- ✅ Login API (`/api/auth/login`)
- ✅ Logout API (`/api/auth/logout`)
- ✅ Password hashing dengan bcrypt
- ✅ HTTP-only cookies
- ✅ Auth middleware (`requireAuth`)
- ✅ Role-based access control

#### 3. **Database Layer (In-Memory MVP)**
- ✅ In-memory database structure (`lib/db.ts`)
- ✅ TypeScript types untuk semua entity
- ✅ Sample data untuk testing
- ✅ Helper functions

#### 4. **UI Components**
- ✅ Navigation component (responsive: mobile bottom nav + desktop sidebar)
- ✅ Reusable TailwindCSS classes
- ✅ Badge components untuk status
- ✅ Card layouts
- ✅ Form inputs

#### 5. **Pages - Warga (User)**
- ✅ Login page (`/login`)
- ✅ Dashboard warga (`/dashboard`)
- ✅ Upload Dokumen page (`/dashboard/dokumen`)
- ✅ Pembayaran IPL page (`/dashboard/ipl`)
- ✅ Layout dengan navigation

#### 6. **Pages - Admin**
- ✅ Dashboard admin (`/admin`)
- ✅ Layout dengan navigation
- ✅ Stats & analytics cards
- ✅ Pending actions overview

#### 7. **API Routes**
- ✅ `/api/auth/login` - Login endpoint
- ✅ `/api/auth/logout` - Logout endpoint
- ✅ `/api/rumah` - CRUD rumah (GET, POST, PUT, DELETE)

---

## ⏳ Yang Masih Perlu Dibuat

### 1. **API Routes (Priority: HIGH)**

#### `/api/dokumen`
```typescript
// Fitur yang diperlukan:
- GET: Ambil dokumen (all untuk admin, filter by rumahId untuk warga)
- POST: Upload dokumen baru (warga)
- PUT: Approve/Reject dokumen (admin)
```

#### `/api/dokumen/upload`
```typescript
// File upload handler
- Terima file dari FormData
- Validate file type & size
- Save ke /public/uploads
- Simpan metadata ke database
```

#### `/api/ipl`
```typescript
// Fitur yang diperlukan:
- GET: Ambil tagihan IPL
- POST: Create tagihan baru (admin)
- PUT: Update status pembayaran (admin verifikasi)
```

#### `/api/ipl/upload-bukti`
```typescript
// Upload bukti transfer
- Terima file bukti transfer
- Save ke /public/uploads
- Update status tagihan ke "Menunggu Verifikasi"
```

#### `/api/rekening`
```typescript
// Manage rekening bendahara
- GET: Ambil info rekening aktif
- POST: Tambah rekening baru (admin)
- PUT: Update rekening (admin)
```

### 2. **Admin Pages (Priority: HIGH)**

#### `/admin/rumah/page.tsx`
```
Fitur:
- Tabel list semua rumah & warga
- Form tambah rumah baru
- Edit rumah
- Delete rumah
- Search & filter
```

#### `/admin/dokumen/page.tsx`
```
Fitur:
- List dokumen pending review
- Preview dokumen (image/PDF)
- Approve button → set status Approved
- Reject button → input catatan → set status Rejected
- Filter by status
- Filter by rumah
```

#### `/admin/ipl/page.tsx`
```
Fitur:
- List pembayaran pending verifikasi
- Preview bukti transfer
- Approve → set status Lunas
- Reject → input catatan → set status Ditolak
- Rekap per bulan (table)
- Generate tagihan baru untuk bulan berikutnya
- Export Excel (opsional)
```

### 3. **File Upload Functionality**

#### Setup Multer (Sudah ada di package.json)
```typescript
// lib/upload.ts
import multer from 'multer';

const storage = multer.diskStorage({
  destination: './public/uploads',
  filename: (req, file, cb) => {
    cb(null, `${Date.now()}-${file.originalname}`);
  }
});

const upload = multer({
  storage,
  limits: { fileSize: 5 * 1024 * 1024 }, // 5MB
  fileFilter: (req, file, cb) => {
    // Allow images and PDFs only
    if (file.mimetype.startsWith('image/') || file.mimetype === 'application/pdf') {
      cb(null, true);
    } else {
      cb(new Error('File type not allowed'));
    }
  }
});
```

### 4. **Improvement & Features**

#### Validation
- Input validation di frontend
- Server-side validation di API
- Error handling yang lebih baik

#### UX Improvements
- Loading states
- Error messages yang jelas
- Success notifications
- Confirmation dialogs

#### Data Persistence
- Migrate dari in-memory ke real database
- PostgreSQL atau MySQL
- Prisma ORM (recommended)

---

## 🚀 Cara Melanjutkan Development

### Prioritas 1: File Upload
1. Buat `/api/dokumen/upload` route
2. Implement multer file handling
3. Test upload dari frontend

### Prioritas 2: Dokumen Management
1. Buat `/api/dokumen` CRUD
2. Buat `/admin/dokumen` page
3. Implement approve/reject flow

### Prioritas 3: IPL Management
1. Buat `/api/ipl` CRUD
2. Buat `/api/ipl/upload-bukti`
3. Buat `/admin/ipl` page
4. Implement verifikasi flow

### Prioritas 4: Data Rumah
1. Buat `/admin/rumah` page
2. Form CRUD rumah
3. Connect dengan existing API

---

## 📁 File Structure

```
rt-app/
├── app/
│   ├── api/
│   │   ├── auth/
│   │   │   ├── login/route.ts          ✅ Done
│   │   │   └── logout/route.ts         ✅ Done
│   │   ├── rumah/route.ts              ✅ Done
│   │   ├── dokumen/                    ⏳ TODO
│   │   │   ├── route.ts
│   │   │   └── upload/route.ts
│   │   ├── ipl/                        ⏳ TODO
│   │   │   ├── route.ts
│   │   │   └── upload-bukti/route.ts
│   │   └── rekening/route.ts           ⏳ TODO
│   │
│   ├── dashboard/                      ✅ Done (Warga)
│   │   ├── page.tsx
│   │   ├── layout.tsx
│   │   ├── dokumen/page.tsx
│   │   ├── ipl/page.tsx
│   │   └── components/Navigation.tsx
│   │
│   ├── admin/                          
│   │   ├── page.tsx                    ✅ Done
│   │   ├── layout.tsx                  ✅ Done
│   │   ├── rumah/page.tsx              ⏳ TODO
│   │   ├── dokumen/page.tsx            ⏳ TODO
│   │   └── ipl/page.tsx                ⏳ TODO
│   │
│   ├── login/page.tsx                  ✅ Done
│   ├── page.tsx                        ✅ Done
│   ├── layout.tsx                      ✅ Done
│   └── globals.css                     ✅ Done
│
├── lib/
│   ├── db.ts                           ✅ Done (In-memory)
│   ├── auth.ts                         ✅ Done
│   └── upload.ts                       ⏳ TODO
│
├── types/
│   └── index.ts                        ✅ Done
│
└── public/
    └── uploads/                        📁 Created (empty)
```

---

## 🛠️ Next.js API Routes Pattern

### Example: GET with Auth
```typescript
import { NextRequest, NextResponse } from 'next/server';
import { requireAuth } from '@/lib/auth';
import { db } from '@/lib/db';

export async function GET(request: NextRequest) {
  try {
    const user = await requireAuth(['admin']);
    
    // Your logic here
    const data = db.dokumen.filter(d => d.status === 'Pending');
    
    return NextResponse.json({ data });
  } catch (error: any) {
    return NextResponse.json({ error: error.message }, { status: 403 });
  }
}
```

### Example: POST with File Upload
```typescript
import { NextRequest, NextResponse } from 'next/server';
import { writeFile } from 'fs/promises';
import { join } from 'path';

export async function POST(request: NextRequest) {
  try {
    const formData = await request.formData();
    const file = formData.get('file') as File;
    
    if (!file) {
      return NextResponse.json({ error: 'No file uploaded' }, { status: 400 });
    }
    
    // Convert file to buffer
    const bytes = await file.arrayBuffer();
    const buffer = Buffer.from(bytes);
    
    // Save file
    const filename = `${Date.now()}-${file.name}`;
    const filepath = join(process.cwd(), 'public/uploads', filename);
    await writeFile(filepath, buffer);
    
    return NextResponse.json({ 
      message: 'File uploaded',
      filename,
      path: `/uploads/${filename}`
    });
  } catch (error: any) {
    return NextResponse.json({ error: error.message }, { status: 500 });
  }
}
```

---

## 🎯 Testing Checklist

### Setelah Development Selesai
- [ ] Login sebagai admin
- [ ] Login sebagai warga
- [ ] Upload dokumen (KK & KTP)
- [ ] Admin review & approve dokumen
- [ ] Upload bukti transfer IPL
- [ ] Admin verifikasi pembayaran IPL
- [ ] Tambah data rumah baru
- [ ] Edit data rumah
- [ ] Generate tagihan IPL baru
- [ ] Export rekap pembayaran

---

## 💡 Tips Development

1. **Start Development Server**
   ```bash
   npm run dev
   ```

2. **Check Console for Errors**
   - Browser console (F12)
   - Terminal console

3. **Test with Demo Accounts**
   - Admin: admin@rt.com / admin123
   - Warga: warga1@gmail.com / warga123

4. **Use React DevTools**
   - Install Chrome extension
   - Debug component state

5. **API Testing**
   - Use Postman or Thunder Client
   - Test all endpoints

---

## 📞 Support

Jika ada pertanyaan atau butuh bantuan:
1. Check README.md
2. Review code comments
3. Check Next.js documentation
4. Ask developer

**Happy Coding! 🚀**

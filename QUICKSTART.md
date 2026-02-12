# ⚡ Quick Start Guide - Aplikasi RT

## 🚀 Langkah Cepat (5 Menit)

### 1. Install Dependencies
```bash
cd rt-app
npm install
```

### 2. Jalankan Development Server
```bash
npm run dev
```

### 3. Buka Browser
```
http://localhost:3000
```

### 4. Login
**Admin:**
- Email: `admin@rt.com`
- Password: `admin123`

**Warga:**
- Email: `warga1@gmail.com`
- Password: `warga123`

---

## 📱 Fitur yang Sudah Bisa Dicoba

### Sebagai Warga
1. Login dengan akun warga
2. Lihat dashboard dengan info rumah
3. Halaman Upload Dokumen (UI ready)
4. Halaman Pembayaran IPL (UI ready)

### Sebagai Admin
1. Login dengan akun admin
2. Dashboard dengan statistik
3. Halaman Data Rumah (CRUD ready)
4. View pending dokumen & pembayaran

---

## 🎯 Yang Sudah Jalan

✅ **Authentication**
- Login/Logout
- JWT-based auth
- Role-based access

✅ **UI/UX**
- Responsive design (mobile & desktop)
- Navigation (bottom nav mobile, sidebar desktop)
- Beautiful cards & forms

✅ **Pages**
- Landing page
- Login page
- Dashboard warga (full)
- Dashboard admin (full)
- Data Rumah admin (CRUD UI)
- Upload Dokumen warga (UI)
- Pembayaran IPL warga (UI)

---

## ⚠️ Yang Perlu Dilengkapi

### API Endpoints
File upload belum connected ke backend. Saat ini masih simulated/mock.

**Todo:**
- `POST /api/dokumen/upload` - Upload file dokumen
- `PUT /api/dokumen` - Approve/reject dokumen
- `POST /api/ipl/upload-bukti` - Upload bukti transfer
- `PUT /api/ipl` - Verifikasi pembayaran

### Admin Pages
UI sudah ada, perlu connect ke API:
- `/admin/dokumen` - Review dokumen (perlu dibuat)
- `/admin/ipl` - Verifikasi pembayaran (perlu dibuat)

---

## 🔧 Struktur Code

### Important Files

**Authentication:**
- `lib/auth.ts` - JWT utilities
- `app/api/auth/login/route.ts` - Login endpoint

**Database:**
- `lib/db.ts` - In-memory database
- `types/index.ts` - TypeScript types

**Pages:**
- `app/dashboard/*` - Warga pages
- `app/admin/*` - Admin pages

**Components:**
- `app/dashboard/components/Navigation.tsx` - Navigation component

---

## 💡 Tips Development

### Hot Reload
Next.js automatically reloads when you save files. No need to restart server.

### Check Console
- Browser console (F12) for frontend errors
- Terminal for backend errors

### Test Responsive
- Browser DevTools (F12) → Device toolbar
- Test mobile view

### Modify Data
Edit `lib/db.ts` to add more dummy data:
```typescript
// Add more rumah
this.rumah.push({
  id: 'rumah-2',
  nomorRumah: 'A-02',
  namaKepalaKeluarga: 'Your Name',
  // ...
});
```

---

## 📝 Common Tasks

### Tambah User Baru
Edit `lib/db.ts`:
```typescript
const newPassword = await bcrypt.hash('password123', 10);
this.users.push({
  id: 'user-2',
  email: 'newuser@example.com',
  password: newPassword,
  role: 'warga',
  rumahId: 'rumah-2',
  createdAt: new Date(),
});
```

### Ubah Styling
- Global styles: `app/globals.css`
- Tailwind classes: Direct in components
- Custom components: Create in `components/` folder

### Add New Page
```bash
# Create new page
touch app/new-page/page.tsx

# Access at http://localhost:3000/new-page
```

---

## 🐛 Troubleshooting

### Port 3000 Already in Use
```bash
# Kill process on port 3000
npx kill-port 3000

# Or use different port
npm run dev -- -p 3001
```

### npm install Error
```bash
# Clear cache
rm -rf node_modules package-lock.json
npm cache clean --force
npm install
```

### TypeScript Errors
```bash
# Check for errors
npm run build

# If error, fix based on message
```

---

## 📚 Next Steps

1. **Read README.md** - Overview & features
2. **Read TECHNICAL.md** - Technical details & what to build next
3. **Read DEPLOYMENT.md** - How to deploy to production

---

## 🎓 Learning Resources

### Next.js
- https://nextjs.org/docs
- https://nextjs.org/learn

### TailwindCSS
- https://tailwindcss.com/docs

### TypeScript
- https://www.typescriptlang.org/docs

### React
- https://react.dev/learn

---

## 🤝 Need Help?

1. Check documentation files
2. Read error messages carefully
3. Google the error
4. Stack Overflow
5. Ask developer

---

## ✨ Feature Ideas

Future enhancements:
- [ ] WhatsApp notifications
- [ ] QR Code payment (QRIS)
- [ ] Broadcast pengumuman
- [ ] Export to Excel/PDF
- [ ] Multi RT support
- [ ] Mobile app (React Native)
- [ ] Dashboard charts
- [ ] Calendar events

---

**Happy coding! 🎉**

Jika ada pertanyaan atau butuh bantuan, jangan ragu untuk bertanya!

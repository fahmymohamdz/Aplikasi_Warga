# 🚀 Deployment Guide - Aplikasi RT

## Pilihan Deployment

### 1. Vercel (Recommended - Paling Mudah)

#### Kenapa Vercel?
- ✅ Gratis untuk personal projects
- ✅ Auto deploy dari Git
- ✅ Built-in CDN
- ✅ Zero configuration
- ✅ Free SSL
- ✅ Serverless functions untuk API

#### Langkah Deploy ke Vercel

1. **Push ke GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/username/rt-app.git
   git push -u origin main
   ```

2. **Deploy via Vercel**
   - Buka https://vercel.com
   - Sign in dengan GitHub
   - Click "New Project"
   - Import repository `rt-app`
   - Click "Deploy"
   - Done! 🎉

3. **Environment Variables (Optional)**
   - Di Vercel dashboard → Settings → Environment Variables
   - Add: `JWT_SECRET=your-secret-key-here`

#### Custom Domain (Optional)
- Di Vercel dashboard → Settings → Domains
- Add domain Anda (gratis untuk .vercel.app)

---

### 2. Netlify

#### Langkah Deploy ke Netlify

1. **Build Settings**
   - Build command: `npm run build`
   - Publish directory: `.next`

2. **Deploy**
   - Drag & drop folder ke Netlify
   - Atau connect dengan GitHub

---

### 3. VPS (Digital Ocean, AWS, dll)

#### Prerequisites
- Node.js 18+
- PM2 (process manager)
- Nginx (reverse proxy)

#### Langkah Deploy

1. **Install Node.js**
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

2. **Upload Code**
   ```bash
   # Via Git
   git clone https://github.com/username/rt-app.git
   cd rt-app
   npm install
   ```

3. **Build**
   ```bash
   npm run build
   ```

4. **Install PM2**
   ```bash
   sudo npm install -g pm2
   ```

5. **Start App**
   ```bash
   pm2 start npm --name "rt-app" -- start
   pm2 save
   pm2 startup
   ```

6. **Setup Nginx**
   ```nginx
   server {
       listen 80;
       server_name yourdomain.com;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

7. **SSL with Let's Encrypt**
   ```bash
   sudo apt install certbot python3-certbot-nginx
   sudo certbot --nginx -d yourdomain.com
   ```

---

## 📦 Production Checklist

### Before Deploy

- [ ] Set strong `JWT_SECRET` in environment variables
- [ ] Change default admin password
- [ ] Test all features locally
- [ ] Remove console.logs from production code
- [ ] Enable error logging
- [ ] Setup monitoring

### Environment Variables

```bash
# .env.production
JWT_SECRET=your-very-strong-secret-key-min-32-chars
NODE_ENV=production
```

### Security

1. **Change Default Passwords**
   ```typescript
   // In lib/db.ts - change:
   const adminPassword = await bcrypt.hash('YOUR_STRONG_PASSWORD', 10);
   ```

2. **Set Strong JWT Secret**
   ```bash
   # Generate random secret
   openssl rand -base64 32
   ```

3. **Enable HTTPS**
   - Always use HTTPS in production
   - Vercel/Netlify provides free SSL
   - For VPS, use Let's Encrypt

---

## 🗄️ Database Migration

### Current: In-Memory Database
- Data hilang saat server restart
- Hanya untuk testing/MVP

### Production: PostgreSQL/MySQL

#### Option 1: Vercel Postgres (Recommended)
```bash
# Install Vercel Postgres
npm install @vercel/postgres

# In Vercel dashboard
# Add Postgres database
# Copy connection string
```

#### Option 2: Supabase (Free tier available)
```bash
# Create project at supabase.com
# Copy database URL
# Install client
npm install @supabase/supabase-js
```

#### Option 3: PlanetScale (MySQL)
```bash
# Create database at planetscale.com
# Install Prisma
npm install prisma @prisma/client
npx prisma init
```

### Database Schema (PostgreSQL)

```sql
-- Users table
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role VARCHAR(20) NOT NULL,
    rumah_id UUID,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Rumah table
CREATE TABLE rumah (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nomor_rumah VARCHAR(10) NOT NULL,
    nama_kepala_keluarga VARCHAR(255) NOT NULL,
    nama_pemilik_rumah VARCHAR(255) NOT NULL,
    status_hunian VARCHAR(20) NOT NULL,
    no_hp VARCHAR(20) NOT NULL,
    alamat_lengkap TEXT,
    catatan TEXT,
    user_id UUID,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Dokumen table
CREATE TABLE dokumen (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    rumah_id UUID NOT NULL,
    jenis_file VARCHAR(10) NOT NULL,
    nama_file VARCHAR(255) NOT NULL,
    file_path VARCHAR(500) NOT NULL,
    status VARCHAR(20) DEFAULT 'Pending',
    catatan TEXT,
    uploaded_at TIMESTAMP DEFAULT NOW(),
    reviewed_at TIMESTAMP,
    reviewed_by UUID
);

-- Tagihan IPL table
CREATE TABLE tagihan_ipl (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    rumah_id UUID NOT NULL,
    bulan VARCHAR(7) NOT NULL,
    nominal INTEGER NOT NULL,
    status VARCHAR(30) DEFAULT 'Belum Bayar',
    bukti_transfer_path VARCHAR(500),
    uploaded_at TIMESTAMP,
    verified_at TIMESTAMP,
    verified_by UUID,
    catatan TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Rekening table
CREATE TABLE rekening (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nama_bank VARCHAR(100) NOT NULL,
    nomor_rekening VARCHAR(50) NOT NULL,
    nama_pemilik VARCHAR(255) NOT NULL,
    is_active BOOLEAN DEFAULT true
);
```

---

## 📊 File Storage Migration

### Current: Local Filesystem
- Files in `/public/uploads`
- Lost on Vercel (serverless)

### Production: Cloud Storage

#### Option 1: Vercel Blob
```bash
npm install @vercel/blob
```

```typescript
import { put } from '@vercel/blob';

const blob = await put(filename, file, {
  access: 'public',
});

return blob.url; // https://...vercel-storage.com/...
```

#### Option 2: AWS S3
```bash
npm install @aws-sdk/client-s3
```

#### Option 3: Cloudflare R2 (Cheaper than S3)
```bash
npm install @cloudflare/workers-types
```

---

## 🔍 Monitoring & Logging

### Vercel Analytics (Free)
```bash
npm install @vercel/analytics
```

```typescript
// app/layout.tsx
import { Analytics } from '@vercel/analytics/react';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  );
}
```

### Error Tracking: Sentry
```bash
npm install @sentry/nextjs
npx @sentry/wizard -i nextjs
```

---

## 🧪 Testing Before Production

```bash
# Run production build locally
npm run build
npm run start

# Test all features:
# 1. Login as admin
# 2. Login as warga
# 3. Upload files
# 4. Approve/reject
# 5. Payment flow
```

---

## 📱 Mobile Testing

Test di berbagai device:
- iOS Safari
- Android Chrome
- Tablet
- Desktop

Responsive breakpoints:
- Mobile: < 768px
- Tablet: 768px - 1024px
- Desktop: > 1024px

---

## 🎯 Post-Deployment

1. **Monitor Performance**
   - Check loading times
   - Monitor API response times
   - Watch for errors

2. **User Feedback**
   - Collect feedback dari warga
   - Fix bugs
   - Add requested features

3. **Regular Updates**
   ```bash
   git pull
   npm install
   npm run build
   pm2 restart rt-app
   ```

4. **Backup Database**
   - Regular backups
   - Test restore process

---

## 💰 Cost Estimation

### Free Tier (MVP)
- **Vercel**: Free (Hobby plan)
- **Supabase**: Free up to 500MB database
- **Vercel Blob**: Free up to 100GB bandwidth
- **Total**: Rp 0/bulan ✅

### Paid (Production)
- **Vercel Pro**: $20/month
- **Supabase Pro**: $25/month  
- **Cloudflare R2**: ~$1/month (10GB storage)
- **Total**: ~$46/month (~Rp 700.000)

### VPS Option
- **Digital Ocean Droplet**: $6/month
- **Domain**: ~$10/year
- **Total**: ~$7/month (~Rp 100.000)

---

## 📞 Support

Need help deploying?
- Check Vercel documentation
- Join Next.js Discord
- Stack Overflow
- Contact developer

**Good luck with deployment! 🚀**

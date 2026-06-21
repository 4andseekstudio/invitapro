# invitapro
Website Undangan Online
# InvitaPro

Platform SaaS undangan digital — modern, cepat, elegan, dan siap bisnis.

## Fitur Utama

- **Landing Page** — Halaman pemasaran dan showcase template
- **SaaS Dashboard** — Manajemen tenant, tamu, RSVP, dan check-in
- **Page Builder** — Editor drag & drop untuk kustomisasi undangan
- **Invitation Preview** — Pratinjau undangan web dengan personalisasi tamu
- **Blog Engine** — Konten SEO untuk marketing
- **Admin Dashboard** — Panel pemilik platform (lisensi, pendapatan, template)

## Prasyarat

- Node.js 18+

## Menjalankan Secara Lokal

**Prasyarat:** Node.js 18+

1. Install dependensi:

   ```bash
   npm install
   ```

2. (Opsional) Salin environment:

   ```bash
   cp .env.example .env
   ```

3. Jalankan server (API + SSR + frontend dalam satu proses):

   ```bash
   npm run dev
   ```

   - **Satu port:** `http://localhost:3000` (API di `/api/*`)
   - Halaman publik di-render **SSR** (HTML crawlable untuk Google)
   - Dashboard/admin tetap SPA setelah hydration

   Production: `npm run build && npm run start`

   API-only (tanpa SSR): `npm run dev:api` → port 3001

### Akun Demo

| Email | Password | Role |
|-------|----------|------|
| `klien@invitapro.id` | `klien123` | Portal klien (tenant `andisiti`) |
| `admin@invitapro.id` | `admin123` | Super admin (semua tenant) |

## Script

| Perintah        | Deskripsi                    |
|-----------------|------------------------------|
| `npm run dev`   | Dev server SSR + API (port 3000) |
| `npm run start` | Production SSR server |
| `npm run build` | Build client ke `dist/client/` |
| `npm run lint`  | Pengecekan TypeScript        |

### Subdomain Undangan (Multi-Tenant)

Akses undangan via subdomain langsung:

- `http://andisiti.localhost:3000?to=NamaTamu`
- Atau path: `http://localhost:3000/i/andisiti?to=NamaTamu`

### Paket Langganan

| Tier | Tamu | Custom Domain | White-label | Watermark |
|------|------|---------------|-------------|-----------|
| Basic | 100 | ✗ | ✗ | ✓ |
| Premium | 500 | ✓ | ✓ | ✗ |
| Enterprise | Unlimited | ✓ | ✓ | ✗ |

| URL | Deskripsi |
|-----|-----------|
| `/` | Landing page |
| `/blog` | Blog & SEO |
| `/blog/:slug` | Artikel blog |
| `/blog/author/:slug` | Profil penulis (EEAT) |
| `/sitemap.xml` | Sitemap dinamis (via API proxy) |
| `/robots.txt` | Robots.txt dinamis |
| `/login` | Halaman masuk |
| `/dashboard` | Portal klien |
| `/dashboard/builder` | Page builder |
| `/dashboard/preview` | Pratinjau undangan |
| `/i/:subdomain?to=Nama` | Undangan publik |
| `/admin` | Panel super admin |

Lihat [ROADMAP.md](./ROADMAP.md) untuk rencana pengembangan lengkap.

### SEO & SSR (31+ Artikel)

Halaman publik berikut di-render **Server-Side** dengan meta lengkap + JSON-LD:

| URL | SSR | Schema |
|-----|-----|--------|
| `/` | ✓ | WebSite, Organization, FAQPage |
| `/blog` | ✓ | Organization, WebSite |
| `/blog/:slug` | ✓ | BlogPosting, FAQPage, Breadcrumb, Speakable |
| `/i/:subdomain` | ✓ | Event, Organization |
| Subdomain tenant `/` | ✓ | Event |

Setiap artikel blog memiliki: **focus keyword**, **meta description**, **author EEAT** (bio + jabatan), **4 FAQ** untuk AI Overview, dan konten 600–1200 kata.

Verifikasi SSR (Googlebot melihat HTML penuh tanpa JS):

```bash
curl -s http://localhost:3000/blog/seo-undangan-pernikahan-google | grep -E '<h1|FAQPage|BlogPosting'
```

### API SEO & Blog

| Endpoint | Auth | Deskripsi |
|----------|------|-----------|
| `GET /api/public/blog` | — | Daftar artikel + JSON-LD organization |
| `GET /api/public/blog/:slug` | — | Detail artikel + JSON-LD |
| `GET /api/admin/blog` | owner | Semua artikel (termasuk draft) |
| `POST/PATCH/DELETE /api/admin/blog` | owner | CRUD artikel |
| `GET /sitemap.xml` | — | Sitemap otomatis (landing, blog, undangan) |
| `GET /robots.txt` | — | Robots.txt |

Landing page menggunakan A/B test hero (`localStorage`: `invitapro_ab_landing_hero`).

## Tech Stack
- Vite 6
- Tailwind CSS 4
- GSAP & Motion (animasi)
- Recharts (grafik dashboard)
- jsPDF & JSZip (export cetak)

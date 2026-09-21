# FSM Watermaker — Denizde Özgürlük, Sınırsız Tatlı Su.

> **Tuzla'da üretilen %100 yerli deniz suyu arıtma sistemleri.** 30L/saat'ten 500L/saat'e, 12V'tan Engine Drive'a kadar lüks ve ticari tekneler için üretim, montaj ve 48 saat servis ağı.

[[Next.js 15](https://img.shields.io/badge/Next.js-15-black?style=flat-square)](https://nextjs.org)
[[Prisma](https://img.shields.io/badge/Prisma-5.20-2D3748?style=flat-square)](https://prisma.io)
[[TypeScript](https://img.shields.io/badge/TypeScript-100%25-3178C6?style=flat-square)](https://www.typescriptlang.org)
[[License: MIT](https://img.shields.io/badge/License-MIT-C5A86A?style=flat-square)](#lisans)
[[Made in Tuzla](https://img.shields.io/badge/Made_in-Tuzla,_TR-0A0A0A?style=flat-square)](#)

**Canlı Önizleme:** [fsmwatermaker.com](https://fsmwatermaker.com) | **Demo:** Konfigüratör → PDF Teklif → WhatsApp → Montaj

---

### ✨ Neden Bu Proje?

Marin sektörü için geliştirilen tipik katalog siteleri 3 problemde çöküyor:

1.  **Kapasite hesabı yok:** Müşteri kaç L/saat alacağını bilmiyor, yanlış ürün alıyor.
2.  **Servis kopuk:** Montaj sonrası QR, bakım geçmişi, membran değişim takibi yok.
3.  **Lüks algısı yok:** 200 bin TL'lik yat ekipmanı, ucuz e-ticaret teması gibi duruyor.

**FSM Watermaker bu 3 sorunu tek mimaride çözer:**

-   **Konfigüratör Motoru:** Tekne boyu + kişi sayısı + enerji tipi → Mühendislik formülü ile doğru model. `Günlük İhtiyaç = Kişi * 35L + 50L (duş/mutfak)`, `Gerekli Kapasite = Günlük * Boy Katsayısı * 1.3 / 4 saat`
-   **Servis OS:** Her cihaza özel QR seri no, TDS/basınç telemetri, ticket FSM (OPEN → DONE), teknisyen PWA offline form.
-   **Lüks Tasarım Sistemi:** Siyah #0A0A0A, Krem #F9F5EB, Fırçalanmış Altın #C5A86A. Sora + Geist Sans, glassmorphism, 16px radius, Linear/Stripe seviyesi estetik.

### 🚀 Özellikler

| Modül | Açıklama |
| :--- | :--- |
| **Kurumsal Web** | Lighthouse 95+, AVIF hero, Framer Motion, SEO silo yapısı |
| **Akıllı Katalog** | Filtre: Kapasite, Enerji (12V/24V/220V/Engine), Ses dB |
| **Konfigüratör + PDF Teklif** | Anında hesaplama, Inngest ile PDF oluşturma, WhatsApp handoff |
| **Bayi & Müşteri Portalı** | RBAC (ADMIN, DEALER, TECHNICIAN, CUSTOMER), NextAuth v5 JWT |
| **Servis & Montaj** | Ticket oluştur, foto yükle (Supabase Storage), teknisyen ata, 48s SLA |
| **Cihaz QR Takibi** | `Device.serialNo` → QR okut → Montaj tarihi, son bakım, membran ömrü |
| **Yedek Parça Mini-Shop** | Filtre, membran, pompa keçesi — Iyzico modüler entegrasyon |
| **IoT Hazır** | `telemetry: {tds, pressure, hours}` alanı — gelecekte ESP32 entegrasyonu |

### 🏗️ Mimari — Modüler Monolit (Microservice'e Hazır)

```
[Edge] Vercel Middleware + Upstash Redis (Rate Limit 100 req/min)
  ↓
[App] Next.js 15 App Router (RSC + Server Actions)
  ↓
[API] /api/rest (OpenAPI + Zod) + /trpc (type-safe internal)
  ↓
[Domain] server/modules/{product-catalog, service-ops, commerce, device-iot}
  - Her modül kendi entities, services, repositories katmanına sahip (Clean Arch)
  ↓
[Data] Prisma → PostgreSQL (Supabase) + RLS + Supabase Storage
[Cache/Queue] Upstash Redis + QStash/Inngest Worker (PDF, hatırlatma)
[Observability] Pino Logger + Sentry + PostHog
```

> Başlangıçta modüler monolit → trafik arttığında sadece `service-ops` klasörünü ayrı servise taşıman yeterli.

### 📁 Klasör Yapısı

```bash
/src
├── /app
│   ├── (marketing)/page.tsx         # Lüks Landing Hero
│   ├── (catalog)/urunler/[slug]     # Ürün Detay + Specs Table
│   ├── (catalog)/konfigurator       # Kapasite hesaplama (aha moment)
│   ├── (portal)/servis              # Ticket Kanban Dashboard
│   └── /api/v1/{products,tickets,devices}
├── /components
│   ├── /ui                          # shadcn - Button, Card, Dialog
│   ├── /marketing                   # HeroLuxury, SpecsTable
│   └── /portal                      # TicketKanban, DeviceQRCard
├── /lib                             # prisma.ts, redis.ts, logger.ts, auth.ts
├── /server/modules                  # DDD modüller
│   ├── product-catalog/service.ts
│   └── service-ops/service.ts       # Ticket FSM logic
└── /hooks                           # useBoatConfigurator, useServiceTicket
/prisma/schema.prisma                # Enterprise şema + index stratejisi
/design-tokens.json                  # Figma import hazır
```

### 🛠️ Tech Stack

**Frontend:** Next.js 15, React 19, TypeScript 5.6 (strict %100), Tailwind CSS 3.4 + shadcn/ui, Zustand + TanStack Query v5, React Hook Form + Zod, Framer Motion

**Backend:** Prisma 5.20, PostgreSQL (Supabase), Auth.js v5 (JWT + Refresh + RBAC), tRPC + REST hibrit, Upstash Redis, Resend (email), Pino, Zod validation

**DevOps:** Docker, GitHub Actions (lint → test → build), Vercel Deploy, Sentry, PostHog, OpenAPI Docs

### ⚡ Hızlı Başlangıç

**1. Kurulum**
```bash
git clone https://github.com/fsm-watermaker/fsm-watermaker.git
cd fsm-watermaker
pnpm i # veya npm i
cp .env.example .env
```

**2. Env Ayarla**
```env
DATABASE_URL="postgresql://postgres:xxx@db.supabase.co:5432/postgres"
NEXTAUTH_SECRET="32 karakter super secret"
NEXTAUTH_URL="http://localhost:3000"
UPSTASH_REDIS_REST_URL="https://..."
UPSTASH_REDIS_REST_TOKEN="..."
RESEND_API_KEY="re_..."
SENTRY_DSN=""
```

**3. DB**
```bash
pnpm prisma migrate dev --name init
pnpm prisma db seed # Demo ürünler: FSM 60, 120, 200 DUO
pnpm dev # http://localhost:3000
```

**4. İlk Ürünü Ekle**
```bash
pnpm prisma studio
# Product → Create → slug: fsm-120, name: FSM 120, capacity: 120, powerType: V24
```

### 🔌 API — Core Endpoints

| Method | Endpoint | Açıklama | Auth |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/products?capacity_gte=100&power=V24` | Katalog listele | Public |
| `POST` | `/api/v1/configurator/quote` | `{boatLength, crew, power} → öneri + PDF` | Public |
| `POST` | `/api/v1/tickets` | Servis talebi + foto upload | CUSTOMER |
| `GET` | `/api/v1/devices/:serialNo` | QR → cihaz geçmişi, bakım log | TECHNICIAN |
| `POST` | `/api/v1/checkout` | Yedek parça sipariş (Iyzico) | CUSTOMER |

OpenAPI dokümanı: `/api/openapi.json`

### 🎨 Tasarım Sistemi

**Renkler:**
- `bg-primary: #0A0A0A` — Rich Black (ana zemin)
- `bg-secondary: #F9F5EB` — Warm Cream (kart)
- `accent-gold-500: #C5A86A` — Brushed Brass (CTA, border)
- `text-muted: #8A8A8A`

**Tipografi:** Display: Sora 600, clamp(40px, 6vw, 72px) | Body: Geist Sans 400, 16px/28px

**Token:** `design-tokens.json` → Figma Variables'a direkt import edilebilir. Auto-layout %100, radius 16-24px, shadow `0 20px 60px rgba(0,0,0,0.4)` + gold glow.

### 📈 SEO — Sıfırdan Top 3 Stratejisi

- Teknik: `app/sitemap.ts`, `robots.ts`, `generateMetadata()` dinamik, Image AVIF/WebP, LCP < 2.5s
- Schema: Organization + Product + FAQPage JSON-LD (her ürün sayfasında)
- Keyword Cluster: `tekne su yapıcı fiyatları` (880 hacim, transactional), `deniz suyu arıtma sistemi tekne` (720), `su yapıcı membran değişimi` (480, informational)
- İçerik: Pillar "Tekne Su Yapıcı Rehberi 2026" → Cluster 12 blog (bakım, membran, 12v vs)

### 📦 Deployment Checklist

- [ ] Env validation `@t3-oss/env-nextjs`
- [ ] Rate limit Upstash middleware'de aktif
- [ ] RLS: `service_tickets` tablosunda `auth.uid() = user_id`
- [ ] Supabase Storage bucket `service-photos` public read kapalı
- [ ] Sentry release tag
- [ ] Vercel Cron: Her 6 ayda bir bakım hatırlatma email

```bash
pnpm build # Lighthouse 95+ kontrolü
vercel --prod
```

### 🗓️ Yol Haritası — İlk 14 Gün MVP → Enterprise

**Gün 1-2:** Prisma + Auth + shadcn kur
**Gün 3-4:** Landing Hero + Katalog + Konfigüratör ship
**Gün 5:** Servis ticket CRUD + foto upload
**Gün 6:** RBAC + Kanban
**Gün 7:** SEO + Vercel deploy → **MVP CANLI**
**Gün 8-9:** Yedek parça shop + Iyzico
**Gün 10:** Teknisyen PWA offline form
**Gün 11-14:** 3 blog + GMB + Ads launch

### 🤝 Katkı

1. Forkla, `feat/xxx` branch aç
2. `pnpm lint` ve `pnpm build` hatasız olmalı
3. PR açıklamasına Loom video ekle (UX değişimlerinde)

### 📄 Lisans

MIT — Ticari kullanıma açık. Tuzla'da üretilen her FSM cihazı için bir yıldız bırakırsan seviniriz.

### 📞 İletişim

**FSM Watermaker** — Tuzla, İstanbul
Web: fsmwatermaker.com | Email: servis@fsmwatermaker.com | Tel: +90 216 XXX XX XX

> Bu repo bir katalog sitesinden fazlasıdır. Bu, bir teknenin su bağımsızlığı için işletim sistemidir.

---

<details>
<summary>🇬🇧 English Summary (for international partners)</summary>

**FSM Watermaker** is a 100% locally manufactured marine watermaker system (30-500L/h) built in Tuzla, Istanbul. This repo is the enterprise operating system: Next.js 15, Prisma, Supabase, luxury UI (Black/Cream/Gold), smart configurator (boat length + crew → capacity), service ticket FSM with QR device tracking, and spare parts commerce. Designed for yacht builders (Sirena, Numarine) and owners. Lighthouse 95+, SEO-ready, modular monolith → microservice ready.

</details>

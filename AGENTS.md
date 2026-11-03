# AGENTS.md — Spesifikasi teknis "Kota Warga"

File ini dibaca oleh AI coding agent (ChatGPT Codex, Claude Code, dll.) setiap kali bekerja di repo ini.
Konsep produk lengkap ada di `docs/KONSEP-LENGKAP.md`. **Baca file itu dulu sebelum mengerjakan fase apa pun.**

---

## 0. Aturan kerja untuk agent

1. Kerjakan **satu fase per tugas**, sesuai bagian "Fase" di bawah. Jangan melompat fase.
2. Sebelum menulis kode, baca struktur repo yang sudah ada dan ikuti pola yang sudah dipakai.
3. Setiap fase harus diakhiri dengan: `npm run lint`, `npm run typecheck`, `npm run test` **lulus**, dan `npm run build` sukses.
4. Perbarui `docs/PROGRESS.md`: apa yang selesai, apa yang belum, keputusan teknis, dan cara mengetes manual.
5. Jangan pernah meng-commit rahasia. Semua kunci lewat environment variable; perbarui `.env.example` setiap ada variabel baru.
6. Semua teks antarmuka pengguna dalam **Bahasa Indonesia** santai dan ramah. Kode, nama variabel, nama tabel, dan komentar kode dalam **bahasa Inggris**.
7. Semua nama produk, mata uang, harga, hadiah, dan batas diambil dari `src/config/` — **tidak boleh ada angka ekonomi yang di-hardcode** di komponen/endpoint.
8. Prioritas: **berfungsi di HP** (layar 360px ke atas) → benar & aman → cepat → cantik.
9. Jika ada keputusan yang ambigu, pilih opsi paling sederhana yang tetap memenuhi kriteria, lalu catat di `docs/PROGRESS.md` bagian "Keputusan".
10. Jangan menambahkan dependensi besar baru tanpa alasan yang ditulis di `docs/PROGRESS.md`.

---

## 1. Stack

| Bagian | Pilihan |
|---|---|
| Framework | Next.js 15 (App Router), React 19, TypeScript strict |
| Styling | Tailwind CSS v4, komponen sendiri (boleh shadcn/ui) |
| Peta | PixiJS v8 + `pixi-viewport` (dibungkus komponen React client-only) |
| State klien | Zustand (state UI) + TanStack Query (data server) |
| Backend | Next.js Route Handlers (`src/app/api/**`) + Supabase |
| Database | Supabase Postgres + Row Level Security + fungsi SQL (RPC) |
| Auth | Supabase Auth: Google OAuth + email magic link (`@supabase/ssr`) |
| Storage | Supabase Storage (bucket `shop-media`, `share-cards`) |
| Validasi | Zod di semua input API |
| Pembayaran | Pakasir QRIS via antarmuka `PaymentProvider` (+ `MockPaymentProvider`) |
| Iklan | Antarmuka `AdProvider` (`MockAdProvider`, `GamRewardedAdProvider`) |
| Gambar share | `next/og` (ImageResponse) |
| PWA | Manifest + service worker sederhana (mis. `@serwist/next`) |
| Test | Vitest (unit), Playwright (e2e alur utama) |
| Lint/format | ESLint + Prettier |
| Deploy | Vercel |
| Zona waktu | Semua logika harian memakai **Asia/Jakarta (WIB)**; simpan `timestamptz` UTC di DB |

Gunakan versi stabil terbaru yang tersedia saat inisialisasi jika versi di atas sudah tidak terbaru; catat versinya di `docs/PROGRESS.md`.

---

## 2. Struktur folder

```
/
├─ AGENTS.md
├─ docs/
│  ├─ KONSEP-LENGKAP.md
│  └─ PROGRESS.md
├─ public/
│  ├─ assets/            # sprite, tile, ikon (placeholder dulu)
│  └─ manifest.webmanifest
├─ supabase/
│  ├─ migrations/        # file SQL berurutan: 0001_init.sql, 0002_...
│  └─ seed.sql           # data contoh: distrik, kavling, 200 warga, 10 toko, item
├─ src/
│  ├─ app/
│  │  ├─ (public)/page.tsx            # peta + pencarian (halaman utama)
│  │  ├─ (public)/warga/[username]/   # profil publik + rumah
│  │  ├─ (public)/toko/[slug]/        # halaman toko (SEO-friendly)
│  │  ├─ (public)/pasang-toko/        # halaman penjualan paket untuk UMKM
│  │  ├─ (public)/peringkat/
│  │  ├─ (game)/main/                 # hub: absen, kerja, panen, misi, game
│  │  ├─ (game)/rumahku/              # upgrade & dekorasi
│  │  ├─ (game)/toko-item/            # beli dekorasi/permata
│  │  ├─ mitra/                       # dashboard pengiklan
│  │  ├─ admin/                       # panel admin
│  │  ├─ masuk/                       # login
│  │  ├─ kebijakan-privasi/, syarat/
│  │  └─ api/...
│  ├─ config/
│  │  ├─ app.ts          # nama kota, nama mata uang, akun IG resmi, warna
│  │  ├─ economy.ts      # semua angka hadiah, harga, batas
│  │  ├─ packages.ts     # paket toko & paket Permata
│  │  └─ districts.ts    # definisi distrik & ambang buka
│  ├─ components/        # UI umum
│  ├─ features/
│  │  ├─ map/            # engine PixiJS, layer, kamera, picking
│  │  ├─ residents/      # warga, klaim, profil
│  │  ├─ shops/          # kartu toko, statistik
│  │  ├─ economy/        # wallet, ledger client helpers
│  │  ├─ checkin/, jobs/, farm/, quests/, minigames/, leaderboard/
│  │  ├─ ads/            # AdProvider + implementasi
│  │  ├─ payments/       # PaymentProvider + implementasi
│  │  └─ admin/, partner/
│  ├─ lib/
│  │  ├─ supabase/       # client browser, server, service-role (server only)
│  │  ├─ time.ts         # helper WIB: todayWIB(), weekStartWIB()
│  │  ├─ rate-limit.ts
│  │  └─ errors.ts
│  └─ types/
└─ tests/ (unit & e2e)
```

---

## 3. Environment variables (`.env.example`)

```
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=            # server only, jangan pernah ke klien

PAYMENT_PROVIDER=mock                 # mock | pakasir
PAKASIR_PROJECT_SLUG=
PAKASIR_API_KEY=
PAKASIR_WEBHOOK_SECRET=               # jika tersedia; jika tidak, verifikasi ulang status via API

AD_PROVIDER=mock                      # mock | gam
NEXT_PUBLIC_GAM_REWARDED_AD_UNIT=     # contoh: /1234567/kota_rewarded

ADMIN_EMAILS=                         # daftar email admin dipisah koma (bootstrap)
CRON_SECRET=                          # untuk endpoint cron Vercel
```

---

## 4. Model data (Supabase Postgres)

Semua tabel memakai `id uuid primary key default gen_random_uuid()`, `created_at timestamptz default now()`, dan `updated_at` jika bisa diubah. Nama di bawah adalah kolom inti; tambahkan index yang wajar.

### 4.1 Pengguna & warga
- **profiles** — `id` (= auth.users.id), `display_name`, `role` (`player` | `partner` | `admin`), `level int default 1`, `xp int default 0`, `referral_code text unique`, `referred_by uuid null`, `is_banned bool`, `last_seen_at`.
- **residents** — warga di peta. `ig_username text unique` (lowercase, tanpa @), `profile_id uuid null` (null = belum diklaim), `verification_status` (`unclaimed` | `pending` | `verified`), `verification_code text`, `source` (`ig_comment` | `web_signup` | `admin`), `joined_day int` (hari ke-berapa kota), `house_id uuid`.
- **houses** — `resident_id`, `plot_id unique`, `level int 1..6`, `style text`, `name text null` (nama papan rumah, difilter), `like_count int`, `decorations jsonb` (daftar `{item_id, slot}`).

### 4.2 Peta
- **districts** — `slug`, `name`, `kind` (`residential` | `commercial` | `market` | `plaza` | `farm` | `games` | `event`), `unlock_resident_count int`, `bounds jsonb` (grid min/max), `theme jsonb`.
- **plots** — kavling grid. `district_id`, `grid_x int`, `grid_y int`, `size` (`s` | `m` | `l` | `xl`), `kind` (`house` | `shop` | `facility` | `decor`), `occupied_by_type`, `occupied_by_id`. Unique (`grid_x`, `grid_y`).
- **facilities** — fasilitas umum. `plot_id`, `name`, `icon`, `description`, `adopted_by_profile_id null`, `feature_key null` (mis. `farm`, `job_board`, `games`, `bank`, `post_office`).

### 4.3 Toko & pengiklan
- **partners** — pemilik usaha. `profile_id`, `business_name`, `contact_whatsapp`, `email`, `status`.
- **shops** — `partner_id`, `slug unique`, `name`, `category`, `logo_url`, `description`, `photos text[]` (maks 6), `links jsonb` (`instagram`, `tiktok`, `whatsapp`, `website`, `shopee`, `tokopedia`, `gmaps`), `promo_text`, `voucher_code`, `opening_hours`, `plot_id`, `package` (`lapak` | `toko` | `utama` | `landmark`), `active_until timestamptz`, `status` (`draft` | `pending_review` | `active` | `expired` | `rejected`), `building_sprite`.
- **shop_revisions** — perubahan yang menunggu moderasi: `shop_id`, `payload jsonb`, `status`, `reviewed_by`.
- **shop_events** — analitik: `shop_id`, `type` (`view` | `click_instagram` | `click_whatsapp` | ... | `job_start` | `quest_complete`), `profile_id null`, `session_hash` (hash anonim), `created_at`. Index (`shop_id`, `created_at`).
- **sponsored_quests** — `shop_id`, `title`, `question`, `answer_options jsonb`, `correct_answer`, `reward_coins`, `starts_at`, `ends_at`.

### 4.4 Ekonomi
- **wallets** — `profile_id unique`, `coins bigint`, `gems bigint` (cache, hanya diubah oleh fungsi SQL).
- **ledger_entries** — append-only. `profile_id`, `currency` (`coin` | `gem` | `xp`), `amount bigint` (+/-), `reason` (enum teks: `checkin`, `job_salary`, `harvest`, `quest`, `minigame`, `ad_reward`, `purchase`, `upgrade`, `decor_buy`, `like_reward`, `referral`, `admin_adjust`, `streak_save`, …), `ref_type`, `ref_id`, `idempotency_key text unique`, `meta jsonb`.
- **items** — katalog: `key unique`, `name`, `category` (`fence`, `garden`, `pet`, `vehicle`, `light`, `sign`, `trophy`, …), `rarity`, `price_coins null`, `price_gems null`, `sponsor_shop_id null`, `sprite`, `is_active`, `limited_until null`.
- **inventory** — `profile_id`, `item_id`, `qty`.

### 4.5 Aktivitas harian
- **checkins** — `profile_id`, `day_wib date`, `streak int`, `reward_coins`. Unique (`profile_id`, `day_wib`).
- **job_shifts** — `profile_id`, `shop_id`, `duration_minutes`, `started_at`, `ends_at`, `salary_coins`, `claimed_at null`, `boosted bool`. Maks 1 shift aktif per pemain.
- **farm_plots** — `profile_id`, `slot int`, `crop_key null`, `planted_at`, `ready_at`, `harvested_at`.
- **quests** — definisi: `key`, `period` (`daily` | `weekly` | `sponsored`), `title`, `target_type` (mis. `visit_shop`, `like_house`, `play_minigame`, `harvest`, `job_complete`), `target_count`, `reward_coins`, `reward_xp`.
- **quest_progress** — `profile_id`, `quest_id`, `period_key` (mis. `2026-11-03` atau `2026-W45`), `progress`, `completed_at`, `claimed_at`.
- **likes** — `from_profile_id`, `house_id`, `day_wib`. Unique (`from_profile_id`, `house_id`, `day_wib`).
- **gifts** — `from_profile_id`, `to_profile_id`, `item_or_coins jsonb`, `day_wib`.

### 4.6 Mini game & peringkat
- **minigame_sessions** — `profile_id`, `game_key`, `seed`, `started_at`, `token_hash`, `submitted_at`, `score`, `duration_ms`, `is_valid`, `reward_coins`.
- **leaderboard_weekly** — view/materialized view per kategori & `week_key`; refresh lewat cron.

### 4.7 Iklan & pembayaran
- **ad_sessions** — `profile_id`, `placement` (`daily_bonus` | `salary_boost` | `harvest_double` | `shift_skip` | `streak_save` | `minigame_retry`), `token_hash`, `status` (`issued` | `granted` | `expired` | `rejected`), `issued_at`, `granted_at`, `ref_id null`.
- **payments** — `profile_id`, `order_id unique` (dibuat server), `provider`, `product_type` (`gem_pack` | `membership` | `tip` | `shop_package`), `product_ref`, `amount_idr int`, `status` (`pending` | `paid` | `expired` | `failed`), `provider_payload jsonb`, `paid_at`, `fulfilled_at`.
- **memberships** — `profile_id`, `active_until`.
- **supporters** — `profile_id`, `total_units int`, `total_idr int` (diperbarui saat tip lunas).

### 4.8 Konten & admin
- **calendar_items** — `day_wib date unique`, `title`, `teaser`, `icon`, `unlock_at timestamptz`, `link_type`, `link_ref`.
- **announcements** — `title`, `body`, `starts_at`, `ends_at`, `kind` (`info` | `warning`).
- **city_goals** — tantangan bersama: `title`, `metric`, `target`, `progress`, `reward_description`, `week_key`.
- **reports** — laporan konten: `reporter_id`, `target_type`, `target_id`, `reason`, `status`.
- **audit_log** — `actor_id`, `action`, `target_type`, `target_id`, `diff jsonb`.
- **app_settings** — key/value untuk saklar fitur (mis. `ads_enabled`, `payments_enabled`, `city_day_zero`).

### 4.9 Row Level Security (wajib)
- Data publik (districts, plots, houses, residents terverifikasi, shops `active`, facilities, items aktif, calendar yang sudah terbuka, leaderboard) → `select` untuk semua.
- Data pribadi (wallets, ledger, inventory, checkins, job_shifts, farm_plots, quest_progress, payments, ad_sessions) → `select` hanya pemiliknya.
- **Tidak ada `insert/update/delete` dari klien** untuk tabel ekonomi. Semua perubahan lewat route handler server memakai service role, atau fungsi SQL `security definer` yang memeriksa `auth.uid()`.
- Partner hanya bisa membaca statistik & membuat `shop_revisions` untuk toko miliknya.
- Admin ditentukan oleh `profiles.role = 'admin'`.

### 4.10 Fungsi SQL inti
- `grant_currency(profile_id, currency, amount, reason, ref_type, ref_id, idempotency_key, meta)` — satu-satunya pintu perubahan saldo: insert ke ledger (gagal diam-diam bila `idempotency_key` sudah ada → kembalikan hasil sebelumnya), update cache wallet dalam transaksi yang sama, tolak jika saldo jadi negatif.
- `spend_currency(...)` — pembungkus `grant_currency` dengan amount negatif + cek saldo.
- `assign_plot_for_resident(resident_id)` — cari kavling kosong `kind='house'` di distrik terbuka (urut distrik lalu spiral dari pusat distrik), kunci baris (`for update skip locked`), buat house.
- `recompute_wallet(profile_id)` — rekonsiliasi dari ledger (untuk admin).

---

## 5. Engine peta (features/map)

- Grid isometrik: tile 128×64 px. Konversi `gridToScreen(x, y)` dan `screenToGrid(px, py)` dalam modul murni (dites unit).
- Layer: tanah/jalan (tilemap), bangunan (diurutkan berdasarkan kedalaman `x + y`), label nama (muncul hanya saat zoom cukup dekat), efek (siang/malam).
- Kamera: `pixi-viewport` dengan drag, pinch, wheel, decelerate, clamp ke batas kota, zoom 0.25–2.5. Tombol: zoom +/−, kembali ke pusat, layar penuh.
- **Culling**: hanya render bangunan dalam viewport (+margin). Target ≥ 50 fps di HP kelas menengah dengan 10.000 rumah.
- Data peta dimuat bertahap: `GET /api/map/chunks?x0&y0&x1&y1` mengembalikan bangunan per chunk 32×32 tile, di-cache (CDN `s-maxage=60`).
- Pengaturan kualitas: Hemat / Seimbang / Tinggi (resolusi, animasi, bayangan). Simpan di localStorage (dibungkus try/catch).
- Ketuk bangunan → buka panel bawah (bottom sheet): Kartu Rumah / Kartu Toko / Kartu Fasilitas.
- Pencarian username → kamera terbang ke rumah + sorot berkedip + buka Kartu Rumah.
- Siklus siang/malam berdasarkan jam WIB (overlay warna + lampu).
- Sprite placeholder: bentuk isometrik digambar dengan `Graphics` + warna per level/kategori, agar bisa jalan tanpa aset. Siapkan `assetManifest` supaya nanti mudah diganti sprite sungguhan (mis. aset CC0 Kenney).
- Suara opsional (default mati), tombol 🔇.

---

## 6. API (Route Handlers)

Semua endpoint: validasi Zod, respons `{ ok: true, data }` atau `{ ok: false, error: { code, message } }` (pesan Bahasa Indonesia), rate limit per user/IP, dan cek auth bila perlu. Endpoint yang mengubah ekonomi menerima header `Idempotency-Key`.

**Publik**
- `GET /api/map/chunks` — bangunan dalam area.
- `GET /api/search/resident?q=` — cari username (prefix, maks 10).
- `GET /api/residents/[username]` — profil publik.
- `GET /api/shops/[slug]` — data toko aktif.
- `POST /api/shops/[slug]/event` — catat `view`/`click_*` (anonim boleh, dedupe per session_hash per jam).
- `GET /api/stats/city` — jumlah warga, hari ke-, toko aktif, bangunan.
- `GET /api/leaderboard?category=&period=`
- `GET /api/calendar/week` — 7 hari; item yang belum `unlock_at` hanya kirim teaser.

**Pemain (login)**
- `POST /api/residents/claim` — mulai klaim username → kode verifikasi.
- `POST /api/checkin` — absen hari ini.
- `GET /api/me` — profil, wallet, level, status harian ringkas (sudah absen?, shift aktif, panen siap, misi).
- `POST /api/jobs/start` `{ shopId, durationMinutes }` — wajib sudah ada event `view` toko tsb oleh pemain dalam 10 menit terakhir.
- `POST /api/jobs/claim`
- `POST /api/farm/plant` `{ slot, cropKey }`, `POST /api/farm/harvest` `{ slot }`, `POST /api/farm/unlock-slot`
- `POST /api/quests/claim` `{ questId }`
- `POST /api/houses/like` `{ houseId }`, `POST /api/gifts/send`
- `POST /api/house/upgrade`, `POST /api/house/decorate` `{ decorations }`, `POST /api/items/buy` `{ itemId }`
- `POST /api/minigames/start` `{ gameKey }` → `{ sessionToken, seed }`
- `POST /api/minigames/submit` `{ sessionToken, score, durationMs, inputLog }`
- `POST /api/ads/session` `{ placement, refId? }` → `{ adToken }` (tolak jika melewati batas harian/jeda)
- `POST /api/ads/grant` `{ adToken }` → beri hadiah sesuai placement
- `POST /api/payments/create` `{ productType, productRef }` → `{ orderId, qrString/qrImageUrl, amount, expiresAt }`
- `GET /api/payments/[orderId]` — status (untuk polling dari halaman bayar)
- `POST /api/referral/apply` `{ code }`
- `GET /api/share/house/[username]` — gambar OG kartu rumah (1080×1920 dan 1200×630)

**Webhook & cron**
- `POST /api/webhooks/pakasir` — verifikasi (signature jika tersedia; **selalu cek ulang status ke API Pakasir** sebelum memenuhi pesanan), idempoten berdasarkan `order_id`, lalu `fulfillPayment()`.
- `GET /api/cron/daily` (Vercel Cron 00.05 WIB, cek `CRON_SECRET`) — kedaluwarsa toko, refresh leaderboard, buat progres misi baru, kedaluwarsa pembayaran pending > 30 menit.
- `GET /api/cron/evening` (19.00 WIB) — buka item kalender, buka distrik bila ambang tercapai.

**Mitra (partner)**
- `GET /api/partner/shops`, `GET /api/partner/shops/[id]/stats?range=7d|30d|90d`
- `POST /api/partner/shops/[id]/revision`
- `POST /api/partner/shops/[id]/renew` → buat payment `shop_package`

**Admin**
- `POST /api/admin/residents/import` — tempel teks/CSV username (dedupe, lowercase, buang @ dan spasi), buat residents + houses.
- `POST /api/admin/residents/[id]/verify`
- CRUD: districts, plots, facilities, shops, partners, items, quests, sponsored_quests, calendar_items, announcements, city_goals, app_settings.
- `POST /api/admin/shop-revisions/[id]/approve|reject`
- `POST /api/admin/wallet/adjust` (wajib `reason`)
- `GET /api/admin/stats`
- Semua aksi admin → `audit_log`.

---

## 7. Logika ekonomi (semua angka dari `src/config/economy.ts`)

```ts
export const economy = {
  checkin: { rewards: [10, 15, 20, 25, 30, 40, 100], day7BonusItemRarity: 'rare', streakSave: { gems: 5, adAllowed: true, maxPerWeek: 1 } },
  jobs: { shifts: [ { minutes: 60, coins: 15 }, { minutes: 240, coins: 50 }, { minutes: 480, coins: 90 } ], premiumShopBonusPct: 10, requireShopViewWithinMinutes: 10 },
  farm: { startSlots: 4, maxSlots: 12, slotUnlockCoins: [200, 300, 400, 600, 800, 1000, 1300, 1600],
          crops: { padi: { minutes: 30, sell: 8 }, jagung: { minutes: 120, sell: 25 }, cabai: { minutes: 360, sell: 60 }, durian: { minutes: 1440, sell: 200 } } },
  house: { upgradeCoins: { 2: 500, 3: 1500, 4: 4000, 5: 10000, 6: 25000 }, upgradeMinPlayerLevel: { 2: 2, 3: 5, 4: 10, 5: 20, 6: 30 } },
  likes: { maxPerDay: 20, coinsToReceiverPerLike: 2, maxReceivedCoinsPerDay: 100 },
  minigames: { maxRewardedPlaysPerDay: 10, coinsPerPlay: { min: 5, max: 40 } },
  ads: { maxPerDay: 8, minSecondsBetween: 60, tokenTtlSeconds: 300,
         rewards: { daily_bonus: { coins: 20 }, salary_boost: { multiplier: 1.5 }, harvest_double: { multiplier: 2 }, shift_skip: {}, streak_save: {}, minigame_retry: {} } },
  referral: { rewardCoins: 100, requiredLevel: 5, maxPerMonth: 20 },
  share: { coinsPerDay: 15 },
  xp: { perAction: { checkin: 10, job: 15, harvest: 5, quest: 20, minigame: 10, like: 1 }, levelCurve: 'round(100 * level^1.5)' },
} as const;
```

Aturan:
- Semua aksi berhadiah memakai `grant_currency` dengan `idempotency_key` deterministik (mis. `checkin:{profileId}:{dayWIB}`, `job:{shiftId}`, `ad:{adSessionId}`).
- Waktu selalu dari server (`now()` di DB).
- Perhitungan "hari" dan "minggu" memakai WIB (`src/lib/time.ts`), minggu mulai Senin.
- Unit test wajib untuk: kurva level, kalkulasi streak (termasuk bolong & selamatkan), gaji dengan bonus, panen ganda, batas iklan, batas like, idempotensi.

---

## 8. Iklan berhadiah (features/ads)

```ts
export interface AdProvider {
  isAvailable(): Promise<boolean>;
  showRewarded(opts: { placement: AdPlacement }): Promise<'granted' | 'closed' | 'unavailable'>;
}
```
- `MockAdProvider`: modal layar penuh dengan video/animasi placeholder 15 detik + hitung mundur, tombol tutup baru aktif setelah selesai → `granted`.
- `GamRewardedAdProvider`: Google Publisher Tag (`securepubads.g.doubleclick.net/tag/js/gpt.js`), `googletag.defineOutOfPageSlot(adUnit, googletag.enums.OutOfPageFormat.REWARDED)`, dengarkan `rewardedSlotReady` (panggil `makeRewardedVisible()` setelah pemain menekan tombol), `rewardedSlotGranted` → `granted`, `rewardedSlotClosed` → tutup. Jika slot null (tidak didukung) → `unavailable` dan sembunyikan tombol.
- Alur: klien `POST /api/ads/session` → dapat `adToken` → tampilkan iklan → jika `granted`, `POST /api/ads/grant`. Server menolak token kedaluwarsa/terpakai, melewati batas, atau terlalu cepat (< durasi minimal 10 detik sejak `issued_at`).
- Saklar `app_settings.ads_enabled`. Tombol iklan selalu berlabel jelas: "📺 Tonton iklan → +20 koin".

---

## 9. Pembayaran (features/payments)

```ts
export interface PaymentProvider {
  createQris(input: { orderId: string; amountIdr: number; description: string }): Promise<{ qrString?: string; qrImageUrl?: string; paymentUrl?: string; expiresAt: string; raw: unknown }>;
  getStatus(orderId: string, amountIdr: number): Promise<'pending' | 'paid' | 'expired' | 'failed'>;
  parseWebhook(req: Request): Promise<{ orderId: string; amountIdr: number; raw: unknown }>;
}
```
- `MockPaymentProvider`: halaman QR palsu + tombol "Simulasikan bayar" (hanya bila `PAYMENT_PROVIDER=mock` dan bukan produksi).
- `PakasirPaymentProvider`: implementasi mengikuti **dokumentasi resmi Pakasir** (endpoint create transaksi QRIS, cek status, webhook). Jangan menebak format — jika dokumentasi tidak tersedia di repo, buat implementasi dengan TODO jelas dan biarkan Mock aktif. Pemilik repo sudah pernah memakai Pakasir di proyek bot Telegram dan bisa memberikan contoh kodenya.
- `order_id` dibuat server: `KW-{yyyymmdd}-{random8}`. Jumlah diambil dari `src/config/packages.ts`, **tidak pernah dari klien**.
- `fulfillPayment(orderId)` idempoten: tandai `paid`, lalu sesuai `product_type`: tambah Permata (`grant_currency` key `payment:{orderId}`), aktifkan/perpanjang membership, tambah supporter, atau perpanjang `shops.active_until`.
- Halaman bayar: tampilkan QR, nominal, hitung mundur kedaluwarsa, polling status tiap 3 detik, animasi sukses.

---

## 10. Antarmuka & desain

- **Mobile-first**, satu tangan. Navigasi bawah 5 tab: 🏘️ Kota · 🎯 Main · 🏠 Rumahku · 🏆 Peringkat · ☰ Menu.
- Peta selalu menjadi latar halaman utama; panel lain berupa bottom sheet/drawer agar pemain "tetap di kota".
- HUD atas: hari ke-N, koin, permata, level (bar XP), tombol notifikasi (badge bila ada gaji/panen/misi siap).
- Gaya visual: cerah, hangat, kartun; sudut membulat; bayangan lembut; font bulat yang mudah dibaca (mis. "Nunito" / "Baloo 2"). Mendukung **mode gelap** lewat token warna CSS.
- Animasi mikro: koin beterbangan ke HUD saat dapat hadiah, konfeti saat naik level, getar ringan (Vibration API, opsional).
- Banner peringatan akun palsu (bisa ditutup, tersimpan).
- Tutorial pertama kali (4 langkah) dipandu maskot.
- Aksesibilitas: kontras cukup, ukuran sentuh ≥ 44px, `aria-label` pada tombol ikon, `prefers-reduced-motion` dihormati.
- Teks contoh: "Selamat datang, Warga!", "Gajimu sudah siap diambil 💰", "Panen siap! 🌾", "Rumahmu naik level! 🎉".

---

## 11. Keamanan & kualitas

- Kunci service role hanya di server (`import 'server-only'`).
- Rate limit (mis. tabel `rate_limits` di Postgres atau Upstash bila tersedia).
- Sanitasi semua teks pengguna; filter kata kasar (daftar di `src/config/bannedWords.ts`).
- CSP dasar, header keamanan di `next.config`.
- Validasi URL link toko (hanya `https://`, domain IG/TikTok/WA/Shopee/Tokopedia/Maps/website umum).
- Tidak ada chat bebas antar-pemain.
- Logging error ke console server (siapkan hook untuk Sentry nanti).
- Tes e2e minimal: buka peta → cari warga → buka kartu toko → klik link tercatat; login (mock) → absen → kerja → klaim gaji; beli Permata dengan mock payment.

---

## 12. Fase pengembangan & kriteria selesai

Setiap fase: kode + migrasi + seed + tes + `docs/PROGRESS.md` diperbarui.

**Fase 0 — Fondasi**
Inisialisasi Next.js + TS + Tailwind + ESLint/Prettier + Vitest + Playwright; Supabase client (browser/server/service); `src/config/*`; layout mobile + navigasi bawah (halaman kosong); `.env.example`; skrip `lint`, `typecheck`, `test`, `build`. ✔ Selesai bila `npm run build` sukses dan halaman utama tampil.

**Fase 1 — Kota yang bisa dipromosikan** *(paling penting)*
Migrasi tabel peta, warga, rumah, toko, fasilitas, kalender, pengumuman; seed (4 distrik, 300 kavling, 200 warga, 10 toko, 15 fasilitas); engine peta PixiJS lengkap (bagian 5); pencarian warga; Kartu Rumah, Kartu Toko (dengan pencatatan view & klik), Kartu Fasilitas; statistik kota di HUD; kalender kejutan; halaman `/toko/[slug]`, `/warga/[username]`, `/pasang-toko`; impor warga di `/admin` sederhana (dilindungi `ADMIN_EMAILS`). ✔ Selesai bila di HP: peta lancar, cari username berhasil, link toko terbuka & tercatat.

**Fase 2 — Akun & koin**
Auth (Google + magic link), profiles, klaim & verifikasi username, wallets + ledger + `grant_currency`, absen harian berantai, HUD koin/level/XP, `GET /api/me`. ✔ Absen dua kali di hari yang sama ditolak; streak benar lintas hari (dites).

**Fase 3 — Rumahku**
Upgrade rumah (syarat koin + level), katalog item, beli item, inventaris, mode dekorasi (pilih slot), like & kunjungi rumah, kirim hadiah. ✔ Saldo tidak pernah negatif; perubahan rumah terlihat di peta.

**Fase 4 — Lingkaran harian**
Bursa Kerja (syarat buka Kartu Toko), shift & klaim gaji, sawah & panen, misi harian/mingguan + progres otomatis dari aksi, level & XP, notifikasi dalam app. ✔ Semua timer memakai jam server.

**Fase 5 — Mini game & peringkat**
Kuis Kota, Hitung Cepat, Tangkap Barang (canvas/Pixi), sesi bertoken, validasi skor, batas hadiah, papan peringkat mingguan & sepanjang masa per kategori, piala top 3 otomatis tiap Senin. ✔ Skor mustahil ditolak.

**Fase 6 — Iklan berhadiah**
`AdProvider` + Mock + GAM, `ad_sessions`, semua placement di bagian 7, batas harian & jeda, saklar admin. ✔ Hadiah tidak bisa diklaim dua kali dengan token yang sama.

**Fase 7 — Pembayaran**
`PaymentProvider` + Mock + Pakasir, paket Permata, Kartu Warga bulanan, Traktir Kota + Tugu Pendukung, halaman bayar, webhook idempoten, cron kedaluwarsa. ✔ Webhook ganda tidak menggandakan Permata.

**Fase 8 — Dashboard mitra**
Login mitra, edit toko → revisi menunggu moderasi, statistik (grafik tayangan & klik per tombol, 7/30/90 hari), perpanjang paket via QRIS, misi sponsor, dekorasi sponsor, ekspor laporan bulanan (gambar/PDF sederhana). ✔ Mitra hanya melihat data tokonya.

**Fase 9 — Admin lengkap & live ops**
Semua CRUD admin, moderasi revisi & laporan, penyesuaian dompet beralasan, audit log, tantangan kota bersama, event musiman (mata uang event & toko event), statistik kota (DAU, retensi D1/D7, pendapatan), saklar fitur. ✔ Semua aksi admin tercatat.

**Fase 10 — Pertumbuhan & polish**
PWA (pasang ke layar utama), kartu share IG Story (OG image), referral, tutorial maskot, mode gelap, optimasi performa (Lighthouse mobile ≥ 85), SEO halaman toko, halaman privasi & syarat, penghapusan akun.

**Fase 11 — (Opsional) Instagram Graph API**
Impor komentar otomatis dari postingan terbaru akun IG Bisnis (token disimpan aman), pencocokan kode verifikasi otomatis, dijalankan via cron 18.30 WIB sebelum update 19.00.

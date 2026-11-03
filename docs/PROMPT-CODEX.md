# PROMPT UNTUK CHATGPT CODEX — langkah demi langkah

## Persiapan (sekali saja, ±10 menit)

1. Buat repo baru di GitHub (github.com/new), misalnya `kota-warga`, centang **Add a README**.
2. Upload 2 file ke repo itu (tombol **Add file → Upload files**):
   - `AGENTS.md` → taruh di **folder paling atas** repo.
   - `KONSEP-LENGKAP.md` → taruh di folder **`docs/`** (saat upload, ketik nama `docs/KONSEP-LENGKAP.md`).
3. Buka **chatgpt.com/codex**, hubungkan GitHub, pilih repo `kota-warga`.
4. Buat akun **Supabase** (supabase.com) → New project. Catat `Project URL`, `anon key`, `service_role key` (jangan dibagikan ke siapa pun).
5. Tempel prompt di bawah **satu per satu**. Tunggu satu fase selesai, cek hasilnya (Codex akan membuat Pull Request), **merge**, baru lanjut fase berikutnya.

> Codex otomatis membaca `AGENTS.md`, jadi prompt per fase cukup singkat. Jangan menempel semua fase sekaligus — hasilnya akan berantakan.

---

## PROMPT FASE 0 — Fondasi

```
Kamu adalah senior full-stack engineer dan game developer. Kita membangun "Kota Warga": kota virtual web isometrik untuk promosi UMKM dengan sistem game harian.

Baca dulu seluruh AGENTS.md dan docs/KONSEP-LENGKAP.md sampai selesai. Patuhi semua aturan di AGENTS.md bagian 0.

Kerjakan FASE 0 — Fondasi (AGENTS.md bagian 12):
- Inisialisasi Next.js (App Router) + TypeScript strict + Tailwind + ESLint + Prettier + Vitest + Playwright sesuai stack di bagian 1.
- Buat struktur folder sesuai bagian 2.
- Buat src/config/app.ts, economy.ts (isi persis dari bagian 7), packages.ts (paket toko & paket Permata dari KONSEP bagian 6.3 dan 8.1), districts.ts.
- Buat src/lib/supabase (browser, server, service-role dengan 'server-only'), src/lib/time.ts (helper WIB + unit test).
- Layout mobile-first dengan navigasi bawah 5 tab (bagian 10), halaman kosong dengan judul untuk tiap rute.
- Token warna CSS untuk mode terang & gelap.
- .env.example lengkap (bagian 3).
- Skrip npm: dev, build, lint, typecheck, test, test:e2e.
- Buat docs/PROGRESS.md berisi status, versi paket yang dipakai, cara menjalankan lokal, dan langkah setup Supabase.

Selesai bila lint, typecheck, test, dan build lulus. Tulis ringkasan perubahan dan cara mengetes di deskripsi PR.
```

---

## PROMPT FASE 1 — Kota yang bisa dipromosikan (paling penting)

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 1 sesuai AGENTS.md bagian 12, dengan detail:
1. Migrasi SQL di supabase/migrations untuk: districts, plots, facilities, residents, houses, partners, shops, shop_events, calendar_items, announcements, app_settings, profiles (minimal), audit_log. Lengkap dengan index dan Row Level Security (bagian 4.9) dan fungsi assign_plot_for_resident (bagian 4.10).
2. supabase/seed.sql: 4 distrik (Alun-Alun, Kampung Awal, Jalan Niaga, Pasar Rakyat), ±300 kavling, 200 warga contoh dengan rumah level acak 1–4, 10 toko UMKM contoh (kuliner, fashion, jasa) dengan link dummy, 15 fasilitas umum, kalender 7 hari, 1 pengumuman peringatan akun palsu.
3. Engine peta PixiJS + pixi-viewport sesuai AGENTS.md bagian 5 SECARA LENGKAP: grid isometrik, sorting kedalaman, culling, chunk loading lewat GET /api/map/chunks, drag/pinch/zoom, tombol zoom/pusat/layar penuh, label saat zoom dekat, siang/malam WIB, pengaturan kualitas Hemat/Seimbang/Tinggi, bangunan placeholder digambar dengan Graphics (warna berbeda per level rumah & kategori toko). Komponen harus client-only (dynamic import, ssr:false).
4. Pencarian "Cari rumahmu" (GET /api/search/resident) → kamera terbang ke rumah + sorot + buka Kartu Rumah.
5. Bottom sheet: Kartu Rumah, Kartu Toko (logo, foto, deskripsi, promo, voucher dengan tombol salin, tombol IG/TikTok/WA/Website/Shopee/Tokopedia/Maps), Kartu Fasilitas. Catat event view & click lewat POST /api/shops/[slug]/event (dedupe per session_hash per jam).
6. HUD atas: Hari ke-N (dari app_settings.city_day_zero), jumlah warga, toko, bangunan (GET /api/stats/city).
7. Kalender kejutan 7 hari (item terkunci hanya menampilkan teaser) dan banner pengumuman yang bisa ditutup.
8. Halaman /toko/[slug] (SEO: metadata + OG), /warga/[username], /pasang-toko (paket dari config/packages.ts + tombol DM/WA).
9. /admin sederhana, hanya untuk email di ADMIN_EMAILS: impor warga (tempel daftar username atau CSV; normalisasi lowercase, buang @ dan spasi, dedupe), lalu otomatis dapat rumah; daftar & edit toko sederhana.
10. Unit test: gridToScreen/screenToGrid, normalisasi username, dedupe event. E2E: buka peta → cari warga → buka toko → klik link tercatat.

Target performa: peta tetap lancar di HP dengan 10.000 rumah (buat skrip seed opsional untuk 10.000 rumah dan uji). Perbarui docs/PROGRESS.md. Lint, typecheck, test, build harus lulus.
```

---

## PROMPT FASE 2 — Akun & koin

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 2:
- Supabase Auth: Google OAuth + email magic link memakai @supabase/ssr, halaman /masuk, middleware sesi, tombol keluar.
- profiles lengkap (role, level, xp, referral_code), dibuat otomatis saat user pertama login (trigger).
- Klaim username IG (POST /api/residents/claim): jika username sudah ada & belum diklaim → kaitkan, jika belum ada → buat resident + rumah. Beri kode verifikasi "KW-XXXX". Status pending sampai admin memverifikasi di /admin. Batasan untuk yang belum terverifikasi sesuai KONSEP bagian 4.3.
- Tabel wallets, ledger_entries, fungsi SQL grant_currency & spend_currency & recompute_wallet (AGENTS.md 4.10) dengan idempotency_key unik; RLS sehingga klien tidak bisa menulis tabel ekonomi.
- Absen harian berantai (POST /api/checkin) memakai economy.checkin dan WIB; streak putus bila bolong.
- GET /api/me: profil, saldo, level, XP, sudah absen hari ini atau belum.
- HUD: koin, permata, level + bar XP, animasi koin terbang ke HUD saat dapat hadiah. Modal absen muncul otomatis saat pertama buka hari itu.
- Fungsi XP & kurva level dari config.
- Unit test: streak (berturut, bolong, ganti minggu, ganti bulan), idempotensi grant_currency, kurva level. E2E: login (mode test) → absen → absen lagi ditolak.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 3 — Rumahku

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 3:
- Tabel items, inventory, likes, gifts + seed 40 item dekorasi (pagar, taman, kolam, pohon, lampu, kendaraan, hewan, papan nama, piala) dengan rarity Biasa/Jarang/Epik/Legenda.
- Halaman /rumahku: pratinjau rumah besar, tombol upgrade (syarat koin & level dari config), mode dekorasi dengan slot (mis. 8 slot sekitar rumah), simpan ke houses.decorations.
- /toko-item: katalog dengan filter kategori & rarity, beli pakai koin atau permata (spend_currency), item sponsor ditandai logo toko.
- Tampilan rumah di peta berubah sesuai level & dekorasi.
- Kunjungi rumah tetangga (dari peta / profil publik), Like (batas harian, penerima dapat koin dengan batas), kirim hadiah 1x/hari per tetangga lewat Kantor Pos, kotak masuk hadiah.
- Nama papan rumah dengan filter kata kasar (src/config/bannedWords.ts) + tombol laporkan.
- Unit test: upgrade ditolak bila koin/level kurang, saldo tidak pernah negatif, batas like.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 4 — Lingkaran harian

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 4:
- Bursa Kerja: daftar lowongan dari toko aktif (toko paket utama/landmark di atas + bonus gaji), syarat sudah membuka Kartu Toko dalam 10 menit terakhir, pilih durasi shift, 1 shift aktif per pemain, klaim gaji setelah selesai (jam server). Catat event job_start ke shop_events.
- Sawah: petak awal 4, buka petak dengan koin, tanam/panen 4 jenis tanaman dari config, jual otomatis ke Lumbung saat panen.
- Misi harian (3) & mingguan (5) dipilih acak dari pool quests; progres otomatis bertambah dari aksi (visit_shop, like_house, harvest, job_complete, play_minigame); klaim hadiah koin + XP.
- Halaman hub /main: kartu Absen, Kerja, Sawah, Misi, Game dengan status & hitung mundur langsung.
- Badge notifikasi di navigasi bila ada gaji/panen/misi siap. Konfeti saat naik level.
- Cron /api/cron/daily (lindungi dengan CRON_SECRET) + vercel.json cron.
- Unit test: kalkulasi gaji + bonus, panen sebelum waktunya ditolak, progres misi lintas aksi.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 5 — Mini game & papan peringkat

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 5:
- Kerangka mini game: POST /api/minigames/start → sessionToken + seed; POST /api/minigames/submit dengan score, durationMs, inputLog; server memvalidasi (durasi wajar, skor maksimum teoretis dari seed, token sekali pakai), hadiah koin dengan batas harian.
- 3 game, masing-masing 30–90 detik, enak dimainkan dengan jari:
  1) Kuis Kota (pilihan ganda, bank soal di DB, bisa ada soal sponsor),
  2) Hitung Cepat (soal dari seed sehingga server bisa memverifikasi jawaban),
  3) Tangkap Barang (Pixi/canvas, barang jatuh, sprite bisa diganti barang sponsor).
- Papan peringkat mingguan (reset Senin 00.00 WIB) & sepanjang masa: Rumah Terpopuler, Terkaya Minggu Ini, Pekerja Teladan, Juara tiap game, Pendukung Kota. Halaman /peringkat dengan tab.
- Cron Senin: beri piala dekorasi untuk top 3 tiap kategori.
- Unit test validasi skor.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 6 — Iklan berhadiah

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 6 sesuai AGENTS.md bagian 8:
- Antarmuka AdProvider, MockAdProvider (modal 15 detik + hitung mundur), GamRewardedAdProvider (Google Publisher Tag rewarded: defineOutOfPageSlot dengan OutOfPageFormat.REWARDED, event rewardedSlotReady/Granted/Closed). Pilih lewat AD_PROVIDER.
- Tabel ad_sessions, POST /api/ads/session dan /api/ads/grant dengan batas harian, jeda minimal, TTL token, durasi minimal, token sekali pakai.
- Pasang tombol di semua placement: bonus harian (Bank Kota), gaji ×1,5, panen ×2, selesaikan shift, selamatkan streak, coba lagi mini game. Label selalu jelas & opsional.
- Saklar app_settings.ads_enabled; tombol tersembunyi bila iklan tidak tersedia.
- Unit test: token ganda ditolak, batas harian, jeda.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 7 — Pembayaran QRIS

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 7 sesuai AGENTS.md bagian 9:
- Antarmuka PaymentProvider, MockPaymentProvider (tombol "Simulasikan bayar", hanya non-produksi), PakasirPaymentProvider mengikuti dokumentasi resmi Pakasir. Jika format API Pakasir tidak tersedia, tulis implementasi dengan TODO yang jelas dan jangan menebak; Mock tetap jalan.
- Tabel payments, memberships, supporters. order_id dibuat server; harga hanya dari config/packages.ts.
- Produk: paket Permata, Kartu Warga Bulanan (bonus harian saat absen + bingkai profil), Traktir Kota (unit "Bata") + Tugu Pendukung di Alun-Alun + papan Pendukung Kota.
- Halaman bayar: QR, nominal, hitung mundur, polling status tiap 3 detik, animasi sukses.
- Webhook /api/webhooks/pakasir: verifikasi, SELALU cek ulang status ke API, idempoten per order_id, fulfillPayment.
- Cron: kedaluwarsakan pembayaran pending > 30 menit.
- Unit test: webhook ganda tidak menggandakan Permata, jumlah dari klien diabaikan.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 8 — Dashboard mitra UMKM

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 8:
- Peran partner: admin mengundang mitra lewat email, mitra login dan masuk /mitra.
- Edit Kartu Toko (logo, foto maks 6 ke Supabase Storage dengan kompresi di klien, deskripsi, link tervalidasi, promo, voucher, jam buka) → disimpan sebagai shop_revisions menunggu moderasi admin.
- Statistik: tayangan, klik per tombol, pekerja shift, partisipasi misi sponsor; grafik 7/30/90 hari; perbandingan periode sebelumnya.
- Perpanjang paket via QRIS (product_type shop_package) → active_until bertambah otomatis.
- Mitra paket utama/landmark bisa membuat misi sponsor & dekorasi sponsor (juga dimoderasi).
- Ekspor laporan bulanan sebagai gambar PNG yang rapi (untuk dikirim ke WA).
- RLS: mitra hanya bisa melihat data tokonya. Test untuk ini.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 9 — Admin lengkap & live ops

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 9:
- Panel /admin lengkap: CRUD distrik, kavling (klik di peta untuk menaruh), fasilitas, toko, mitra, item, misi, misi sponsor, kalender kejutan, pengumuman, tantangan kota bersama, app_settings (saklar fitur).
- Moderasi revisi toko, laporan konten, verifikasi klaim warga (dengan pencarian kode KW-XXXX).
- Penyesuaian dompet manual wajib alasan; semua aksi admin → audit_log, halaman untuk melihatnya.
- Tantangan kota bersama: progress bar di Alun-Alun, progres dihitung dari aksi pemain.
- Event musiman: mata uang event sementara + toko event + item terbatas waktu.
- Cron 19.00 WIB: buka kalender & buka distrik baru bila ambang warga tercapai (dengan animasi pengumuman).
- Dasbor statistik: warga baru/hari, DAU, retensi D1/D7, klik toko, pendapatan per produk.
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT FASE 10 — Pertumbuhan & polish

```
Lanjutkan proyek. Baca AGENTS.md, docs/KONSEP-LENGKAP.md, dan docs/PROGRESS.md.

Kerjakan FASE 10:
- PWA: manifest, ikon, service worker (cache aset statis), tombol "Pasang di layar utama" (beforeinstallprompt + petunjuk iOS).
- Kartu share IG Story: GET /api/share/house/[username] (next/og 1080x1920 & 1200x630), tombol bagikan (Web Share API), hadiah share 1x/hari.
- Referral: link undangan, hadiah saat teman mencapai level 5, batas per bulan.
- Tutorial pertama kali 4 langkah dipandu maskot "Pak Lurah".
- Mode gelap penuh, prefers-reduced-motion, aksesibilitas tombol ikon.
- Halaman Kebijakan Privasi & Syarat Layanan (Bahasa Indonesia, sesuai KONSEP bagian 12), fitur hapus akun.
- Optimasi: Lighthouse mobile ≥ 85, ukuran bundle peta dipisah, gambar dioptimalkan, SEO halaman toko (sitemap.xml, robots.txt).
Perbarui docs/PROGRESS.md. Semua cek harus lulus.
```

---

## PROMPT PERBAIKAN (pakai kapan saja bila ada error)

```
Ada masalah: [jelaskan apa yang kamu lakukan, apa yang terjadi, dan tempel pesan error lengkapnya].
Baca AGENTS.md dan docs/PROGRESS.md. Cari penyebab utamanya (jangan hanya menutupi gejala), perbaiki, tambahkan test yang mencegah masalah ini terulang, lalu jelaskan penyebab dan perbaikannya dengan bahasa sederhana.
```

## PROMPT CEK KUALITAS (setelah tiap 2–3 fase)

```
Lakukan audit menyeluruh pada repo ini terhadap AGENTS.md: keamanan (RLS, service role bocor ke klien, validasi input, idempotensi ekonomi), celah curang ekonomi, bug zona waktu WIB, performa peta di HP, dan teks UI yang bukan Bahasa Indonesia. Buat daftar temuan berurut dari paling parah, lalu perbaiki semuanya dan tambahkan test. Perbarui docs/PROGRESS.md.
```

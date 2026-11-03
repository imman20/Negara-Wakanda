# KONSEP LENGKAP — "Kota Warga" (nama sementara)

> Kota virtual berbasis web tempat setiap pengikut media sosial mendapat satu rumah, dan setiap UMKM/brand bisa membuka toko yang benar-benar mengarah ke usahanya (IG, TikTok, WhatsApp, website, marketplace).
> Nama "Kota Warga" hanya placeholder. Semua nama, mata uang, dan teks diatur dari satu file konfigurasi agar mudah diganti.

---

## 1. Ringkasan satu paragraf

Pengunjung datang dari konten Instagram/TikTok, menemukan rumahnya di peta kota (dibuat dari komentar/follow), lalu **bermain setiap hari**: absen, kerja di toko UMKM, panen, upgrade rumah, mini game, dan bersaing di papan peringkat. Setiap aktivitas itu sengaja membuat pemain **melewati dan mengunjungi toko sponsor**. Pendapatan datang dari **sewa kavling toko**, **paket promosi**, **konten kolaborasi**, **iklan berhadiah**, **pembelian Permata (QRIS Pakasir)**, dan **traktiran pendukung**.

---

## 2. Tujuan & target

| Target | Ukuran keberhasilan |
|---|---|
| Bulan 1 | 1.000 warga terdaftar, 150 pemain aktif harian (DAU), 5 toko UMKM pertama (boleh gratis/promo) |
| Bulan 3 | 5.000 warga, 600 DAU, 20 toko berbayar, pendapatan menutup biaya server + konten |
| Bulan 6 | 15.000 warga, 2.000 DAU, 50+ toko, 2–4 kolaborasi brand per bulan |

Metrik utama yang dipantau: DAU, retensi D1/D7/D30, rata-rata sesi per hari, klik keluar ke toko (outbound click), konversi pembelian Permata, pendapatan per 1.000 pemain.

**Target pasar**
- **Pemain**: remaja–dewasa muda (15–30 tahun) pengguna IG/TikTok, main dari HP.
- **Pembeli iklan**: UMKM lokal (kuliner, fashion, jasa, toko online), kreator, komunitas, sekolah/kampus, event lokal.

---

## 3. Struktur dunia

### 3.1 Peta
- Peta **isometrik 2.5D** (seperti game kota), bisa digeser, dicubit/zoom, dan diketuk.
- Dibagi menjadi **Distrik**. Setiap distrik punya tema, warna, dan kapasitas kavling.
- Distrik baru **dibuka bertahap** ketika jumlah warga mencapai ambang tertentu (memberi rasa "kota tumbuh").

| Distrik | Fungsi | Dibuka |
|---|---|---|
| Alun-Alun (pusat) | Balai kota, papan pengumuman, tugu pendukung, panggung event | Sejak awal |
| Kampung Awal | Rumah warga pertama | Sejak awal |
| Jalan Niaga | Toko UMKM premium (kavling paling mahal, paling ramai dilewati) | Sejak awal |
| Pasar Rakyat | Toko UMKM murah, lapak kecil | 500 warga |
| Kampung Baru 1..N | Rumah warga tambahan | Setiap +1.000 warga |
| Taman Bermain | Gedung mini game, arena kompetisi | 1.000 warga |
| Sawah & Kebun | Panen (aktivitas harian) | 1.500 warga |
| Pantai / Wisata | Event musiman, area brand besar | 3.000 warga |

### 3.2 Jenis bangunan
1. **Rumah warga** — milik pemain. Level 1–6 (Gubuk → Rumah Kayu → Rumah Bata → Rumah Tingkat → Rumah Mewah → Villa). Bisa dihias.
2. **Toko UMKM/brand** — disewa pengiklan. Saat diketuk muncul **Kartu Toko**: logo, nama, deskripsi, foto produk (maks 6), promo aktif, kode voucher, jam buka, dan tombol: Instagram, TikTok, WhatsApp, Website, Shopee/Tokopedia, Google Maps.
3. **Fasilitas umum** — milik kota (Balai Kota, Pos Ronda, Halte, Masjid/Gereja/Pura netral sebagai dekorasi umum, Rumah Sakit, Sekolah). Bisa "diadopsi" pendukung (nama mereka tampil sebagai "Bos").
4. **Gedung kegiatan** — Lumbung (panen), Gedung Game, Bursa Kerja, Bank Kota (riwayat koin), Kantor Pos (hadiah dari tetangga).
5. **Bangunan event** — sementara, terkait kampanye brand atau musim (Ramadan, 17 Agustus, Tahun Baru).

### 3.3 Waktu & suasana
- Siklus siang/malam mengikuti jam WIB pemain (pagi, siang, sore, malam; lampu rumah menyala malam hari).
- Opsional: cuaca harian (cerah/hujan) yang diumumkan "pembawa berita" kota.
- **Update kota setiap hari pukul 19.00 WIB**: warga baru masuk, bangunan baru, kejutan dibuka. Waktu ini diselaraskan dengan unggahan konten IG/TikTok.

---

## 4. Alur pemain (player journey)

### 4.1 Pengunjung baru (belum login)
1. Datang dari link bio IG/TikTok.
2. Langsung melihat peta (tanpa harus login), ada kolom **"Cari rumahmu"** (username IG).
3. Bisa jelajah, mengetuk toko, melihat papan peringkat.
4. Ajakan jelas: **"Komen di postingan terbaru @akunmu untuk dapat rumah"** atau **"Klaim rumahmu"**.

### 4.2 Mendapatkan rumah
Dua jalur, keduanya didukung:
- **Jalur A — dari komentar IG (seperti konsep asli)**: admin mengimpor daftar username dari komentar (manual/CSV di tahap awal; otomatis via Instagram Graph API di tahap lanjut). Setiap username baru mendapat rumah berstatus **"belum diklaim"**.
- **Jalur B — daftar langsung di web**: login (Google atau email magic link), isi username IG, langsung dapat rumah.

### 4.3 Klaim & verifikasi kepemilikan username IG
Supaya orang tidak mengaku-ngaku username orang lain:
1. Pemain login, mengetik username IG-nya.
2. Sistem memberi **kode verifikasi** 6 karakter (misalnya `KW-7F2Q`).
3. Pemain menulis kode itu sebagai komentar di postingan yang ditentukan, atau DM ke akun resmi.
4. Admin menandai "terverifikasi" di panel admin (tahap awal), atau sistem mencocokkan otomatis lewat Graph API (tahap lanjut).
5. Sebelum terverifikasi pemain tetap bisa main, tetapi **tidak bisa tampil di papan peringkat publik dan tidak bisa membeli Permata** (mencegah penyalahgunaan nama).

### 4.4 Sesi harian tipikal (target 5–10 menit)
1. Buka app → animasi singkat "Selamat pagi, Warga!" → **hadiah absen**.
2. Ambil **gaji kerja** dari toko tempat bekerja → lamar shift berikutnya.
3. **Panen** di sawah → tanam lagi.
4. Kerjakan **3 misi harian** (contoh: "kunjungi 2 toko di Jalan Niaga", "beri like 3 rumah tetangga", "main 1 mini game").
5. Cek **kalender kejutan** & papan peringkat.
6. Opsional: tonton iklan untuk bonus, belanja dekorasi, upgrade rumah.
7. Bagikan rumah ke IG Story (dapat koin bonus 1x sehari).

---

## 5. Sistem permainan (retensi)

### 5.1 Absen harian berantai (streak)
- Hari 1–7 hadiah naik: 10, 15, 20, 25, 30, 40, **100 koin + 1 kotak dekorasi** di hari ke-7.
- Siklus berulang tiap 7 hari; setiap siklus ke-4 (hari 28) hadiah spesial (dekorasi langka).
- Jika bolong 1 hari: streak bisa **diselamatkan** dengan 1 iklan berhadiah atau 5 Permata (maks 1x per minggu).

### 5.2 Kerja di toko (inti integrasi dengan UMKM)
- Gedung **Bursa Kerja** menampilkan lowongan dari toko-toko UMKM.
- Pemain memilih shift: **1 jam (15 koin)**, **4 jam (50 koin)**, **8 jam (90 koin)**. Nilai final diatur di config.
- Untuk melamar, pemain **harus membuka Kartu Toko** dulu (melihat promo) → jaminan eksposur untuk pengiklan.
- Setelah shift selesai, pemain kembali untuk **ambil gaji** (alasan kembali).
- Toko berbayar paket lebih tinggi mendapat **posisi lowongan teratas** dan "bonus gaji +10%" sehingga lebih banyak dipilih.
- Pemain punya **rekam jejak kerja** ("Sudah 30 shift di Kopi Senja") → lencana loyalitas yang bisa dipakai brand untuk hadiah nyata (voucher) — opsional, diatur pengiklan.

### 5.3 Panen (sawah/kebun)
- 4 petak awal, bisa dibuka sampai 12 petak dengan koin.
- Tanaman: Padi (30 menit), Jagung (2 jam), Cabai (6 jam), Durian (24 jam). Hasil dijual ke Lumbung untuk koin.
- Bonus panen ×2 dengan iklan berhadiah.

### 5.4 Misi
- **Harian** (3 misi, reset 00.00 WIB) — hadiah koin + XP.
- **Mingguan** (5 misi) — hadiah lebih besar + kotak dekorasi.
- **Misi sponsor** — dibuat oleh pengiklan dari dashboard (contoh: "Kunjungi Kartu Toko Bakso Mas Joko dan jawab: apa menu barunya?"). Ini produk iklan yang dijual.

### 5.5 Level warga & XP
- XP dari semua aktivitas. Level 1–50.
- Gelar: Warga Baru (1), Warga Tetap (5), Tetangga Baik (10), Juragan (20), Tokoh Kampung (30), Sesepuh (40), Legenda Kota (50).
- Level membuka: petak sawah, slot dekorasi, model rumah, akses mini game tertentu.

### 5.6 Upgrade & dekorasi rumah
- Upgrade level rumah memakai koin (contoh: L2 500, L3 1.500, L4 4.000, L5 10.000, L6 25.000) **+ syarat level warga** agar tidak bisa diborong instan.
- Dekorasi: pagar, taman, kolam, pohon, lampu, kendaraan, hewan peliharaan, papan nama.
- Kelangkaan dekorasi: Biasa / Jarang / Epik / Legenda. Legenda hanya dari event atau koleksi.
- **Dekorasi sponsor**: misal "Gerobak Kopi Senja" — gratis untuk pemain, tampil dengan logo brand di rumah pemain (iklan yang tersebar ke seluruh kota).

### 5.7 Koleksi
- **Album Stiker Warga**: stiker didapat dari misi/kotak; set lengkap memberi hadiah.
- **Lencana prestasi**: "100 hari absen", "Pekerja Teladan", "Juara Kuis", dll. Tampil di profil.

### 5.8 Sosial
- **Kunjungi rumah** tetangga, beri **Like** (maks 20/hari), kirim **hadiah kecil** (1x/hari ke tiap tetangga) lewat Kantor Pos.
- **Profil publik** warga: rumah, level, lencana, toko tempat kerja favorit.
- **Bagikan ke IG Story**: server membuat gambar kartu rumah (OG image 1080×1920) + link ke rumah pemain.
- **Undang teman**: link referral; pengundang dapat 100 koin saat teman mencapai level 5 (bukan saat daftar, untuk cegah akun palsu).
- **Tantangan kota bersama**: target kolektif mingguan, contoh "Seluruh warga panen 20.000 kali → Pantai dibuka". Progress bar tampil di Alun-Alun.

### 5.9 Mini game (di Gedung Game)
Setiap game: sesi singkat 30–90 detik, hadiah koin dengan batas harian, papan peringkat mingguan sendiri.
1. **Kuis Kota** — pertanyaan pilihan ganda (umum + pertanyaan sponsor).
2. **Hitung Cepat** — matematika sederhana berbatas waktu.
3. **Tangkap Barang** — tangkap barang jatuh (bisa diganti barang sponsor saat kampanye).
4. **Tebak Gambar** — tebak bangunan/produk dari potongan gambar.
5. (Tahap lanjut) **Balapan Motor** sederhana di jalan kota.

Anti-curang: skor dikirim dengan durasi & jejak input sederhana, server menolak skor mustahil, batas hadiah harian.

### 5.10 Papan peringkat
- Mingguan (reset Senin 00.00 WIB) dan Sepanjang Masa.
- Kategori: Rumah Terpopuler (like), Terkaya (koin terkumpul minggu ini, bukan saldo), Pekerja Teladan (shift), Juara tiap mini game, Pendukung Kota (traktiran).
- Top 3 mingguan mendapat **piala dekorasi** + disebut di konten IG mingguan (motivasi kuat).

### 5.11 Kalender kejutan
- Tampilan 7 hari; hari yang belum tiba **terkunci** dengan teks teaser.
- Isi diatur admin: bangunan baru, mini game baru, event, kolaborasi brand.
- Pemain diajak menebak di kolom komentar konten IG → mendongkrak engagement akun.

### 5.12 Event musiman
- Ramadan (ngabuburit, dekorasi lampu), 17 Agustus (lomba), Tahun Baru, Hari Pahlawan, musim liburan sekolah.
- Mata uang event sementara + toko event + dekorasi edisi terbatas.

---

## 6. Ekonomi game

### 6.1 Mata uang
| Nama | Jenis | Didapat dari | Dipakai untuk | Bisa dicairkan? |
|---|---|---|---|---|
| **Koin** | Gratis (soft) | Absen, kerja, panen, misi, mini game, iklan, like | Upgrade rumah, dekorasi biasa, petak sawah | **Tidak** |
| **Permata** | Premium | Pembelian QRIS, hadiah langka event, top papan peringkat | Dekorasi eksklusif, skin rumah, percepat waktu, selamatkan streak, nama di Tugu Pendukung | **Tidak** |
| **XP** | Progres | Semua aktivitas | Naik level | — |

**Aturan mutlak**: tidak ada penukaran Koin/Permata ke rupiah, pulsa, atau saldo e-wallet. Aliran uang **satu arah**: uang nyata → game. Hadiah fisik/voucher hanya diberikan langsung oleh sponsor sebagai bagian kampanye, dengan syarat jelas, bukan ditukar dari mata uang.

### 6.2 Keseimbangan (nilai awal, diatur di config)
- Pemain aktif rata-rata mendapat ±300–500 koin/hari tanpa iklan, ±150 tambahan dengan iklan.
- Upgrade rumah L2 tercapai hari ke-2, L3 sekitar minggu ke-1, L4 minggu ke-3, L5 bulan ke-2, L6 bulan ke-4 → tujuan jangka panjang.
- **Penyerap koin** (coin sinks) agar inflasi terkendali: upgrade, dekorasi, petak sawah, biaya ganti gaya rumah, tiket mini game turnamen.
- Semua transaksi tercatat di **buku besar (ledger)** append-only; saldo = hasil penjumlahan ledger (dicache).

### 6.3 Paket Permata (contoh harga)
| Paket | Harga | Permata | Bonus |
|---|---|---|---|
| Kantong | Rp5.000 | 50 | — |
| Dompet | Rp15.000 | 160 | +7% |
| Peti | Rp50.000 | 560 | +12% |
| Brankas | Rp100.000 | 1.200 | +20% |
| **Kartu Warga Bulanan** | Rp25.000/30 hari | 30 Permata langsung + 10 Permata/hari saat absen + bingkai profil eksklusif | Nilai terbaik, pendapatan rutin |

### 6.4 Traktiran pendukung (tip)
- Tombol **"Traktir Kota"** (mis. Rp5.000/unit "Bata"). Nama pendukung masuk **Tugu Pendukung** & papan peringkat pendukung, plus 1 Bata = bonus Permata kecil.
- Pembayaran lewat QRIS Pakasir; username otomatis tercatat (karena pemain sudah login), tidak perlu menulis username manual.

---

## 7. Iklan berhadiah (rewarded ads)

### 7.1 Penempatan
| Tempat | Hadiah |
|---|---|
| Tombol "Bonus Harian" di Bank Kota | +20 koin |
| Saat ambil gaji | Gaji ×1,5 |
| Saat panen | Panen ×2 |
| Shift sedang berjalan | Selesaikan shift instan |
| Streak bolong | Selamatkan streak (maks 1x/minggu) |
| Kalah di mini game | Coba lagi 1x |

### 7.2 Aturan
- **Opsional selalu** — tidak ada iklan paksa di tengah permainan, tidak ada iklan pop-up yang menutup layar.
- Batas: **8 iklan berhadiah per hari per akun** (diatur config). Jeda minimal 60 detik antar iklan.
- Hadiah hanya diberikan setelah jaringan iklan mengirim event "reward granted" **dan** server memvalidasi token sesi iklan (sekali pakai, kedaluwarsa 5 menit).
- Akun belum login tidak bisa menonton iklan berhadiah.
- Banner iklan statis biasa (non-berhadiah) hanya di area non-game (halaman info), bukan di peta.

### 7.3 Jaringan iklan
- Tahap awal: **penyedia iklan simulasi** (video placeholder 15 detik) agar seluruh alur bisa dites.
- Produksi: **Google Ad Manager** format *rewarded* untuk web (butuh pendaftaran & persetujuan). Alternatif: jaringan lain yang mendukung rewarded web. Cek kebijakan terbaru penyedia sebelum aktif — AdSense biasa melarang membujuk orang menonton/mengklik iklan.
- Kode dibuat dengan antarmuka `AdProvider` sehingga penyedia bisa diganti tanpa mengubah logika game.

### 7.4 Ekspektasi pendapatan
Pendapatan per tayangan untuk trafik Indonesia **kecil (kira-kira puluhan rupiah, sangat bervariasi)**. Iklan berhadiah adalah **tambahan**, bukan sumber utama. Sumber utama: sewa toko UMKM, kolaborasi brand, dan Permata.

---

## 8. Produk iklan untuk UMKM & brand (sumber utama pendapatan)

### 8.1 Paket toko
| Paket | Harga/bulan (contoh) | Isi |
|---|---|---|
| **Lapak** (Pasar Rakyat) | Rp25.000 | Kavling kecil, Kartu Toko (logo, deskripsi, 2 foto, 3 link) |
| **Toko** (Pasar/Kampung) | Rp75.000 | Bangunan sedang, 6 foto, semua link, 1 promo aktif, lowongan kerja standar |
| **Toko Utama** (Jalan Niaga) | Rp200.000 | Bangunan besar khas brand, lowongan teratas + bonus gaji, 1 misi sponsor/minggu, statistik lengkap |
| **Landmark** (Alun-Alun/Pantai) | Rp500.000+ | Bangunan ikonik custom, dekorasi sponsor untuk pemain, mini game bertema brand, disebut di konten IG |

Diskon untuk 3/6/12 bulan. Toko pertama di setiap kategori bisa digratiskan 1 bulan sebagai etalase.

### 8.2 Produk tambahan
- **Sorotan Alun-Alun** (banner papan reklame di peta): Rp50.000/minggu.
- **Misi sponsor**: Rp100.000/misi/minggu.
- **Dekorasi sponsor** (pemain memasang logo brand di rumahnya): Rp150.000/kampanye.
- **Konten kolaborasi** (video IG/TikTok "toko baru buka di kota" + bangunan): Rp150.000–500.000 per konten.
- **Event brand** (kuis berhadiah dari brand, turnamen mini game): harga paket.

### 8.3 Dashboard pengiklan
Setiap pemilik toko mendapat akses dashboard:
- Edit Kartu Toko (logo, foto, deskripsi, link, promo, kode voucher, jam buka).
- **Statistik**: tayangan Kartu Toko, klik per tombol (IG/WA/website/marketplace), jumlah pekerja shift, partisipasi misi sponsor — harian/mingguan/bulanan, grafik.
- Status paket, tanggal berakhir, tombol perpanjang (bayar QRIS).
- Semua perubahan konten toko **dimoderasi admin** sebelum tampil.

### 8.4 Cara menjual ke UMKM
1. Kumpulkan warga dulu (bukti penonton).
2. Isi 5–10 toko awal gratis/murah sebagai contoh.
3. Kirim **laporan statistik** bulanan ke pengiklan (ekspor PDF/gambar dari dashboard) — bukti nyata = mudah perpanjang.
4. Halaman publik **"Pasang Tokomu"** berisi paket, contoh toko, statistik kota (warga, DAU), dan tombol daftar/DM.

---

## 9. Monetisasi lengkap & perkiraan

| Sumber | Asumsi bulan ke-6 | Perkiraan/bulan |
|---|---|---|
| Sewa toko | 30 toko × rata-rata Rp80.000 | Rp2,4 juta |
| Produk tambahan & misi sponsor | 8 transaksi × Rp100.000 | Rp0,8 juta |
| Kolaborasi konten | 3 × Rp300.000 | Rp0,9 juta |
| Permata & Kartu Warga | 2.000 DAU × 3% pembeli × Rp25.000 | Rp1,5 juta |
| Traktiran | — | Rp0,3–1 juta |
| Iklan berhadiah | 2.000 DAU × 3 iklan/hari × 30 hari × ~Rp30 | ~Rp0,5 juta |
| **Total** | | **± Rp6–7 juta** |

Angka hanya ilustrasi untuk perencanaan; hasil nyata bergantung pada pertumbuhan pengikut dan kemampuan menjual ke UMKM.

**Biaya**: Vercel (gratis → Pro bila ramai), Supabase (gratis → Pro ±US$25/bulan bila ramai), domain (±Rp150.000/tahun), biaya QRIS per transaksi, waktu produksi konten.

---

## 10. Strategi konten & pertumbuhan

- **Konten harian 19.00 WIB** (Reels/TikTok 15–30 detik): "Warga baru hari ini", "Toko baru buka", "Rumah terbagus minggu ini", "Kejutan besok…".
- **Seri mingguan**: Senin papan peringkat, Rabu tur toko UMKM, Jumat kuis, Minggu rekap kota.
- **Karakter maskot** (mis. "Pak Lurah" dan robot kota) sebagai narator konten dan pemandu tutorial.
- **Ajakan komentar** di setiap konten: "Tebak kejutan besok", "Komen untuk dapat rumah".
- Peringatan resmi soal **akun palsu** tampil di web (seperti situs asli) dan di bio.
- Kolaborasi dengan kreator lokal: rumah spesial kreator, kreator mengajak pengikutnya pindah ke kota.

---

## 11. Admin (Pak Lurah)

Panel admin berisi:
1. **Warga**: impor username (tempel daftar / CSV), lihat, verifikasi klaim, blokir, reset.
2. **Peta**: tambah distrik, kavling, fasilitas umum; atur posisi bangunan (klik di peta).
3. **Toko & pengiklan**: buat toko, tetapkan paket & masa aktif, moderasi perubahan, lihat statistik.
4. **Kalender kejutan & pengumuman**: jadwalkan isi tiap hari, teaser, waktu buka.
5. **Ekonomi**: ubah nilai hadiah/harga di config, lihat total koin beredar, beri/kurangi koin manual (tercatat di ledger dengan alasan).
6. **Pembayaran**: daftar transaksi QRIS, status, rekonsiliasi.
7. **Laporan & moderasi**: laporan nama/konten tidak pantas, log audit semua aksi admin.
8. **Statistik kota**: DAU, warga baru, retensi, klik toko, pendapatan.

---

## 12. Keamanan, anti-curang, dan aturan

- **Semua perubahan mata uang hanya di server** (fungsi database/API dengan service role). Klien tidak pernah bisa menulis saldo.
- Ledger append-only + saldo cache; rekonsiliasi berkala.
- **Rate limit** semua endpoint aksi (absen, like, klaim gaji, skor game, iklan).
- Waktu shift/panen dihitung dari **jam server**, bukan jam HP.
- Deteksi multi-akun sederhana: batas referral, hadiah referral ditunda sampai level 5, tandai IP/perangkat mencurigakan untuk ditinjau admin (tanpa memblokir otomatis).
- Filter kata kasar untuk nama rumah/papan nama; semua teks bebas dari pengguna bisa dilaporkan.
- **Ramah anak**: tanpa chat bebas antar-pemain (hanya like & hadiah), tanpa gacha berbayar langsung (kotak dekorasi berbayar menampilkan peluang secara jelas, atau lebih aman: jual dekorasi langsung tanpa acak).
- **Privasi** (UU PDP): hanya simpan data yang perlu (email, username IG publik); halaman Kebijakan Privasi & Syarat Layanan; fitur hapus akun.
- **Legal**: cek kewajiban pendaftaran **PSE (Penyelenggara Sistem Elektronik)** di Komdigi bila menerima pembayaran; pajak penghasilan dari iklan/penjualan; perjanjian sederhana dengan pengiklan (larangan produk ilegal, judi, rokok, alkohol, pinjol ilegal).
- Tidak ada fitur yang menyerupai judi/undian berbayar.

---

## 13. Teknologi (ringkas — detail di AGENTS.md)

- **Next.js (App Router) + TypeScript + Tailwind CSS**, deploy di **Vercel**.
- **PixiJS v8 + pixi-viewport** untuk peta isometrik (geser, zoom, culling).
- **Supabase**: Postgres, Auth (Google + email magic link), Storage (logo/foto toko), Row Level Security, fungsi RPC untuk ekonomi.
- **PWA**: bisa dipasang di layar utama HP.
- **Pembayaran**: Pakasir QRIS lewat antarmuka `PaymentProvider` + webhook.
- **Iklan**: antarmuka `AdProvider` (Mock → Google Ad Manager rewarded).
- **Aset grafis**: tahap awal bentuk sederhana/sprite placeholder atau aset isometrik berlisensi CC0 (mis. Kenney); selanjutnya aset custom.

---

## 14. Peta jalan pengembangan

| Fase | Isi | Hasil |
|---|---|---|
| 0 | Fondasi proyek, config, desain dasar, deploy kosong | Situs tampil di Vercel |
| 1 | Peta isometrik, distrik, rumah warga, pencarian, Kartu Toko + link | **Bisa dipromosikan ke publik** |
| 2 | Login, klaim & verifikasi, profil, ledger koin, absen harian | Pemain punya akun & koin |
| 3 | Upgrade & dekorasi rumah, toko item, inventaris, like & kunjungi | Ada tujuan jangka panjang |
| 4 | Bursa Kerja, panen, misi harian/mingguan, level & XP | Lingkaran harian lengkap |
| 5 | Mini game (Kuis, Hitung Cepat, Tangkap Barang), papan peringkat | Kompetisi |
| 6 | Iklan berhadiah (Mock → GAM) | Pendapatan iklan |
| 7 | Pembayaran Pakasir: Permata, Kartu Warga, Traktiran, Tugu Pendukung | Pendapatan langsung |
| 8 | Dashboard pengiklan + statistik + paket toko + perpanjang via QRIS | Produk jualan ke UMKM |
| 9 | Panel admin lengkap, kalender kejutan, event, tantangan bersama | Operasional harian |
| 10 | PWA, share ke IG Story, referral, optimasi performa, SEO, analitik | Siap tumbuh |
| 11 | (Opsional) Integrasi Instagram Graph API untuk impor komentar otomatis | Otomatisasi |

# DinoRace — Testing Prompts

Dua prompt siap pakai. Tinggal copy-paste ke Claude Code kapan pun abis ubah kode dan mau dites sebelum di-deploy/push.

---

## 1. 🚀 Push Test (cepat, ~5 menit)

Pakai ini tiap kali abis edit kecil dan mau langsung deploy. Fokus: pastikan nggak ada yang patah.

```
Jalankan push test buat DinoRace (folder dinorace/index.html) sebelum aku deploy:

1. Cek sintaks JS valid (parse script block-nya, jangan langsung percaya "keliatan bener").
2. Serve index.html lokal (python http.server), buka di browser preview.
3. Cek console/network — pastikan nol error JS saat load awal.
4. Test alur inti secara live (bukan cuma baca kode):
   - Main Sendiri: klik → masuk game screen → jalan beberapa detik → distance naik → mati → game over screen muncul dengan hasil.
   - Buat Game (2 pemain default): dapat kode 6 karakter, QR muncul.
   - Buka tab kedua, join pakai kode itu → dua-duanya masuk ke layar pilih dino → countdown → game screen dengan 2 lane.
5. Screenshot hasil akhir tiap alur biar aku bisa lihat.
6. Bersihkan data test dari Firebase (dinorace_games/<kode-yang-dipakai>) sebelum selesai.
7. Laporin singkat: PASS/FAIL per alur, dan kalau FAIL sebutkan persis di mana + kenapa.

Jangan cuma bilang "sudah saya cek kodenya" — harus benar-benar dijalankan di browser.
```

---

## 2. 🔍 QA Test Lengkap (~20-30 menit)

Pakai ini abis nambah fitur besar, atau sesekali buat mastiin semua masih jalan end-to-end. Jalankan di 3 tab/device biar ketauan sinkronisasinya beneran jalan, bukan cuma di 1 device.

```
Jalankan QA test menyeluruh buat DinoRace (folder dinorace/index.html). Ini game
racing dino 2 pemain (Firebase Realtime Database buat sync), dengan mode solo dan
3-pemain juga. Serve lokal, buka di browser preview, dan benar-benar jalankan tiap
skenario di bawah (bukan cuma baca kode) — pakai beberapa tab buat simulasi
beberapa device kalau perlu.

### A. Home screen & pairing
- [ ] Home screen render tanpa error, 3 kartu kelihatan: Main Sendiri, Buat Game
      Baru (dengan pilihan 2/3 Pemain), Gabung Game.
- [ ] Buat Game → kode 6 karakter unik ter-generate, QR code muncul dan valid
      (scan/decode QR-nya, isinya harus link ?join=<kode> yang benar).
- [ ] Tombol Copy nyalin kode ke clipboard.
- [ ] Tombol Share ke WhatsApp navigasi ke wa.me dengan pesan + link yang benar
      (cek location.href setelah klik, jangan cuma asumsi).
- [ ] Masukin kode 6 karakter yang SALAH/nggak ada di Gabung Game → muncul pesan
      error yang jelas, bukan diam aja.
- [ ] Masukin kode kurang dari 6 karakter → validasi jalan.
- [ ] Buka index.html?join=<kode-valid> langsung → otomatis keisi & auto-join.

### B. Solo mode
- [ ] Main Sendiri → langsung countdown, TANPA butuh Firebase/koneksi internet
      (matiin network kalau perlu buat pastiin ini bener-bener lokal).
- [ ] Cuma 1 lane kelihatan (lane 2 & 3 hidden), HUD cuma nampilin "Kamu".
- [ ] Jeda antar obstacle terasa longgar (rata-rata ~1.4 detik di awal, ~0.55 detik
      di akhir) dan tetap bisa dihindari satu-satu.
- [ ] Obstacle muncul & bisa dihindari (jalanin manual: lompat pas kaktus/batu,
      JANGAN lompat pas burung).
- [ ] Nyawa (5 hati) berkurang tiap nabrak, dino kedip pas kebal sesaat.
- [ ] Mati di nyawa ke-0 → game over muncul dengan jarak akhir.
- [ ] Kalau jarak > best score sebelumnya → muncul pesan "Rekor pribadi baru".
- [ ] Main Lagi di solo → restart instan, TANPA Firebase call apa pun.
- [ ] Refresh browser → best score di localStorage tetap ke-load benar di home
      screen.

### C. Multiplayer 2 pemain (pakai 2 tab)
- [ ] Player 1 buat game (2 pemain, default) → dapat kode.
- [ ] Player 2 join pakai kode → dua-duanya otomatis pindah ke layar pilih dino
      (SELECT_SECONDS detik hitung mundur di kedua tab, sinkron).
- [ ] Tiap pemain pilih dino berbeda → pilihan lawan kelihatan live di layar
      satunya (cek field "P1: <emoji>" update real-time).
- [ ] Countdown 3-2-1 jalan otomatis di kedua tab bareng-bareng.
- [ ] Game screen: 2 lane warna beda, dino sesuai yang dipilih, dino ngadep
      kanan (arah lari) dari awal, TIDAK bisa balik badan pas gerak kiri.
- [ ] Lompat: puncak naik ~94px dari tanah (+4px lantai = `bottom` ~98px) dan
      bawa dino maju ~59px (bukan cuma naik-turun di tempat). Di mode 3 pemain
      kepala dino nggak kepotong atas lane pas di puncak.
- [ ] Obstacle sinkron identik di kedua tab (spawn di waktu simulasi yang sama
      persis — verifikasi lewat cek elapsed time saat masing-masing mati kalau
      keduanya AFK, harusnya mati di jarak yang sama).
- [ ] Satu pemain mati duluan → muncul banner "game over, nunggu lawan" + tanda
      [DIED] merah di lane-nya, TAPI game TIDAK berhenti — lawan tetap lanjut
      lari sampai dia juga mati.
- [ ] Kedua pemain mati → game over muncul di KEDUA tab dengan hasil identik
      (jarak sama persis di kedua layar) dan pemenang yang benar (jarak
      terjauh).
- [ ] Kalau jarak sama PERSIS → muncul "Seri!" bukan salah satu menang.
- [ ] Main Lagi (1 klik dari salah satu pemain) → ronde baru otomatis mulai di
      KEDUA tab tanpa perlu share kode ulang, nyawa balik ke 5 penuh, obstacle
      di-acak ulang (beda dari ronde sebelumnya).
- [ ] Salah satu pemain klik "Menu Utama" → keluar bersih, kalau dia player1
      (host) dan game belum selesai, data game kehapus dari Firebase.

### D. Multiplayer 3 pemain (pakai 3 tab)
- [ ] Player 1 buat game, pilih "3 Pemain" SEBELUM klik Buat Game.
- [ ] Wait screen host nampilin "Menunggu 2 pemain lagi... (1/3)".
- [ ] Player 2 join → wait screen ke-2 tab update jadi "(2/3)", dua-duanya
      masih di wait screen (BUKAN langsung ke game).
- [ ] Player 3 join → room penuh, SEMUA (3 tab) otomatis pindah ke layar
      pilih dino barengan.
- [ ] Game screen: 3 lane (biru/hijau/ungu), 3 kotak HUD, 3 dino berbeda.
- [ ] Tiap pemain bisa mati & lanjut nonton independen — game baru selesai
      kalau SEMUA 3 pemain sudah mati.
- [ ] Game over nampilin 3 hasil (bukan 2), pemenang = jarak tertinggi dari
      ke-3nya, hasil identik di 3 tab.
- [ ] Setelah selesai, balik ke Buat Game dengan pilihan 2 pemain lagi (default)
      → pastikan lane 3/HUD 3/result 3 kembali hidden (regresi check, mode 2P
      harus tetap sama seperti sebelum ada fitur 3P).

### E. Tema & responsive
- [ ] Toggle tema (ikon 🎨 di kanan atas) → ganti Colorful ↔ Light, tersimpan
      di localStorage, konsisten dipakai lagi setelah refresh.
- [ ] Resize ke ukuran mobile (375x812) → semua layar tetap kebaca dan kepencet
      dengan baik, nggak ada elemen kepotong/overflow horizontal.
- [ ] Tombol kontrol (◀ ▲JUMP ▶) nggak muncul highlight seleksi biru pas ditekan
      di WebKit/Safari-like context.

### F. Ketahanan / edge case
- [ ] Coba join game yang statusnya udah "playing" atau "finished" → ditolak
      dengan pesan yang jelas, bukan nyangkut diam.
- [ ] Coba join kode yang sama sampai penuh (2P), device ke-3 coba join →
      ditolak "Game sudah penuh".
- [ ] Tutup tab salah satu pemain di tengah game (tanpa klik Menu Utama) →
      pastikan `onDisconnect` di Firebase nurunin flag "connected" pemain itu
      (cek langsung datanya di Firebase, bukan cuma UI).
- [ ] Cek nggak ada leftover data game test di Firebase Realtime Database
      setelah semua test selesai (hapus manual kalau ada yang nyangkut).

### Laporan akhir
Untuk tiap section (A-F), laporin:
- Checklist mana yang PASS
- Checklist mana yang FAIL, dengan detail: skenario persis, apa yang
  diharapkan vs apa yang kejadian, dan dugaan lokasi bug di index.html
  (nama fungsi/baris kalau bisa)
- Screenshot buat tiap game-over screen (solo, 2P, 3P) sebagai bukti visual

Jangan tandai PASS kalau cuma "kelihatannya bakal jalan dari baca kode" —
harus beneran dijalankan dan diverifikasi lewat browser.
```

---

### Catatan soal environment testing

Browser preview di Claude Code kadang nge-throttle tab yang lagi nggak
"fronted" (nggak keliatan aktif) — timer/animasi bisa nge-lag atau macet kalau
tab dibiarkan background lama. Ini limitasi tooling, BUKAN bug DinoRace — di
HP asli tiap device selalu foreground di layarnya sendiri jadi ini nggak akan
kejadian. Kalau nemu device/tab yang "macet" pas testing multi-tab, coba front-in
tab itu sebentar biar animasinya lanjut, baru lanjutin cek.

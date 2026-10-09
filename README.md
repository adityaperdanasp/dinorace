# 🦕 DinoRace

Game balapan dinosaurus **2 pemain real-time** — pairing pakai kode 6 karakter, main bareng dari 2 HP berbeda lewat Firebase.

**🎮 Main sekarang: [dinorace.lol](https://dinorace.lol)**

---

## ✨ Fitur

- **Pairing kode 6 karakter** — Player 1 buat game & dapat kode, Player 2 masukin kode buat gabung (mirip moodsync.mom)
- **Real-time multiplayer** — posisi, jarak, & status ke-sync antar gadget via Firebase Realtime Database
- **Pilih dino** — 5 karakter (🦕 🦖 🐉 🐊 🦎), 10 detik buat milih sebelum balapan
- **Sistem 5 nyawa** — tiap dino punya 5 hati ❤️, tiap nabrak hilang 1 + kebal sesaat (kedip), mati setelah 5x
- **Obstacle** — kaktus 🌵, burung 🐦, batu 🪨 dengan aturan beda (burung: jangan lompat, darat: lompat)
- **Difficulty naik** — obstacle makin sering & cepat seiring waktu
- **Survivor mode** — pemain yang mati bisa nonton (tanda `[DIED]`), yang hidup lanjut sampai mati juga
- **Menang** — jarak terjauh / survive paling lama
- **Rematch 1 klik** — main lagi tanpa share kode ulang, obstacle diacak ulang
- **2 tema** — Colorful (comical, buat anak-anak) & Light (pastel), bisa di-toggle 🎨
- **Leaderboard** — best distance personal tersimpan di localStorage
- **Share ke WhatsApp** — kirim link + kode langsung ke WA
- **Responsive & PWA** — mobile-friendly, bisa "Add to Home Screen" dengan icon dino

## 🕹️ Cara Main

1. Buka [dinorace.lol](https://dinorace.lol) di HP kamu → **Buat Game** → dapat kode 6 karakter
2. Share kode/link ke temanmu (ada tombol **Share ke WhatsApp**)
3. Teman buka link yang sama → masukin kode di **Gabung Game**
4. Kedua pemain pilih dino (10 detik) → countdown 3-2-1 → balapan!
5. **Kontrol:** tombol ◀ ▶ buat gerak, **▲ JUMP** buat lompat (atau keyboard: panah / WASD / spasi)

## 🛠️ Tech Stack

- HTML + CSS + JavaScript murni (single file `index.html`, no build step)
- [Firebase Realtime Database](https://firebase.google.com/docs/database) via CDN
- Obstacle di-generate lokal dari *shared seed* (deterministic PRNG) — sinkron tanpa bergantung salah satu device tetap aktif

## 🎛️ Tuning Gameplay

Angka-angka feel game ada di blok `constants` di `index.html`. Update terakhir (Okt 2026):

| Konstanta | Nilai | Efek |
|-----------|-------|------|
| `JUMP_HEIGHT` | `94` px (sebelumnya 78) | Tinggi puncak lompatan, +20% |
| `JUMP_FORWARD_PX` | `59` px (sebelumnya 54) | Dorongan maju selama lompat, +10% (fixed px, nggak ikut lebar layar) |
| `JUMP_DURATION` | `520` ms | Lama lompat naik+turun (nggak diubah) |
| `BASE_SPAWN_MS` | `1550` ms (sebelumnya 1350) | Jeda antar obstacle di awal game, +~15% |
| `MIN_SPAWN_MS` | `550` ms (sebelumnya 480) | Jeda minimum antar obstacle saat paling sulit, +~15% |

Catatan: mode 3 pemain tinggi lane-nya dinaikin (108→124px, layar kecil 96→112px) supaya puncak lompatan 94px nggak kepotong. Hit-test burung/darat (`jumpY > 20` / `< 22`) nggak diubah.

## 🚀 Setup Sendiri

1. Clone repo ini
2. Buat project di [Firebase Console](https://console.firebase.google.com), aktifkan **Realtime Database**
3. Isi `firebaseConfig` di `index.html` dengan config project kamu
4. Deploy folder ke hosting statis apa aja (Vercel, Netlify, GitHub Pages, dll)

## 🧪 Testing

Lihat [TESTING.md](TESTING.md) buat prompt siap pakai — ada versi cepat
(push test, ~5 menit) dan versi lengkap (QA test, semua fitur + edge case).

---

Made with ❤️ by **Adit** · Powered by Firebase

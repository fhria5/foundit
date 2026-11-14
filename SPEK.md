# FoundIt — Spek

**Nama:** FoundIt — Global Lost & Found with Matching Engine
**Kategori Kiroween:** Frankenstein (terverifikasi dari kiroween.devpost.com/rules, 14 Nov 2026: "Stitch together a chimera of technologies into one app. Bring together seemingly incompatible elements to build something unexpectedly powerful.")
**Repo:** https://github.com/fhria5/foundit (publik, MIT, `/.kiro` di root)
**Status:** TAHAP SPEK SAJA (14 Nov 2026). Belum ada satu baris kode aplikasi pun. Build dimulai HANYA setelah FreshTrack dinyatakan sempurna oleh user.

## Masalah

Barang hilang hampir tidak pernah kembali. Laporan tersebar di grup WA, Facebook, dan papan pengumuman, tanpa pencocokan otomatis. Penemu tidak tahu siapa pemiliknya, pemilik tidak tahu barangnya ditemukan di mana. Masalah ini global, dialami semua orang, di semua kota.

## Pengguna

Siapa saja yang kehilangan atau menemukan barang; komunitas kampus, kota, dan transportasi umum.

## Alur inti (satu alur)

Lapor hilang/ketemu (foto + deskripsi + lokasi + waktu) → matching engine memberi skor kecocokan (teks fuzzy + kategori + lokasi + waktu) → pemilik/penemu mengajukan klaim → verifikasi klaim (3 pertanyaan bukti kepemilikan + foto bukti) → klaim ditahan 24 jam (jendela sanggah) → serah terima tercatat + foto serah terima → arsip.

## Fitur

1. Posting hilang dan ketemu: foto, deskripsi, kategori, lokasi (peta/pin), waktu.
2. Matching engine: skor 0–100 per pasangan hilang–ketemu, diurutkan; alasan skor ditampilkan (kata cocok, jarak, selisih waktu).
3. Klaim + verifikasi berlapis: pengklaim jawab 3 pertanyaan bukti kepemilikan (ciri khusus, isi, tempat hilang) + lampirkan foto bukti (foto barang serupa atau struk); penemu setuju/tolak; klaim yang disetujui ditahan 24 jam (jendela sanggah — pemilik lain bisa menyanggah dengan bukti sendiri); serah terima wajib foto bukti akhir.
4. Moderasi: laporkan posting palsu; skor kepercayaan; posting bermasalah ditahan.
5. Arsip serah terima + statistik kota (barang kembali, kategori tersering).
6. PWA: install, offline baca daftar, foto tetap lokal di perangkat.

## Backend (kedalaman)

- Matching engine: tokenisasi + fuzzy (Jaro–Winkler/Levenshtein), bobot kategori 40 / teks 30 / jarak 20 / waktu 10; ambang tampil ≥ 50.
- Claim workflow (state machine): reported → matched → claimed → verifying → holding (24 jam) → returned / rejected / contested → archived. Tiap transisi tercatat + timestamp. Sanggahan yang masuk selama holding mengembalikan klaim ke verifying; foto serah terima wajib ada sebelum status jadi returned.
- Sinkron multi-perangkat: pola FreshTrack — Workers KV, debounce 1,5 dtk, last-write-wins, cap 64 KB / 200 item, foto di-strip dari payload sync (foto tetap lokal).
- Moderasi: laporan + skor kepercayaan per pengguna; ambang tahan otomatis.
- Audit log: semua transisi klaim + moderasi tercatat (hash-chain sederhana, pola SentinelBot).
- Keamanan klaim (5 lapis): (1) 3 pertanyaan bukti kepemilikan, (2) foto bukti, (3) penemu yang memutuskan (bukan otomatis), (4) jendela sanggah 24 jam, (5) foto serah terima + audit log + skor kepercayaan. Tanpa cek KTP — trade-off privasi yang sengaja dipilih; risiko sisa diakui jujur di write-up.
- Rate limit 30 req/IP/menit; CORS allowlist; validasi input (cap 2.000 karakter, foto ≤ 2 MB).
- AI opsional (fase lanjut): deskripsi foto otomatis + saran kategori via Verboo (glm-5.3-flash), fallback ke input manual bila AI gagal.

## Kenapa Frankenstein

Empat elemen yang tidak nyambung dijahit jadi satu app: papan pengumuman lost & found + mesin pencocokan fuzzy + verifikasi bukti kepemilikan + moderasi komunitas. Tidak ada satu pun yang berdiri sendiri sebagai FoundIt.

## Tech

- Frontend: PWA statis (HTML/CSS/JS vanilla, tanpa framework), GitHub Pages.
- Backend: Cloudflare Worker klasik di route `grokingstore.my.id/foundit-api/*` (pola Gembala/FreshTrack — Worker baru TIDAK dapat rute workers.dev) + Workers KV `foundit-items`.
- Bahasa UI: Inggris (audiens internasional).
- Kiro: spec-driven (`.kiro/specs/foundit`), steering, hook check-js-on-save, MCP server read-only, PBT (property-based tests, pola FreshTrack Phase 26/28), agen QA.

## Aturan Kiroween yang mengikat

- Proyek baru (periode 30 Okt–4 Des 2026); repo publik + lisensi terdeteksi + `/.kiro` di root, JANGAN di-gitignore.
- Video demo < 3 menit, YouTube Public.
- Write-up pemakaian Kiro wajib jujur soal porsi yang dibangun Kiro vs tangan (pelajaran FreshTrack).
- Satu proyek = satu kategori; multi-submit boleh asal tiap proyek jauh berbeda (FoundIt beda domain dan kategori dari FreshTrack/Costume Contest).
- Deadline submit: 5 Des 2026 06:00 WITA. Target selesai build: 30 Nov–2 Des.

## Non-goal (tidak dibangun)

Chat di dalam app, pembayaran/imbalan, pelacakan GPS real-time, verifikasi identitas resmi (KTP). Klaim cukup bukti kepemilikan lewat jawaban, bukan dokumen resmi.

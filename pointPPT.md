# Poin-Poin Presentasi Skripsi
## Sistem Face Recognition Berbasis IP CCTV dan FaceNet untuk Presensi Karyawan di PT Dinamika Mediakom

---

## Slide 1 — Judul
- Judul: Sistem Face Recognition Berbasis IP CCTV dan FaceNet untuk Presensi Karyawan di PT Dinamika Mediakom
- Nama, NIM, prodi, dosen pembimbing (isi sesuai halaman judul)

---

## Slide 2 — Latar Belakang
- Presensi saat ini pakai 2 metode: fingerprint (otomatis masuk Sistem Presensi Pusat) dan manual tulis tangan
- Jan–Apr 2026: rata-rata 45%–55% karyawan tercatat terlambat karena sensor fingerprint gagal baca jari basah/kotor/luka → antrean panjang
- Data presensi menentukan remunerasi (gaji, bonus, THR) → kerugian material bagi karyawan
- Solusi: Face Recognition berbasis Computer Vision memanfaatkan 6 unit IP CCTV yang sudah ada (pakai 1 unit di pintu masuk utama)
- Bukan mengganti Sistem Presensi Pusat, tapi menambahkan modul Face Recognition sebagai client yang terhubung via REST API (GET untuk sync data wajah, POST untuk kirim data presensi)
- Model: BlazeFace (MediaPipe) untuk deteksi + Inception-ResNet-v1 (FaceNet) untuk ekstraksi fitur — dipilih agar ringan di komputer non-GPU (CPU only, 8GB RAM)

---

## Slide 3 — Rumusan Masalah
- Bagaimana membangun sistem Face Recognition dengan IP CCTV & FaceNet untuk presensi karyawan?
- Bagaimana menguji akurasi (Confusion Matrix) dan fungsionalitas sistem (Blackbox Testing)?
- Bagaimana merancang integrasi otomatis hasil pengenalan wajah ke Sistem Presensi Pusat?

---

## Slide 4 — Batasan Masalah
- Python + MediaPipe + FaceNet, input dari 1 unit IP CCTV via RTSP
- Populasi pengguna: 30 karyawan PT Dinamika Mediakom
- Presensi otomatis tanpa kontak fisik
- Error handling: koneksi RTSP terputus & kegagalan kirim API
- Integrasi via REST API (GET sync foto, POST data presensi)
- Threshold biometrik 0.75 pada pencahayaan stabil

---

## Slide 5 — Tujuan & Manfaat Penelitian
- Tujuan: membangun sistem, mengukur akurasi (Confusion Matrix + Blackbox), mewujudkan integrasi otomatis ke Sistem Presensi Pusat
- Manfaat industri: efisiensi antrean, hemat biaya perangkat baru (pakai CCTV eksisting), akurasi data lebih terjamin
- Manfaat institusi & penulis: kontribusi ilmiah & pengasahan skill teknis

---

## Slide 6 — Metode Penelitian
- Jenis: Mixed Method – Concurrent Embedded (kualitatif primer, kuantitatif embedded)
- Sifat: Eksperimen
- Pendekatan: Prototyping (Analisis Kebutuhan → Perancangan → Pembentukan Prototipe → Evaluasi/Perbaikan → Implementasi Akhir → Pengujian)
- Teknik pengumpulan data: observasi, wawancara (Direktur, HR, Kepala IT), studi pustaka, pengumpulan dataset wajah
- Teknik analisis: analisis fungsional, performa algoritma (Confusion Matrix), uji konektivitas RTSP, analisis kualitatif PIECES

---

## Slide 7 — Analisis Kebutuhan
- Kebutuhan fungsional per aktor: Karyawan (deteksi otomatis, bounding box hijau/merah, cooldown 30 detik), Admin (live log, riwayat harian, sinkronisasi database wajah, konfigurasi parameter), Tim IT/HR (kirim data via REST API, logging ke 3 folder)
- Kebutuhan non-fungsional dipetakan pakai kerangka PIECES (Performance, Information, Economic, Control, Efficiency, Service)
- Spesifikasi software: Python 3.8–3.10, OpenCV, MediaPipe v0.10.21, Keras-FaceNet v0.3.0, NumPy, CustomTkinter, Requests
- Spesifikasi hardware: PC/Mini PC (i5 Gen-10/Ryzen 5, RAM 8GB), IP CCTV 1080p RTSP, jaringan LAN/Wi-Fi min 100 Mbps

---

## Slide 8 — Perancangan & Implementasi Sistem
- Arsitektur multi-threading: thread pembaca RTSP, thread model (deteksi+pengenalan), thread pengirim data — biar real-time tanpa lag/crash
- Pipeline: RTSP stream → Face Detection (BlazeFace) → Face Alignment → 128-D Embedding Extraction (FaceNet) → Euclidean Distance & klasifikasi (threshold 0.75) → integrasi ke Sistem Presensi Pusat
- Fitur tambahan: validasi lokasi GPS (geofencing), sistem logging untuk audit jejak presensi

---

## Slide 9 — Hasil Pengujian: Blackbox Testing
- 10 skenario uji (TS-1 s/d TS-10) mencakup aktor Karyawan, Admin, dan Tim IT
- Seluruh skenario dinyatakan SUKSES (konfigurasi parameter, konektivitas RTSP, sinkronisasi database wajah, pengiriman data ke server pusat, dsb.)

---

## Slide 10 — Hasil Pengujian: Confusion Matrix (Akurasi Model)
- 100 sampel uji (80 karyawan + 20 tamu): TP=76, FN=4, TN=19, FP=1
- Accuracy = 95,0%
- Precision = 98,7%
- Recall = 95,0%
- F1-Score = 96,8%
- Kesimpulan: threshold 0.75 optimal — aman dari orang asing tapi tetap nyaman untuk karyawan

---

## Slide 11 — Hasil Pengujian: Operasional Lapangan
- Uji 30 karyawan di pintu masuk, dibandingkan fingerprint vs face recognition
- Fingerprint: total 113,62 detik, rata-rata 3,79 detik/orang, 56,67% (17 dari 30) gagal scan awal
- Face Recognition: total 66,70 detik, rata-rata 2,22 detik/orang, 0% error rate
- Peningkatan efisiensi waktu: 41,42%

---

## Slide 12 — Analisis Komparatif PIECES (Sebelum vs Sesudah)
- Performance: 3,32 detik → 2,22 detik, nirkontak
- Information: manual/offline → real-time via REST API, log dapat diaudit
- Economic: biaya alat baru Rp1,5–3 juta → Rp0 (pakai CCTV eksisting)
- Control: rawan titip absen → biometrik wajah + validasi GPS
- Efficiency: 45–55% karyawan telat → keterlambatan akibat antrean turun jadi 0%
- Service: karyawan kurang nyaman → layanan otomatis & nyaman

---

## Slide 13 — Kesimpulan
- Sistem berhasil dibangun, fleksibel untuk penambahan karyawan baru tanpa reset ulang
- Andal, aman (precision 98,7%), dan nyaman (recall 95,0%) — seimbang antara keamanan & kenyamanan
- Alur data dari IP CCTV sampai Sistem Presensi Pusat berjalan otomatis di background tanpa mengganggu performa aplikasi

---

## Slide 14 — Saran Pengembangan
- Tambahkan liveness detection (deteksi kedipan/senyum) agar tidak bisa ditipu foto/video
- Gunakan perangkat pemroses khusus (embedded/edge device) agar lebih hemat daya
- Penyesuaian pencahayaan otomatis kamera untuk atasi silau matahari

---

## Slide 15 — Penutup
- Ucapan terima kasih & sesi tanya jawab

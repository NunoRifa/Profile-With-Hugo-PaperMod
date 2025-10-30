---
author: ["Nuno Rigo Fadilah"]
date: "2025-08-27"
title: "Membangun Server Capture Kamera dan Redirect Otomatis Menggunakan Golang"
description: "Tutorial membangun server Golang yang menampilkan landing page, meminta izin kamera, mengambil foto otomatis, memperoleh data lokasi dari IP publik, lalu mengirim foto + data ke Telegram Bot. Cocok untuk eksperimen teknis dan pembelajaran integrasi web–backend."
summary: "Langkah demi langkah membuat server Golang yang mengambil foto pengguna lewat browser, mengonversi gambar base64, mengambil IP publik & lokasi via IP2Location, lalu mengirimkan foto dan data lokasi ke Telegram Bot. Termasuk contoh konfigurasi .env dan instruksi menjalankan server."
tags: ["Tools"]
keywords: ["golang", "camera capture", "telegram bot", "ip2location", "web camera", "base64 image"]
categories: ["tutorial", "security", "web", "golang"]
series: ["Golang Experiments"]
ShowToc: true
TocOpen: true
---

### Ringkasan
Artikel ini menjelaskan cara membuat server sederhana menggunakan Golang yang menampilkan halaman landing sementara, meminta izin kamera dari pengguna (depan / belakang), menangkap foto, mengirim foto (base64 → bytes) dan data lokasi (diperoleh dari IP publik) ke Telegram. Setelah pengiriman selesai, pengguna diarahkan ke URL tujuan yang awalnya diberikan melalui query parameter `?url=`.

**Catatan:** artikel ini untuk tujuan pembelajaran / eksperimen. Jangan gunakan untuk melanggar privasi atau hukum.

**Source code:** [github](https://github.com/NunoRifa/Spy-With-Golang.git)

### Apa yang akan Anda pelajari
- Struktur aplikasi backend Golang yang melayani HTML + JavaScript.
- Cara meminta akses kamera di browser, menangkap gambar, dan mengirimkannya sebagai JSON.
- Mengubah data `data:image/jpeg;base64,...` menjadi byte di server Go.
- Mengambil IP publik dan melakukan lookup lokasi menggunakan IP2Location API.
- Mengirim pesan dan foto ke Telegram melalui Bot API.
- Contoh konfigurasi `.env` untuk menyimpan token rahasia.

### Persiapan & Persyaratan
Pastikan Anda memiliki:
- Go (disarankan 1.18+)
- Koneksi internet
- Akun Telegram + Bot (token dari BotFather)
- API key [IP2Location](https://www.ip2location.io/)
- Editor teks / terminal

### Struktur proyek (sederhana)
```bash
camera-capture-server/
├── main.go
├── .env
└── README.md
```

### Contoh isi `.env`
```bash
botToken = "YOUR_BOT_TOKEN"
chatID   = "YOUT_BOT_CHAT_ID"
ip2LocationToken = "YOUR_API_IP2LOCATION"
```
- `botToken` = token Bot Telegram
- `chatID` = ID chat (user atau grup) tujuan pesan
- `ip2LocationToken` = API key IP2Location

### Potongan arsitektur & alur kerja singkat
1. Pengguna mengunjungi:
```bash
http://localhost:8080/?url=https://example.com
```
2. Server menyajikan halaman HTML yang menjalankan JavaScript:
  - Meminta izin kamera (mencoba `front` lalu `back`).
  - Mengambil snapshot ke `canvas` → `toDataURL('image/jpeg', 0.8)`.
  - Mengirim JSON POST ke `/upload-photo` berisi: `image`, `userId`, `urlId`, `camera`, `message`.

3. Server Go menerima JSON:
  - Mengambil IP publik dengan `https://api.ipify.org/?format=json`.
  - Memanggil IP2Location: `https://api.ip2location.io/?key=TOKEN&ip=IP`.
  - Men-decode base64 image menjadi `[]byte`.
  - Menyusun pesan teks berisi IP, lokasi, Google Maps link, dan info ASN.
  - Mengirim pesan teks dan foto ke Telegram via `sendMessageToTelegram` & `sendPhotoToTelegram`.

4. Setelah selesai, browser diarahkan ke URL tujuan (url query param).

### Contoh payload JSON dari browser
```bash
{
  "image": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD...",
  "userId": "123456789",
  "urlId": "https://example.com",
  "camera": "front",
  "message": "Foto berhasil diambil."
}
```

### Keamanan & Etika
- Jangan jalankan server ini dalam lingkungan produksi tanpa penambahan kontrol akses, enkripsi transport (HTTPS), dan kebijakan privasi yang jelas.
- Selalu minta izin eksplisit dari pengguna sebelum mengakses kamera.
- Batasi penggunaan data, simpanan, dan akses bot Telegram agar tidak bocor.
- Gunakan hanya untuk eksperimen yang legal dan etis.

### Contoh pesan yang dikirim ke Telegram
Pesan teks yang dikirim berisi:
- IP publik
- URL asal (`From URL`)
- URL host (link yang mengarah kembali ke server dengan query `?url=...`)
- Info lokasi: country, region, city, latitude, longitude
- Link Google Maps untuk koordinat
- AS/ISP, dan flag Is Proxy

Contoh format (tekstual):
```bash
IP Address  : 103.123.45.67
From URL    : https://example.com
URL Host    : http://yourserver.com/?url=https://example.com

Location
Country Code: ID
Country Name: Indonesia
Region Name : Jawa Timur
City Name   : Surabaya
Latitude    : -7.2575
Longitude   : 112.7521
Maps        : https://www.google.co.id/maps/place/-7.2575,112.7521

Network
AS/ISP      : PT Telkom Indonesia
Is Proxy    : false
```

Foto akan diunggah sebagai file `user_photo.jpg` ke chat Telegram tujuan.

### Potensi perbaikan & fitur lanjutan
- Tambahkan HTTPS (TLS) agar komunikasi aman.
- Buat autentikasi / token validasi untuk endpoint `/upload-photo` agar tidak sembarang pihak bisa POST.
- Simpan foto ke cloud storage (S3, GCS) atau database dengan retention policy.
- Batasi resolusi dan ukuran upload untuk efisiensi.
- Tampilkan fallback dan pemberitahuan bila kamera tidak tersedia.

### Penutup
Proyek ini merupakan contoh praktis integrasi frontend-browser (akses kamera) dengan backend Golang serta layanan pihak ketiga (IP2Location dan Telegram Bot). Cocok sebagai eksperimen untuk memahami alur data (media + metadata) dari klien ke server dan integrasi API.

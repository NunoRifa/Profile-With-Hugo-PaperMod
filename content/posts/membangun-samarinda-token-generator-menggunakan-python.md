---
author: ["Nuno Rigo Fadilah"]
date: "2025-04-19"
title: "Membangun Samarinda Token Generator Menggunakan Python"
description: "Tutorial membuat script Python untuk mengotomatisasi proses pengambilan token dari halaman web Samarinda Generator menggunakan metode HTTP POST dan regex parsing. Script ini mensimulasikan pengisian form HWID dan mengekstrak token dari respon HTML tanpa perlu interaksi browser."
summary: "Panduan membuat tool sederhana di Python yang mengirim permintaan POST ke server, memproses respon HTML yang berisi token, lalu menyalin hasil token ke clipboard. Cocok untuk pembelajaran automasi request–response dan parsing data dari web."
tags: ["Tools"]
keywords: ["python", "web automation", "requests", "regex", "token generator"]
categories: ["tutorial", "automation", "python"]
series: ["Python Experiments"]
ShowToc: true
TocOpen: true
---

### Ringkasan
Artikel ini membahas pembuatan **Samarinda Token Generator**, sebuah script Python yang secara otomatis mengambil token dari situs `https://generatetoken.my.id/samarinda/`.  
Tool ini bekerja dengan mengirimkan **HWID (Hardware ID)** ke server menggunakan metode `POST`, lalu mengekstrak token hasil dari respon HTML menggunakan **regex**, tanpa perlu membuka browser.

**Source code:** [github](https://github.com/NunoRifa/KeyGenSamarindaPointBlank.git)

### Apa yang akan Anda pelajari
- Cara membuat script otomatisasi HTTP POST dengan Python.
- Penggunaan library `requests` dan `urllib3` untuk komunikasi web.
- Teknik **regex parsing** untuk mengekstrak data dari HTML.
- Menyalin hasil token langsung ke clipboard menggunakan `pyperclip`.
- Penanganan error jaringan dengan `try-except`.

### Persiapan & Persyaratan
Pastikan Anda sudah menyiapkan:
- Python 3.x  
- Modul berikut:
  ```bash
  pip install requests urllib3 pyperclip
  ```
- Koneksi internet aktif
- HWID valid (32 karakter hex)

### Struktur proyek
```bash
samarinda-token-generator/
├── NewKeyGen.py
└── README.md
```

### Penjelasan Alur Program
1. Input HWID
Pengguna diminta memasukkan HWID (32 karakter hex) melalui input terminal.
```bash
hwid = input("Masukkan Licensed Key (hex 32 karakter): ").strip()
```

2. Kirim Request POST
Script membuat sesi HTTP dan mengirim permintaan `POST` ke:
```bash
https://generatetoken.my.id/samarinda/
```
dengan payload:
```bash
data = {'hwid': hwid, 'generate_token': ''}
```

3. Parsing Token
Respon HTML dari server mengandung potongan teks:
```bash
html: "<p id='generated-token'>SMD-OL1HRD7JB2</p>"
```
Token diekstrak menggunakan regex:
```bash
match = re.search(r"html:\s*\"<p id='generated-token'>(.*?)</p>\"", response.text)
```

4. Salin ke Clipboard
Jika token ditemukan, hasil langsung disalin ke clipboard:
```bash
pyperclip.copy(token)
print("Token telah disalin ke clipboard.")
```

5. Penanganan Error
Jika server gagal diakses, script akan menampilkan pesan error dari `requests.exceptions.RequestException`.

### Contoh Output Terminal
```bash
Masukkan Licensed Key (hex 32 karakter): AE9EEB8ACEBAC3808006F786543210AJ
==== Mencari token ====
Token ditemukan: SMD-OL1HRD7JB2
Token telah disalin ke clipboard.
Tekan sembarang tombol untuk keluar...
```

### Cuplikan Kode Utama
```bash
match = re.search(r"html:\s*\"<p id='generated-token'>(.*?)</p>\"", response.text)
if match:
    token = match.group(1)
    print("Token ditemukan:", token)
    return token
else:
    print("Token tidak ditemukan.")
```

### Catatan Teknis
- Script ini **tidak menggunakan Selenium atau JavaScript engine** - cukup dengan parsing teks HTML dari respon.
- Header request diatur sedemikian rupa agar menyerupai browser sungguhan.
- Penggunaan `verify=False` menonaktifkan verifikasi SSL untuk mencegah error pada situs self-signed (tidak disarankan untuk produksi).
- Dapat dijalankan langsung di terminal Windows (menggunakan `msvcrt.getch()` untuk menunggu tombol keluar).

### Keamanan & Etika
- Jangan gunakan script ini untuk melakukan scraping atau request berulang yang dapat membebani server target.
- Gunakan hanya untuk **eksperimen dan pembelajaran**.
- Hindari menyebarkan token yang dihasilkan tanpa izin.
- Pastikan Anda memahami batas etika dan hukum dalam mengakses sistem pihak lain.

### Penutup
Proyek ini adalah contoh praktis bagaimana Python dapat digunakan untuk **mengotomatiskan interaksi web sederhana** - dari mengirim form, membaca respon HTML, hingga menyalin hasil ke clipboard.
Cocok untuk mempelajari dasar komunikasi client-server, parsing HTML ringan, dan struktur script utilitas berbasis CLI.
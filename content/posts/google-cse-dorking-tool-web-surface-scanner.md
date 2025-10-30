---
author: ["Nuno Rigo Fadilah"]
date: "2025-10-30"
title: "Google CSE Dorking Tool - Web Surface Scanner"
description: "Sharing ilmu membuat Google CSE Dorking Tool dengan Python mencakup instalasi, penggunaan CLI, pemeriksaan status HTTP paralel, ekspor hasil ke TXT/JSON, serta praktik keamanan dan etika. Kode lengkap tersedia di repository untuk eksperimen dan pembelajaran."
summary: "Panduan praktis langkah-demi-langkah membangun dan menjalankan tool dorking berbasis Google Custom Search API. Pembaca akan belajar cara mengumpulkan URL dari CSE, memeriksa respons secara paralel, menyimpan hasil, serta menerapkan konfigurasi dan langkah keamanan untuk eksperimen yang bertanggung jawab."
tags: ["Tools"]
keywords: ["tools", "python", "dorking", "google dorking", "google cse", "recon", "web scanning", "google custom search"]
categories: ["tutorial", "security", "tools", "python"]
series: ["Python Experiments"]
ShowToc: true
TocOpen: true
---

### Ringkasan
Artikel ini merupakan **sharing ilmu pribadi** saya tentang bagaimana cara membuat dan mendokumentasikan *Google Custom Search Engine (CSE) Dorking Tool* menggunakan bahasa Python.  
Tool ini berfungsi untuk mencari URL potensial berdasarkan pola *dork* tertentu melalui API resmi Google, memeriksa status HTTP tiap hasil, dan menyimpan output-nya dalam format teks dan JSON.

Tujuan saya menulis ini bukan untuk mendorong eksploitasi, melainkan untuk memperkenalkan bagaimana konsep *reconnaissance automation* bekerja dengan API publik, threading, dan parsing hasil pencarian.

**Catatan:** Artikel ini untuk tujuan pembelajaran dan eksplorasi teknis. Jangan gunakan untuk menyerang, mengeksploitasi, atau mengakses sistem tanpa izin.

### Apa yang akan Anda pelajari
- Struktur sederhana sebuah tool dorking berbasis Python.
- Cara menggunakan Google Custom Search API (CSE) untuk query otomatis.
- Proses pemeriksaan status HTTP secara paralel menggunakan `ThreadPoolExecutor`.
- Menyimpan hasil scanning ke file `.txt` dan `.json`.
- Cara membuat *installer script* sederhana agar tool mudah dijalankan di terminal.
- Praktik aman, etika, dan batas penggunaan API saat melakukan eksperimen.

### Persiapan & Persyaratan
Pastikan Anda memiliki:
- **Python 3.8+**  
- **Pip** (package manager bawaan Python)  
- **Koneksi internet**  
- **Google API Key** dan **CSE CX ID**  
  (dapat dibuat di [Google Custom Search Engine](https://programmablesearchengine.google.com/))
- Terminal / Command Line

### Struktur proyek
Berikut struktur folder yang saya gunakan:
```bash
google-cse-dorktool/
├── dork_tool.py           # Script utama tool dorking
├── install_dork_tool.sh   # Script instalasi dan symlink CLI
└── README.md              # Dokumentasi dan panduan penggunaan
```

### Penjelasan singkat
- `dork_tool.py` → berisi logika utama (Google CSE request, parallel check, output formatter).
- `install_dork_tool.sh` → script opsional untuk instalasi cepat ke sistem.
- `README.md` → dokumentasi tool dan contoh penggunaan CLI.

### Contoh isi `.env` (opsional)
Jika Anda ingin menyembunyikan API key dari kode sumber, buat file `.env` di root proyek seperti berikut:
```bash
GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"
CSE_CX="YOUR_CUSTOM_SEARCH_ENGINE_CX"
```

Kemudian, Anda bisa membaca variabel ini di `dork_tool.py` dengan:
```bash
import os
GOOGLE_API_KEY = os.getenv("GOOGLE_API_KEY")
CSE_CX = os.getenv("CSE_CX")
```

### Potongan kode penting dari `dork_tool.py`
Agar tidak terlalu panjang, saya tampilkan hanya bagian inti untuk pembelajaran:
```python
# Bagian inisialisasi dan request API CSE
def google_cse_search(query, api_key, cse_cx, num_results=10):
    url = f"https://www.googleapis.com/customsearch/v1?q={query}&key={api_key}&cx={cse_cx}"
    response = requests.get(url)
    data = response.json()
    return [item["link"] for item in data.get("items", [])]

# Pemeriksaan status halaman secara paralel
def check_status(url):
    try:
        r = requests.get(url, timeout=8, verify=False)
        return (url, r.status_code, len(r.content))
    except Exception:
        return (url, None, 0)
```
Potongan di atas sudah cukup untuk memahami flow dasar: ambil URL dari CSE, lalu cek responnya

### Cara instalasi (menggunakan `install_dork_tool.sh`)
File `install_dork_tool.sh` saya letakkan di root repository.
Isinya membantu Anda menginstal dependency dan menambahkan symlink ke sistem agar bisa menjalankan tool langsung lewat terminal.
```bash
#!/bin/bash
# install_dork_tool.sh
pip3 install --user requests beautifulsoup4 urllib3
chmod +x dork_tool.py
sudo ln -sf $(pwd)/dork_tool.py /usr/local/bin/dorktool
echo "Instalasi selesai. Jalankan: dorktool --help"
```

Untuk menginstal:
```bash
chmod +x install_dork_tool.sh
./install_dork_tool.sh
```

### Cara penggunaan (CLI)
Setelah instalasi, Anda bisa menjalankan tool ini dengan berbagai argumen:
```bash
# Menampilkan bantuan
dorktool --help

# Contoh mencari parameter ?id= di seluruh domain
dorktool -u "?id=" -n 20 --google-api-key "YOUR_KEY" --cse-cx "YOUR_CX"

# Contoh dengan domain spesifik dan output file
dorktool -d "example.com" -u "?page=" -s 200 301 -n 50 -o result.txt --json
```
**Penjelasan singkat argumen:**
- `-d` atau `--domain` → filter domain target
- `-u` atau `--url-pattern` → pola dork (?id=, ?page=, dsb.)
- `-s` → status code filter
- `-n` → jumlah hasil pencarian
- `-o` → output file
- `--google-api-key` & `--cse-cx` → kredensial API CSE

### Contoh hasil output
Hasil TXT:
```bash
URL: https://target.example/?id=123
Status Code: 200
Title: Example Page
Content Length: 3245 bytes
Final URL: https://target.example/?id=123
--------------------------------------------------
```

### Keamanan & Etika
Bagian ini sangat penting, saya tekankan kembali untuk pembaca:
- Gunakan tool ini hanya pada sistem yang Anda miliki atau dengan izin tertulis.
- Jangan menyimpan API key di repo publik.
- Jangan mematikan SSL verification di lingkungan produksi.
- Tambahkan rate limit / delay agar tidak mengirim terlalu banyak request ke server target.
- Hargai kebijakan `robots.txt` setiap situs.
- Gunakan hasil pencarian hanya untuk pembelajaran, riset keamanan, atau uji internal.

**Disclaimer:** Semua contoh dan kode pada artikel ini bersifat edukatif. Penulis tidak bertanggung jawab atas penyalahgunaan kode di luar konteks pembelajaran.

### Potensi pengembangan
Bagi pembaca yang ingin melanjutkan eksperimen:
- Menambahkan fallback ke DuckDuckGo / Bing jika Google API limit tercapai.
- Menyimpan hasil scan ke database (mis. SQLite atau MongoDB).
- Menambahkan proxy rotation atau user-agent rotation otomatis.
- Mengimplementasikan rate limit berbasis domain.
- Menambahkan GUI sederhana (mis. `tkinter` / `streamlit`).

### Penutup
Proyek ini saya tulis sebagai bentuk sharing ilmu mengenai teknik dorking automation secara legal dan etis. Diharapkan pembaca dapat memahami bagaimana integrasi API, threading, dan pengolahan hasil dilakukan di Python.

Jika Anda ingin mencoba atau berkontribusi, seluruh source code bisa dilihat di:
**Repository:** [github](https://github.com/NunoRifa/Python-Dorking-Tools-CSE)
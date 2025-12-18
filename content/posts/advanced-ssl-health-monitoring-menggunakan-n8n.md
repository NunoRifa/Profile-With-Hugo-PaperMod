---
author: ["Nuno Rigo Fadilah"]
date: "2025-12-18"
title: "Advanced SSL Health Monitoring Menggunakan n8n"
description: "Membangun sistem monitoring SSL tingkat lanjut menggunakan n8n untuk memeriksa masa berlaku sertifikat, protokol TLS, kerentanan keamanan, serta mengirimkan alert otomatis ke Telegram."
summary: "Tutorial membangun workflow n8n untuk monitoring kesehatan SSL secara otomatis. Mulai dari pengecekan expiry sertifikat, penilaian SSL grade, deteksi protokol usang & vulnerability, hingga pengiriman notifikasi Telegram berbasis severity."
tags: ["Tools", "Automation"]
keywords: ["ssl monitoring", "n8n", "tls", "ssl health check", "telegram alert", "devops"]
categories: ["tutorial", "security", "automation", "devops"]
series: ["Security Automation"]
ShowToc: true
TocOpen: true
---

### Ringkasan
SSL/TLS certificate sering kali dianggap *"pasang sekali lalu lupa"*. Padahal, sertifikat yang **kedaluwarsa**, **hostname mismatch**, atau **menggunakan protokol lama** dapat menyebabkan downtime, error di browser, bahkan celah keamanan serius.

Pada artikel ini, saya akan membahas bagaimana membangun **Advanced SSL Health Monitoring** menggunakan **n8n**. Workflow ini tidak hanya mengecek masa berlaku sertifikat, tetapi juga:

- Menilai **SSL grade**
- Mendeteksi **protokol TLS yang deprecated**
- Mengidentifikasi **kerentanan keamanan**
- Memberikan **alert bertingkat (low → critical)** ke Telegram

Semua proses berjalan otomatis dan terjadwal tanpa perlu intervensi manual.

**Catatan:** artikel ini ditujukan untuk pembelajaran dan praktik terbaik DevOps / security monitoring. Gunakan secara etis dan bertanggung jawab.

**Source code / workflow:** [github](https://github.com/NunoRifa/n8n-JSON-code/blob/master/advanced-SSL-health-monitoring.js)

### Apa yang akan Anda pelajari
Setelah membaca artikel ini, Anda akan memahami:

- Cara menggunakan **n8n sebagai security automation tool**
- Teknik membaca daftar domain dari **Data Table**
- Integrasi **API SSL Checker** dan **custom SSL assessment script**
- Parsing output SSL scanner menggunakan **Code Node**
- Membuat **logic alert berbasis severity**
- Mengirim **notifikasi Telegram otomatis**

Artikel ini cocok untuk:

- Sysadmin
- DevOps Engineer
- Security Engineer
- Web administrator yang mengelola banyak domain

### Persiapan & Persyaratan
Sebelum memulai, pastikan Anda sudah menyiapkan:
1. **n8n instance** (self-hosted atau cloud)
2. **Telegram Bot Token & Chat ID**
3. **Data Table n8n** berisi daftar domain

Minimal memiliki kolom:
```makefile
domain
---
example.com
example.com:8443
```

4. Server dengan:
    - Node.js
    - Akses SSH dari n8n
5. Script SSL assessment:
```bash
sysadmin-toolkit/scripts/ssl/ssl-health-assessment.js
```

### Potongan arsitektur & alur kerja singkat
Secara garis besar, workflow ini berjalan sebagai berikut:
1. **Schedule Trigger**
    - Menjalankan workflow setiap hari (contoh: jam 10 pagi)
2. **Get Row(s) - Data Table**
    - Mengambil daftar domain yang ingin dimonitor
3. **Split Host & Port**
    - Memisahkan domain dan port jika ada (`example.com:8443`)
4. **Check SSL (API)**
    - Mengambil data dasar SSL dari `ssl-checker.io`
5. **Expiry Check**
    - Jika sertifikat ≤ 7 hari → kirim alert cepat
6. **Advanced SSL Assessment (SSH)**
    - Menjalankan script lokal untuk:
        - SSL grade
        - TLS version
        - Vulnerabilities
        - Rekomendasi keamanan
7. **Format Output**
    - Parsing JSON
    - Menentukan severity alert
8. **Telegram Notification**
    - Mengirim alert terstruktur ke Telegram

### Contoh payload JSON hasil SSL assessment
Output dari script SSL assessment diproses dalam format JSON, contoh sederhananya:
```json
{
  "hostname": "example.com",
  "overallGrade": "B",
  "certificate": {
    "issuer": "Let's Encrypt",
    "daysUntilExpiry": 12,
    "isValid": true,
    "hostnameMatch": true
  },
  "protocols": {
    "tls13": true,
    "tls12": true,
    "tls10": false
  },
  "vulnerabilities": {
    "hasVulnerabilities": false
  },
  "recommendations": [
    "Disable TLS 1.0",
    "Enable HSTS"
  ]
}
```

Data ini kemudian digunakan untuk menentukan apakah perlu alert atau tidak.

### Keamanan & Etika
Beberapa hal penting yang perlu diperhatikan:
- Monitoring SSL **tidak melakukan eksploitasi**
- Semua pengecekan bersifat **read-only**
- Gunakan hanya untuk:
    - Domain milik sendiri
    - Domain yang Anda kelola secara legal
- Jangan gunakan workflow ini untuk *scanning tanpa izin*

Security monitoring bertujuan **mencegah masalah**, bukan menciptakan masalah baru.

### Contoh pesan alert Telegram
Jika ditemukan masalah, bot akan mengirim pesan seperti berikut:
```yaml
🔐 SSL Health Check: example.com

⚠️ SSL issues detected for example.com
Grade: C | Expires in 12 days

Issues found:
• Poor SSL grade: C
• Deprecated protocols enabled
• Certificate expires in 12 days
```

Untuk kasus **critical**, bot bisa memicu mention seperti `@here` agar segera diperhatikan.

### Potensi perbaikan & fitur lanjutan
Workflow ini masih bisa dikembangkan lebih jauh, misalnya:
- Integrasi **Discord / Slack**
- Export hasil scan ke **Grafana / Prometheus**
- Auto-create ticket **Jira / GitLab Issue**
- Tracking histori SSL grade per domain
- Auto reminder renewal + siapa PIC domain
- Monitoring cipher suite & OCSP stapling

### Penutup
Dengan memanfaatkan **n8n**, kita bisa membangun sistem **SSL Health Monitoring** yang jauh lebih cerdas dibanding sekadar cek expiry manual. Pendekatan ini sangat membantu ketika Anda mengelola banyak domain dan ingin **proaktif**, bukan reaktif.

Automation bukan hanya soal efisiensi, tapi juga soal **keamanan** dan **keandalan sistem**.

Semoga artikel ini bermanfaat dan bisa menjadi inspirasi untuk membangun security automation lainnya 🚀
Jika Anda tertarik, workflow ini bisa dengan mudah diperluas ke monitoring service lain seperti HTTP health, port scanning, atau certificate transparency.

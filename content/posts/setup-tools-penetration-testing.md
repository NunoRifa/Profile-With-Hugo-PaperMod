---
author: ["Nuno Rigo Fadilah"]
date: "2025-10-10"
title: "Setup Tools Penetration Testing"
description: "Menyiapkan lingkungan kerja untuk penetration testing memerlukan kombinasi alat dan sistem yang tepat. Dalam panduan ini, Anda akan menemukan tautan unduhan untuk VMware Workstation 17.5.2, image Windows MSEdge–Win10 siap pakai untuk VMware, serta opsi Kali Linux yang telah dimodifikasi oleh ZSecurity. Semua disusun untuk mempermudah Anda membangun sistem uji keamanan yang stabil, aman, dan siap eksplorasi."
summary: "Dokumen ini adalah panduan praktis untuk menyiapkan workstation pentesting. Pembaca diberikan dua sumber unduhan untuk VMware Workstation 17.5.2 (link Sync dan Google Drive) dan dua sumber untuk image Windows MSEdge–Win10 yang siap dipakai pada VMware. Sebagai tambahan terdapat opsi untuk mengunduh Kali Linux yang telah dimodifikasi dari ZSecurity."
tags: ["Tools"]
keywords: ["Tools", "Kali Linux", "VMware Workstation", "MSEdge–Win10"]
categories: ["security"]
series: ["Setup Tools"]
ShowToc: true
TocOpen: true
---

### Pendahuluan
Menyiapkan workstation untuk penetration testing membutuhkan beberapa komponen inti: hypervisor/virtualizer yang stabil, image target yang realistis (mis. Windows), serta mesin serangan yang berisi tool pentest (mis. Kali Linux). Dokumen ini menyajikan tautan unduhan untuk beberapa komponen yang umum dipakai dalam lab pentesting pribadi—ditujukan untuk mempercepat setup lab uji tanpa harus mengonfigurasi semuanya dari nol.

**Catatan penting:** semua berkas dan image yang dibagikan di sini digunakan untuk tujuan pembelajaran dan pengujian pada lingkungan yang Anda miliki izin untuk menguji. Jangan gunakan image atau tools ini untuk aktivitas ilegal. Selalu pastikan Anda memiliki persetujuan eksplisit sebelum melakukan pengujian terhadap sistem pihak ketiga.

### Download: VMware Workstation 17.5.2 (Windows x86-64)
VMware Workstation adalah aplikasi virtualisasi yang memungkinkan menjalankan beberapa sistem operasi tamu (guest OS) di dalam satu host. Untuk keperluan pentesting, Workstation berguna untuk mengisolasi target dan environment serangan sehingga tidak mengganggu host utama.

Sumber unduhan:
- [Download dari ln5.sync.com - link 1](https://ln5.sync.com/dl/a524d0280/fgbzw355-bzuq9n6t-yypf24kv-7rfsi8xu)
- [Download dari drive.google.com - link 2](https://drive.google.com/file/d/1z0MXYPYIYoYJKp6NubzZ2zOXlFyjnMgQ/view?usp=sharing)

**Tips:** setelah mengunduh, verifikasi checksum (jika tersedia) dan jalankan file di lingkungan terisolasi bila Anda ragu terhadap integritas berkas.

### Download: Windows MSEdge–Win10 (VM image)
Image Windows MSEdge–Win10 adalah mesin virtual Windows yang sudah dikonfigurasi cocok untuk mensimulasikan target Windows asli. Image prebuilt mempercepat pengujian exploit, kompatibilitas, maupun analisis forensik tanpa perlu memasang OS dari awal.

Sumber unduhan:
- [Download dari ln5.sync.com - link 1](https://ln5.sync.com/dl/69a8cb2b0/view/default/11829848200004?sync_id=0#k2xyv9ke-qevy6hgz-tavwxu3c-78858267)
- [Download dari drive.google.com - link 2](https://drive.google.com/file/d/1-TIp1Jnj5avio3v_hpLiWrZgKXIDAZIU/view)

**Perhatian:** Pastikan lisensi dan ketentuan penggunaan image tersebut sesuai. Untuk pengujian yang lebih aman dan terjamin update, pertimbangkan menggunakan image resmi Microsoft yang disediakan untuk pengembang/pengetesan.

### Download: Metasploitable (Lab Target Rentan)
Metasploitable adalah VM yang sengaja dirancang rentan untuk keperluan latihan. Sangat cocok untuk belajar exploitasi, konfigurasi Metasploit, serta menguji teknik hardening setelah eksploitasi.

Sumber unduhan:
- [Download dari sourceforge.net- link 1](https://sourceforge.net/projects/metasploitable)

**Catatan:** gunakan Metasploitable hanya di jaringan terisolasi. Jangan sambungkan VM rentan ke jaringan produksi.

### (Opsional) Kali Linux - versi custom dari ZSecurity
Kali Linux adalah distribusi standar untuk penetration testing, berisi banyak alat populer (nmap, Metasploit, John, dll.). Ada versi yang dimodifikasi oleh pihak ketiga seperti ZSecurity yang menyediakan image dengan konfigurasi dan paket tambahan untuk kemudahan setup.

Sumber unduhan:
- [Download Kali Linux Patched ZSecurity](https://zsecurity.org/download-custom-kali)

**Rekomendasi:** bila Anda mengutamakan keamanan dan update, gunakan versi resmi Kali dari [kali.org](https://www.kali.org/). Gunakan image custom hanya jika Anda mempercayai sumber dan memahami perubahan yang dilakukan.

### Penutup
Dokumen ini dirancang untuk membantu Anda **mempercepat pembuatan lab pentest**, mulai dari hypervisor (VMware Workstation), image target Windows, hingga mesin latihan rentan (Metasploitable) dan opsi Kali Linux. Gunakan tautan dengan bijak, dan selalu prioritaskan keamanan serta etika dalam setiap pengujian.
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

### Download Workstation 17.5.2 Windows x86-64
VMware Workstation adalah aplikasi virtualisasi yang memungkinkan Anda menjalankan satu atau beberapa sistem operasi tamu (guest OS) di atas sistem operasi host. Dalam konteks penetration testing, Workstation berguna untuk membuat lingkungan uji terisolasi, menjalankan mesin target, mesin pentester, atau lab eksperimen tanpa memengaruhi sistem utama.

- [Download dari ln5.sync.com - link 1](https://ln5.sync.com/dl/a524d0280/fgbzw355-bzuq9n6t-yypf24kv-7rfsi8xu)
- [Download dari drive.google.com - link 2](https://drive.google.com/file/d/1z0MXYPYIYoYJKp6NubzZ2zOXlFyjnMgQ/view?usp=sharing)

### Download Windows MSEdge-Win10-VMware
Image Windows MSEdge–Win10 adalah mesin virtual Windows yang telah dikonfigurasi sebelumnya (prebuilt) dan siap dipakai pada VMware. Image ini berguna untuk mensimulasikan target berplatform Windows, misalnya menguji exploit, kompatibilitas aplikasi, atau melakukan analisis forensik pada lingkungan yang menyerupai sistem pengguna nyata.

- [Download dari ln5.sync.com - link 1](https://ln5.sync.com/dl/69a8cb2b0/view/default/11829848200004?sync_id=0#k2xyv9ke-qevy6hgz-tavwxu3c-78858267)
- [Download dari drive.google.com - link 2](https://drive.google.com/file/d/1-TIp1Jnj5avio3v_hpLiWrZgKXIDAZIU/view)

### Download Metasploitable
Metasploitable adalah mesin virtual yang sengaja dibuat rentan untuk tujuan pembelajaran dan pelatihan keamanan. Cocok dipakai untuk latihan exploitasi, pengujian kerentanan, dan praktek penggunaan framework seperti Metasploit tanpa risiko merusak sistem produksi.

- [Download dari sourceforge.net- link 1](https://sourceforge.net/projects/metasploitable)

### (Opsional) Download Kali Linux dari ZSecurity
Kali Linux adalah distribusi Linux yang dipaketkan khusus untuk penetration testing dan forensik, berisi banyak tool keamanan populer (nmap, metasploit, john, dll.). Versi custom/patched dari ZSecurity menyediakan image yang telah disesuaikan atau dilengkapi, memudahkan setup awal untuk lab pentest. Gunakan versi resmi bila mengutamakan keaslian dan update keamanan.

- [Download Kali Linux Patched ZSecurity](https://zsecurity.org/download-custom-kali)
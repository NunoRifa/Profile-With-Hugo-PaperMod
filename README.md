# 🌐 Nuno Rigo Fadilah — Personal Website With Hugo

[![Netlify Status](https://api.netlify.com/api/v1/badges/c0d8ef44-29ba-4ff6-8a82-04590f89c8ad/deploy-status)](https://app.netlify.com/projects/nunorifa/deploys)

Ini adalah **website pribadi** saya — dibangun menggunakan [Hugo](https://gohugo.io/) dengan tema [PaperMod](https://github.com/adityatelange/hugo-PaperMod), dan di-*deploy* melalui [Netlify](https://www.netlify.com/).

Website ini berisi dokumentasi pribadi, catatan pembelajaran, serta tulisan seputar pengembangan web dan cybersecurity.

---

## 🧑‍💻 Tentang Saya

Saya **Nuno Rigo Fadilah**, seorang **Web Developer** sekaligus **IT Support** yang senang mengeksplorasi sisi teknis di balik sistem, khususnya dalam pengembangan web.

Dengan pengalaman membangun aplikasi menggunakan **C#, ASP.NET, Svelte, dan Laravel**, serta mengelola database **MySQL** dan **SQL Server**, saya terbiasa menciptakan solusi yang cepat, stabil, dan efisien.

Saat ini, saya sedang mempelajari **Cybersecurity** dan **Penetration Testing** untuk memperluas keahlian saya di dunia IT serta mempersiapkan transisi menuju peran sebagai **Cybersecurity Specialist / Pentester**.

---

## ⚙️ Tech Stack

| Kategori | Teknologi |
|-----------|------------|
| **Static Site Generator** | [Hugo](https://gohugo.io/) (Extended v0.151.0) |
| **Theme** | [PaperMod](https://github.com/adityatelange/hugo-PaperMod) |
| **Language** | Markdown, HTML, YAML |
| **Deployment** | [Netlify](https://www.netlify.com/) |
| **Version Control** | Git & GitHub |

---

## 🚀 Cara Build & Jalankan di Lokal

1. **Clone repository**
   ```bash
   git clone https://github.com/nunorifa/nuno-profile-papermod-hugo.git
   ```
   cd nuno-profile-papermod-hugo

2. **Jalankan server lokal**
   ```bash
   hugo serve
   ```
   Akses di http://localhost:1313

3. **Build untuk production**
   ```bash
   hugo --gc --minify
   ```
   Hasil build tersimpan di folder public/

## 🌍 Deployment
Website ini secara otomatis di-deploy ke Netlify setiap kali ada perubahan di branch `master`.
**Netlify Settings**
```bash
Build command: hugo --gc --minify
Publish directory: public
```
🔗 Live Site: https://nunorifa.netlify.app/

## 📝 Lisensi
Proyek ini menggunakan tema PaperMod yang berlisensi MIT.
Konten pribadi dan tulisan di website ini adalah milik Nuno Rigo Fadilah © 2025.

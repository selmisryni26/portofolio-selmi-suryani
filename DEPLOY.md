# Cara Deploy — Menjadikan Portofolio Link Publik

Situs ini **statis** (HTML + CSS + JS, tanpa build step), jadi bisa di-host gratis.
Hasil akhir: alamat seperti `https://selmi-portofolio.netlify.app` yang bisa dibuka
siapa saja, bisa ditaruh di CV / LinkedIn.

**Sudah disiapkan** agar langsung bisa dipakai:

| File | Untuk |
|---|---|
| `netlify.toml` | konfigurasi Netlify (tanpa build, + header keamanan) |
| `_headers` | cadangan header untuk Netlify/Cloudflare Pages |
| `vercel.json` | konfigurasi Vercel |
| `.gitignore` | mengecualikan folder CLI & arsip zip dari Git |

Situs sudah diperiksa: **52 referensi file cocok semua**, tidak ada nama file yang
beda huruf besar/kecil (penting karena server hosting pakai Linux yang
case-sensitive), dan tidak ada path lokal/`localhost`.

---

## Opsi 1 — Netlify Drop (paling cepat, tanpa install, ±1 menit)

Cocok kalau tidak mau menyentuh terminal.

1. Zip isi folder situs. **Penting:** file `index.html` harus berada di *dalam* zip,
   bukan di dalam satu folder lagi. Dari terminal:

   ```bash
   cd "C:/Users/ASUS/Documents/berkas kerja/Web Portofolio Selmi"
   zip -r ../portofolio-selmi.zip . -x "*.zip"
   ```

   Atau lewat File Explorer: masuk ke folder, pilih semua isinya
   (`index.html`, `journal.html`, `assets`, `netlify.toml`, dll) → klik kanan →
   *Send to* → *Compressed (zipped) folder*.

2. Buka <https://app.netlify.com/drop>
3. Drag & drop zip-nya ke halaman itu (atau pilih foldernya langsung).
4. Tunggu beberapa detik → Netlify memberi link seperti
   `https://random-name-123.netlify.app`. Link itu **langsung bisa diakses semua orang**.

**Ganti nama link:** Site settings → *Change site name* → misal
`selmi-suryani` → jadi `https://selmi-suryani.netlify.app`.
**Update isi nanti:** buka tab *Deploys* → drag & drop zip baru ke sana.

---

## Opsi 2 — Netlify lewat CLI (bisa update berulang dari terminal)

Sekali login, berikutnya tinggal satu perintah.

```bash
# 1. masuk ke folder situs
cd "C:/Users/ASUS/Documents/berkas kerja/Web Portofolio Selmi"

# 2. jalankan Netlify CLI (tanpa install permanen)
npx netlify-cli deploy --prod --dir .

# 3. pertama kali akan diminta login lewat browser, lalu:
#    - "Create & configure a new site"  → pilih
#    - pilih team, lalu isi nama situs (mis. selmi-suryani)
```

Setelah itu, setiap ada perubahan cukup jalankan lagi:

```bash
npx netlify-cli deploy --prod --dir .
```

---

## Opsi 3 — GitHub Pages (gratis, terhubung ke repository)

Cocok kalau ingin ada riwayat perubahan dan alamat `github.io`.

**Cara A — lewat terminal + halaman web GitHub (tanpa install apa pun):**

```bash
cd "C:/Users/ASUS/Documents/berkas kerja/Web Portofolio Selmi"

git init -b main
git add .
git commit -m "Portofolio Selmi Suryani"
git remote add origin https://github.com/USERNAME/portofolio.git   # ganti USERNAME
git push -u origin main
```

Saat `git push`, GitHub akan meminta login. **Jangan pakai password biasa** —
GitHub sudah tidak menerimanya. Gunakan salah satu:

- **Git Credential Manager** (biasanya sudah terpasang bersama Git for Windows):
  akan muncul jendela browser untuk login GitHub. Paling mudah.
- **Personal Access Token (PAT):** buat di
  <https://github.com/settings/tokens> → *Generate new token (classic)* →
  centang scope **repo** → pakai token itu sebagai password saat diminta.

Lalu di GitHub: **Settings → Pages → Source: Deploy from a branch →
Branch: `main` / `root` → Save.** Situs terbit di
`https://USERNAME.github.io/portofolio/` (butuh 1–2 menit).

**Cara B — sekaligus install GitHub CLI (`gh`), paling otomatis:**

```bash
winget install --id GitHub.cli          # sekali saja
gh auth login                           # login lewat browser
cd "C:/Users/ASUS/Documents/berkas kerja/Web Portofolio Selmi"
git init -b main && git add . && git commit -m "Portofolio"
gh repo create portofolio --public --source=. --push
gh api -X POST repos/:owner/portofolio/pages -f "source[branch]=main" -f "source[path]=/"
```

---

## Opsi 4 — Vercel (alternatif, juga gratis)

```bash
cd "C:/Users/ASUS/Documents/berkas kerja/Web Portofolio Selmi"
npx vercel --prod
```

Ikuti prompt (login lewat browser, nama project bebas). Untuk situs statis,
Vercel tidak perlu setting build — cukup tekan Enter pada pertanyaan default.

---

## Opsi 5 — Cloudflare Pages

Bisa lewat drag & drop di dashboard Cloudflare → *Workers & Pages* → *Create* →
*Pages* → *Upload assets*, atau lewat CLI:

```bash
npx wrangler pages deploy .
```

---

## Setelah online — yang perlu dicek

1. Buka link-nya, pastikan **halaman depan tampil** dan **foto-foto muncul**.
2. Klik menu **Journal** → pastikan pindah ke `journal.html` (bukan halaman kosong).
3. Coba tombol **Download Resume** → PDF harus terbuka.
4. Buka dari **HP** untuk memastikan tampilan mobile rapi.

Kalau ada yang gagal, penyebab paling sering adalah **nama file beda huruf besar/kecil**
(Windows tidak peduli, Linux peduli). Sudah diperiksa dan saat ini tidak ada masalah —
tapi ingat hal ini setiap kali mengganti/menambah file baru.

---

## Update isi setelah online

Ubah file di folder ini, lalu:

- **Netlify Drop / dashboard:** drag & drop ulang zip/folder-nya.
- **Netlify CLI:** `npx netlify-cli deploy --prod --dir .`
- **GitHub Pages:** `git add . && git commit -m "update" && git push`
- **Vercel:** `npx vercel --prod`

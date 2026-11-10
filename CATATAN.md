# Belajar Tailwind dari CSS yang sudah ada

Status: Tailwind v4 terpasang lokal (CLI)
File asli (HTML + CSS murni)

## Struktur folder

```
tailwind/
├── index.html        <- halaman, di root
├── style.css         <- HASIL BUILD, jangan diedit manual
├── src/
│   └── input.css     <- sumber Tailwind (1 baris @import)
├── package.json      <- script dev/build
├── .gitignore        <- node_modules/
└── node_modules/     <- jangan di-commit
```

Aturan yang paling sering bikin bingung:

| File | Fungsi | Di-link ke HTML? |
|---|---|---|
| `src/input.css` | bahan mentah | **tidak** |
| `style.css` | hasil build | **ya** |

`index.html` dan `style.css` harus satu folder. Kalau HTML dipindah ke
subfolder, path link ikut berubah (`../style.css`).

## Perintah

```powershell
npm run dev     # watch: edit HTML -> simpan -> refresh browser
npm run build   # sekali jalan + minify, untuk production
```

`npm run dev` menggantung di terminal. Itu benar, sedang memantau file.
Stop dengan `Ctrl+C`.

## Konsep inti

1. **Utility-first** — tidak ada `.card { ... }`. Gaya ditulis langsung di
   `class=""` elemen, satu class = satu properti CSS.
2. **Preflight** — Tailwind punya reset bawaan. Jadi `* { box-sizing }`,
   `margin: 0`, `a { text-decoration: none }`, `ul { list-style: none }`
   otomatis, tidak perlu ditulis.
3. **Skala jarak** — angka class = kelipatan 0.25rem (4px).
   `p-4` = 16px, `gap-6` = 24px, `py-16` = 64px, `px-10` = 40px.
4. **Nilai bebas** — kalau skala tidak cocok, pakai `[]`:
   `max-w-[1000px]`, `text-[#1c2333]`, `max-w-[60ch]`.
5. **Variant** — awalan untuk kondisi: `hover:text-blue-600`,
   `md:grid-cols-3`, `focus:ring-2`.
6. **Tidak ada `tailwind.config.js` di v4.** Kustomisasi lewat `@theme` di
   `src/input.css`. Tutorial yang menyuruh bikin config file itu v3.

## Peta CSS lama -> Tailwind

| CSS lama | Tailwind |
|---|---|
| `* { box-sizing; margin; padding }` | otomatis (preflight) |
| `body { font-family }` | `font-sans` |
| `body { line-height: 1.6 }` | `leading-relaxed` |
| `body { color: #1c2333 }` | `text-[#1c2333]` |
| `a { text-decoration: none; color: inherit }` | otomatis (preflight) |
| `ul { list-style: none }` | otomatis (preflight) |
| `.site-header { display:flex }` | `flex` |
| `align-items: center` | `items-center` |
| `justify-content: space-between` | `justify-between` |
| `padding: 16px 40px` | `px-10 py-4` |
| `border-bottom: 1px solid #ddd` | `border-b border-gray-300` |
| `.logo { font-weight:bold; font-size:1.1rem }` | `font-bold text-lg` |
| `nav ul { display:flex; gap:24px }` | `flex gap-6` |
| `nav a:hover { color:#2f6bff }` | `hover:text-blue-600` |
| `.section { padding:64px 40px }` | `px-10 py-16` |
| `max-width:1000px; margin:0 auto` | `max-w-[1000px] mx-auto` |
| `.section-alt { background:#f5f7fb }` | `bg-[#f5f7fb]` |
| `h1 { font-size:2rem; margin-bottom:12px }` | `text-3xl font-bold mb-3` |
| `h2 { font-size:1.5rem; margin-bottom:24px }` | `text-2xl font-bold mb-6` |
| `.section p { color:#5b6478 }` | `text-slate-500` |
| `max-width:60ch` | `max-w-[60ch]` |
| `.cards { display:grid; grid-template-columns:repeat(3,1fr); gap:20px }` | `grid grid-cols-3 gap-5` |
| `.card { background:#fff; border:1px solid #ddd; border-radius:8px; padding:20px }` | `bg-white border border-gray-300 rounded-lg p-5` |
| `.card h3 { margin-bottom:8px }` | `mb-2 font-semibold` |
| `.site-footer { display:flex; justify-content:space-between; gap:32px }` | `flex justify-between gap-8` |
| `padding:32px 40px` | `px-10 py-8` |
| `background:#1c2333; color:#d5dae5` | `bg-[#1c2333] text-[#d5dae5]` |
| `.footer-col { flex:1 }` | `flex-1` |
| `.site-footer .logo { color:#fff }` | `text-white` |

## Dua hal yang berubah cara pikir

**1. Tidak ada cascade antar class.** Di CSS lama, `.section p` mewarnai semua
`<p>` di dalam section, termasuk di dalam card. Di Tailwind setiap `<p>` harus
diberi `text-slate-500` sendiri. Konsekuensinya: class jadi panjang dan berulang.

**2. Class berulang diekstrak.** Kalau pola `bg-white border ... p-5` dipakai di
10 tempat, jangan copy 10x. Pilih satu:
- buat komponen (React/Vue/partial HTML), atau
- `@apply` di file CSS sendiri.
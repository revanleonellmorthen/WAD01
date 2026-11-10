# Belajar Tailwind dari CSS yang sudah ada

File: `index.html` (versi Tailwind). File asli tidak diubah, ada di `../index.html` + `../style.css`.

## Cara pakai

Buka `index.html` di browser. Tidak ada install, tidak ada build. Tailwind v4 CDN
membaca class di HTML lalu membuat CSS-nya saat halaman dibuka.

CDN hanya untuk belajar. Untuk project nyata pakai build step (`npm i tailwindcss`
+ `npx @tailwindcss/cli`) supaya CSS di-generate sekali, bukan tiap reload.

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

## Latihan (urut, kerjakan satu-satu)

1. Ganti `text-3xl` jadi `text-5xl` di h1. Lihat efeknya.
2. Tambah `hover:bg-gray-50` di `<article>` card.
3. Bikin grid jadi responsif: `grid-cols-1 md:grid-cols-3`.
4. Ganti `max-w-[1000px]` jadi `max-w-4xl`. Bandingkan lebarnya.
5. Tambah `shadow-sm` di card, lihat bedanya dengan border saja.
6. Buka DevTools, cek CSS yang di-generate Tailwind. Cari `.p-5`.

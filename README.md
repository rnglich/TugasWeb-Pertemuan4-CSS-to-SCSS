# Tugas 4 — Konversi CSS ke SCSS

Konversi CSS dari Tugas Pertemuan 2 (`style.css`, layout Dashboard/Portofolio: Grid + Flexbox)
ke SCSS dengan struktur modular **7-1 pattern**.

Sumber acuan: `style.css` Tugas Pertemuan 2 (269 baris).

## Struktur Proyek

```text
.
├── index.html          # sudah mengarah ke ./scss/main.css + Google Fonts via <link>
└── scss/
    ├── main.scss       # hanya berisi @use (tanpa @import)
    ├── main.css        # hasil compile 
    ├── main.css.map
    ├── abstracts/
    │   ├── _variables.scss  # colors, spacing, radius, breakpoint
    │   └── _mixins.scss     # font-setup, flex-center, box-style, respond-to
    ├── base/
    │   └── _base.scss       # box-sizing global + tipografi h2/p
    ├── components/
    │   ├── _buttons.scss    # button 120px, hover brightness
    │   ├── _forms.scss      # #kontak + @each(text, email)
    │   └── _media.scss      # figure/img/figcaption
    ├── layout/
    │   ├── _grid.scss       # grid body mobile + 768px (3fr 1fr) + grid-area
    │   ├── _header.scss     # header flex + nav.info flex
    │   ├── _sidebars.scss   # aside + input range
    │   └── _footer.scss     # footer + border-top dashed
    ├── pages/
    │   └── _home.scss       # #pengalaman + @for utilities
    └── themes/
        └── _dark-theme.scss # prefers-color-scheme: dark
```

## Pemenuhan Requirements

| No | Requirement | Status | Bukti |
|----|-------------|--------|-------|
| 1 | Konversi CSS existing ke SCSS | ✅ | `scss/main.css` (±279 baris) setara ref 269 baris: grid, header, nav, `h2`, `button`, `figure`, `aside`, `#kontak`, `footer` |
| 2 | Variables untuk colors & spacing | ✅ | 26 `$` di `_variables.scss`: `$primary-color`, `$border-color`, `$bg-light/aside/contact`, `$spacing-sm/md/lg/xl/xxl`, `$radius-pill/card/sm`, `$breakpoint-md` |
| 3 | Nesting (max 3 level) | ✅ | Contoh: `#kontak > input > :focus` (`_forms.scss`), `.info > a > &:hover` (`_header.scss`), `@media > .info > a` (`_dark-theme.scss`) |
| 4 | Minimal 3 mixins reusable | ✅ | 4 mixin di `_mixins.scss` dan semuanya dipakai: `font-setup` (`base`), `box-style` (`sidebars`, `forms`), `flex-center` (`header`), `respond-to` (`grid`) |
| 5 | Struktur 7-1 pattern (partials) | ✅ | `abstracts/base/components/layout/pages/themes` + `main.scss` sebagai entry point |
| 6 | Gunakan `@use` (bukan `@import`) | ✅ | `@use` di semua partial + `main.scss`; hasil `grep @import scss` = 0 (font Google dimuat via `<link>` di HTML) |
| 7 | Minimal 1 `@each` atau `@for` | ✅ | `@each $type in (text, email)` (`_forms.scss`) + `@for $i from 1 through 5` generate `.mt-1..5` (`_home.scss`) |
| 8 | Compile dengan Vite / Dart SASS | ✅ | `sass ^1.105.1` (Dart Sass) via npm, script `npm run build`, output `scss/main.css + .map` |

## Cara Menjalankan SASS

### Prasyarat

- Node.js + npm sudah terinstal. Cek versinya:

```powershell
node -v
npm -v
npx sass --version
```

### 1. Install dependencies (cukup sekali)

Buka terminal di root folder repo ini, lalu:

```powershell
npm install
```

### 2. Compile sekali (bukti untuk tugas)

```powershell
npm run build
```

Perintah ini menjalankan:

```text
sass scss/main.scss scss/main.css --style=expanded --source-map
```

Hasilnya: `scss/main.css` + `scss/main.css.map` diperbarui.

### 3. Mode watch (compile otomatis tiap save)

```powershell
npx sass scss/main.scss scss/main.css --watch
```

Biarkan terminal ini terbuka selama mengedit file `.scss`.

### 4. Versi minified (opsional)

```powershell
npm run sass:compressed
```

### 5. Kalau pakai extension Live Sass Compiler

Tetap bisa dipakai harian (klik **Watch Sass** di VS Code) karena di belakang juga memakai Dart Sass sehingga hasilnya sama. Namun bukti resmi untuk syarat no.8 adalah compile via npm pada langkah 2 di atas.

### 6. Buka hasilnya

Buka `index.html` di browser (sudah mengarah ke `./scss/main.css`). Uji:
- Lebar < 768px = 1 kolom, ≥ 768px = 2 kolom (`main` + `aside`).
- Dark mode: ubah tema OS/browser ke gelap (`prefers-color-scheme: dark`).

## Catatan Kesetaraan dengan Referensi

- Grid, 2 Flexbox (header + nav `.info`), `h2` pill, `button` hover, `figure`, `aside`, `#kontak` + `:focus` glow, `footer` dashed, dan dark mode disamakan dengan `style.css` Tugas Pertemuan 2.
- Perbaikan penamaan: `gaya_main` → `gaya-main` (kebab-case), file `_butttons.scss` → `_buttons.scss`.
- Verifikasi cepat:

```powershell
npm run build
Select-String -Path "scss\*.scss","scss\*\*.scss" -Pattern "@import"
Select-String -Path "scss\*\*.scss" -Pattern "@mixin|@each|@for"
```

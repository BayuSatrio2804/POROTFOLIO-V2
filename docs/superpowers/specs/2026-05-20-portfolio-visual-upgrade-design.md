# Portfolio Visual Upgrade — Design Spec
**Date:** 2026-05-20  
**Approach:** B — Lenis smooth scroll + Enhanced Framer Motion  
**Goal:** Tambahkan efek "wah" (cinematic scroll, neon glow, physics interactivity) tanpa mengorbankan performa, terutama di mobile.

---

## 1. Stack Changes

### Tambah
- **lenis** (~3KB) — smooth scroll engine. Diintegrasikan di `main.jsx` sebagai global provider.

### Tidak Perlu Install
- Three.js, @react-three/fiber, @react-three/drei — sudah ada, di-reuse
- Framer Motion — sudah ada, di-upgrade variannya
- Glow/neon effects — pure CSS animations
- Tilt effect — vanilla JS `mousemove` handler (no library)

---

## 2. Komponen yang Di-upgrade

### 2.1 Background3D (`Background3D.jsx`)
- Tambah **aurora gradient** animasi (CSS radial-gradient yang bergerak lambat) sebagai layer di belakang canvas Three.js
- Tambah **noise texture overlay** via CSS (`background-image: url('data:image/svg+xml,...')`) untuk kedalaman visual
- Star count: `800` di desktop → `400` di mobile via `window.matchMedia('(max-width: 768px)')`

### 2.2 CustomCursor (`CustomCursor.jsx`)
- Tambah **cursor trail**: 6 dot kecil (`4px`, opacity menurun) yang mengikuti cursor dengan delay staggered menggunakan array of positions
- Trail dinonaktifkan di touch devices (`'ontouchstart' in window`)
- Dot trail berwarna `var(--accent)` dengan opacity `0.6, 0.45, 0.32, 0.2, 0.12, 0.06`

### 2.3 Hero (`Hero.jsx`)
- **Foto profil**: tambah tilt 3D effect via `mousemove` pada `.hero-photo-ring` — `perspective(600px) rotateY(Xdeg) rotateX(Ydeg)`. Disabled di touch.
- **Skill cards**: magnetic hover — kartu bergerak `translate(dx*0.3, dy*0.3)` mengikuti posisi mouse relatif terhadap card center
- **Nama accent**: "Satrio" menggunakan animated gradient text (`background: linear-gradient(90deg, #CD7F32, #FFD700, #CD7F32)`, `background-size: 200%`, `animation: shimmer 3s linear infinite`)

### 2.4 Section Titles (semua section)
- Ganti `initial={{ y: '100%' }}` Framer Motion ke **clip-path cinematic wipe**: `initial={{ clipPath: 'inset(0 100% 0 0)' }}` → `animate={{ clipPath: 'inset(0 0% 0 0)' }}`
- Eyebrow `.eyebrow`: tambah CSS `shimmer sweep` — pseudo-element `::after` dengan gradient yang bergerak dari kiri ke kanan. Trigger via Framer Motion `onViewportEnter` yang menambahkan class `shimmer-active` sekali saja (`once: true`)

### 2.5 Project Cards (`Projects.jsx`)
- **Tilt 3D hover**: `mousemove` handler pada setiap `.project-card` — `perspective(800px) rotateY(Xdeg) rotateX(Ydeg) scale(1.02)`. Max tilt ±8°. Disabled di touch.
- **Spotlight effect**: `::before` pseudo-element dengan `radial-gradient` yang posisinya mengikuti mouse via CSS custom properties `--mouse-x`, `--mouse-y`
- Border glow on hover: `box-shadow: 0 0 30px var(--accent-glow-strong)`

### 2.6 Experience Timeline (`Experience.jsx`)
- **Timeline line draw**: ganti `background: var(--border)` static ke animasi `scaleY(0 → 1)` dengan `transform-origin: top` saat timeline masuk viewport
- **Dot pulse glow**: `.timeline-dot` pada entry pertama mendapat animasi `box-shadow` pulse `0 0 0 0 → 0 0 0 8px rgba(205,127,50,0)` saat masuk viewport

### 2.7 Navbar (`Navbar.jsx`)
- **Active link underline**: tambah `::after` pseudo-element yang slide-in dari kiri ke kanan saat link aktif
- **Logo gradient**: logo text menggunakan gradient subtle `#F5F5F5 → #CD7F32`

---

## 3. Section Baru

### 3.1 StatsCounter (`StatsCounter.jsx`) — **NEW**
**Posisi:** Antara Hero dan BentoSection (About)  
**Layout:** 1 baris horizontal, 4 stat item, centered  
**Stats (dari data aktual `portfolioContent.js`):**
- `4` Projects
- `4` Work Experiences / Roles
- `1` National Award (GEMASTIK XVIII Silver)
- `3+` Years Building (sejak mulai kuliah 2023, suffix `+`)

**Implementasi:**
- Custom hook `useCountUp(target, duration, delay)` — menggunakan `requestAnimationFrame` dengan easing `easeOutCubic`
- Trigger saat masuk viewport via Framer Motion `whileInView`
- Separator vertikal `|` antar stat dengan `var(--border)`
- Suffix support: `+`, `x` — tiap stat punya `suffix` dan `label` field di `portfolioContent.js`
- Stats data ditambahkan ke `portfolioContent.js` agar i18n-ready (EN & ID)

**CSS:** Compact strip, `padding: 2rem 0`, border top & bottom `var(--border)`, background `var(--bg-card)`

### 3.2 TechStackGrid — **NEW** (di dalam `BentoSection.jsx`)
**Posisi:** Di bawah capability grid dalam About section  
**Layout:** Flex-wrap, grouped by kategori  
**Kategori & items** (ditambahkan ke `portfolioContent.js` sebagai `techStack` array, i18n tidak diperlukan — nama teknologi universal):
```
ML / AI:    Python, PyTorch, Scikit-learn, ONNX, IndoBERTweet, Hugging Face
Frontend:   React, Vite, Framer Motion, Tailwind CSS
Backend:    FastAPI, Node.js, Express
Tools:      Docker, Git, Jupyter, VS Code
```

**Implementasi:**
- Badge pill: `padding: 4px 12px`, `border-radius: 20px`, `border: 1px solid var(--border)`
- Hover: `border-color: var(--accent)`, `box-shadow: 0 0 12px var(--accent-glow)`, `background: var(--accent-dim)`
- Stagger reveal via Framer Motion `whileInView` dengan `staggerChildren: 0.04`
- Kategori label kecil di atas setiap group

---

## 4. Smooth Scroll (Lenis)

Integrasi di `main.jsx`:
```js
import Lenis from 'lenis'
const lenis = new Lenis({ lerp: 0.1, smoothWheel: true })
function raf(time) { lenis.raf(time); requestAnimationFrame(raf) }
requestAnimationFrame(raf)
```

Lenis diaktifkan di semua device (ringan, tidak ada masalah di mobile).

---

## 5. Mobile Strategy

| Efek | Desktop | Mobile (≤768px) |
|------|---------|-----------------|
| Cursor trail | ✅ Aktif | ❌ Dinonaktifkan (`ontouchstart` check) |
| Tilt 3D (cards & foto) | ✅ Aktif | ❌ Dinonaktifkan, diganti `scale(1.02)` on tap |
| Magnetic hover | ✅ Aktif | ❌ Dinonaktifkan |
| Three.js star count | 800 | 400 |
| Aurora gradient | ✅ Aktif | ✅ Aktif |
| Glow effects | ✅ Aktif | ✅ Aktif |
| Lenis scroll | ✅ Aktif | ✅ Aktif |
| CSS animations | ✅ Aktif | ✅ Aktif |
| `prefers-reduced-motion` | Matikan semua animasi | Matikan semua animasi |

---

## 6. File Changes Summary

| File | Perubahan |
|------|-----------|
| `package.json` | + lenis |
| `main.jsx` | + Lenis init |
| `index.css` | + aurora, noise, shimmer, gradient text, spotlight CSS |
| `App.css` | + tilt, trail, timeline draw, underline classes |
| `App.jsx` | + `<StatsCounter />` antara Hero dan BentoSection |
| `Background3D.jsx` | + aurora layer, noise overlay, mobile star count |
| `CustomCursor.jsx` | + trail dots, touch detection |
| `Hero.jsx` | + tilt foto, magnetic skill cards, gradient text |
| `BentoSection.jsx` | + TechStackGrid, clip-path eyebrow |
| `Projects.jsx` | + tilt cards, spotlight, enhanced hover |
| `Experience.jsx` | + timeline draw, dot pulse |
| `Navbar.jsx` | + underline active, logo gradient |
| `components/StatsCounter.jsx` | **NEW** |
| `data/portfolioContent.js` | + stats data |

---

## 7. Out of Scope
- Penggantian color scheme (tetap bronze `#CD7F32`)
- Penambahan section lain (Globe, Skills Bar)
- GitHub API / WakaTime integration
- GSAP (tidak diperlukan)
- Perubahan layout/struktur section yang sudah ada

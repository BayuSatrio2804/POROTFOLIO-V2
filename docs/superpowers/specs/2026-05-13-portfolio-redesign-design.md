# Portfolio Redesign — Muhammad Bayu Satrio (v2)

**Date:** 2026-05-13  
**Approach:** Fresh rewrite, same stack (React + Vite + framer-motion)  
**Checkpoint tag:** `checkpoint-before-redesign-v2026-05-13` (in `react-version/`)

---

## 1. Goals

Redesign the existing portfolio (`react-version/`) with a new visual identity while preserving all personal content. The sidebar layout is a structural change that makes patching the existing code impractical — a clean rewrite with the same tooling is the right call.

**Preserve without change:**
- All content data in `src/data/portfolioContent.js`
- All public assets (images, PDFs, fonts)
- Bilingual EN/ID support
- All section types: Hero, About, Projects, Certificates, Experience, Education, Terminal, Contact

---

## 2. Visual Identity

### Color Palette

| Variable         | Value                      | Usage                        |
|------------------|----------------------------|------------------------------|
| `--bg`           | `#1C2B2D`                  | Page background              |
| `--bg-card`      | `#162224`                  | Sidebar, cards, modals       |
| `--bg-elevated`  | `rgba(28,43,45,0.92)`      | Glassmorphism layers         |
| `--accent`       | `#CD7F32`                  | Bronze — all accent elements |
| `--accent-dim`   | `rgba(205,127,50,0.12)`    | Card hover bg, tag bg        |
| `--accent-glow`  | `rgba(205,127,50,0.22)`    | Hover glow, active borders   |
| `--text`         | `#F5F5F5`                  | Primary text                 |
| `--text-muted`   | `rgba(245,245,245,0.55)`   | Secondary text, descriptions |
| `--text-soft`    | `rgba(245,245,245,0.75)`   | Mid-tone text                |
| `--border`       | `rgba(205,127,50,0.18)`    | Card borders, dividers       |
| `--border-strong`| `rgba(205,127,50,0.45)`    | Active/hover borders         |

### Typography

| Role     | Font          | Weight     | Notes                        |
|----------|---------------|------------|------------------------------|
| Heading  | Syne          | 700–900    | h1, h2, h3, section titles  |
| Body     | Space Grotesk | 400–600    | paragraphs, nav, UI labels  |

Google Fonts import:
```
Syne:wght@700;800;900
Space+Grotesk:wght@400;500;600;700
```

### Background

- Base: `#1C2B2D` solid
- Subtle radial gradient: `radial-gradient(circle at 80% 10%, rgba(205,127,50,0.06), transparent 40rem)`  
- Three.js canvas: retained but with reduced opacity (`0.4` max) and bronze-tinted particles (`#CD7F32` at low alpha) instead of cyan

---

## 3. Layout Architecture

### Desktop (≥ 768px)

```
┌─────────────────────────────────────────────────┐
│  SIDEBAR (240px fixed)  │  CONTENT AREA (flex 1) │
│                         │                        │
│  Bayu.                  │  ┌──────────────────┐  │
│  ─────────────────      │  │   Section content │  │
│  • Home    ←active      │  │   max-width 860px │  │
│  · About                │  │   centered        │  │
│  · Projects             │  └──────────────────┘  │
│  · Certificates         │                        │
│  · Experience           │  (scrolls vertically)  │
│  · Education            │                        │
│  · Contact              │                        │
│  ─────────────────      │                        │
│  EN | ID                │                        │
│  © 2026 Bayu Satrio     │                        │
└─────────────────────────────────────────────────┘
```

**Sidebar details:**
- `position: fixed`, full height, `overflow-y: auto`
- Background: `--bg-card` (`#162224`)
- Right border: `1px solid var(--border)`
- Logo: `"Bayu."` — Syne 800, `<span>` on `.` colored `--accent`
- Nav items: Space Grotesk 500, `padding: 10px 20px`
- Active state: `border-left: 2px solid var(--accent)`, background `--accent-dim`, text `--text`
- Inactive state: text `--text-muted`, no border
- Language toggle: small `EN | ID` buttons at bottom, active lang highlighted bronze
- Footer text: `© 2026 Muhammad Bayu Satrio`, font-size `11px`, muted

### Mobile (< 768px)

Sidebar collapses. Bottom navigation bar appears:
- Fixed to bottom, height `60px`, background `--bg-card`, top border `var(--border)`
- Icon + label for each nav item (SVG icons from `public/icons.svg` sprite)
- Active item: bronze color
- Language toggle moves to top-right corner of page as a small button

---

## 4. Section Designs

### 4.1 Hero

Layout: single column, full content area height (min `100vh - sidebar offset`).

```
[eyebrow: "AI Engineer · GEMASTIK XVIII 🥈"]
Muhammad              ← Syne 900, clamp(44px, 7vw, 72px)
Bayu Satrio          ← "Satrio" colored #CD7F32 (bronze)
─────────────────────
AI Engineer / Data Analyst   ← Space Grotesk 500, bronze
Short bio tagline            ← Space Grotesk 400, muted
─────────────────────
┌──────────────┐ ┌──────────────┐
│ 🧠 IndoBERT  │ │ ⚡ ONNX      │
│ NLP          │ │ Runtime      │
└──────────────┘ └──────────────┘
┌──────────────┐ ┌──────────────┐
│ 📊 Analytics │ │ 🔧 Applied   │
│              │ │ Systems      │
└──────────────┘ └──────────────┘
─────────────────────
[View Projects ▶]  [Download Resume]
```

Skills grid: 2×2, `--bg-card` background, `--border` border, bronze icon/label.  
CTA: primary button = bronze solid (`#CD7F32`, text `#1C2B2D`); secondary = ghost border.

### 4.2 About (Bento)

Bento grid (12-column) with capability cards:
- Section eyebrow + title (Syne)
- 2-paragraph bio
- 4 capability cards: AI/NLP, Analytics, Deployment, Applied Systems
- Each card: bronze icon, title Syne 700, body Space Grotesk 400

### 4.3 Projects

Card grid (2 columns on desktop, 1 on mobile).

Each project card:
- Top label: category + period (bronze, uppercase, small)
- Title: Syne 700
- Summary: Space Grotesk, muted
- Tech stack pills: `--accent-dim` bg, `--border` border
- Actions: "View Details" toggle + "View Repo" link
- Expandable detail section: responsibilities list + image gallery

### 4.4 Certificates

GEMASTIK XVIII gets featured card treatment:
- Larger card with image preview
- Bronze badge overlay: "🥈 2nd Place · National Level"
- Issuer + year metadata
- "View Certificate" link

### 4.5 Experience

Vertical timeline:
- Center line: `1px solid var(--border)` 
- Active/latest dot: bronze filled circle
- Past dots: outline circle
- Each entry: role (Syne 700) + company (bronze) + period (muted) + description

### 4.6 Education

Simple card per institution:
- Institution name (Syne 700) + degree + period
- Logo if available (Telkom University)
- Short description

### 4.7 Terminal

Retained as-is functionally. Visual updates:
- Background: `--bg-card`
- Border: `--border`
- Prompt color: `#CD7F32` (bronze instead of cyan)
- Text output: `--text` and `--text-muted`
- Cursor blink: bronze

### 4.8 Contact

Two-panel layout:
- Left: heading + bio + GitHub/LinkedIn social buttons
- Right: form (name, email, subject, message, send button)
- Form fields: `--bg-card` bg, `--border` border, focus `--border-strong`
- Send button: bronze solid

---

## 5. Special Components

### Custom Cursor

Replace existing heavy cursor with lightweight version:
- Inner dot: `6px`, `#CD7F32`, `position: fixed`, no lag
- Outer ring: `28px`, `1px solid rgba(205,127,50,0.6)`, follows with `150ms` CSS transition lag
- Ring expands to `38px` on hover over interactive elements (`a`, `button`)
- Ring hidden on mobile (touch devices)
- Implementation: single `useEffect` with `mousemove` listener, two `div` refs — no canvas, no particle trail

### Preloader

Clean minimal sequence:
1. Full screen `#1C2B2D` background
2. Center: `"Muhammad Bayu Satrio"` fades in (Syne 700, `--text`)
3. Below name: role line `"AI Engineer / Data Analyst"` fades in with 200ms delay (Space Grotesk, bronze)
4. Thin progress bar (`2px`, `#CD7F32`) animates from 0→100% width over `1.2s`
5. Entire preloader slides up (`translateY(-100%)`) and unmounts
- No glitch effect, no particle burst, no multiple text flickers

### Background3D

Retain Three.js canvas. Adjustments:
- Particle color: `#CD7F32` at `alpha 0.35` (was cyan)
- Canvas opacity: `0.4` max
- Z-index: `-1`, `position: fixed`, full screen

### Language Toggle

Retained as-is. UI placement: bottom of sidebar on desktop, top-right corner on mobile.

---

## 6. Animations

All via `framer-motion`:

| Element               | Animation                            | Duration |
|-----------------------|--------------------------------------|----------|
| Section entry         | `opacity: 0→1, y: 24→0`             | `0.6s`   |
| Card hover            | `translateY(-3px)`, border glow      | `180ms`  |
| Sidebar nav active    | background + border-left, smooth     | `150ms`  |
| Hero name             | `opacity: 0→1, y: 30→0` on load     | `0.8s`   |
| Preloader exit        | `y: 0→-100%`                         | `0.5s`   |
| Project card expand   | height animate, `opacity: 0→1`       | `0.3s`   |

---

## 7. File Structure

New files replace existing `src/` contents. `public/` and `src/data/portfolioContent.js` are untouched.

```
react-version/src/
  index.css             ← CSS variables, reset, global styles
  App.css               ← layout shell (sidebar + content area)
  App.jsx               ← layout shell component
  main.jsx              ← unchanged
  data/
    portfolioContent.js ← unchanged
  components/
    Sidebar.jsx         ← NEW: replaces Navigation.jsx
    CustomCursor.jsx    ← rewrite (lightweight)
    Preloader.jsx       ← rewrite (clean)
    Background3D.jsx    ← update colors only
    Hero.jsx            ← rewrite
    BentoSection.jsx    ← rewrite
    Projects.jsx        ← rewrite
    Certificates.jsx    ← rewrite
    Experience.jsx      ← rewrite
    Education.jsx       ← rewrite
    Terminal.jsx        ← update colors only
    Contact.jsx         ← rewrite
```

---

## 8. Out of Scope

- No new sections beyond what currently exists
- No backend changes
- No new projects added to `portfolioContent.js`
- No change to PDF/resume generation scripts
- No deployment config changes

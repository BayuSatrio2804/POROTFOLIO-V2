# Portfolio Redesign v2 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite the React portfolio with a dark-teal + bronze visual identity, fixed sidebar navigation, and Syne + Space Grotesk typography — preserving all personal content data.

**Architecture:** Fresh rewrite of all `src/` files inside `react-version/`. The `public/` directory and `src/data/portfolioContent.js` are untouched. All components receive a clean implementation with shared CSS variables defined in `index.css`.

**Tech Stack:** React 19, Vite 8, framer-motion 12, @react-three/fiber 9, @react-three/drei 10, lucide-react, Space Grotesk + Syne (Google Fonts)

**Working directory for all commands:** `C:\Portofolio\react-version`

---

## File Map

| File | Action | Responsibility |
|------|--------|---------------|
| `index.html` | Modify | Add Google Fonts link tags |
| `src/index.css` | Rewrite | CSS variables, reset, global classes (btn, eyebrow, etc.) |
| `src/App.css` | Rewrite | Layout shell + all component styles |
| `src/App.jsx` | Rewrite | App shell: sidebar + content-area layout |
| `src/components/Sidebar.jsx` | Create (replaces Navigation.jsx) | Fixed sidebar + mobile bottom nav + active section tracking |
| `src/components/CustomCursor.jsx` | Rewrite | Lightweight dot + ring cursor |
| `src/components/Preloader.jsx` | Rewrite | Name + role fade-in + progress bar + slide-out |
| `src/components/Background3D.jsx` | Update | Bronze colors, lower opacity |
| `src/components/Hero.jsx` | Rewrite | Big name (Syne 900) + 2×2 skills grid + CTAs |
| `src/components/BentoSection.jsx` | Rewrite | About section: bio + 4 capability cards |
| `src/components/Projects.jsx` | Rewrite | 2-col expandable project cards |
| `src/components/Certificates.jsx` | Rewrite | Featured GEMASTIK card with bronze badge |
| `src/components/Experience.jsx` | Rewrite | Vertical bronze timeline |
| `src/components/Education.jsx` | Rewrite | Institution cards with logo |
| `src/components/Terminal.jsx` | Update | Bronze color replacements only |
| `src/components/Contact.jsx` | Rewrite | Two-panel: text+socials left, form right |
| `src/data/portfolioContent.js` | No change | All content preserved |
| `src/main.jsx` | No change | Entry point |

---

## Task 1: Google Fonts + CSS Foundation

**Files:**
- Modify: `index.html`
- Rewrite: `src/index.css`

- [ ] **Step 1: Add Google Fonts to index.html**

Replace the existing `<head>` content (keep all meta tags, just add font preconnect + link before `</head>`):

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/jpeg" href="/foto-saya.jpg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Muhammad Bayu Satrio | AI Engineer & Data Analyst</title>

    <meta name="title" content="Muhammad Bayu Satrio | AI Engineer Portfolio" />
    <meta name="description" content="Portfolio of Muhammad Bayu Satrio — Silver Medalist GEMASTIK XVIII. AI Engineering, NLP, Analytics, and Applied Systems." />
    <meta name="keywords" content="Muhammad Bayu Satrio, AI Engineer, NLP, Data Analyst, IndoBERTweet, ONNX, Telkom University" />
    <meta name="author" content="Muhammad Bayu Satrio" />

    <meta property="og:type" content="website" />
    <meta property="og:title" content="Muhammad Bayu Satrio | AI Engineer Portfolio" />
    <meta property="og:description" content="Silver Medalist GEMASTIK XVIII. Indonesian NLP, Analytics, Applied Systems." />
    <meta property="og:image" content="/foto-saya.jpg" />

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800;900&family=Space+Grotesk:wght@400;500;600;700&display=swap" rel="stylesheet" />
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

- [ ] **Step 2: Rewrite src/index.css**

```css
@import './App.css';

:root {
  --bg: #1C2B2D;
  --bg-card: #162224;
  --bg-elevated: rgba(28, 43, 45, 0.92);
  --accent: #CD7F32;
  --accent-dim: rgba(205, 127, 50, 0.12);
  --accent-glow: rgba(205, 127, 50, 0.22);
  --text: #F5F5F5;
  --text-muted: rgba(245, 245, 245, 0.55);
  --text-soft: rgba(245, 245, 245, 0.75);
  --border: rgba(205, 127, 50, 0.18);
  --border-strong: rgba(205, 127, 50, 0.45);
  --sidebar-width: 240px;
  --radius: 8px;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  background: var(--bg);
  scroll-behavior: smooth;
  color-scheme: dark;
}

body {
  background:
    radial-gradient(circle at 80% 10%, rgba(205, 127, 50, 0.06), transparent 40rem),
    var(--bg);
  color: var(--text);
  font-family: 'Space Grotesk', system-ui, sans-serif;
  min-height: 100vh;
  overflow-x: hidden;
  line-height: 1.6;
}

h1, h2, h3 {
  font-family: 'Syne', sans-serif;
  letter-spacing: -0.02em;
  line-height: 1.05;
  text-wrap: balance;
}

p { color: var(--text-soft); }

a { color: inherit; text-decoration: none; }

button, input, textarea, select { font: inherit; }

button, a { -webkit-tap-highlight-color: transparent; cursor: pointer; }

img { max-width: 100%; display: block; }

::selection { background: var(--accent); color: var(--bg); }

/* ---- Global UI classes ---- */

.eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  font-family: 'Space Grotesk', sans-serif;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 1rem;
}
.eyebrow::before {
  content: '';
  width: 24px;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent));
  flex-shrink: 0;
}

.section-title {
  font-size: clamp(1.8rem, 4vw, 3rem);
  color: var(--text);
  margin-bottom: 1.5rem;
}

.section-body {
  color: var(--text-muted);
  max-width: 600px;
  margin-bottom: 2.5rem;
  line-height: 1.7;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 44px;
  padding: 0.75rem 1.5rem;
  border-radius: var(--radius);
  border: 1px solid transparent;
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 700;
  font-size: 0.9rem;
  text-decoration: none;
  transition: transform 150ms ease, box-shadow 150ms ease, border-color 150ms ease, background 150ms ease;
}
.btn:hover,
.btn:focus-visible {
  transform: translateY(-2px);
  outline: none;
}
.btn-primary {
  background: var(--accent);
  color: var(--bg);
  border-color: var(--accent);
  box-shadow: 0 0 20px var(--accent-glow);
}
.btn-primary:hover {
  box-shadow: 0 0 32px rgba(205, 127, 50, 0.4);
}
.btn-secondary {
  background: transparent;
  border-color: var(--border-strong);
  color: var(--text);
}
.btn-secondary:hover {
  border-color: var(--accent);
  background: var(--accent-dim);
}

::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb {
  background: var(--accent);
  border-radius: 999px;
  border: 2px solid var(--bg);
}
```

- [ ] **Step 3: Start dev server and verify fonts load**

```
cd C:\Portofolio\react-version
npm run dev
```

Open http://localhost:5173. The page will be broken (old App.jsx still references old CSS). That's expected. Verify in DevTools → Network that `fonts.googleapis.com` requests are made for Syne and Space Grotesk.

- [ ] **Step 4: Run data tests to confirm baseline**

```
npm test
```

Expected: `All tests passed` (tests data integrity in portfolioContent.js, which is untouched).

- [ ] **Step 5: Commit**

```
git add index.html src/index.css
git commit -m "feat: css foundation — bronze palette + Syne/Space Grotesk fonts"
```

---

## Task 2: Layout Shell CSS (App.css)

**Files:**
- Rewrite: `src/App.css`

- [ ] **Step 1: Rewrite src/App.css with full layout + component styles**

```css
/* =========================================
   LAYOUT SHELL
   ========================================= */

.app-shell {
  display: flex;
  min-height: 100vh;
}

/* ---- Sidebar ---- */

.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: var(--sidebar-width);
  height: 100vh;
  background: var(--bg-card);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  z-index: 100;
  overflow-y: auto;
}

.sidebar-logo {
  font-family: 'Syne', sans-serif;
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--text);
  padding: 1.5rem 1.5rem 1.25rem;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
}
.sidebar-logo-dot { color: var(--accent); }

.sidebar-nav {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 1rem 0;
}

.sidebar-nav-item {
  display: block;
  width: 100%;
  padding: 0.65rem 1.5rem;
  background: transparent;
  border: none;
  border-left: 2px solid transparent;
  text-align: left;
  font-family: 'Space Grotesk', sans-serif;
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--text-muted);
  transition: color 150ms ease, background 150ms ease, border-color 150ms ease;
}
.sidebar-nav-item:hover {
  color: var(--text-soft);
  background: var(--accent-dim);
}
.sidebar-nav-item.active {
  color: var(--text);
  background: var(--accent-dim);
  border-left-color: var(--accent);
  font-weight: 600;
}

.sidebar-footer {
  padding: 1.25rem 1.5rem;
  border-top: 1px solid var(--border);
  flex-shrink: 0;
}

.lang-toggle {
  display: flex;
  align-items: center;
  gap: 0.35rem;
  margin-bottom: 0.75rem;
}
.lang-toggle button {
  background: transparent;
  border: none;
  font-size: 0.78rem;
  font-weight: 700;
  font-family: 'Space Grotesk', sans-serif;
  letter-spacing: 0.05em;
  color: var(--text-muted);
  padding: 0.2rem 0.4rem;
  border-radius: 4px;
  transition: color 150ms ease, background 150ms ease;
}
.lang-toggle button.active {
  color: var(--accent);
  background: var(--accent-dim);
}
.lang-divider {
  color: var(--border-strong);
  font-size: 0.75rem;
}

.sidebar-copyright {
  font-size: 0.7rem;
  color: var(--text-muted);
  line-height: 1.5;
}

/* ---- Content Area ---- */

.content-area {
  flex: 1;
  margin-left: var(--sidebar-width);
  padding: 0 4rem;
}

/* Mobile — hidden by default */
.mobile-nav { display: none; }
.mobile-lang-toggle { display: none; }

/* =========================================
   CURSOR
   ========================================= */

.cursor-dot,
.cursor-ring {
  position: fixed;
  pointer-events: none;
  transform: translate(-50%, -50%);
  z-index: 9999;
  border-radius: 50%;
}
.cursor-dot {
  width: 6px;
  height: 6px;
  background: var(--accent);
}
.cursor-ring {
  width: 28px;
  height: 28px;
  border: 1px solid rgba(205, 127, 50, 0.6);
  transition: width 150ms ease, height 150ms ease, left 150ms ease, top 150ms ease;
}
.cursor-ring.expanded {
  width: 38px;
  height: 38px;
}

/* =========================================
   PRELOADER
   ========================================= */

.preloader {
  position: fixed;
  inset: 0;
  background: var(--bg);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 9998;
  gap: 0.5rem;
}
.preloader-name {
  font-family: 'Syne', sans-serif;
  font-size: clamp(1.25rem, 4vw, 2rem);
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.02em;
}
.preloader-role {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--accent);
  letter-spacing: 0.05em;
}
.preloader-bar {
  width: 180px;
  height: 2px;
  background: var(--border);
  border-radius: 2px;
  overflow: hidden;
  margin-top: 1.5rem;
}
.preloader-bar-fill {
  height: 100%;
  background: var(--accent);
  border-radius: 2px;
  animation: preloader-fill 1.2s ease-out forwards;
}
@keyframes preloader-fill {
  from { width: 0%; }
  to { width: 100%; }
}

/* =========================================
   HERO
   ========================================= */

.hero-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 6rem 0 4rem;
}
.hero-inner {
  width: 100%;
  max-width: 860px;
}
.hero-name {
  font-size: clamp(44px, 7vw, 72px);
  font-weight: 900;
  color: var(--text);
  line-height: 1.0;
  margin-bottom: 0.5rem;
}
.hero-name-accent { color: var(--accent); }
.hero-role {
  font-size: 1.1rem;
  font-weight: 500;
  color: var(--accent);
  margin-bottom: 1rem;
  font-family: 'Space Grotesk', sans-serif;
}
.hero-body {
  font-size: 1rem;
  color: var(--text-muted);
  max-width: 540px;
  line-height: 1.7;
  margin-bottom: 2rem;
}
.hero-skills-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.75rem;
  margin-bottom: 2.5rem;
  max-width: 560px;
}
.hero-skill-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 0.875rem 0.75rem;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  transition: border-color 150ms ease, transform 150ms ease;
}
.hero-skill-card:hover {
  border-color: var(--border-strong);
  transform: translateY(-2px);
}
.hero-skill-icon { font-size: 1.1rem; }
.hero-skill-label {
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--text);
}
.hero-skill-sub {
  font-size: 0.65rem;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 0.1em;
}
.hero-actions {
  display: flex;
  gap: 0.875rem;
  flex-wrap: wrap;
}

/* =========================================
   SECTION COMMON SPACING
   ========================================= */

.bento-section,
.projects-section,
.certificates-section,
.experience-section,
.education-section,
.contact-section {
  max-width: 860px;
  padding: 6rem 0 4rem;
}

/* =========================================
   ABOUT / BENTO
   ========================================= */

.about-bio {
  display: flex;
  flex-direction: column;
  gap: 0.875rem;
  max-width: 640px;
  margin-bottom: 2.5rem;
}
.about-bio p {
  color: var(--text-soft);
  line-height: 1.7;
}
.capability-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}
.capability-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.5rem;
  transition: border-color 150ms ease, transform 150ms ease;
}
.capability-card:hover {
  border-color: var(--border-strong);
  transform: translateY(-3px);
}
.capability-title {
  font-size: 1rem;
  font-weight: 700;
  color: var(--accent);
  margin-bottom: 0.5rem;
}
.capability-body {
  font-size: 0.875rem;
  color: var(--text-muted);
  line-height: 1.6;
}

/* =========================================
   PROJECTS
   ========================================= */

.projects-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.25rem;
}
.project-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: border-color 150ms ease;
}
.project-card:hover { border-color: var(--border-strong); }
.project-card-body {
  padding: 1.5rem;
  flex: 1;
}
.project-meta-row {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  margin-bottom: 0.75rem;
}
.project-role-label {
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent);
  background: var(--accent-dim);
  border: 1px solid var(--border);
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
}
.project-period-label {
  font-size: 0.68rem;
  font-weight: 600;
  color: var(--text-muted);
  border: 1px solid var(--border);
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
}
.project-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 0.6rem;
}
.project-summary {
  font-size: 0.875rem;
  color: var(--text-muted);
  line-height: 1.65;
  margin-bottom: 1rem;
}
.project-tech-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-bottom: 0.5rem;
}
.tech-pill {
  font-size: 0.7rem;
  padding: 0.2rem 0.6rem;
  border-radius: 20px;
  background: var(--accent-dim);
  border: 1px solid var(--border);
  color: var(--text-soft);
}
.project-expanded {
  overflow: hidden;
  border-top: 1px solid var(--border);
  margin-top: 1rem;
  padding-top: 1rem;
}
.project-gallery { margin-bottom: 1.25rem; }
.project-gallery-label {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 0.75rem;
}
.project-gallery-track {
  display: flex;
  gap: 0.75rem;
  overflow-x: auto;
  padding-bottom: 0.75rem;
}
.project-gallery-img {
  height: 180px;
  width: auto;
  flex-shrink: 0;
  border-radius: 6px;
  border: 1px solid var(--border);
  object-fit: cover;
}
.project-contributions-label {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 0.5rem;
}
.project-details-list {
  padding-left: 1.2rem;
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  margin-bottom: 1rem;
}
.project-details-list li {
  font-size: 0.875rem;
  color: var(--text-soft);
  line-height: 1.6;
}
.project-repo-link {
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--accent);
}
.project-repo-link:hover { text-decoration: underline; }
.project-toggle-btn {
  width: 100%;
  padding: 0.875rem;
  background: transparent;
  border: none;
  border-top: 1px solid var(--border);
  color: var(--text-muted);
  font-size: 0.875rem;
  font-weight: 600;
  font-family: 'Space Grotesk', sans-serif;
  transition: background 150ms ease, color 150ms ease;
}
.project-toggle-btn:hover,
.project-toggle-btn.expanded {
  background: var(--accent-dim);
  color: var(--accent);
}

/* =========================================
   CERTIFICATES
   ========================================= */

.certificates-list {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}
.certificate-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  display: grid;
  grid-template-columns: 240px 1fr;
  transition: border-color 150ms ease;
}
.certificate-card:hover { border-color: var(--border-strong); }
.certificate-image-wrap {
  position: relative;
  overflow: hidden;
  min-height: 160px;
}
.certificate-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.certificate-badge {
  position: absolute;
  top: 0.75rem;
  left: 0.75rem;
  background: var(--bg-card);
  border: 1px solid var(--accent);
  color: var(--accent);
  font-size: 0.68rem;
  font-weight: 700;
  padding: 0.25rem 0.6rem;
  border-radius: 4px;
  letter-spacing: 0.05em;
}
.certificate-info {
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 0.35rem;
}
.certificate-category {
  font-size: 0.68rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--accent);
  font-weight: 700;
}
.certificate-title {
  font-size: 1.05rem;
  font-weight: 700;
  color: var(--text);
  line-height: 1.3;
}
.certificate-issuer {
  font-size: 0.875rem;
  color: var(--text-muted);
}
.certificate-year {
  font-size: 0.8rem;
  color: var(--text-muted);
}
.certificate-view-btn {
  margin-top: 0.75rem;
  align-self: flex-start;
}

/* =========================================
   EXPERIENCE
   ========================================= */

.timeline {
  position: relative;
  padding-left: 1.75rem;
}
.timeline::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0.5rem;
  bottom: 0;
  width: 1px;
  background: var(--border);
}
.timeline-entry {
  position: relative;
  margin-bottom: 1.5rem;
}
.timeline-dot {
  position: absolute;
  left: -1.75rem;
  top: 1rem;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 2px solid var(--accent);
  background: var(--bg);
  transform: translateX(-4px);
}
.timeline-entry:first-child .timeline-dot {
  background: var(--accent);
}
.timeline-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.25rem 1.5rem;
  transition: border-color 150ms ease, transform 150ms ease;
}
.timeline-card:hover {
  border-color: var(--border-strong);
  transform: translateX(4px);
}
.timeline-period {
  font-size: 0.75rem;
  color: var(--text-muted);
  display: block;
  margin-bottom: 0.3rem;
}
.timeline-role {
  font-size: 1rem;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 0.2rem;
}
.timeline-company {
  font-size: 0.875rem;
  color: var(--accent);
  font-weight: 600;
  margin-bottom: 0.75rem;
}
.timeline-desc {
  font-size: 0.875rem;
  color: var(--text-muted);
  line-height: 1.65;
}

/* =========================================
   EDUCATION
   ========================================= */

.education-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.education-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.5rem;
  display: flex;
  gap: 1.25rem;
  align-items: flex-start;
  transition: border-color 150ms ease;
}
.education-card:hover { border-color: var(--border-strong); }
.education-logo {
  width: 48px;
  height: 48px;
  object-fit: contain;
  border-radius: 6px;
  flex-shrink: 0;
  background: white;
  padding: 4px;
}
.education-info { flex: 1; }
.education-institution {
  font-size: 1.05rem;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 0.2rem;
}
.education-degree {
  font-size: 0.875rem;
  color: var(--accent);
  font-weight: 600;
  margin-bottom: 0.2rem;
}
.education-period {
  font-size: 0.8rem;
  color: var(--text-muted);
  margin-bottom: 0.5rem;
}
.education-desc {
  font-size: 0.875rem;
  color: var(--text-muted);
  line-height: 1.6;
}

/* =========================================
   CONTACT
   ========================================= */

.contact-grid {
  display: grid;
  grid-template-columns: 1fr 1.4fr;
  gap: 1.5rem;
  align-items: start;
}
.contact-title {
  font-size: clamp(1.4rem, 3vw, 2rem);
  margin-bottom: 0.875rem;
}
.contact-body {
  font-size: 0.9rem;
  color: var(--text-muted);
  line-height: 1.65;
  margin-bottom: 1.5rem;
}
.contact-social-links {
  display: flex;
  flex-direction: column;
  gap: 0.625rem;
}
.contact-social-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  color: var(--text-soft);
  font-size: 0.875rem;
  font-weight: 500;
  transition: border-color 150ms ease, background 150ms ease;
}
.contact-social-link:hover {
  border-color: var(--border-strong);
  background: var(--accent-dim);
  color: var(--text);
}
.contact-form-panel {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.75rem;
}
.contact-form {
  display: flex;
  flex-direction: column;
  gap: 0.875rem;
}
.contact-form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.875rem;
}
.form-input,
.form-textarea {
  width: 100%;
  padding: 0.75rem 1rem;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  color: var(--text);
  font-size: 0.875rem;
  outline: none;
  transition: border-color 150ms ease;
}
.form-input:focus,
.form-textarea:focus { border-color: var(--border-strong); }
.form-input::placeholder,
.form-textarea::placeholder { color: var(--text-muted); }
.form-textarea { resize: vertical; min-height: 120px; }
.contact-sent {
  text-align: center;
  padding: 2rem 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
}
.contact-sent-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--accent);
  font-family: 'Syne', sans-serif;
}
.contact-sent-body {
  color: var(--text-muted);
  max-width: 300px;
}

/* =========================================
   RESPONSIVE
   ========================================= */

@media (max-width: 768px) {
  .sidebar { display: none; }

  .mobile-nav {
    display: flex;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    height: 60px;
    background: var(--bg-card);
    border-top: 1px solid var(--border);
    z-index: 100;
  }
  .mobile-nav-item {
    flex: 1;
    background: transparent;
    border: none;
    color: var(--text-muted);
    font-size: 0.6rem;
    font-weight: 600;
    font-family: 'Space Grotesk', sans-serif;
    letter-spacing: 0.04em;
    text-transform: uppercase;
    padding: 0.5rem 0.25rem;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: color 150ms ease, background 150ms ease;
  }
  .mobile-nav-item.active {
    color: var(--accent);
    background: var(--accent-dim);
  }

  .mobile-lang-toggle {
    display: flex;
    position: fixed;
    top: 1rem;
    right: 1rem;
    align-items: center;
    gap: 0.35rem;
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 0.3rem 0.6rem;
    z-index: 101;
  }
  .mobile-lang-toggle button {
    background: transparent;
    border: none;
    font-size: 0.75rem;
    font-weight: 700;
    color: var(--text-muted);
    padding: 0.15rem 0.3rem;
    border-radius: 3px;
  }
  .mobile-lang-toggle button.active { color: var(--accent); }
  .mobile-lang-toggle span { color: var(--border-strong); font-size: 0.7rem; }

  .content-area {
    margin-left: 0;
    padding: 1.5rem 1.25rem;
    padding-bottom: calc(60px + 2rem);
  }

  .hero-section { min-height: auto; padding: 5rem 0 3rem; }
  .hero-skills-grid { grid-template-columns: repeat(2, 1fr); }
  .projects-grid { grid-template-columns: 1fr; }
  .capability-grid { grid-template-columns: 1fr; }
  .certificate-card { grid-template-columns: 1fr; }
  .contact-grid { grid-template-columns: 1fr; }
  .contact-form-row { grid-template-columns: 1fr; }

  .bento-section,
  .projects-section,
  .certificates-section,
  .experience-section,
  .education-section,
  .contact-section {
    padding: 4rem 0 3rem;
  }
}

@media (max-width: 480px) {
  .content-area { padding: 1rem; padding-bottom: calc(60px + 1.5rem); }
  .hero-name { font-size: clamp(36px, 11vw, 52px); }
  .hero-skills-grid { grid-template-columns: repeat(2, 1fr); }
}
```

- [ ] **Step 2: Verify no CSS parse errors**

```
npm run dev
```

Open http://localhost:5173. Check DevTools console — zero CSS parse errors expected. Visual layout still broken (App.jsx not updated yet).

- [ ] **Step 3: Commit**

```
git add src/App.css
git commit -m "feat: layout shell + all component CSS styles"
```

---

## Task 3: App.jsx — Layout Shell

**Files:**
- Rewrite: `src/App.jsx`

- [ ] **Step 1: Rewrite src/App.jsx**

```jsx
import { useEffect, useMemo, useState } from 'react'
import Background3D from './components/Background3D'
import Sidebar from './components/Sidebar'
import CustomCursor from './components/CustomCursor'
import Preloader from './components/Preloader'
import Hero from './components/Hero'
import BentoSection from './components/BentoSection'
import Projects from './components/Projects'
import Certificates from './components/Certificates'
import Experience from './components/Experience'
import Education from './components/Education'
import Terminal from './components/Terminal'
import Contact from './components/Contact'
import { certificates, education, experiences, getCopy, projects } from './data/portfolioContent'

function App() {
  const [language, setLanguage] = useState(() =>
    localStorage.getItem('portfolio-language') || 'en'
  )
  const copy = useMemo(() => getCopy(language), [language])

  useEffect(() => {
    document.documentElement.lang = language
    localStorage.setItem('portfolio-language', language)
  }, [language])

  return (
    <>
      <Preloader />
      <CustomCursor />
      <Background3D />
      <div className="app-shell">
        <Sidebar copy={copy.nav} language={language} onLanguageChange={setLanguage} />
        <main className="content-area">
          <Hero copy={copy.hero} />
          <BentoSection copy={copy.about} />
          <Projects copy={copy.projects} language={language} projects={projects} />
          <Certificates copy={copy.certificates} certificates={certificates} />
          <Experience copy={copy.experience} language={language} experiences={experiences} />
          <Education copy={copy.education} language={language} education={education} />
          <Terminal language={language} />
          <Contact copy={copy.contact} />
        </main>
      </div>
    </>
  )
}

export default App
```

Note: `Sidebar.jsx` doesn't exist yet — Vite will show an import error. That's expected. Continue to Task 4 immediately.

- [ ] **Step 2: Commit**

```
git add src/App.jsx
git commit -m "feat: app shell — sidebar + content-area layout"
```

---

## Task 4: Sidebar.jsx

**Files:**
- Create: `src/components/Sidebar.jsx`

- [ ] **Step 1: Create src/components/Sidebar.jsx**

```jsx
import { useEffect, useState } from 'react'

const NAV_ITEMS = [
  { id: 'hero', labelKey: 'home' },
  { id: 'about', labelKey: 'about' },
  { id: 'projects', labelKey: 'projects' },
  { id: 'certificates', labelKey: 'certificates' },
  { id: 'experience', labelKey: 'experience' },
  { id: 'education', labelKey: 'education' },
  { id: 'contact', labelKey: 'contact' },
]

export default function Sidebar({ copy, language, onLanguageChange }) {
  const [active, setActive] = useState('hero')

  useEffect(() => {
    const sections = NAV_ITEMS
      .map(item => document.getElementById(item.id))
      .filter(Boolean)

    const observer = new IntersectionObserver(
      entries => {
        entries.forEach(entry => {
          if (entry.isIntersecting) setActive(entry.target.id)
        })
      },
      { rootMargin: '-40% 0px -55% 0px', threshold: 0 }
    )

    sections.forEach(s => observer.observe(s))
    return () => observer.disconnect()
  }, [])

  const scrollTo = id => {
    document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
  }

  return (
    <>
      <aside className="sidebar">
        <div className="sidebar-logo">
          Bayu<span className="sidebar-logo-dot">.</span>
        </div>

        <nav className="sidebar-nav" aria-label="Main navigation">
          {NAV_ITEMS.map(item => (
            <button
              key={item.id}
              type="button"
              className={`sidebar-nav-item${active === item.id ? ' active' : ''}`}
              onClick={() => scrollTo(item.id)}
            >
              {copy[item.labelKey]}
            </button>
          ))}
        </nav>

        <div className="sidebar-footer">
          <div className="lang-toggle">
            <button
              type="button"
              className={language === 'en' ? 'active' : ''}
              onClick={() => onLanguageChange('en')}
            >EN</button>
            <span className="lang-divider">|</span>
            <button
              type="button"
              className={language === 'id' ? 'active' : ''}
              onClick={() => onLanguageChange('id')}
            >ID</button>
          </div>
          <p className="sidebar-copyright">© 2026 Muhammad Bayu Satrio</p>
        </div>
      </aside>

      <nav className="mobile-nav" aria-label="Mobile navigation">
        {NAV_ITEMS.slice(0, 5).map(item => (
          <button
            key={item.id}
            type="button"
            className={`mobile-nav-item${active === item.id ? ' active' : ''}`}
            onClick={() => scrollTo(item.id)}
          >
            {copy[item.labelKey]}
          </button>
        ))}
      </nav>

      <div className="mobile-lang-toggle">
        <button
          type="button"
          className={language === 'en' ? 'active' : ''}
          onClick={() => onLanguageChange('en')}
        >EN</button>
        <span>|</span>
        <button
          type="button"
          className={language === 'id' ? 'active' : ''}
          onClick={() => onLanguageChange('id')}
        >ID</button>
      </div>
    </>
  )
}
```

- [ ] **Step 2: Verify sidebar renders**

Open http://localhost:5173. The sidebar should be visible on the left with "Bayu." logo and nav links. Other components will still show import errors — that's fine.

- [ ] **Step 3: Commit**

```
git add src/components/Sidebar.jsx
git commit -m "feat: Sidebar — fixed nav + IntersectionObserver active tracking + mobile bottom nav"
```

---

## Task 5: CustomCursor.jsx

**Files:**
- Rewrite: `src/components/CustomCursor.jsx`

- [ ] **Step 1: Rewrite src/components/CustomCursor.jsx**

```jsx
import { useEffect, useRef } from 'react'

export default function CustomCursor() {
  const dotRef = useRef(null)
  const ringRef = useRef(null)

  useEffect(() => {
    if (window.matchMedia('(hover: none)').matches) return

    const dot = dotRef.current
    const ring = ringRef.current
    if (!dot || !ring) return

    const onMove = e => {
      dot.style.left = `${e.clientX}px`
      dot.style.top = `${e.clientY}px`
      ring.style.left = `${e.clientX}px`
      ring.style.top = `${e.clientY}px`
    }

    const onEnter = () => ring.classList.add('expanded')
    const onLeave = () => ring.classList.remove('expanded')

    window.addEventListener('mousemove', onMove)

    const els = document.querySelectorAll('a, button, [role="button"]')
    els.forEach(el => {
      el.addEventListener('mouseenter', onEnter)
      el.addEventListener('mouseleave', onLeave)
    })

    return () => {
      window.removeEventListener('mousemove', onMove)
      els.forEach(el => {
        el.removeEventListener('mouseenter', onEnter)
        el.removeEventListener('mouseleave', onLeave)
      })
    }
  }, [])

  return (
    <>
      <div className="cursor-dot" ref={dotRef} />
      <div className="cursor-ring" ref={ringRef} />
    </>
  )
}
```

- [ ] **Step 2: Verify cursor**

In the browser at http://localhost:5173, move the mouse. Verify a small bronze dot follows the cursor precisely, with a slightly lagging ring around it. Hover a button — ring should expand.

- [ ] **Step 3: Commit**

```
git add src/components/CustomCursor.jsx
git commit -m "feat: lightweight custom cursor — dot + lag ring, no canvas"
```

---

## Task 6: Preloader.jsx

**Files:**
- Rewrite: `src/components/Preloader.jsx`

- [ ] **Step 1: Rewrite src/components/Preloader.jsx**

```jsx
import { useState, useEffect } from 'react'
import { motion, AnimatePresence } from 'framer-motion'

export default function Preloader() {
  const [visible, setVisible] = useState(true)

  useEffect(() => {
    const timer = setTimeout(() => setVisible(false), 2200)
    return () => clearTimeout(timer)
  }, [])

  return (
    <AnimatePresence>
      {visible && (
        <motion.div
          className="preloader"
          exit={{ y: '-100%' }}
          transition={{ duration: 0.5, ease: [0.76, 0, 0.24, 1] }}
        >
          <motion.p
            className="preloader-name"
            initial={{ opacity: 0, y: 16 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6, delay: 0.2 }}
          >
            Muhammad Bayu Satrio
          </motion.p>
          <motion.p
            className="preloader-role"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.5, delay: 0.5 }}
          >
            AI Engineer / Data Analyst
          </motion.p>
          <div className="preloader-bar">
            <div className="preloader-bar-fill" />
          </div>
        </motion.div>
      )}
    </AnimatePresence>
  )
}
```

- [ ] **Step 2: Verify preloader sequence**

Reload http://localhost:5173. Verify:
1. Screen starts as solid `#1C2B2D`
2. "Muhammad Bayu Satrio" fades in (no glitch, no flicker)
3. "AI Engineer / Data Analyst" in bronze fades in below
4. Bronze progress bar fills from left to right
5. After ~2.2s, the preloader slides up and out (not a fade — a slide)
6. No leftover DOM element blocking interaction after dismissal

- [ ] **Step 3: Commit**

```
git add src/components/Preloader.jsx
git commit -m "feat: clean preloader — fade-in name + progress bar + slide-out"
```

---

## Task 7: Background3D.jsx (Color Update)

**Files:**
- Update: `src/components/Background3D.jsx`

- [ ] **Step 1: Update src/components/Background3D.jsx**

```jsx
import { Canvas, useFrame } from '@react-three/fiber'
import { Stars } from '@react-three/drei'
import { useEffect, useRef, useState } from 'react'

function RotatingKnot() {
  const meshRef = useRef()
  const [mousePos, setMousePos] = useState({ x: 0, y: 0 })

  useEffect(() => {
    const handleMouseMove = e => {
      setMousePos({
        x: (e.clientX / window.innerWidth) * 2 - 1,
        y: -(e.clientY / window.innerHeight) * 2 + 1,
      })
    }
    window.addEventListener('mousemove', handleMouseMove)
    return () => window.removeEventListener('mousemove', handleMouseMove)
  }, [])

  useFrame((state, delta) => {
    if (meshRef.current) {
      meshRef.current.rotation.x += delta * 0.2
      meshRef.current.rotation.y += delta * 0.3
      meshRef.current.rotation.x +=
        (mousePos.y * 0.5 - meshRef.current.rotation.x) * 0.05
      meshRef.current.rotation.y +=
        (mousePos.x * 0.5 - meshRef.current.rotation.y) * 0.05
    }
  })

  return (
    <mesh ref={meshRef} position={[0, 0, 0]}>
      <torusKnotGeometry args={[9, 2.5, 120, 16]} />
      <meshBasicMaterial color="#CD7F32" wireframe transparent opacity={0.12} />
    </mesh>
  )
}

export default function Background3D() {
  return (
    <div style={{
      position: 'fixed',
      top: 0,
      left: 0,
      width: '100vw',
      height: '100vh',
      zIndex: -1,
      pointerEvents: 'none',
      opacity: 0.4,
    }}>
      <Canvas camera={{ position: [0, 0, 30] }}>
        <RotatingKnot />
        <Stars
          radius={100}
          depth={50}
          count={800}
          factor={3}
          saturation={0}
          fade
          speed={0.8}
          color="#CD7F32"
        />
      </Canvas>
    </div>
  )
}
```

- [ ] **Step 2: Verify 3D background**

In the browser, verify the torus knot wireframe and stars are warm bronze tones instead of cyan/purple. The effect should be subtle (opacity 0.4).

- [ ] **Step 3: Commit**

```
git add src/components/Background3D.jsx
git commit -m "feat: Background3D — bronze particles + lowered opacity"
```

---

## Task 8: Hero.jsx

**Files:**
- Rewrite: `src/components/Hero.jsx`

- [ ] **Step 1: Rewrite src/components/Hero.jsx**

```jsx
import { motion } from 'framer-motion'

const SKILL_CARDS = [
  { icon: '🧠', label: 'IndoBERTweet', sub: 'NLP' },
  { icon: '⚡', label: 'ONNX Runtime', sub: 'Inference' },
  { icon: '📊', label: 'Analytics', sub: 'Data' },
  { icon: '🔧', label: 'Applied', sub: 'Systems' },
]

export default function Hero({ copy }) {
  return (
    <section id="hero" className="hero-section">
      <motion.div
        className="hero-inner"
        initial={{ opacity: 0, y: 30 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.8, ease: 'easeOut' }}
      >
        <span className="eyebrow">{copy.eyebrow}</span>

        <h1 className="hero-name">
          Muhammad<br />
          Bayu <span className="hero-name-accent">Satrio</span>
        </h1>

        <p className="hero-role">{copy.chipTitle}</p>
        <p className="hero-body">{copy.body}</p>

        <div className="hero-skills-grid">
          {SKILL_CARDS.map(card => (
            <div key={card.label} className="hero-skill-card">
              <span className="hero-skill-icon">{card.icon}</span>
              <span className="hero-skill-label">{card.label}</span>
              <span className="hero-skill-sub">{card.sub}</span>
            </div>
          ))}
        </div>

        <div className="hero-actions">
          <a href="#projects" className="btn btn-primary">{copy.primaryCta}</a>
          <a
            href="/Resume_Muhammad_Bayu_Satrio.pdf"
            download="Resume_Muhammad_Bayu_Satrio.pdf"
            className="btn btn-secondary"
          >
            {copy.secondaryCta}
          </a>
        </div>
      </motion.div>
    </section>
  )
}
```

- [ ] **Step 2: Verify Hero**

In the browser: the hero should show "Muhammad / Bayu Satrio" in large Syne 900 font, "Satrio" in bronze. Below: role in bronze, bio in muted white, 2×2 skills grid with dark cards, and two CTA buttons.

- [ ] **Step 3: Commit**

```
git add src/components/Hero.jsx
git commit -m "feat: Hero — Syne 900 name + 2×2 skills grid + bronze CTAs"
```

---

## Task 9: BentoSection.jsx

**Files:**
- Rewrite: `src/components/BentoSection.jsx`

- [ ] **Step 1: Rewrite src/components/BentoSection.jsx**

```jsx
import { motion } from 'framer-motion'

export default function BentoSection({ copy }) {
  return (
    <motion.section
      id="about"
      className="bento-section"
      initial={{ opacity: 0, y: 24 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.2 }}
      transition={{ duration: 0.6 }}
    >
      <span className="eyebrow">{copy.eyebrow}</span>
      <h2 className="section-title">{copy.title}</h2>

      <div className="about-bio">
        {copy.body.map((para, i) => (
          <p key={i}>{para}</p>
        ))}
      </div>

      <div className="capability-grid">
        {copy.capabilities.map((cap, i) => (
          <motion.div
            key={cap.title}
            className="capability-card"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: i * 0.08 }}
          >
            <h3 className="capability-title">{cap.title}</h3>
            <p className="capability-body">{cap.body}</p>
          </motion.div>
        ))}
      </div>
    </motion.section>
  )
}
```

- [ ] **Step 2: Verify About section**

Scroll to the About section. Should show 2 bio paragraphs then a 2×2 grid of capability cards (AI/NLP, Analytics, Deployment, Applied Systems) with bronze titles.

- [ ] **Step 3: Commit**

```
git add src/components/BentoSection.jsx
git commit -m "feat: BentoSection — about bio + 4 capability cards"
```

---

## Task 10: Projects.jsx

**Files:**
- Rewrite: `src/components/Projects.jsx`

- [ ] **Step 1: Rewrite src/components/Projects.jsx**

```jsx
import { useState } from 'react'
import { motion, AnimatePresence } from 'framer-motion'

export default function Projects({ copy, language, projects }) {
  const [expandedId, setExpandedId] = useState(null)

  return (
    <motion.section
      id="projects"
      className="projects-section"
      initial={{ opacity: 0, y: 24 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.1 }}
      transition={{ duration: 0.6 }}
    >
      <span className="eyebrow">{copy.eyebrow}</span>
      <h2 className="section-title">{copy.title}</h2>

      <div className="projects-grid">
        {projects.map((project, index) => {
          const id = project.slug
          const isExpanded = expandedId === id
          const summary = project.summary[language] ?? project.summary.en
          const details = project.details[language] ?? project.details.en

          return (
            <motion.article
              key={id}
              className="project-card"
              initial={{ opacity: 0, y: 30 }}
              whileInView={{ opacity: 1, y: 0 }}
              viewport={{ once: true, amount: 0.1 }}
              transition={{ duration: 0.5, delay: index * 0.08 }}
              layout
            >
              <div className="project-card-body">
                <div className="project-meta-row">
                  <span className="project-role-label">{project.role}</span>
                  <span className="project-period-label">{project.period}</span>
                </div>

                <h3 className="project-title">{project.title}</h3>
                <p className="project-summary">{summary}</p>

                <div className="project-tech-list">
                  {project.tech.map(tech => (
                    <span key={tech} className="tech-pill">{tech}</span>
                  ))}
                </div>

                <AnimatePresence>
                  {isExpanded && (
                    <motion.div
                      className="project-expanded"
                      initial={{ opacity: 0, height: 0 }}
                      animate={{ opacity: 1, height: 'auto' }}
                      exit={{ opacity: 0, height: 0 }}
                      transition={{ duration: 0.3 }}
                    >
                      {project.images.length > 0 && (
                        <div className="project-gallery">
                          <p className="project-gallery-label">{copy.visualDocumentation}</p>
                          <div className="project-gallery-track">
                            {project.images.map((img, i) => (
                              <img
                                key={img}
                                src={img}
                                alt={`${project.title} ${i + 1}`}
                                className="project-gallery-img"
                              />
                            ))}
                          </div>
                        </div>
                      )}

                      <p className="project-contributions-label">{copy.contributions}</p>
                      <ul className="project-details-list">
                        {details.map(detail => (
                          <li key={detail}>{detail}</li>
                        ))}
                      </ul>

                      {project.repo && (
                        <a
                          href={project.repo}
                          target="_blank"
                          rel="noreferrer"
                          className="project-repo-link"
                        >
                          {copy.repo} →
                        </a>
                      )}
                    </motion.div>
                  )}
                </AnimatePresence>
              </div>

              <button
                type="button"
                className={`project-toggle-btn${isExpanded ? ' expanded' : ''}`}
                onClick={() => setExpandedId(isExpanded ? null : id)}
                aria-expanded={isExpanded}
              >
                {isExpanded ? copy.collapse : copy.expand}
              </button>
            </motion.article>
          )
        })}
      </div>
    </motion.section>
  )
}
```

- [ ] **Step 2: Verify Projects**

Scroll to Projects. Should see a 2-column grid of 4 project cards. Click "View Details" on any card — it should animate open showing the image gallery and bullet points. Click "View Repo" — should open GitHub in new tab (ACETRA has no repo, so repo link should not appear for that card).

- [ ] **Step 3: Commit**

```
git add src/components/Projects.jsx
git commit -m "feat: Projects — 2-col expandable cards with gallery + details"
```

---

## Task 11: Certificates.jsx

**Files:**
- Rewrite: `src/components/Certificates.jsx`

- [ ] **Step 1: Rewrite src/components/Certificates.jsx**

```jsx
import { motion } from 'framer-motion'

export default function Certificates({ copy, certificates }) {
  return (
    <motion.section
      id="certificates"
      className="certificates-section"
      initial={{ opacity: 0, y: 24 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.2 }}
      transition={{ duration: 0.6 }}
    >
      <span className="eyebrow">{copy.eyebrow}</span>
      <h2 className="section-title">{copy.title}</h2>
      <p className="section-body">{copy.body}</p>

      <div className="certificates-list">
        {certificates.map(cert => (
          <motion.article
            key={cert.title}
            className="certificate-card"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5 }}
          >
            {cert.image && (
              <div className="certificate-image-wrap">
                <img src={cert.image} alt={cert.title} className="certificate-image" />
                <div className="certificate-badge">🥈 2nd Place · National</div>
              </div>
            )}
            <div className="certificate-info">
              <span className="certificate-category">{cert.category}</span>
              <h3 className="certificate-title">{cert.title}</h3>
              <p className="certificate-issuer">{cert.issuer}</p>
              <p className="certificate-year">{cert.year}</p>
              {cert.image && (
                <a
                  href={cert.image}
                  target="_blank"
                  rel="noreferrer"
                  className="btn btn-secondary certificate-view-btn"
                >
                  {copy.view}
                </a>
              )}
            </div>
          </motion.article>
        ))}
      </div>
    </motion.section>
  )
}
```

- [ ] **Step 2: Verify Certificates**

Scroll to Certificates. The GEMASTIK XVIII card should show the certificate image on the left with the bronze "🥈 2nd Place · National" badge overlaid. Right side: Competition label, full title, issuer, date, and "View Certificate" button.

- [ ] **Step 3: Commit**

```
git add src/components/Certificates.jsx
git commit -m "feat: Certificates — featured GEMASTIK card with bronze badge"
```

---

## Task 12: Experience.jsx

**Files:**
- Rewrite: `src/components/Experience.jsx`

- [ ] **Step 1: Rewrite src/components/Experience.jsx**

```jsx
import { motion } from 'framer-motion'

export default function Experience({ copy, language, experiences }) {
  return (
    <motion.section
      id="experience"
      className="experience-section"
      initial={{ opacity: 0, y: 24 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.1 }}
      transition={{ duration: 0.6 }}
    >
      <span className="eyebrow">{copy.eyebrow}</span>
      <h2 className="section-title">{copy.title}</h2>

      <div className="timeline">
        {experiences.map((exp, i) => (
          <motion.div
            key={exp.company}
            className="timeline-entry"
            initial={{ opacity: 0, x: -20 }}
            whileInView={{ opacity: 1, x: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: i * 0.1 }}
          >
            <div className="timeline-dot" />
            <div className="timeline-card">
              <span className="timeline-period">{exp.period}</span>
              <h3 className="timeline-role">{exp.role}</h3>
              <p className="timeline-company">{exp.company}</p>
              <p className="timeline-desc">{exp.desc[language] ?? exp.desc.en}</p>
            </div>
          </motion.div>
        ))}
      </div>
    </motion.section>
  )
}
```

- [ ] **Step 2: Verify Experience**

Scroll to Experience. Should show 4 timeline entries with a vertical bronze line on the left. The first entry (most recent: AI Engineer) should have a filled bronze dot; others have outlined dots. Cards slide in from left on scroll.

- [ ] **Step 3: Commit**

```
git add src/components/Experience.jsx
git commit -m "feat: Experience — vertical bronze timeline"
```

---

## Task 13: Education.jsx

**Files:**
- Rewrite: `src/components/Education.jsx`

- [ ] **Step 1: Rewrite src/components/Education.jsx**

```jsx
import { motion } from 'framer-motion'

export default function Education({ copy, language, education }) {
  return (
    <motion.section
      id="education"
      className="education-section"
      initial={{ opacity: 0, y: 24 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.2 }}
      transition={{ duration: 0.6 }}
    >
      <span className="eyebrow">{copy.eyebrow}</span>
      <h2 className="section-title">{copy.title}</h2>

      <div className="education-list">
        {education.map((edu, i) => (
          <motion.article
            key={edu.institution}
            className="education-card"
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: i * 0.1 }}
          >
            {edu.logo ? (
              <img
                src={edu.logo}
                alt={edu.institution}
                className="education-logo"
              />
            ) : (
              <div className="education-logo" style={{
                background: 'var(--accent-dim)',
                border: '1px solid var(--border)',
                display: 'flex',
                alignItems: 'center',
                justifyContent: 'center',
                fontSize: '1.25rem',
              }}>🎓</div>
            )}
            <div className="education-info">
              <h3 className="education-institution">{edu.institution}</h3>
              <p className="education-degree">{edu.degree}</p>
              <p className="education-period">{edu.period}</p>
              <p className="education-desc">{edu.desc[language] ?? edu.desc.en}</p>
            </div>
          </motion.article>
        ))}
      </div>
    </motion.section>
  )
}
```

- [ ] **Step 2: Verify Education**

Scroll to Education. Telkom University card should show the university logo on the left. SMAN 3 Banjarmasin (no logo) should show a 🎓 fallback icon in a bronze-tinted box.

- [ ] **Step 3: Commit**

```
git add src/components/Education.jsx
git commit -m "feat: Education — institution cards with logo fallback"
```

---

## Task 14: Terminal.jsx (Color Update)

**Files:**
- Update: `src/components/Terminal.jsx`

- [ ] **Step 1: Update only the inline color values in Terminal.jsx**

The logic is unchanged. Replace only the hardcoded color strings:

| Find | Replace |
|------|---------|
| `background: '#050505'` | `background: 'var(--bg-card)'` |
| `background: '#1e293b'` | `background: 'var(--bg-card)'` |
| `border: '1px solid rgba(255,255,255,0.15)'` | `border: '1px solid var(--border)'` |
| `color: '#38bdf8'` (prompt color, line 123) | `color: 'var(--accent)'` |
| `color: '#38bdf8'` (response color, line 115) | `color: 'var(--accent)'` |
| `color: '#a3e635'` | `color: 'var(--text)'` |
| `color: '#94a3b8'` (title bar text) | `color: 'var(--text-muted)'` |

Full updated file `src/components/Terminal.jsx`:

```jsx
import { useState, useRef, useEffect } from 'react';

const terminalCopy = {
    en: {
        intro: [
            'MBS-OS v1.0.0 (Node.js/React Fiber Kernel)',
            'Encrypted access to Muhammad Bayu Satrio AI engineering workspace.',
            'Type "help" to see available commands.',
        ],
        help: 'Available commands: whoami, skills, education, sudo, clear',
        whoami: 'Muhammad Bayu Satrio - AI Engineer focused on Indonesian NLP, analytics workflows, and production-aware applied systems.',
        skills: '[AI/NLP] IndoBERTweet, Hugging Face Transformers, ONNX Runtime, Scikit-learn  |  [Languages] Python, JavaScript, PHP, Go, C++, Java  |  [Frameworks] React.js, Node.js, Express.js, Laravel, Tailwind CSS, Three.js  |  [Databases] MySQL, Firebase, Supabase  |  [IoT] ESP32, Arduino',
        education: 'Currently pursuing a Bachelor of Information Technology degree at Telkom University (2023 - 2027)',
        sudo: 'Access denied. This incident has been logged by the system administrator.',
        notFound: (command) => `Bash command not found: ${command}. Type "help".`,
    },
    id: {
        intro: [
            'MBS-OS v1.0.0 (Node.js/React Fiber Kernel)',
            'Akses terenkripsi ke workspace AI engineering Muhammad Bayu Satrio.',
            'Ketik "help" untuk melihat command yang tersedia.',
        ],
        help: 'Command tersedia: whoami, skills, education, sudo, clear',
        whoami: 'Muhammad Bayu Satrio - AI Engineer yang berfokus pada NLP bahasa Indonesia, workflow analytics, dan applied systems yang siap produksi.',
        skills: '[AI/NLP] IndoBERTweet, Hugging Face Transformers, ONNX Runtime, Scikit-learn  |  [Bahasa] Python, JavaScript, PHP, Go, C++, Java  |  [Framework] React.js, Node.js, Express.js, Laravel, Tailwind CSS, Three.js  |  [Database] MySQL, Firebase, Supabase  |  [IoT] ESP32, Arduino',
        education: 'Sedang menempuh Bachelor of Information Technology di Telkom University (2023 - 2027)',
        sudo: 'Akses ditolak. Insiden ini sudah dicatat oleh administrator sistem.',
        notFound: (command) => `Command bash tidak ditemukan: ${command}. Ketik "help".`,
    },
};

function getTerminalCopy(language) {
    return terminalCopy[language] ?? terminalCopy.en;
}

function getIntroHistory(copy) {
    return copy.intro.map((text) => ({ type: 'system', text }));
}

function TerminalSession({ copy }) {
    const [input, setInput] = useState('');
    const [history, setHistory] = useState(() => getIntroHistory(copy));
    const inputRef = useRef(null);
    const bottomRef = useRef(null);

    const handleCommand = (cmd) => {
        const cleanCmd = cmd.trim().toLowerCase();
        let response = '';

        switch (cleanCmd) {
            case 'help': response = copy.help; break;
            case 'whoami': response = copy.whoami; break;
            case 'skills': response = copy.skills; break;
            case 'education': response = copy.education; break;
            case 'sudo': response = copy.sudo; break;
            case 'clear': setHistory([]); return;
            case '': break;
            default: response = copy.notFound(cleanCmd);
        }

        if (cleanCmd !== 'clear') {
            setHistory(prev => [
                ...prev,
                { type: 'user', text: `guest@bayusatrio:~$ ${cleanCmd}` },
                ...(response ? [{ type: 'response', text: response }] : []),
            ]);
        }
    };

    const handleKeyDown = (e) => {
        if (e.key === 'Enter') {
            handleCommand(input);
            setInput('');
        }
    };

    useEffect(() => {
        bottomRef.current?.scrollIntoView({ behavior: 'smooth' });
    }, [history]);

    return (
        <section id="terminal-section" style={{ padding: '0 0 4rem', display: 'flex', justifyContent: 'center' }}>
            <div
                style={{
                    width: '100%',
                    maxWidth: '860px',
                    background: 'var(--bg-card)',
                    borderRadius: '12px',
                    border: '1px solid var(--border)',
                    overflow: 'hidden',
                    boxShadow: '0 20px 40px rgba(0,0,0,0.4)',
                    fontFamily: 'monospace',
                    fontSize: '0.9rem',
                }}
                onClick={() => inputRef.current?.focus()}
            >
                <div style={{
                    background: 'rgba(10,18,19,0.9)',
                    padding: '0.7rem 1rem',
                    display: 'flex',
                    gap: '0.5rem',
                    alignItems: 'center',
                    borderBottom: '1px solid var(--border)',
                }}>
                    <div style={{ width: '13px', height: '13px', borderRadius: '50%', background: '#ef4444' }} />
                    <div style={{ width: '13px', height: '13px', borderRadius: '50%', background: '#eab308' }} />
                    <div style={{ width: '13px', height: '13px', borderRadius: '50%', background: '#22c55e' }} />
                    <span style={{ color: 'var(--text-muted)', margin: '0 auto', fontSize: '0.82rem', fontWeight: 600 }}>
                        guest@bayusatrio:~ (bash)
                    </span>
                </div>

                <div style={{
                    padding: '1.5rem',
                    height: '280px',
                    overflowY: 'auto',
                    display: 'flex',
                    flexDirection: 'column',
                    gap: '0.6rem',
                }}>
                    {history.map((line, idx) => (
                        <div key={idx} style={{
                            color: line.type === 'user'
                                ? 'var(--accent)'
                                : line.type === 'response'
                                ? 'var(--text)'
                                : 'var(--text-muted)',
                            lineHeight: 1.5,
                        }}>
                            {line.text}
                        </div>
                    ))}

                    <div style={{ display: 'flex', gap: '0.6rem', alignItems: 'center', marginTop: '0.5rem' }}>
                        <span style={{ color: 'var(--accent)', fontWeight: 600 }}>guest@bayusatrio:~$</span>
                        <input
                            ref={inputRef}
                            type="text"
                            value={input}
                            onChange={(e) => setInput(e.target.value)}
                            onKeyDown={handleKeyDown}
                            style={{
                                background: 'transparent',
                                border: 'none',
                                color: 'var(--text)',
                                flex: 1,
                                outline: 'none',
                                fontFamily: 'monospace',
                                fontSize: '0.9rem',
                                fontWeight: 500,
                            }}
                            spellCheck="false"
                            autoComplete="off"
                        />
                    </div>
                    <div ref={bottomRef} />
                </div>
            </div>
        </section>
    );
}

export default function Terminal({ language = 'en' }) {
    const copy = getTerminalCopy(language);
    return <TerminalSession key={language} copy={copy} />;
}
```

- [ ] **Step 2: Verify Terminal**

Scroll to the Terminal section. The terminal window should have a dark teal background, bronze prompt (`guest@bayusatrio:~$`), and muted text. Type `whoami` and press Enter — response text should appear in `var(--text)` (off-white), not green.

- [ ] **Step 3: Commit**

```
git add src/components/Terminal.jsx
git commit -m "feat: Terminal — bronze prompt + dark teal card colors"
```

---

## Task 15: Contact.jsx

**Files:**
- Rewrite: `src/components/Contact.jsx`

- [ ] **Step 1: Rewrite src/components/Contact.jsx**

```jsx
import { useState } from 'react'
import { motion } from 'framer-motion'

const EMAIL_TARGET = 'bayusatrio0235@gmail.com'

export default function Contact({ copy }) {
  const [isSent, setIsSent] = useState(false)
  const [isSubmitting, setIsSubmitting] = useState(false)

  const handleSubmit = async e => {
    e.preventDefault()
    setIsSubmitting(true)
    try {
      const res = await fetch(`https://formsubmit.co/ajax/${EMAIL_TARGET}`, {
        method: 'POST',
        body: new FormData(e.target),
      })
      if (!res.ok) throw new Error(`FormSubmit ${res.status}`)
      setIsSent(true)
    } catch (err) {
      console.error(err)
      alert(copy.form.error)
    } finally {
      setIsSubmitting(false)
    }
  }

  return (
    <motion.section
      id="contact"
      className="contact-section"
      initial={{ opacity: 0, y: 24 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.1 }}
      transition={{ duration: 0.6 }}
    >
      <span className="eyebrow">{copy.eyebrow}</span>

      <div className="contact-grid">
        <div>
          <h2 className="contact-title">{copy.title}</h2>
          <p className="contact-body">{copy.body}</p>
          <div className="contact-social-links">
            <a
              href="https://github.com/BayuSatrio2804"
              target="_blank"
              rel="noreferrer"
              className="contact-social-link"
            >
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" aria-hidden="true">
                <path d="M15 22v-4a4.8 4.8 0 0 0-1-3.24c3-.34 6-1.53 6-6.76a5.5 5.5 0 0 0-1.5-3.8 5.4 5.4 0 0 0-.1-3.7s-1.2-.4-3.9 1.5a13.4 13.4 0 0 0-7 0C6.3 1.8 5.1 2.2 5.1 2.2a5.4 5.4 0 0 0-.1 3.8A5.5 5.5 0 0 0 3.5 9.8c0 5.2 3 6.4 6 6.76a4.8 4.8 0 0 0-1 3.24v4" />
                <path d="M5 19c-3 1-4-3-4-3" />
              </svg>
              {copy.github}
            </a>
            <a
              href="https://www.linkedin.com/in/muhammad-bayu-satrio-52826a2a5/"
              target="_blank"
              rel="noreferrer"
              className="contact-social-link"
            >
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" aria-hidden="true">
                <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z" />
                <rect x="2" y="9" width="4" height="12" />
                <circle cx="4" cy="4" r="2" />
              </svg>
              {copy.linkedin}
            </a>
          </div>
        </div>

        <div className="contact-form-panel">
          {isSent ? (
            <div className="contact-sent">
              <p className="contact-sent-title">{copy.form.sentTitle}</p>
              <p className="contact-sent-body">{copy.form.sentBody}</p>
              <button
                type="button"
                className="btn btn-secondary"
                style={{ marginTop: '1rem' }}
                onClick={() => setIsSent(false)}
              >
                {copy.form.another}
              </button>
            </div>
          ) : (
            <form className="contact-form" onSubmit={handleSubmit}>
              <input type="hidden" name="_captcha" value="false" />
              <div className="contact-form-row">
                <input
                  type="text"
                  name="name"
                  placeholder={copy.form.name}
                  aria-label={copy.form.name}
                  required
                  className="form-input"
                />
                <input
                  type="email"
                  name="email"
                  placeholder={copy.form.email}
                  aria-label={copy.form.email}
                  required
                  className="form-input"
                />
              </div>
              <input
                type="text"
                name="subject"
                placeholder={copy.form.subject}
                aria-label={copy.form.subject}
                required
                className="form-input"
              />
              <textarea
                name="message"
                placeholder={copy.form.message}
                aria-label={copy.form.message}
                rows="5"
                required
                className="form-textarea"
              />
              <button
                type="submit"
                disabled={isSubmitting}
                className="btn btn-primary"
                style={{ alignSelf: 'flex-start', opacity: isSubmitting ? 0.6 : 1 }}
              >
                {isSubmitting ? copy.form.sending : copy.form.send}
              </button>
            </form>
          )}
        </div>
      </div>
    </motion.section>
  )
}
```

- [ ] **Step 2: Verify Contact**

Scroll to Contact. Left side should show the title, body text, and GitHub/LinkedIn buttons styled as dark cards with bronze hover. Right side shows the form panel with dark teal inputs. Fill in the form and verify bronze Submit button.

- [ ] **Step 3: Commit**

```
git add src/components/Contact.jsx
git commit -m "feat: Contact — two-panel form with FormSubmit + social links"
```

---

## Task 16: Final Verification + Cleanup

**Files:**
- Delete: `src/components/Navigation.jsx` (replaced by Sidebar.jsx)
- Delete: `src/components/About.jsx` (if exists, replaced by BentoSection.jsx)

- [ ] **Step 1: Delete the old Navigation component**

```
# In react-version directory
rm src/components/Navigation.jsx
```

Check if `src/components/About.jsx` exists. If it does, delete it too (the about functionality is now in BentoSection.jsx and portfolioContent.js already had the data there):

```
# Only run if the file exists
rm src/components/About.jsx 2>/dev/null || true
```

- [ ] **Step 2: Run data tests**

```
npm test
```

Expected: `All tests passed`

- [ ] **Step 3: Full visual walkthrough**

Open http://localhost:5173 and verify each section:

| Check | Expected |
|-------|----------|
| Preloader | Name + role fade in, bronze progress bar, slides up after ~2.2s |
| Sidebar | Fixed left at 240px, "Bayu." logo, nav links, EN/ID toggle |
| Background | Subtle bronze torus knot + stars, low opacity |
| Cursor | Bronze dot + ring, ring expands on hover |
| Hero | "Muhammad / Bayu Satrio" in Syne 900, "Satrio" bronze, 2×2 skills grid |
| About | Bio + 2×2 capability cards |
| Projects | 2-col grid, expand/collapse works, image gallery scrolls |
| Certificates | GEMASTIK card with image + bronze badge |
| Experience | Bronze timeline, 4 entries |
| Education | Telkom logo card + SMAN fallback icon card |
| Terminal | Bronze prompt, dark teal bg, commands work |
| Contact | Two-panel, form submits |
| Sidebar active | Scrolling changes active highlight |
| Language toggle | EN/ID toggle changes all section text |
| Mobile (< 768px) | Bottom nav appears, sidebar hides |

- [ ] **Step 4: Build check**

```
npm run build
```

Expected: Build completes with no errors. Note any warnings but do not fail for warnings.

- [ ] **Step 5: Final commit**

```
git add -A
git commit -m "feat: portfolio redesign v2 complete — dark teal + bronze + sidebar layout"
```

---

## Quick Reference

**Dev server:** `cd C:\Portofolio\react-version && npm run dev` → http://localhost:5173

**Data tests:** `npm test` (runs `src/data/portfolioContent.test.mjs`)

**Build:** `npm run build`

**CSS variables** (all in `src/index.css :root`):
- Background: `var(--bg)` = `#1C2B2D`
- Card: `var(--bg-card)` = `#162224`
- Accent: `var(--accent)` = `#CD7F32`
- Text: `var(--text)` = `#F5F5F5`
- Border: `var(--border)` = `rgba(205,127,50,0.18)`

**Section IDs** (used by Sidebar IntersectionObserver): `hero`, `about`, `projects`, `certificates`, `experience`, `education`, `contact`

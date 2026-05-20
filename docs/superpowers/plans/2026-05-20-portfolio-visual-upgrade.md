# Portfolio Visual Upgrade Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add cinematic scroll reveals, neon glow, physics interactivity, cursor trail, smooth scroll, Stats Counter, and Tech Stack Grid to the portfolio without significant bundle weight increase.

**Architecture:** All effects use CSS animations + Framer Motion (already installed) + vanilla JS mousemove handlers. One new dependency: `lenis` (~3KB) for smooth scroll. Two new components: `StatsCounter.jsx` and refactored `TechStack.jsx → TechStackGrid`. Mobile safety via `ontouchstart` and `matchMedia` checks.

**Tech Stack:** React 18, Framer Motion, Three.js/@react-three/fiber/@react-three/drei, Lenis (new), Vite, CSS custom properties

---

## File Map

| File | Action | Responsibility |
|------|--------|---------------|
| `react-version/src/main.jsx` | Modify | Lenis init |
| `react-version/src/index.css` | Modify | Aurora, noise, gradient text, eyebrow shimmer, cursor trail, prefers-reduced-motion |
| `react-version/src/App.css` | Modify | Navbar underline, project spotlight, timeline draw, stats strip, techstack grid |
| `react-version/src/App.jsx` | Modify | Add `<StatsCounter />` between Hero and BentoSection |
| `react-version/src/components/Navbar.jsx` | Modify | Remove conflicting `behavior:'smooth'`, logo gradient is CSS-only |
| `react-version/src/components/Background3D.jsx` | Modify | Aurora div layer, mobile star count |
| `react-version/src/components/CustomCursor.jsx` | Modify | Cursor trail dots |
| `react-version/src/components/Hero.jsx` | Modify | Gradient text on "Satrio", tilt on photo, magnetic on skill cards |
| `react-version/src/components/BentoSection.jsx` | Modify | Clip-path title, eyebrow shimmer ref, TechStackGrid import |
| `react-version/src/components/Certificates.jsx` | Modify | Clip-path title, eyebrow shimmer ref |
| `react-version/src/components/Education.jsx` | Modify | Clip-path title, eyebrow shimmer ref |
| `react-version/src/components/Contact.jsx` | Modify | Clip-path title, eyebrow shimmer ref |
| `react-version/src/components/Projects.jsx` | Modify | Clip-path title, eyebrow shimmer, tilt + spotlight on cards |
| `react-version/src/components/Experience.jsx` | Modify | Clip-path title, eyebrow shimmer, timeline line draw, first dot pulse |
| `react-version/src/components/TechStack.jsx` | Modify | Replace old badge-image impl with new TechStackGrid |
| `react-version/src/components/StatsCounter.jsx` | Create | Count-up stat strip |
| `react-version/src/data/portfolioContent.js` | Modify | Add `stats` and `techStack` arrays |

---

## Task 1: Install Lenis + Smooth Scroll

**Files:**
- Modify: `react-version/src/main.jsx`
- Modify: `react-version/src/components/Navbar.jsx` (line 46)

- [ ] **Step 1: Install lenis**

```bash
cd react-version && npm install lenis
```

Expected output: `added 1 package` or similar, no errors.

- [ ] **Step 2: Replace react-version/src/main.jsx**

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import Lenis from 'lenis'
import './index.css'
import App from './App.jsx'

const lenis = new Lenis()
function raf(time) {
  lenis.raf(time)
  requestAnimationFrame(raf)
}
requestAnimationFrame(raf)

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

- [ ] **Step 3: Fix scrollTo in Navbar.jsx — remove behavior:'smooth' (conflicts with Lenis)**

In `react-version/src/components/Navbar.jsx`, find and replace:

```js
// BEFORE
document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })

// AFTER
document.getElementById(id)?.scrollIntoView()
```

- [ ] **Step 4: Verify smooth scroll**

```bash
cd react-version && npm run dev
```

Open `http://localhost:5173`. Click navbar links — scrolling should feel buttery. No console errors.

- [ ] **Step 5: Commit**

```bash
git add react-version/src/main.jsx react-version/src/components/Navbar.jsx react-version/package.json react-version/package-lock.json
git commit -m "feat: add lenis smooth scroll"
```

---

## Task 2: CSS Foundation — All New Visual Effect Classes

**Files:**
- Modify: `react-version/src/index.css` (append)
- Modify: `react-version/src/App.css` (append)

- [ ] **Step 1: Append to react-version/src/index.css**

Add at the very end of the file:

```css
/* =========================================
   AURORA BACKGROUND
   ========================================= */

.aurora {
  position: absolute;
  inset: 0;
  pointer-events: none;
  background:
    radial-gradient(ellipse 80% 50% at 20% 20%, rgba(205, 127, 50, 0.07) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 80% 80%, rgba(205, 127, 50, 0.05) 0%, transparent 50%);
  animation: aurora-shift 14s ease-in-out infinite alternate;
}
@keyframes aurora-shift {
  from { transform: scale(1) rotate(0deg); opacity: 0.8; }
  to   { transform: scale(1.06) rotate(3deg); opacity: 1; }
}

/* =========================================
   NOISE TEXTURE OVERLAY
   ========================================= */

body::after {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  opacity: 0.025;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  background-size: 180px;
}

/* =========================================
   ANIMATED GRADIENT TEXT
   ========================================= */

.text-gradient {
  background: linear-gradient(90deg, #CD7F32 0%, #FFD700 50%, #CD7F32 100%);
  background-size: 200% auto;
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: text-shimmer 3s linear infinite;
}
@keyframes text-shimmer {
  to { background-position: 200% center; }
}

/* =========================================
   EYEBROW SHIMMER SWEEP
   ========================================= */

.eyebrow {
  position: relative;
  overflow: hidden;
}
.eyebrow.shimmer-active::after {
  content: '';
  position: absolute;
  top: 0;
  left: -80%;
  width: 60%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(205, 127, 50, 0.5), transparent);
  animation: eyebrow-sweep 0.75s ease forwards;
}
@keyframes eyebrow-sweep {
  to { left: 120%; }
}

/* =========================================
   CURSOR TRAIL
   ========================================= */

.cursor-trail {
  position: fixed;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: var(--accent);
  pointer-events: none;
  transform: translate(-50%, -50%);
  z-index: 9997;
  opacity: var(--trail-opacity, 0.4);
}

/* =========================================
   PREFERS-REDUCED-MOTION
   ========================================= */

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

- [ ] **Step 2: Append to react-version/src/App.css**

Add at the very end of the file:

```css
/* =========================================
   NAVBAR GRADIENT LOGO + ACTIVE UNDERLINE
   ========================================= */

.navbar-logo {
  background: linear-gradient(100deg, var(--text) 55%, var(--accent));
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.navbar-link {
  position: relative;
}
.navbar-link::after {
  content: '';
  position: absolute;
  bottom: 3px;
  left: 50%;
  right: 50%;
  height: 1px;
  background: var(--accent);
  transition: left 220ms ease, right 220ms ease;
}
.navbar-link.active::after {
  left: 0.75rem;
  right: 0.75rem;
}

/* =========================================
   PROJECT CARD TILT + SPOTLIGHT
   ========================================= */

.project-card {
  position: relative;
  overflow: hidden;
  will-change: transform;
  transition: transform 150ms ease, box-shadow 150ms ease, border-color 150ms ease;
}
.project-card::before {
  content: '';
  position: absolute;
  width: 350px;
  height: 350px;
  background: radial-gradient(circle, rgba(205, 127, 50, 0.1) 0%, transparent 65%);
  left: var(--mouse-x, -999px);
  top: var(--mouse-y, -999px);
  transform: translate(-50%, -50%);
  pointer-events: none;
  opacity: 0;
  transition: opacity 200ms;
  z-index: 0;
  border-radius: 50%;
}
.project-card:hover::before { opacity: 1; }
.project-card:hover { box-shadow: 0 0 30px var(--accent-glow-strong); }
.project-card > * { position: relative; z-index: 1; }

/* =========================================
   TIMELINE LINE DRAW + DOT PULSE
   ========================================= */

.timeline::before {
  transform-origin: top;
  transform: scaleY(0);
}
.timeline.line-drawn::before {
  animation: timeline-draw 1s ease forwards;
}
@keyframes timeline-draw {
  from { transform: scaleY(0); }
  to   { transform: scaleY(1); }
}

.timeline-dot-pulse {
  animation: dot-pulse 1.4s ease-out 0.3s forwards;
}
@keyframes dot-pulse {
  0%   { box-shadow: 0 0 0 0 rgba(205, 127, 50, 0.7); }
  60%  { box-shadow: 0 0 0 10px rgba(205, 127, 50, 0); }
  100% { box-shadow: 0 0 0 0 rgba(205, 127, 50, 0); }
}

/* =========================================
   STATS COUNTER STRIP
   ========================================= */

.stats-strip {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  padding: 2.25rem 2rem;
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  background: var(--bg-card);
  margin: 0 -2rem;
}
.stats-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 2.5rem;
  position: relative;
}
.stats-item + .stats-item::before {
  content: '';
  position: absolute;
  left: 0;
  top: 15%;
  bottom: 15%;
  width: 1px;
  background: var(--border);
}
.stats-number {
  font-family: 'Syne', sans-serif;
  font-size: clamp(1.75rem, 4vw, 2.5rem);
  font-weight: 900;
  color: var(--accent);
  line-height: 1;
  letter-spacing: -0.02em;
}
.stats-label {
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-top: 0.4rem;
}
@media (max-width: 600px) {
  .stats-item { padding: 0 1.25rem; }
}

/* =========================================
   TECH STACK GRID
   ========================================= */

.techstack-section {
  margin-top: 2.5rem;
  padding-top: 2rem;
  border-top: 1px solid var(--border);
}
.techstack-group { margin-bottom: 1.25rem; }
.techstack-group:last-child { margin-bottom: 0; }
.techstack-group-label {
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 0.6rem;
}
.techstack-pills {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
.techstack-pill {
  font-size: 0.75rem;
  font-weight: 500;
  font-family: 'Space Grotesk', sans-serif;
  padding: 4px 12px;
  border-radius: 20px;
  border: 1px solid var(--border);
  color: var(--text-soft);
  background: transparent;
  transition: border-color 150ms, box-shadow 150ms, background 150ms, color 150ms;
  cursor: default;
  user-select: none;
}
.techstack-pill:hover {
  border-color: var(--accent);
  box-shadow: 0 0 12px var(--accent-glow);
  background: var(--accent-dim);
  color: var(--text);
}
```

- [ ] **Step 3: Verify CSS compiles**

```bash
cd react-version && npm run dev
```

Open `http://localhost:5173`. Check devtools console — no CSS errors. Logo "Bayu." should have a white-to-bronze gradient.

- [ ] **Step 4: Commit**

```bash
git add react-version/src/index.css react-version/src/App.css
git commit -m "feat: add CSS foundation for all visual effects"
```

---

## Task 3: Background3D — Aurora Layer + Mobile Stars

**Files:**
- Modify: `react-version/src/components/Background3D.jsx`

- [ ] **Step 1: Replace Background3D.jsx**

```jsx
import { Canvas, useFrame } from '@react-three/fiber'
import { Stars } from '@react-three/drei'
import { useEffect, useRef, useState } from 'react'

const isMobile = window.matchMedia('(max-width: 768px)').matches

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
      <div className="aurora" />
      <Canvas camera={{ position: [0, 0, 30] }}>
        <RotatingKnot />
        <Stars
          radius={100}
          depth={50}
          count={isMobile ? 400 : 800}
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

- [ ] **Step 2: Verify aurora appears**

```bash
cd react-version && npm run dev
```

On desktop, you should see a subtle warm glow in the upper-left and lower-right corners animating slowly. The torus knot and stars remain. No console errors.

- [ ] **Step 3: Commit**

```bash
git add react-version/src/components/Background3D.jsx
git commit -m "feat: background3D aurora layer and mobile star reduction"
```

---

## Task 4: CustomCursor Trail

**Files:**
- Modify: `react-version/src/components/CustomCursor.jsx`

- [ ] **Step 1: Replace CustomCursor.jsx**

```jsx
import { useEffect, useRef } from 'react'

const TRAIL_COUNT = 6
const TRAIL_OPACITIES = [0.6, 0.45, 0.32, 0.2, 0.12, 0.06]
const isTouch = 'ontouchstart' in window

export default function CustomCursor() {
  const dotRef = useRef(null)
  const ringRef = useRef(null)
  const trailRefs = useRef([])
  const history = useRef(
    Array.from({ length: TRAIL_COUNT + 1 }, () => ({ x: -100, y: -100 }))
  )

  useEffect(() => {
    if (window.matchMedia('(hover: none)').matches) return

    const dot = dotRef.current
    const ring = ringRef.current
    if (!dot || !ring) return

    const onMove = e => {
      const { clientX: x, clientY: y } = e

      dot.style.left = `${x}px`
      dot.style.top = `${y}px`
      ring.style.left = `${x}px`
      ring.style.top = `${y}px`

      if (!isTouch) {
        history.current.unshift({ x, y })
        history.current.length = TRAIL_COUNT + 1
        trailRefs.current.forEach((el, i) => {
          if (el && history.current[i + 1]) {
            el.style.left = `${history.current[i + 1].x}px`
            el.style.top = `${history.current[i + 1].y}px`
          }
        })
      }
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
      {!isTouch && TRAIL_OPACITIES.map((opacity, i) => (
        <div
          key={i}
          className="cursor-trail"
          ref={el => { trailRefs.current[i] = el }}
          style={{ '--trail-opacity': opacity }}
        />
      ))}
    </>
  )
}
```

- [ ] **Step 2: Verify trail on desktop**

```bash
cd react-version && npm run dev
```

Move the mouse quickly — a fading comet trail of 6 dots should follow the cursor. On mobile/touch, no trail appears.

- [ ] **Step 3: Commit**

```bash
git add react-version/src/components/CustomCursor.jsx
git commit -m "feat: cursor trail with staggered opacity dots"
```

---

## Task 5: Hero Upgrades — Gradient Text, Photo Tilt, Magnetic Cards

**Files:**
- Modify: `react-version/src/components/Hero.jsx`

- [ ] **Step 1: Replace Hero.jsx**

```jsx
import { useEffect, useRef, useState } from 'react'
import { motion } from 'framer-motion'

const SKILL_CARDS = [
  { icon: '🧠', label: 'IndoBERTweet', sub: 'NLP' },
  { icon: '⚡', label: 'ONNX Runtime', sub: 'Inference' },
  { icon: '📊', label: 'Analytics', sub: 'Data' },
  { icon: '🔧', label: 'Applied', sub: 'Systems' },
]

const EASE = [0.76, 0, 0.24, 1]
const isTouch = 'ontouchstart' in window

function useTypewriter(text, speed = 55, delay = 1400) {
  const [displayed, setDisplayed] = useState('')
  useEffect(() => {
    setDisplayed('')
    let i = 0
    const t = setTimeout(() => {
      const iv = setInterval(() => {
        i++
        setDisplayed(text.slice(0, i))
        if (i >= text.length) clearInterval(iv)
      }, speed)
      return () => clearInterval(iv)
    }, delay)
    return () => clearTimeout(t)
  }, [text])
  return displayed
}

function WordReveal({ children, delay = 0, className = '' }) {
  return (
    <span className="word-reveal-wrap">
      <motion.span
        className={className}
        style={{ display: 'inline-block' }}
        initial={{ y: '110%' }}
        animate={{ y: 0 }}
        transition={{ duration: 0.85, delay, ease: EASE }}
      >
        {children}
      </motion.span>
    </span>
  )
}

function useTiltRef(maxDeg = 10) {
  const ref = useRef(null)
  useEffect(() => {
    if (isTouch || !ref.current) return
    const el = ref.current
    const onMove = e => {
      const r = el.getBoundingClientRect()
      const x = (e.clientX - r.left) / r.width - 0.5
      const y = (e.clientY - r.top) / r.height - 0.5
      el.style.transform = `perspective(600px) rotateY(${x * maxDeg * 2}deg) rotateX(${-y * maxDeg * 2}deg)`
    }
    const onLeave = () => { el.style.transform = '' }
    el.addEventListener('mousemove', onMove)
    el.addEventListener('mouseleave', onLeave)
    return () => {
      el.removeEventListener('mousemove', onMove)
      el.removeEventListener('mouseleave', onLeave)
    }
  }, [maxDeg])
  return ref
}

function MagneticCard({ children, className, style, ...props }) {
  const ref = useRef(null)
  useEffect(() => {
    if (isTouch || !ref.current) return
    const el = ref.current
    const onMove = e => {
      const r = el.getBoundingClientRect()
      const dx = (e.clientX - r.left - r.width / 2) * 0.25
      const dy = (e.clientY - r.top - r.height / 2) * 0.25
      el.style.transform = `translate(${dx}px, ${dy}px)`
    }
    const onLeave = () => { el.style.transform = '' }
    el.addEventListener('mousemove', onMove)
    el.addEventListener('mouseleave', onLeave)
    return () => {
      el.removeEventListener('mousemove', onMove)
      el.removeEventListener('mouseleave', onLeave)
    }
  }, [])
  return (
    <div ref={ref} className={className} style={style} {...props}>
      {children}
    </div>
  )
}

export default function Hero({ copy }) {
  const role = useTypewriter(copy.chipTitle, 55, 1500)
  const photoRef = useTiltRef(8)

  return (
    <section id="hero" className="hero-section">
      <div className="hero-left">
        <motion.span
          className="eyebrow"
          initial={{ opacity: 0, x: -32 }}
          animate={{ opacity: 1, x: 0 }}
          transition={{ duration: 0.7, ease: EASE }}
        >
          {copy.eyebrow}
        </motion.span>

        <h1 className="hero-name">
          <span className="hero-name-line">
            <WordReveal delay={0.2}>Muhammad</WordReveal>
          </span>
          <span className="hero-name-line">
            <WordReveal delay={0.38}>Bayu </WordReveal>
            <WordReveal delay={0.52} className="hero-name-accent text-gradient">Satrio</WordReveal>
          </span>
        </h1>

        <div className="hero-role-wrap">
          <span className="hero-role">
            {role}
            <span className="hero-cursor">|</span>
          </span>
        </div>

        <motion.p
          className="hero-body"
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.6, delay: 1.2, ease: 'easeOut' }}
        >
          {copy.body}
        </motion.p>

        <motion.div
          className="hero-actions"
          initial={{ opacity: 0, y: 16 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.5, delay: 2.0, ease: 'easeOut' }}
        >
          <a href="#projects" className="btn btn-primary">{copy.primaryCta}</a>
          <a
            href="/Resume_Muhammad_Bayu_Satrio.pdf"
            download="Resume_Muhammad_Bayu_Satrio.pdf"
            className="btn btn-secondary"
          >
            {copy.secondaryCta}
          </a>
        </motion.div>
      </div>

      <motion.div
        className="hero-visual"
        initial={{ opacity: 0, scale: 0.92 }}
        animate={{ opacity: 1, scale: 1 }}
        transition={{ duration: 0.8, delay: 0.5, ease: EASE }}
      >
        <div className="hero-photo-ring" ref={photoRef} style={{ transition: 'transform 150ms ease' }}>
          <img src="/foto-saya.jpg" alt="Muhammad Bayu Satrio" className="hero-photo" />
          <div className="hero-gemastik-badge">🥈 GEMASTIK XVIII · Silver</div>
        </div>

        <div className="hero-skills-grid">
          {SKILL_CARDS.map((card, i) => (
            <motion.div
              key={card.label}
              initial={{ opacity: 0, y: 30, scale: 0.88 }}
              animate={{ opacity: 1, y: 0, scale: 1 }}
              transition={{ duration: 0.55, delay: 1.5 + i * 0.1, ease: EASE }}
            >
              <MagneticCard
                className="hero-skill-card"
                style={{ transition: 'transform 200ms ease, border-color 150ms ease, box-shadow 150ms ease' }}
              >
                <span className="hero-skill-icon">{card.icon}</span>
                <span className="hero-skill-label">{card.label}</span>
                <span className="hero-skill-sub">{card.sub}</span>
              </MagneticCard>
            </motion.div>
          ))}
        </div>
      </motion.div>
    </section>
  )
}
```

- [ ] **Step 2: Verify hero effects**

```bash
cd react-version && npm run dev
```

Check:
- "Satrio" cycles through a gold–bronze shimmer gradient
- Hovering the profile photo creates a 3D tilt
- Hovering skill cards (NLP, Inference, etc.) creates a subtle magnetic pull toward the cursor

- [ ] **Step 3: Commit**

```bash
git add react-version/src/components/Hero.jsx
git commit -m "feat: hero gradient text, photo tilt, magnetic skill cards"
```

---

## Task 6: Section Titles — Clip-Path Wipe + Eyebrow Shimmer

**Files:**
- Modify: `react-version/src/components/BentoSection.jsx`
- Modify: `react-version/src/components/Certificates.jsx`
- Modify: `react-version/src/components/Education.jsx`
- Modify: `react-version/src/components/Contact.jsx`

The pattern for every component is:
1. Add `useRef` import
2. Add `eyebrowRef` ref + `onViewportEnter` to the eyebrow `motion.span`
3. Change section title from `initial={{ y: '100%' }}` → `initial={{ clipPath: 'inset(0 100% 0 0)' }}`

Note: `Projects.jsx` and `Experience.jsx` are handled in Tasks 7 and 8 since they have additional changes.

- [ ] **Step 1: Update BentoSection.jsx**

Replace entire `react-version/src/components/BentoSection.jsx`:

```jsx
import { useRef } from 'react'
import { motion } from 'framer-motion'

const EASE = [0.76, 0, 0.24, 1]

export default function BentoSection({ copy }) {
  const eyebrowRef = useRef(null)

  return (
    <motion.section
      id="about"
      className="bento-section"
      initial={{ opacity: 0 }}
      whileInView={{ opacity: 1 }}
      viewport={{ once: true, amount: 0.1 }}
      transition={{ duration: 0.4 }}
    >
      <motion.span
        ref={eyebrowRef}
        className="eyebrow"
        initial={{ opacity: 0, x: -20 }}
        whileInView={{ opacity: 1, x: 0 }}
        viewport={{ once: true }}
        transition={{ duration: 0.6, ease: EASE }}
        onViewportEnter={() => eyebrowRef.current?.classList.add('shimmer-active')}
      >
        {copy.eyebrow}
      </motion.span>

      <div className="section-title-wrap">
        <motion.h2
          className="section-title"
          initial={{ clipPath: 'inset(0 100% 0 0)' }}
          whileInView={{ clipPath: 'inset(0 0% 0 0)' }}
          viewport={{ once: true }}
          transition={{ duration: 0.85, ease: EASE }}
        >
          {copy.title}
        </motion.h2>
      </div>

      <div className="about-bio">
        {copy.body.map((para, i) => (
          <motion.p
            key={i}
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: i * 0.12, ease: 'easeOut' }}
          >
            {para}
          </motion.p>
        ))}
      </div>

      <div className="capability-grid">
        {copy.capabilities.map((cap, i) => (
          <motion.div
            key={cap.title}
            className="capability-card"
            initial={{ opacity: 0, y: 40, scale: 0.95 }}
            whileInView={{ opacity: 1, y: 0, scale: 1 }}
            viewport={{ once: true }}
            transition={{ duration: 0.55, delay: i * 0.1, ease: EASE }}
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

- [ ] **Step 2: Update Certificates.jsx — eyebrow ref + clip-path title**

In `react-version/src/components/Certificates.jsx`, make these changes:

Add `useRef` to the import:
```jsx
// BEFORE
import { motion } from 'framer-motion'

// AFTER
import { useRef } from 'react'
import { motion } from 'framer-motion'
```

Add ref and onViewportEnter to the eyebrow:
```jsx
// BEFORE
export default function Certificates({ copy, certificates }) {
  return (
    <motion.section
      ...
    >
      <motion.span
        className="eyebrow"
        initial={{ opacity: 0, x: -20 }}
        whileInView={{ opacity: 1, x: 0 }}
        viewport={{ once: true }}
        transition={{ duration: 0.6, ease: EASE }}
      >

// AFTER
export default function Certificates({ copy, certificates }) {
  const eyebrowRef = useRef(null)
  return (
    <motion.section
      ...
    >
      <motion.span
        ref={eyebrowRef}
        className="eyebrow"
        initial={{ opacity: 0, x: -20 }}
        whileInView={{ opacity: 1, x: 0 }}
        viewport={{ once: true }}
        transition={{ duration: 0.6, ease: EASE }}
        onViewportEnter={() => eyebrowRef.current?.classList.add('shimmer-active')}
      >
```

Change the section title animation:
```jsx
// BEFORE
        <motion.h2
          className="section-title"
          initial={{ y: '100%' }}
          whileInView={{ y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.75, ease: EASE }}

// AFTER
        <motion.h2
          className="section-title"
          initial={{ clipPath: 'inset(0 100% 0 0)' }}
          whileInView={{ clipPath: 'inset(0 0% 0 0)' }}
          viewport={{ once: true }}
          transition={{ duration: 0.85, ease: EASE }}
```

- [ ] **Step 3: Apply same pattern to Education.jsx**

In `react-version/src/components/Education.jsx`:

Add `useRef` import (same as Step 2).

Add `const eyebrowRef = useRef(null)` inside the component.

Add `ref={eyebrowRef}` and `onViewportEnter={() => eyebrowRef.current?.classList.add('shimmer-active')}` to the eyebrow `motion.span`.

Change `initial={{ y: '100%' }} whileInView={{ y: 0 }} transition={{ duration: 0.75` to `initial={{ clipPath: 'inset(0 100% 0 0)' }} whileInView={{ clipPath: 'inset(0 0% 0 0)' }} transition={{ duration: 0.85`.

- [ ] **Step 4: Apply same pattern to Contact.jsx**

In `react-version/src/components/Contact.jsx`:

The eyebrow `motion.span` starts at line 38. Read the current eyebrow code and apply the same pattern as Steps 2-3.

Add `useRef` import, `const eyebrowRef = useRef(null)`, `ref={eyebrowRef}`, `onViewportEnter`, and change the `h2` clip-path animation.

Note: `Contact.jsx` doesn't have a section title `motion.h2` in the same pattern — check if there's one and apply the clip-path only if there is.

- [ ] **Step 5: Verify section reveals**

```bash
cd react-recipe && npm run dev
```

Wait — the project is in `react-version`. Run:
```bash
cd react-version && npm run dev
```

Scroll down to About, Certificates, Education, Contact. Each section title should wipe in from left-to-right instead of sliding up. Eyebrows should flash a shimmer when they first appear.

- [ ] **Step 6: Commit**

```bash
git add react-version/src/components/BentoSection.jsx react-version/src/components/Certificates.jsx react-version/src/components/Education.jsx react-version/src/components/Contact.jsx
git commit -m "feat: clip-path title reveals and eyebrow shimmer on About/Certs/Education/Contact"
```

---

## Task 7: Project Cards — Clip-Path Title + Tilt + Spotlight

**Files:**
- Modify: `react-version/src/components/Projects.jsx`

- [ ] **Step 1: Replace Projects.jsx**

```jsx
import { useEffect, useRef, useState } from 'react'
import { motion, AnimatePresence } from 'framer-motion'

const EASE = [0.76, 0, 0.24, 1]
const isTouch = 'ontouchstart' in window

function useTiltSpotlight(ref) {
  useEffect(() => {
    if (isTouch || !ref.current) return
    const el = ref.current
    const onMove = e => {
      const r = el.getBoundingClientRect()
      const x = (e.clientX - r.left) / r.width - 0.5
      const y = (e.clientY - r.top) / r.height - 0.5
      el.style.transform = `perspective(800px) rotateY(${x * 16}deg) rotateX(${-y * 16}deg) scale(1.02)`
      el.style.setProperty('--mouse-x', `${e.clientX - r.left}px`)
      el.style.setProperty('--mouse-y', `${e.clientY - r.top}px`)
    }
    const onLeave = () => {
      el.style.transform = ''
    }
    el.addEventListener('mousemove', onMove)
    el.addEventListener('mouseleave', onLeave)
    return () => {
      el.removeEventListener('mousemove', onMove)
      el.removeEventListener('mouseleave', onLeave)
    }
  }, [ref])
}

function ProjectCard({ project, copy, language, isExpanded, onToggle, index }) {
  const cardRef = useRef(null)
  useTiltSpotlight(cardRef)
  const summary = project.summary[language] ?? project.summary.en
  const details = project.details[language] ?? project.details.en

  return (
    <motion.article
      key={project.slug}
      ref={cardRef}
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
        onClick={onToggle}
        aria-expanded={isExpanded}
      >
        {isExpanded ? copy.collapse : copy.expand}
      </button>
    </motion.article>
  )
}

export default function Projects({ copy, language, projects }) {
  const [expandedId, setExpandedId] = useState(null)
  const eyebrowRef = useRef(null)

  return (
    <motion.section
      id="projects"
      className="projects-section"
      initial={{ opacity: 0 }}
      whileInView={{ opacity: 1 }}
      viewport={{ once: true, amount: 0.05 }}
      transition={{ duration: 0.4 }}
    >
      <motion.span
        ref={eyebrowRef}
        className="eyebrow"
        initial={{ opacity: 0, x: -20 }}
        whileInView={{ opacity: 1, x: 0 }}
        viewport={{ once: true }}
        transition={{ duration: 0.6, ease: EASE }}
        onViewportEnter={() => eyebrowRef.current?.classList.add('shimmer-active')}
      >
        {copy.eyebrow}
      </motion.span>
      <div className="section-title-wrap">
        <motion.h2
          className="section-title"
          initial={{ clipPath: 'inset(0 100% 0 0)' }}
          whileInView={{ clipPath: 'inset(0 0% 0 0)' }}
          viewport={{ once: true }}
          transition={{ duration: 0.85, ease: EASE }}
        >
          {copy.title}
        </motion.h2>
      </div>

      <div className="projects-grid">
        {projects.map((project, index) => (
          <ProjectCard
            key={project.slug}
            project={project}
            copy={copy}
            language={language}
            isExpanded={expandedId === project.slug}
            onToggle={() => setExpandedId(expandedId === project.slug ? null : project.slug)}
            index={index}
          />
        ))}
      </div>
    </motion.section>
  )
}
```

- [ ] **Step 2: Verify project card effects**

```bash
cd react-version && npm run dev
```

Scroll to Projects. Hover a project card — it should tilt in 3D and show a warm spotlight glow following your cursor. Section title wipes in from left.

- [ ] **Step 3: Commit**

```bash
git add react-version/src/components/Projects.jsx
git commit -m "feat: project cards 3D tilt, spotlight, clip-path title"
```

---

## Task 8: Experience Timeline — Clip-Path Title + Line Draw + Dot Pulse

**Files:**
- Modify: `react-version/src/components/Experience.jsx`

- [ ] **Step 1: Replace Experience.jsx**

```jsx
import { useEffect, useRef } from 'react'
import { motion, useInView } from 'framer-motion'

const EASE = [0.76, 0, 0.24, 1]

function FirstDot() {
  const ref = useRef(null)
  const isInView = useInView(ref, { once: true, amount: 0.5 })
  return (
    <div
      ref={ref}
      className={`timeline-dot${isInView ? ' timeline-dot-pulse' : ''}`}
    />
  )
}

export default function Experience({ copy, language, experiences }) {
  const eyebrowRef = useRef(null)
  const timelineRef = useRef(null)
  const isTimelineInView = useInView(timelineRef, { once: true, amount: 0.2 })

  useEffect(() => {
    if (isTimelineInView && timelineRef.current) {
      timelineRef.current.classList.add('line-drawn')
    }
  }, [isTimelineInView])

  return (
    <motion.section
      id="experience"
      className="experience-section"
      initial={{ opacity: 0 }}
      whileInView={{ opacity: 1 }}
      viewport={{ once: true, amount: 0.05 }}
      transition={{ duration: 0.4 }}
    >
      <motion.span
        ref={eyebrowRef}
        className="eyebrow"
        initial={{ opacity: 0, x: -20 }}
        whileInView={{ opacity: 1, x: 0 }}
        viewport={{ once: true }}
        transition={{ duration: 0.6, ease: EASE }}
        onViewportEnter={() => eyebrowRef.current?.classList.add('shimmer-active')}
      >
        {copy.eyebrow}
      </motion.span>
      <div className="section-title-wrap">
        <motion.h2
          className="section-title"
          initial={{ clipPath: 'inset(0 100% 0 0)' }}
          whileInView={{ clipPath: 'inset(0 0% 0 0)' }}
          viewport={{ once: true }}
          transition={{ duration: 0.85, ease: EASE }}
        >
          {copy.title}
        </motion.h2>
      </div>

      <div ref={timelineRef} className="timeline">
        {experiences.map((exp, i) => (
          <motion.div
            key={exp.company}
            className="timeline-entry"
            initial={{ opacity: 0, x: -20 }}
            whileInView={{ opacity: 1, x: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.5, delay: i * 0.1 }}
          >
            {i === 0 ? <FirstDot /> : <div className="timeline-dot" />}
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

- [ ] **Step 2: Verify timeline effects**

```bash
cd react-version && npm run dev
```

Scroll to Experience. The timeline vertical line should animate growing from top to bottom. The first dot should pulse with a glow ring when it enters view.

- [ ] **Step 3: Commit**

```bash
git add react-version/src/components/Experience.jsx
git commit -m "feat: experience timeline line draw animation and first dot pulse"
```

---

## Task 9: Data — Add Stats + TechStack to portfolioContent.js

**Files:**
- Modify: `react-version/src/data/portfolioContent.js`

- [ ] **Step 1: Add stats and techStack exports to portfolioContent.js**

Append at the end of `react-version/src/data/portfolioContent.js` (after the `getCopy` function):

```js
export const stats = [
  { value: 4, suffix: '', label: { en: 'Projects', id: 'Proyek' } },
  { value: 4, suffix: '', label: { en: 'Work Roles', id: 'Peran Kerja' } },
  { value: 1, suffix: '', label: { en: 'National Award', id: 'Penghargaan' } },
  { value: 3, suffix: '+', label: { en: 'Years Building', id: 'Tahun Berkarya' } },
]

export const techStack = [
  {
    group: { en: 'ML / AI', id: 'ML / AI' },
    items: ['Python', 'PyTorch', 'Scikit-learn', 'ONNX Runtime', 'IndoBERTweet', 'Hugging Face'],
  },
  {
    group: { en: 'Frontend', id: 'Frontend' },
    items: ['React', 'Vite', 'Framer Motion', 'Tailwind CSS'],
  },
  {
    group: { en: 'Backend', id: 'Backend' },
    items: ['FastAPI', 'Node.js', 'Express', 'Laravel'],
  },
  {
    group: { en: 'Tools', id: 'Tools' },
    items: ['Docker', 'Git', 'MySQL', 'Jupyter', 'VS Code'],
  },
]
```

- [ ] **Step 2: Verify no import errors**

```bash
cd react-version && npm run dev
```

No console errors. App loads normally.

- [ ] **Step 3: Commit**

```bash
git add react-version/src/data/portfolioContent.js
git commit -m "feat: add stats and techStack data to portfolioContent"
```

---

## Task 10: StatsCounter Component

**Files:**
- Create: `react-version/src/components/StatsCounter.jsx`
- Modify: `react-version/src/App.jsx`

- [ ] **Step 1: Create react-version/src/components/StatsCounter.jsx**

```jsx
import { useCallback, useRef, useState } from 'react'
import { motion } from 'framer-motion'

function easeOutCubic(t) {
  return 1 - Math.pow(1 - t, 3)
}

function useCountUp(target, duration = 1400) {
  const [count, setCount] = useState(0)
  const started = useRef(false)

  const start = useCallback(() => {
    if (started.current) return
    started.current = true
    const startTime = performance.now()
    function tick(now) {
      const elapsed = now - startTime
      const progress = Math.min(elapsed / duration, 1)
      setCount(Math.floor(easeOutCubic(progress) * target))
      if (progress < 1) requestAnimationFrame(tick)
      else setCount(target)
    }
    requestAnimationFrame(tick)
  }, [target, duration])

  return [count, start]
}

function StatItem({ stat, language }) {
  const [count, start] = useCountUp(stat.value)
  const label = stat.label[language] ?? stat.label.en

  return (
    <motion.div
      className="stats-item"
      initial={{ opacity: 0, y: 20 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true }}
      transition={{ duration: 0.5 }}
      onViewportEnter={start}
    >
      <span className="stats-number">
        {count}{stat.suffix}
      </span>
      <span className="stats-label">{label}</span>
    </motion.div>
  )
}

export default function StatsCounter({ stats, language }) {
  return (
    <div className="stats-strip">
      {stats.map((stat, i) => (
        <StatItem key={i} stat={stat} language={language} />
      ))}
    </div>
  )
}
```

- [ ] **Step 2: Add StatsCounter to App.jsx**

In `react-version/src/App.jsx`, add the import:

```jsx
// BEFORE (existing imports)
import { certificates, education, experiences, getCopy, projects } from './data/portfolioContent'

// AFTER
import { certificates, education, experiences, getCopy, projects, stats, techStack } from './data/portfolioContent'
import StatsCounter from './components/StatsCounter'
```

In the JSX, add `<StatsCounter />` between `<Hero />` and `<BentoSection />`:

```jsx
// BEFORE
        <Hero copy={copy.hero} />
        <BentoSection copy={copy.about} />

// AFTER
        <Hero copy={copy.hero} />
        <StatsCounter stats={stats} language={language} />
        <BentoSection copy={copy.about} />
```

- [ ] **Step 3: Verify count-up animation**

```bash
cd react-version && npm run dev
```

Just below the Hero section, a horizontal strip with 4 stats should appear: "4 Projects", "4 Work Roles", "1 National Award", "3+ Years Building". Numbers should count up from 0 when they scroll into view.

- [ ] **Step 4: Commit**

```bash
git add react-version/src/components/StatsCounter.jsx react-version/src/App.jsx
git commit -m "feat: StatsCounter component with count-up animation"
```

---

## Task 11: TechStackGrid Component + BentoSection Integration

**Files:**
- Modify: `react-version/src/components/TechStack.jsx` (replace content)
- Modify: `react-version/src/components/BentoSection.jsx`

- [ ] **Step 1: Replace TechStack.jsx with TechStackGrid**

Replace the entire content of `react-version/src/components/TechStack.jsx`:

```jsx
import { motion } from 'framer-motion'

const containerVariants = {
  hidden: {},
  visible: { transition: { staggerChildren: 0.04 } },
}
const pillVariants = {
  hidden: { opacity: 0, scale: 0.85 },
  visible: { opacity: 1, scale: 1, transition: { duration: 0.3 } },
}

export default function TechStackGrid({ techStack, language }) {
  return (
    <div className="techstack-section">
      {techStack.map((group, gi) => (
        <div key={gi} className="techstack-group">
          <div className="techstack-group-label">
            {group.group[language] ?? group.group.en}
          </div>
          <motion.div
            className="techstack-pills"
            variants={containerVariants}
            initial="hidden"
            whileInView="visible"
            viewport={{ once: true, amount: 0.2 }}
          >
            {group.items.map(item => (
              <motion.span
                key={item}
                className="techstack-pill"
                variants={pillVariants}
              >
                {item}
              </motion.span>
            ))}
          </motion.div>
        </div>
      ))}
    </div>
  )
}
```

- [ ] **Step 2: Add TechStackGrid to BentoSection.jsx**

In `react-version/src/components/BentoSection.jsx`, add the import and prop:

```jsx
// BEFORE
import { useRef } from 'react'
import { motion } from 'framer-motion'

// AFTER
import { useRef } from 'react'
import { motion } from 'framer-motion'
import TechStackGrid from './TechStack'
```

Change the component signature to accept `techStack` and `language`:

```jsx
// BEFORE
export default function BentoSection({ copy }) {

// AFTER
export default function BentoSection({ copy, techStack, language }) {
```

Add `<TechStackGrid />` after the `capability-grid` div (inside the section, after the closing `</div>` of `.capability-grid`):

```jsx
      {/* ...capability-grid div ends here... */}
      </div>

      <TechStackGrid techStack={techStack} language={language} />
    </motion.section>
```

- [ ] **Step 3: Pass techStack and language from App.jsx to BentoSection**

In `react-version/src/App.jsx`, update the BentoSection usage:

```jsx
// BEFORE
        <BentoSection copy={copy.about} />

// AFTER
        <BentoSection copy={copy.about} techStack={techStack} language={language} />
```

- [ ] **Step 4: Verify TechStackGrid**

```bash
cd react-version && npm run dev
```

Scroll to the About section. Below the capability cards, a grouped grid of tech badges should stagger-fade in. Hovering a badge shows a bronze glow.

- [ ] **Step 5: Commit**

```bash
git add react-version/src/components/TechStack.jsx react-version/src/components/BentoSection.jsx react-version/src/App.jsx
git commit -m "feat: TechStackGrid with stagger reveal and glow hover"
```

---

## Self-Review Checklist

**Spec coverage:**
- [x] Lenis smooth scroll → Task 1
- [x] Aurora gradient + noise overlay → Task 2 (CSS) + Task 3 (Background3D)
- [x] Mobile star count reduction → Task 3
- [x] Cursor trail (6 dots, touch disabled) → Task 4
- [x] Hero gradient text "Satrio" → Task 5
- [x] Hero photo tilt 3D → Task 5
- [x] Hero skill cards magnetic hover → Task 5
- [x] Clip-path section title wipe (all sections) → Tasks 6, 7, 8
- [x] Eyebrow shimmer sweep (all sections) → Tasks 6, 7, 8
- [x] Project cards tilt 3D + spotlight → Task 7
- [x] Timeline line draw → Task 8
- [x] Timeline first dot pulse → Task 8
- [x] Navbar logo gradient → Task 2 (CSS only)
- [x] Navbar active underline → Task 2 (CSS only)
- [x] StatsCounter (4 stats, count-up) → Tasks 9+10
- [x] TechStackGrid (4 groups, stagger, glow) → Tasks 9+11
- [x] prefers-reduced-motion → Task 2 (CSS)
- [x] Touch/mobile disabling for tilt + trail → Tasks 4, 5, 7

**Type consistency:** `stats` exported from portfolioContent is consumed by `StatsCounter` with shape `{ value, suffix, label }`. `techStack` consumed by `TechStackGrid` with shape `{ group, items }`. Both shapes consistent across Tasks 9–11.

**No placeholders:** All steps have complete code.

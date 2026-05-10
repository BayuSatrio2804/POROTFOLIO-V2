# AI Portfolio Upgrade Design

## Goal

Upgrade the existing React/Vite portfolio in `react-version` into a cinematic AI-focused portfolio for Muhammad Bayu Satrio. The site should position Bayu as an **AI Engineer focused on NLP, Analytics, and Applied Systems**, with fullstack and IoT experience presented as supporting strengths.

English is the default language. The site will include a simple `EN | ID` segmented toggle in the navigation so visitors can switch core copy between English and Indonesian without reloading.

## Existing Context

The current repository root is connected to `BayuSatrio2804/POROTFOLIO-V2`. The working frontend lives in `react-version` and uses React, Vite, Framer Motion, Three.js, React Three Fiber, Drei, and Lucide React.

Existing assets include:

- profile photo: `react-version/public/foto-saya.jpg`
- GEMASTIK certificate image: `react-version/public/sertifikat-gemastik.png`
- resume files: `react-version/public/Resume_Muhammad_Bayu_Satrio.*`
- project screenshots for Bidanku, ACETRA, and DonasiKu

The current app already has sections for hero, terminal, bento/about, experience, education, projects, and contact. The upgrade should reuse the current project and asset base instead of rebuilding from an empty app.

## Content Positioning

The primary identity is:

> AI Engineer focused on NLP, Analytics, and Applied Systems.

The portfolio should emphasize:

- Indonesian NLP and machine learning workflow
- data validation, metrics, thresholding, and model evaluation
- production-aware model export and inference, especially ONNX Runtime CPU
- applied systems experience across backend, IoT, and product implementation
- GEMASTIK XVIII silver medal as a strong proof point

Fullstack web development remains visible, but it should not be the dominant identity.

## Site Structure

The final page order will be:

1. Hero
2. About Me + Profile Photo + AI Capability Bento
3. Projects
4. Certificates & Achievements
5. Experience
6. Education
7. Contact

Navigation should include compact links to the main sections and the `EN | ID` toggle.

## Hero Design

The hero will use a dark cinematic tech direction. It should feel like a serious AI/data portfolio, not a generic neon developer template.

Hero content:

- name: Muhammad Bayu Satrio
- headline: "AI Engineer focused on NLP, Analytics, and Applied Systems"
- short supporting copy about building Indonesian NLP models, analytics workflows, and production-aware inference systems
- proof tags such as NLP, analytics, ONNX Runtime, applied systems, and GEMASTIK
- calls to action: view projects and download resume
- a small profile chip or avatar badge using the profile photo

The profile photo should not dominate the hero. The hero should mainly sell the technical positioning and proof.

## About Me Design

The About section will make the site more personal and include the profile photo clearly. It should combine:

- a larger profile image card using `foto-saya.jpg`
- concise personal copy explaining Bayu's AI/data direction
- capability cards for AI/NLP, analytics, model deployment, backend/fullstack integration, and IoT/product systems

This section replaces the current broad "Fullstack. AI. IoT. Lifelong Learner." messaging with a sharper AI/data narrative.

## Projects Design

Projects should be shown as four equal portfolio items:

- Indonesia Toxic Speech Detector
- Bidanku - Midwifery Management System
- ACETRA - Smart IoT Fermentation System
- DonasiKu - Second-Hand Goods Platform

Indonesia Toxic Speech Detector must be added as a first-class project, not a secondary highlight. Its summary should mention:

- Indonesian toxic speech classification
- IndoBERTweet-based binary classifier
- stratified train, validation, and test workflow
- threshold tuning and final metrics
- Hugging Face and FP32 ONNX CPU inference artifact export
- ONNX Runtime smoke tests and probability drift checks
- repository link: `https://github.com/BayuSatrio2804/Indonesia-Toxic-Speech-Detector`

The project cards should support summaries, tech tags, role/category labels, repo links, and expanded details. Screenshot galleries remain for projects that have images. The toxic speech project may use a designed code/model visual if no screenshot asset exists.

## Certificates & Achievements

Add a dedicated section for certificates and achievements. Initially it will feature the existing GEMASTIK XVIII certificate because no other certificate files are available yet.

The section should be data-driven so new certificates can be added later by placing files in `react-version/public/certificates/` and adding entries to a certificate array. Certificate entries should support:

- title
- issuer
- year
- category
- image or file path
- optional view/download action

## Internationalization

Implement a lightweight local translation system inside the React app. English is the default. Indonesian is available through the `EN | ID` segmented toggle.

The toggle should cover:

- navigation labels
- hero copy and buttons
- About section copy
- project summaries and details
- certificates section headings and supporting copy
- experience and education headings/descriptions
- contact section copy and form labels

Project names, technology names, dates, repository URLs, file paths, and institution names remain unchanged across languages.

The chosen language should be held in React state. Persisting it to `localStorage` is acceptable if it stays simple.

## Visual System

Use a restrained dark cinematic AI aesthetic:

- deep near-black background
- cyan/blue technical accents
- subtle glass or panel surfaces
- signal-grid, model-inference, or data-flow inspired details
- controlled Framer Motion transitions
- no generic purple gradient hero treatment
- no decorative orb-only background
- no oversized portrait-first hero

Cards should be cleaner and more consistent than the current inline-heavy implementation. Avoid nested cards and overly rounded panels where possible.

## Technical Approach

Keep the existing React/Vite app and refactor only the parts needed for the upgrade. Prefer local data arrays for portfolio content and translations so future edits are straightforward.

Expected component changes:

- update `Hero`
- update `Navigation`
- update or replace `BentoSection` with an AI-focused About section
- update `Projects` and add Toxic Speech Detector
- add a dedicated `Certificates` component
- update `Experience`, `Education`, and `Contact` copy for the new positioning
- update global CSS for the refined visual system and responsive behavior

The implementation should avoid broad unrelated refactors.

## Responsiveness And Accessibility

The site must work on desktop and mobile. Text should not overflow buttons, cards, or nav controls. The language toggle must be keyboard-accessible. Images need meaningful alt text. Links to external repositories should open safely with `target="_blank"` and `rel="noreferrer"`.

Motion should be polished but not disruptive. Existing animations can remain if they serve the new design and do not cause layout instability.

## Verification

Before claiming completion, run fresh verification commands from `react-version`:

- `npm run build`
- `npm run lint`, if the existing lint configuration can run successfully

Also inspect the local site in a browser after starting the dev server. Check desktop and mobile-sized layouts, hero rendering, language toggle behavior, project expansion, certificate display, resume link, and contact form layout.

If a verification command fails due to an existing unrelated configuration issue, report the exact failure and what still was verified.

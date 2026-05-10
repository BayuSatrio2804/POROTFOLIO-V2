# AI Portfolio Upgrade Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Upgrade the existing React/Vite portfolio into a cinematic AI Engineer/Data Analyst portfolio with bilingual EN/ID content, a new Toxic Speech Detector project, and a certificates section.

**Architecture:** Keep the current Vite app in `react-version`, but move portfolio copy into data modules so components render from a single bilingual content source. Use React state in `App.jsx` for the active language and pass translated content to sections as props.

**Tech Stack:** React 19, Vite, Framer Motion, Lucide React, local CSS, Node-based content integrity test, existing static assets in `react-version/public`.

---

## File Structure

- Create `react-version/src/data/portfolioContent.js`
  - Owns bilingual content, project data, certificates, experience, education, and helper functions.
- Create `react-version/src/data/portfolioContent.test.mjs`
  - Verifies required translation keys and portfolio data integrity using Node's built-in `assert`.
- Modify `react-version/package.json`
  - Add `test:content` script.
- Modify `react-version/src/App.jsx`
  - Owns `language` state and passes translated content to sections.
- Modify `react-version/src/components/Navigation.jsx`
  - Replace alert-based language button with accessible `EN | ID` segmented toggle and updated section links.
- Modify `react-version/src/components/Hero.jsx`
  - Rewrite hero copy and add profile chip and AI proof visual.
- Modify `react-version/src/components/BentoSection.jsx`
  - Convert into About Me + profile photo + AI capability bento.
- Modify `react-version/src/components/Projects.jsx`
  - Render four equal projects from content data, including Toxic Speech Detector.
- Create `react-version/src/components/Certificates.jsx`
  - Render GEMASTIK certificate and future-ready certificate cards.
- Modify `react-version/src/components/Experience.jsx`
  - Render translated experience data.
- Modify `react-version/src/components/Education.jsx`
  - Render translated education/certificate-support copy or remove duplicated certificate emphasis if `Certificates` owns the proof section.
- Modify `react-version/src/components/Contact.jsx`
  - Render translated contact copy and form labels.
- Modify `react-version/src/components/Terminal.jsx`
  - Update terminal text to match AI/NLP positioning.
- Modify `react-version/src/index.css`
  - Add cinematic AI visual system, responsive layout rules, segmented language toggle, project/certificate cards.
- Modify `react-version/src/App.css`
  - Remove or leave obsolete starter styles unused; do not rely on this file for new layout.

---

### Task 1: Add Portfolio Content Data And Integrity Test

**Files:**
- Create: `react-version/src/data/portfolioContent.test.mjs`
- Create: `react-version/src/data/portfolioContent.js`
- Modify: `react-version/package.json`

- [ ] **Step 1: Write the failing content integrity test**

Create `react-version/src/data/portfolioContent.test.mjs`:

```js
import assert from 'node:assert/strict';
import {
  content,
  getCopy,
  languages,
  projects,
  certificates,
} from './portfolioContent.js';

assert.deepEqual(languages, ['en', 'id']);

for (const language of languages) {
  const copy = getCopy(language);
  assert.equal(typeof copy.nav.home, 'string');
  assert.equal(typeof copy.hero.headline, 'string');
  assert.equal(copy.projects.title.length > 0, true);
  assert.equal(copy.contact.form.name.length > 0, true);
}

assert.equal(getCopy('unknown'), content.en);
assert.equal(projects.length, 4);
assert.equal(
  projects.some((project) => project.slug === 'indonesia-toxic-speech-detector'),
  true,
);
assert.equal(
  projects.find((project) => project.slug === 'indonesia-toxic-speech-detector').repo,
  'https://github.com/BayuSatrio2804/Indonesia-Toxic-Speech-Detector',
);
assert.equal(certificates.length >= 1, true);
assert.equal(certificates[0].image, '/sertifikat-gemastik.png');

console.log('portfolio content integrity ok');
```

- [ ] **Step 2: Run the test to verify it fails**

Run:

```powershell
cd C:\Portofolio\react-version
node src\data\portfolioContent.test.mjs
```

Expected: FAIL with `Cannot find module` for `portfolioContent.js`.

- [ ] **Step 3: Add the content data module**

Create `react-version/src/data/portfolioContent.js` with:

```js
export const languages = ['en', 'id'];

export const content = {
  en: {
    nav: {
      home: 'Home',
      about: 'About',
      projects: 'Projects',
      certificates: 'Certificates',
      experience: 'Experience',
      education: 'Education',
      contact: 'Contact',
    },
    hero: {
      eyebrow: 'GEMASTIK XVIII Silver Medalist',
      name: 'Muhammad Bayu Satrio',
      headline: 'AI Engineer focused on NLP, Analytics, and Applied Systems.',
      body: 'I build Indonesian NLP models, data-driven workflows, and production-aware systems that connect machine learning with useful software products.',
      primaryCta: 'View Projects',
      secondaryCta: 'Download Resume',
      chipTitle: 'AI Engineer / Data Analyst',
      chipMeta: 'NLP, analytics, model deployment',
      proof: ['Indonesian NLP', 'Analytics', 'ONNX Runtime', 'Applied Systems'],
    },
    about: {
      eyebrow: 'About Me',
      title: 'Turning data and models into applied systems.',
      body: [
        'I am an Information Technology student at Telkom University focused on AI engineering, NLP, analytics, and software systems that can be used in real environments.',
        'My work combines model training, metric-driven validation, backend development, and product thinking across projects such as toxic speech detection, healthcare records, IoT fermentation, and donation platforms.',
      ],
      capabilities: [
        { title: 'AI / NLP', body: 'Indonesian text classification workflows using transformer-based models.' },
        { title: 'Analytics', body: 'Validation splits, threshold tuning, metrics, and decision-oriented reporting.' },
        { title: 'Deployment', body: 'Production-aware artifact export for CPU inference with ONNX Runtime.' },
        { title: 'Applied Systems', body: 'Backend, fullstack, IoT, and product execution for real project constraints.' },
      ],
    },
    projects: {
      eyebrow: 'Selected Work',
      title: 'Projects that connect models, data, and software.',
      expand: 'View Details',
      collapse: 'Hide Details',
      repo: 'View Repository',
      visualDocumentation: 'Visual Documentation',
      contributions: 'Responsibilities & Contributions',
    },
    certificates: {
      eyebrow: 'Proof & Recognition',
      title: 'Certificates & Achievements',
      body: 'A focused record of competitions, certificates, and recognitions. More certificates can be added through the certificate data list later.',
      view: 'View Certificate',
    },
    experience: {
      eyebrow: 'Experience',
      title: 'Applied technical journey',
    },
    education: {
      eyebrow: 'Education',
      title: 'Academic foundation',
      certificatesTitle: 'Recognition Highlight',
    },
    contact: {
      eyebrow: 'Contact',
      title: "Let's build useful AI and data systems.",
      body: 'Open to collaboration, internship opportunities, AI/data roles, and applied software projects.',
      github: 'GitHub',
      linkedin: 'LinkedIn',
      form: {
        name: 'Your Name',
        email: 'Your Email',
        subject: 'Subject / Purpose',
        message: 'Write your message here...',
        send: 'Send Message',
        sending: 'Sending Message...',
        sentTitle: 'Message Sent',
        sentBody: 'Thank you for reaching out. I will respond to your email as soon as possible.',
        another: 'Send Another Message',
      },
    },
  },
  id: {
    nav: {
      home: 'Beranda',
      about: 'Tentang',
      projects: 'Proyek',
      certificates: 'Sertifikat',
      experience: 'Pengalaman',
      education: 'Pendidikan',
      contact: 'Kontak',
    },
    hero: {
      eyebrow: 'Peraih Medali Perak GEMASTIK XVIII',
      name: 'Muhammad Bayu Satrio',
      headline: 'AI Engineer yang berfokus pada NLP, Analytics, dan Applied Systems.',
      body: 'Saya membangun model NLP bahasa Indonesia, workflow berbasis data, dan sistem siap produksi yang menghubungkan machine learning dengan produk software yang berguna.',
      primaryCta: 'Lihat Proyek',
      secondaryCta: 'Unduh Resume',
      chipTitle: 'AI Engineer / Data Analyst',
      chipMeta: 'NLP, analytics, model deployment',
      proof: ['NLP Indonesia', 'Analytics', 'ONNX Runtime', 'Applied Systems'],
    },
    about: {
      eyebrow: 'Tentang Saya',
      title: 'Mengubah data dan model menjadi sistem yang bisa dipakai.',
      body: [
        'Saya adalah mahasiswa Information Technology di Telkom University dengan fokus pada AI engineering, NLP, analytics, dan sistem software yang bisa digunakan di konteks nyata.',
        'Pekerjaan saya menggabungkan training model, validasi berbasis metrik, backend development, dan product thinking melalui proyek toxic speech detection, rekam medis, IoT fermentasi, dan platform donasi.',
      ],
      capabilities: [
        { title: 'AI / NLP', body: 'Workflow klasifikasi teks bahasa Indonesia dengan model berbasis transformer.' },
        { title: 'Analytics', body: 'Validation split, threshold tuning, metrik, dan pelaporan untuk pengambilan keputusan.' },
        { title: 'Deployment', body: 'Export artifact siap produksi untuk CPU inference dengan ONNX Runtime.' },
        { title: 'Applied Systems', body: 'Backend, fullstack, IoT, dan eksekusi produk sesuai constraint nyata.' },
      ],
    },
    projects: {
      eyebrow: 'Karya Pilihan',
      title: 'Proyek yang menghubungkan model, data, dan software.',
      expand: 'Lihat Detail',
      collapse: 'Sembunyikan Detail',
      repo: 'Lihat Repository',
      visualDocumentation: 'Dokumentasi Visual',
      contributions: 'Tanggung Jawab & Kontribusi',
    },
    certificates: {
      eyebrow: 'Bukti & Pengakuan',
      title: 'Sertifikat & Pencapaian',
      body: 'Catatan kompetisi, sertifikat, dan pencapaian. Sertifikat lain bisa ditambahkan nanti melalui data certificate.',
      view: 'Lihat Sertifikat',
    },
    experience: {
      eyebrow: 'Pengalaman',
      title: 'Perjalanan teknis terapan',
    },
    education: {
      eyebrow: 'Pendidikan',
      title: 'Fondasi akademik',
      certificatesTitle: 'Highlight Pencapaian',
    },
    contact: {
      eyebrow: 'Kontak',
      title: 'Mari bangun sistem AI dan data yang berguna.',
      body: 'Terbuka untuk kolaborasi, internship, peran AI/data, dan proyek software terapan.',
      github: 'GitHub',
      linkedin: 'LinkedIn',
      form: {
        name: 'Nama Anda',
        email: 'Email Anda',
        subject: 'Subjek / Tujuan',
        message: 'Tulis pesan Anda...',
        send: 'Kirim Pesan',
        sending: 'Mengirim Pesan...',
        sentTitle: 'Pesan Terkirim',
        sentBody: 'Terima kasih sudah menghubungi saya. Saya akan membalas email Anda secepatnya.',
        another: 'Kirim Pesan Lain',
      },
    },
  },
};

export const projects = [
  {
    slug: 'indonesia-toxic-speech-detector',
    title: 'Indonesia Toxic Speech Detector',
    period: '2026',
    role: 'AI Engineer / NLP',
    tech: ['Python', 'IndoBERTweet', 'Transformers', 'ONNX Runtime', 'Scikit-learn'],
    repo: 'https://github.com/BayuSatrio2804/Indonesia-Toxic-Speech-Detector',
    summary: {
      en: 'Notebook-first Indonesian toxic speech classifier with threshold tuning and production CPU inference artifacts.',
      id: 'Classifier toxic speech bahasa Indonesia berbasis notebook dengan threshold tuning dan artifact CPU inference siap produksi.',
    },
    details: {
      en: [
        'Fine-tuned IndoBERTweet for binary Indonesian toxic speech classification.',
        'Built a stratified train, validation, and test workflow with final test metrics reported once.',
        'Tuned the toxic-class decision threshold on validation data before final evaluation.',
        'Exported Hugging Face and FP32 ONNX Runtime CPU inference artifacts.',
        'Kept FP32 ONNX after quantization produced unacceptable probability drift.',
      ],
      id: [
        'Melakukan fine-tuning IndoBERTweet untuk klasifikasi toxic speech bahasa Indonesia.',
        'Membangun workflow stratified train, validation, dan test dengan final metrics yang dilaporkan sekali.',
        'Melakukan tuning threshold kelas toxic pada data validation sebelum evaluasi final.',
        'Mengekspor artifact Hugging Face dan FP32 ONNX Runtime untuk CPU inference.',
        'Mempertahankan FP32 ONNX karena quantization menghasilkan probability drift yang terlalu besar.',
      ],
    },
    images: [],
  },
  {
    slug: 'bidanku',
    title: 'Bidanku - Midwifery Management System',
    period: 'Aug 2025 - Jan 2026',
    role: 'Back-End Web Development',
    tech: ['Node.js', 'SQL', 'Database Architecture'],
    repo: 'https://github.com/Xaverria30/bidanku',
    summary: {
      en: 'Digital medical record system for midwifery clinics with secure relational data workflows.',
      id: 'Sistem rekam medis digital untuk klinik kebidanan dengan workflow data relasional yang aman.',
    },
    details: {
      en: [
        'Designed relational schemas for ANC, family planning, deliveries, immunizations, and patient workflows.',
        'Implemented medical calculations for LMP and EDD.',
        'Optimized SQL reporting and created audit logging for data modifications.',
      ],
      id: [
        'Merancang skema relasional untuk ANC, KB, persalinan, imunisasi, dan workflow pasien.',
        'Mengimplementasikan kalkulasi medis untuk LMP dan EDD.',
        'Mengoptimalkan pelaporan SQL dan membuat audit log untuk perubahan data.',
      ],
    },
    images: ['/bidanku-1.jpg', '/bidanku-2.jpg', '/bidanku-3.jpg', '/bidanku-4.jpg'],
  },
  {
    slug: 'acetra',
    title: 'ACETRA - Smart IoT Fermentation System',
    period: 'Jun 2025 - Oct 2025',
    role: 'CFO & ICT Business Development',
    tech: ['IoT', 'ESP32', 'Firebase', 'Business Strategy'],
    repo: '',
    summary: {
      en: 'Smart IoT monitoring system for coffee husk fermentation and GEMASTIK XVIII ICT Business Development.',
      id: 'Sistem monitoring IoT untuk fermentasi limbah kulit kopi dan GEMASTIK XVIII ICT Business Development.',
    },
    details: {
      en: [
        'Monitored pH, temperature, and gas parameters through ESP32 and Firebase integration.',
        'Managed hardware procurement strategy and financial planning.',
        'Connected IoT capabilities with product and business feasibility.',
      ],
      id: [
        'Memonitor pH, temperatur, dan gas melalui integrasi ESP32 dan Firebase.',
        'Mengelola strategi pengadaan hardware dan perencanaan finansial.',
        'Menghubungkan kapabilitas IoT dengan kelayakan produk dan bisnis.',
      ],
    },
    images: ['/acetra-1.jpg', '/acetra-2.jpg', '/acetra-poster.jpg'],
  },
  {
    slug: 'donasiku',
    title: 'DonasiKu - Second-Hand Goods Platform',
    period: '2025',
    role: 'Fullstack Developer',
    tech: ['React.js', 'Vite', 'Laravel', 'PHP'],
    repo: 'https://github.com/Mazkad12/DONASIKU-WEBPRO',
    summary: {
      en: 'Donation platform for second-hand goods distribution with tracking and communication workflows.',
      id: 'Platform donasi barang bekas dengan workflow tracking distribusi dan komunikasi.',
    },
    details: {
      en: [
        'Built a responsive landing experience to improve donor trust and interaction.',
        'Developed donation tracking for distribution transparency.',
        'Integrated real-time chat workflows between donors and recipients.',
      ],
      id: [
        'Membangun landing experience responsif untuk meningkatkan kepercayaan dan interaksi donor.',
        'Mengembangkan donation tracking untuk transparansi distribusi.',
        'Mengintegrasikan workflow live chat antara donor dan penerima.',
      ],
    },
    images: ['/donasiku-1.jpg', '/donasiku-2.jpg', '/donasiku-3.jpg'],
  },
];

export const certificates = [
  {
    title: '2nd Place GEMASTIK XVIII National Level in ICT Business Development',
    issuer: 'Ministry of Higher Education, Science, and Technology RI',
    year: 'Oct 2025',
    category: 'Competition',
    image: '/sertifikat-gemastik.png',
  },
];

export const experiences = [
  {
    role: 'Back-End Web Developer',
    company: 'Bidanku Digital Transformation Group',
    period: 'Aug 2025 - Jan 2026',
    desc: {
      en: 'Designed relational data structures and backend workflows for digital medical record operations.',
      id: 'Merancang struktur data relasional dan workflow backend untuk operasional rekam medis digital.',
    },
  },
  {
    role: 'CFO & ICT Business Development',
    company: 'ACETRA Smart IoT System',
    period: 'Jun 2025 - Oct 2025',
    desc: {
      en: 'Connected IoT engineering, financial planning, and product feasibility for a fermentation monitoring system.',
      id: 'Menghubungkan engineering IoT, perencanaan finansial, dan kelayakan produk untuk sistem monitoring fermentasi.',
    },
  },
  {
    role: 'Fullstack Web Developer',
    company: 'DonasiKu Platform',
    period: '2025',
    desc: {
      en: 'Built React and Laravel-based workflows for donation discovery, tracking, and communication.',
      id: 'Membangun workflow berbasis React dan Laravel untuk discovery, tracking, dan komunikasi donasi.',
    },
  },
];

export const education = [
  {
    institution: 'Telkom University',
    degree: 'Information Technology',
    period: 'Sep 2023 - Sep 2027',
    logo: 'https://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Logo_Telkom_University_potrait.png/600px-Logo_Telkom_University_potrait.png',
    desc: {
      en: 'Studying software engineering, modern computing systems, analytics, and applied information technology.',
      id: 'Mempelajari software engineering, sistem komputasi modern, analytics, dan teknologi informasi terapan.',
    },
  },
  {
    institution: 'SMAN 3 Banjarmasin',
    degree: 'High School Diploma, Science',
    period: '2020 - 2023',
    logo: '',
    desc: {
      en: 'Built a foundation in logical thinking, mathematics, science, and analytical problem solving.',
      id: 'Membangun fondasi berpikir logis, matematika, sains, dan pemecahan masalah analitis.',
    },
  },
];

export function getCopy(language) {
  return content[language] ?? content.en;
}
```

- [ ] **Step 4: Add the test script**

Modify `react-version/package.json` scripts to include:

```json
"test:content": "node src/data/portfolioContent.test.mjs"
```

- [ ] **Step 5: Run the test to verify it passes**

Run:

```powershell
cd C:\Portofolio\react-version
npm run test:content
```

Expected: PASS and output includes `portfolio content integrity ok`.

- [ ] **Step 6: Commit**

Run:

```powershell
cd C:\Portofolio
git add react-version/package.json react-version/src/data/portfolioContent.js react-version/src/data/portfolioContent.test.mjs
git commit -m "Add bilingual portfolio content data"
```

---

### Task 2: Wire App State, Navigation, And Certificates Section

**Files:**
- Modify: `react-version/src/App.jsx`
- Modify: `react-version/src/components/Navigation.jsx`
- Create: `react-version/src/components/Certificates.jsx`

- [ ] **Step 1: Update App language state and section props**

Modify `react-version/src/App.jsx` so it imports the data module, owns `language`, and renders the new section order:

```jsx
import { useEffect, useMemo, useState } from 'react'
import Background3D from './components/Background3D'
import Navigation from './components/Navigation'
import CustomCursor from './components/CustomCursor'
import Preloader from './components/Preloader'
import Terminal from './components/Terminal'
import Hero from './components/Hero'
import BentoSection from './components/BentoSection'
import Experience from './components/Experience'
import Education from './components/Education'
import Projects from './components/Projects'
import Certificates from './components/Certificates'
import Contact from './components/Contact'
import {
  certificates,
  education,
  experiences,
  getCopy,
  projects,
} from './data/portfolioContent'

function App() {
  const [language, setLanguage] = useState(() => localStorage.getItem('portfolio-language') || 'en')
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
      <Navigation copy={copy.nav} language={language} onLanguageChange={setLanguage} />
      <main>
        <Hero copy={copy.hero} />
        <BentoSection copy={copy.about} />
        <Projects copy={copy.projects} language={language} projects={projects} />
        <Certificates copy={copy.certificates} certificates={certificates} />
        <Experience copy={copy.experience} language={language} experiences={experiences} />
        <Education copy={copy.education} language={language} education={education} />
        <Terminal language={language} />
        <Contact copy={copy.contact} />
      </main>
      <footer className="site-footer">
        <p>&copy; 2026 Muhammad Bayu Satrio. Built with React.</p>
      </footer>
    </>
  )
}

export default App
```

- [ ] **Step 2: Replace Navigation with accessible section links and language toggle**

Modify `react-version/src/components/Navigation.jsx` to accept `copy`, `language`, and `onLanguageChange`. Keep the live clock and fullscreen button, remove the language alert, and add buttons:

```jsx
<div className="language-toggle" aria-label="Language selector">
  {['en', 'id'].map((item) => (
    <button
      key={item}
      type="button"
      className={language === item ? 'active' : ''}
      onClick={() => onLanguageChange(item)}
      aria-pressed={language === item}
    >
      {item.toUpperCase()}
    </button>
  ))}
</div>
```

Use nav links for `#hero`, `#about`, `#projects`, `#certificates`, and `#contact`.

- [ ] **Step 3: Add Certificates component**

Create `react-version/src/components/Certificates.jsx`:

```jsx
import { Award, ExternalLink } from 'lucide-react'
import { motion } from 'framer-motion'

function Certificates({ copy, certificates }) {
  return (
    <section id="certificates" className="section certificates-section">
      <div className="section-heading">
        <span className="eyebrow">{copy.eyebrow}</span>
        <h2>{copy.title}</h2>
        <p>{copy.body}</p>
      </div>
      <div className="certificate-grid">
        {certificates.map((certificate) => (
          <motion.article
            key={`${certificate.title}-${certificate.year}`}
            className="certificate-card"
            initial={{ opacity: 0, y: 28 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true, amount: 0.2 }}
            transition={{ duration: 0.45 }}
          >
            <img src={certificate.image} alt={`${certificate.title} certificate`} />
            <div>
              <div className="card-kicker">
                <Award size={16} />
                <span>{certificate.category}</span>
              </div>
              <h3>{certificate.title}</h3>
              <p>{certificate.issuer}</p>
              <span className="certificate-year">{certificate.year}</span>
              <a href={certificate.image} target="_blank" rel="noreferrer" className="text-link">
                {copy.view}
                <ExternalLink size={15} />
              </a>
            </div>
          </motion.article>
        ))}
      </div>
    </section>
  )
}

export default Certificates
```

- [ ] **Step 4: Run content test and build**

Run:

```powershell
cd C:\Portofolio\react-version
npm run test:content
npm run build
```

Expected: content test passes and Vite build exits 0.

- [ ] **Step 5: Commit**

Run:

```powershell
cd C:\Portofolio
git add react-version/src/App.jsx react-version/src/components/Navigation.jsx react-version/src/components/Certificates.jsx
git commit -m "Wire bilingual navigation and certificate section"
```

---

### Task 3: Rebuild Hero And About Sections

**Files:**
- Modify: `react-version/src/components/Hero.jsx`
- Modify: `react-version/src/components/BentoSection.jsx`
- Modify: `react-version/src/components/Terminal.jsx`

- [ ] **Step 1: Replace Hero with AI-first content**

Modify `Hero.jsx` to accept `copy`, render the profile chip, proof pills, CTA buttons, and a non-portrait-dominant AI signal visual.

Use this component shape:

```jsx
function Hero({ copy }) {
  return (
    <section id="hero" className="hero-section">
      <div className="hero-copy">
        <span className="eyebrow">{copy.eyebrow}</span>
        <h1>{copy.headline}</h1>
        <p>{copy.body}</p>
        <div className="hero-proof">
          {copy.proof.map((item) => <span key={item}>{item}</span>)}
        </div>
        <div className="hero-actions">
          <a href="#projects" className="btn primary-btn">{copy.primaryCta}</a>
          <a href="/Resume_Muhammad_Bayu_Satrio.pdf?v=new" download="Resume_Muhammad_Bayu_Satrio.pdf" className="btn secondary-btn">{copy.secondaryCta}</a>
        </div>
      </div>
      <div className="hero-visual" aria-label="AI inference signal visualization">
        <div className="profile-chip">
          <img src="/foto-saya.jpg" alt="Muhammad Bayu Satrio" />
          <div>
            <strong>{copy.name}</strong>
            <span>{copy.chipTitle}</span>
          </div>
        </div>
        <div className="signal-panel">
          {Array.from({ length: 24 }).map((_, index) => (
            <span key={index} className={index % 5 === 0 ? 'hot' : ''} />
          ))}
        </div>
        <p>{copy.chipMeta}</p>
      </div>
    </section>
  )
}
```

- [ ] **Step 2: Replace BentoSection with About Me and capability cards**

Modify `BentoSection.jsx` to accept `copy`, render `foto-saya.jpg`, about paragraphs, and `copy.capabilities`.

Use `className="about-section section"` and capability cards with stable class names:

```jsx
<section id="about" className="section about-section">
  <div className="about-photo-card">
    <img src="/foto-saya.jpg" alt="Muhammad Bayu Satrio profile portrait" />
  </div>
  <div className="about-content">
    <span className="eyebrow">{copy.eyebrow}</span>
    <h2>{copy.title}</h2>
    {copy.body.map((paragraph) => <p key={paragraph}>{paragraph}</p>)}
    <div className="capability-grid">
      {copy.capabilities.map((capability) => (
        <article key={capability.title} className="capability-card">
          <h3>{capability.title}</h3>
          <p>{capability.body}</p>
        </article>
      ))}
    </div>
  </div>
</section>
```

- [ ] **Step 3: Update Terminal text**

Modify `Terminal.jsx` so `whoami` returns AI positioning and `skills` includes Python, NLP, analytics, ONNX Runtime, React, Laravel, SQL, Firebase, and ESP32. Accept `language` prop and use Indonesian responses when `language === 'id'`.

- [ ] **Step 4: Run verification**

Run:

```powershell
cd C:\Portofolio\react-version
npm run test:content
npm run build
```

Expected: test passes and build exits 0.

- [ ] **Step 5: Commit**

Run:

```powershell
cd C:\Portofolio
git add react-version/src/components/Hero.jsx react-version/src/components/BentoSection.jsx react-version/src/components/Terminal.jsx
git commit -m "Refocus hero and about sections on AI engineering"
```

---

### Task 4: Rebuild Projects From Data

**Files:**
- Modify: `react-version/src/components/Projects.jsx`

- [ ] **Step 1: Render projects from props**

Modify `Projects.jsx` to accept `copy`, `language`, and `projects`. Remove the hardcoded local `projects` array. For each project use:

```jsx
const summary = project.summary[language] ?? project.summary.en
const details = project.details[language] ?? project.details.en
```

Keep one expanded project state. Render four equal cards, tech pills, optional image gallery, optional repo link, and a generated AI visual for Toxic Speech Detector when `project.images.length === 0`.

- [ ] **Step 2: Add generated model visual for no-image projects**

Inside the project card, when no images exist, render:

```jsx
<div className="model-visual" aria-label={`${project.title} model workflow visual`}>
  <span>dataset.csv</span>
  <span>IndoBERTweet</span>
  <span>threshold.json</span>
  <span>model.onnx</span>
</div>
```

- [ ] **Step 3: Run verification**

Run:

```powershell
cd C:\Portofolio\react-version
npm run test:content
npm run build
```

Expected: test passes and build exits 0.

- [ ] **Step 4: Commit**

Run:

```powershell
cd C:\Portofolio
git add react-version/src/components/Projects.jsx
git commit -m "Render AI-focused project portfolio from data"
```

---

### Task 5: Update Supporting Sections

**Files:**
- Modify: `react-version/src/components/Experience.jsx`
- Modify: `react-version/src/components/Education.jsx`
- Modify: `react-version/src/components/Contact.jsx`

- [ ] **Step 1: Update Experience to render data props**

Modify `Experience.jsx` to accept `copy`, `language`, and `experiences`. Render `experience.desc[language] ?? experience.desc.en`.

- [ ] **Step 2: Update Education to render data props**

Modify `Education.jsx` to accept `copy`, `language`, and `education`. Keep the Telkom logo fallback behavior. Remove the large GEMASTIK certificate card from Education because certificates now have a dedicated section.

- [ ] **Step 3: Update Contact copy and form labels**

Modify `Contact.jsx` to accept `copy`. Replace all hardcoded text and input label text with `copy` fields. Keep the existing FormSubmit behavior and contact links.

- [ ] **Step 4: Run verification**

Run:

```powershell
cd C:\Portofolio\react-version
npm run test:content
npm run build
```

Expected: test passes and build exits 0.

- [ ] **Step 5: Commit**

Run:

```powershell
cd C:\Portofolio
git add react-version/src/components/Experience.jsx react-version/src/components/Education.jsx react-version/src/components/Contact.jsx
git commit -m "Localize supporting portfolio sections"
```

---

### Task 6: Apply Visual System And Final Verification

**Files:**
- Modify: `react-version/src/index.css`
- Modify: `react-version/src/App.css`
- Optional modify: any component from Tasks 2-5 only to replace leftover inline layout with class names required by CSS.

- [ ] **Step 1: Add global cinematic AI CSS**

Update `index.css` with stable classes used above:

```css
:root {
  --bg-color: #05070d;
  --surface: rgba(255, 255, 255, 0.045);
  --surface-strong: rgba(255, 255, 255, 0.075);
  --border: rgba(255, 255, 255, 0.105);
  --text-color: #eef6ff;
  --muted: #94a3b8;
  --accent: #38bdf8;
  --accent-strong: #67e8f9;
  --ink: #020617;
}

.section {
  width: min(1180px, calc(100% - 32px));
  margin: 0 auto;
  padding: 7rem 0 3rem;
}

.section-heading {
  max-width: 760px;
  margin-bottom: 2rem;
}

.eyebrow {
  display: inline-flex;
  color: var(--accent);
  font-size: 0.78rem;
  font-weight: 800;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  margin-bottom: 0.9rem;
}

.hero-section {
  min-height: 100vh;
  width: min(1180px, calc(100% - 32px));
  margin: 0 auto;
  display: grid;
  grid-template-columns: minmax(0, 1.1fr) minmax(320px, 0.9fr);
  gap: 3rem;
  align-items: center;
  padding: 8rem 0 4rem;
}

.hero-copy h1 {
  font-size: clamp(2.8rem, 7vw, 6.7rem);
  line-height: 0.93;
  max-width: 900px;
}

.hero-copy p,
.section-heading p,
.about-content p,
.project-card p,
.certificate-card p {
  color: var(--muted);
  line-height: 1.7;
}

.hero-proof,
.hero-actions,
.nav-links,
.language-toggle,
.contact-links {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  align-items: center;
}

.hero-proof span,
.project-tech span {
  border: 1px solid rgba(56, 189, 248, 0.22);
  color: #bae6fd;
  background: rgba(56, 189, 248, 0.08);
  border-radius: 999px;
  padding: 0.42rem 0.72rem;
  font-size: 0.8rem;
}

.hero-visual,
.about-photo-card,
.project-card,
.certificate-card,
.capability-card,
.timeline-card,
.education-card,
.contact-panel {
  background: linear-gradient(145deg, rgba(255,255,255,0.07), rgba(255,255,255,0.025));
  border: 1px solid var(--border);
  border-radius: 8px;
  box-shadow: 0 24px 70px rgba(0,0,0,0.35);
}

.profile-chip {
  display: flex;
  align-items: center;
  gap: 0.9rem;
}

.profile-chip img {
  width: 58px;
  height: 58px;
  object-fit: cover;
  border-radius: 50%;
}

.signal-panel,
.model-visual {
  display: grid;
  gap: 0.6rem;
}

.language-toggle {
  padding: 0.25rem;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: rgba(255,255,255,0.04);
}

.language-toggle button {
  border: 0;
  border-radius: 999px;
  background: transparent;
  color: var(--muted);
  padding: 0.42rem 0.7rem;
  cursor: pointer;
  font-weight: 800;
}

.language-toggle button.active {
  background: var(--accent);
  color: var(--ink);
}

@media (max-width: 860px) {
  .hero-section,
  .about-section,
  .contact-grid {
    grid-template-columns: 1fr;
  }

  .hero-section {
    padding-top: 7rem;
  }

  .nav-links {
    display: none;
  }
}
```

Expand this CSS with the exact card grid classes needed by implemented components. Keep card radius at 8px unless a circular avatar is intentional.

- [ ] **Step 2: Run lint**

Run:

```powershell
cd C:\Portofolio\react-version
npm run lint
```

Expected: exit 0. If it fails on pre-existing generated warnings, capture exact output and fix only issues introduced by this work.

- [ ] **Step 3: Run build and content test**

Run:

```powershell
cd C:\Portofolio\react-version
npm run test:content
npm run build
```

Expected: content test passes and Vite build exits 0.

- [ ] **Step 4: Browser verification**

Run the dev server:

```powershell
cd C:\Portofolio\react-version
npm run dev -- --host 127.0.0.1
```

Open the local URL in the browser and verify:

- hero renders with AI headline and profile chip
- About section shows the larger profile photo
- `EN | ID` changes text without reload
- four project cards render, including Toxic Speech Detector
- project expansion works
- GEMASTIK certificate renders in Certificates section
- resume link points to `/Resume_Muhammad_Bayu_Satrio.pdf?v=new`
- contact form layout is usable on desktop and mobile widths

- [ ] **Step 5: Final commit**

Run:

```powershell
cd C:\Portofolio
git add react-version/src/index.css react-version/src/App.css react-version/src
git commit -m "Polish cinematic AI portfolio interface"
```

---

## Self-Review Notes

Spec coverage:

- AI Engineer/Data Analyst positioning is implemented in Tasks 1, 3, and 5.
- English default plus `EN | ID` toggle is implemented in Tasks 1 and 2.
- About Me and profile photo are implemented in Task 3.
- Toxic Speech Detector as equal project is implemented in Tasks 1 and 4.
- Certificates section and future-ready certificate data are implemented in Tasks 1 and 2.
- Dark cinematic AI visual direction is implemented in Task 6.
- Verification commands and browser inspection are implemented in Task 6.

No intentionally deferred requirements are left in this plan.

# Business Website Template Conversion Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Convert the existing 14-page Najifa International recruitment website into a reusable, premium-looking, generic-branded recruitment-industry template using Open Sans typography, a green/black/white light theme, and a unified component design language.

**Architecture:** Inline CSS per file (file structure preserved). Establish `index.html` as the gold-standard reference page first, then propagate the same design tokens, utility classes, navbar, and footer markup verbatim to all 13 remaining pages. Replace all "Najifa" branding and personal info with generic placeholder strings via a defined dictionary. No new files except this plan and the spec.

**Tech Stack:** Vanilla HTML5, CSS3 (CSS custom properties, `clamp()`, CSS Grid, Flexbox), inline `<style>` blocks per file, minimal vanilla JS (one `IntersectionObserver` for scroll-fade). Google Fonts (Open Sans). Hosted on GitHub + Vercel (auto-deploy on push).

**Spec reference:** [`docs/superpowers/specs/2026-05-07-business-template-conversion-design.md`](../specs/2026-05-07-business-template-conversion-design.md)

**Working directory for HTML edits:** `Najifa Clone/` (subfolder; outer dir is repo root with `vercel.json`, `docs/`, etc.)

**Verification model:** This is template/UI work — no unit tests apply. Each task lists explicit **acceptance criteria** the engineer verifies in browser at four widths (375 / 768 / 1024 / 1280) before committing. Vercel auto-deploys each commit; the engineer can use the Vercel preview URL for visual verification.

**Shell:** All commands assume **PowerShell** on Windows. `bash`-style forward-slash paths work in PowerShell. For `grep` invocations, prefer Git Bash (bundled with Git for Windows) or use `Select-String` as a PowerShell equivalent (e.g., `Select-String -Path "Najifa Clone\*.html" -Pattern "najifa" -CaseSensitive:$false`).

---

## File Structure

| File | Action | Purpose |
|---|---|---|
| `Najifa Clone/index.html` | Modify | Gold-standard reference page (Phase 1) |
| `Najifa Clone/about.html` | Modify | Phase 2 propagate |
| `Najifa Clone/ceo-message.html` | Modify | Phase 2 propagate |
| `Najifa Clone/management.html` | Modify | Phase 2 propagate |
| `Najifa Clone/mission-vision.html` | Modify | Phase 2 propagate |
| `Najifa Clone/for-clients.html` | Modify | Phase 2 propagate |
| `Najifa Clone/for-applicants.html` | Modify | Phase 2 propagate |
| `Najifa Clone/our-success.html` | Modify | Phase 2 propagate |
| `Najifa Clone/clients.html` | Modify | Phase 2 propagate |
| `Najifa Clone/job-categories.html` | Modify | Phase 2 propagate |
| `Najifa Clone/gallery.html` | Modify | Phase 2 propagate |
| `Najifa Clone/licences.html` | Modify | Phase 2 propagate |
| `Najifa Clone/contact.html` | Modify | Phase 2 propagate (preserve Google Maps iframe) |
| `Najifa Clone/apply.html` | Modify | Phase 2 propagate (preserve Google Maps iframe) |

**No file creates, no renames, no deletes.** Spec doc and `vercel.json` already exist.

---

## Reference Snippets (canonical — copy verbatim into every page)

These reusable snippets appear in **every** task that mentions them. Treat as authoritative. Any inconsistency between this section and a later task is a plan bug — defer to this section.

### R1. Google Fonts link (replaces existing Syne + DM Sans link)

```html
<link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet"/>
```

### R2. Canonical `:root` block (replaces entire existing `:root` block)

```css
:root {
  /* === BRAND GREENS === */
  --green-primary: #155E45;
  --green-dark:    #0D402F;
  --green-light:   #E8F5F1;

  /* === SURFACES === */
  --white-pure:    #FFFFFF;
  --white-soft:    #F8FAF9;
  --black-main:    #0B120F;
  --black-accent:  #1E2924;

  /* === TEXT === */
  --text-main:       #0B120F;
  --text-secondary:  #4A5852;
  --text-muted:      #8A9B94;
  --text-on-dark:    #FFFFFF;
  --text-muted-dark: #B8C5C0;

  /* === BORDERS === */
  --border-light:  rgba(21, 94, 69, 0.15);
  --border-subtle: rgba(11, 18, 15, 0.08);

  /* === ELEVATION === */
  --shadow-soft: 0 4px 20px rgba(11, 18, 15, 0.06);
  --shadow-md:   0 8px 32px rgba(11, 18, 15, 0.08);
  --shadow-lg:   0 16px 48px rgba(21, 94, 69, 0.12);

  /* === MOTION === */
  --transition: all 0.32s cubic-bezier(0.4, 0, 0.2, 1);

  /* === RADIUS === */
  --radius-sm: 8px;
  --radius:    14px;
  --radius-lg: 24px;

  /* === SPACING (8px base grid) === */
  --space-xs:  8px;
  --space-sm:  16px;
  --space-md:  24px;
  --space-lg:  40px;
  --space-xl:  64px;
  --space-2xl: 96px;
  --space-3xl: 128px;

  /* === LEGACY ALIASES (back-compat with old Najifa class names — keep until full audit) === */
  --green: var(--green-primary);
  --green-glow: rgba(21, 94, 69, 0.10);
  --dark: var(--white-pure);     /* old "dark" surfaces become white now */
  --dark2: var(--white-soft);
  --dark3: var(--white-soft);
  --text: var(--text-main);
  --white: var(--white-pure);
  --card-bg: var(--white-pure);
  --card-border: var(--border-subtle);
  --shadow: var(--shadow-md);
}
```

> **Why legacy aliases:** existing CSS in pages references `--green`, `--dark`, `--text`, `--white`, `--card-bg`, etc. Aliasing them to the new tokens lets us swap variables in one place without rewriting every selector. Phase 3 will audit and remove orphans.

### R3. Reset + base body (replaces existing reset block)

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: 'Open Sans', system-ui, -apple-system, sans-serif;
  background: var(--white-pure);
  color: var(--text-main);
  font-size: 16px;
  font-weight: 400;
  line-height: 1.7;
  overflow-x: hidden;
  -webkit-font-smoothing: antialiased;
}
a { text-decoration: none; color: inherit; }
img, iframe, svg { max-width: 100%; display: block; }
button, input, textarea, select { font-family: inherit; }

/* Scrollbar */
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: var(--white-soft); }
::-webkit-scrollbar-thumb { background: var(--green-primary); border-radius: 10px; }
::-webkit-scrollbar-thumb:hover { background: var(--green-dark); }

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### R4. Utility classes (replaces existing `.container`, `.section-tag`, `.section-title`, `.section-sub`, button/card classes)

```css
/* Container */
.container { max-width: 1200px; margin: 0 auto; padding: 0 24px; }

/* Section header trio */
.section-tag {
  display: inline-block;
  font-family: 'Open Sans', sans-serif;
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 2.5px;
  text-transform: uppercase;
  color: var(--green-primary);
  background: var(--green-light);
  border: 1px solid var(--border-light);
  padding: 6px 14px;
  border-radius: 30px;
  margin-bottom: 16px;
}
.section-title {
  font-family: 'Open Sans', sans-serif;
  font-weight: 700;
  font-size: clamp(28px, 4vw, 40px);
  line-height: 1.15;
  letter-spacing: -0.015em;
  color: var(--text-main);
}
.section-title span { color: var(--green-primary); }
.section-sub {
  font-size: clamp(15px, 1.4vw, 18px);
  font-weight: 400;
  color: var(--text-secondary);
  line-height: 1.7;
  margin-top: 12px;
  max-width: 680px;
}

/* Buttons */
.btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: var(--green-primary);
  color: var(--text-on-dark);
  font-family: 'Open Sans', sans-serif;
  font-weight: 600;
  font-size: 15px;
  padding: 14px 28px;
  border-radius: var(--radius-sm);
  border: none;
  cursor: pointer;
  transition: var(--transition);
  white-space: nowrap;
}
.btn-primary:hover {
  background: var(--green-dark);
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(21, 94, 69, 0.25);
}
.btn-outline {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: transparent;
  color: var(--text-main);
  font-family: 'Open Sans', sans-serif;
  font-weight: 600;
  font-size: 15px;
  padding: 14px 28px;
  border-radius: var(--radius-sm);
  border: 1.5px solid var(--border-light);
  cursor: pointer;
  transition: var(--transition);
}
.btn-outline:hover {
  border-color: var(--green-primary);
  color: var(--green-primary);
  background: var(--green-light);
}
.btn-dark {
  background: var(--black-main);
  color: var(--text-on-dark);
}

/* Cards */
.card {
  background: var(--white-pure);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  padding: 32px 28px;
  box-shadow: var(--shadow-soft);
  transition: var(--transition);
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--green-light);
}

/* Form inputs */
.form-input, input[type="text"], input[type="email"], input[type="tel"], input[type="number"], textarea, select {
  width: 100%;
  background: var(--white-soft);
  border: 1.5px solid var(--border-subtle);
  border-radius: var(--radius-sm);
  padding: 14px 18px;
  font-family: 'Open Sans', sans-serif;
  font-size: 15px;
  color: var(--text-main);
  transition: var(--transition);
}
.form-input:focus, input:focus, textarea:focus, select:focus {
  outline: none;
  border-color: var(--green-primary);
  background: var(--white-pure);
  box-shadow: 0 0 0 4px rgba(21, 94, 69, 0.08);
}
.form-input::placeholder, input::placeholder, textarea::placeholder { color: var(--text-muted); }
label { display: block; font-size: 14px; font-weight: 600; color: var(--text-main); margin-bottom: 6px; }

/* Section padding rhythm */
section { padding: var(--space-2xl) 0; }
@media (max-width: 768px) { section { padding: 64px 0; } }

/* Scroll fade-up */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}
.animate-fade-up { animation: fadeUp 0.6s ease-out both; }
```

### R5. Canonical Navbar HTML (replaces existing `<header>` block)

```html
<header id="mainHeader" class="site-header">
  <div class="container nav-container">
    <a href="index.html" class="logo">
      <span class="logo-mark">Your</span><span class="logo-text">Logo</span>
    </a>
    <nav class="main-nav" id="mainNav">
      <div class="nav-item has-dropdown">
        <span>Company</span>
        <div class="dropdown">
          <a href="about.html">About</a>
          <a href="ceo-message.html">CEO Message</a>
          <a href="management.html">Management</a>
          <a href="mission-vision.html">Mission &amp; Vision</a>
          <a href="licences.html">Certifications</a>
        </div>
      </div>
      <div class="nav-item has-dropdown">
        <span>Services</span>
        <div class="dropdown">
          <a href="for-clients.html">For Clients</a>
          <a href="for-applicants.html">For Applicants</a>
          <a href="job-categories.html">Job Categories</a>
          <a href="gallery.html">Gallery</a>
        </div>
      </div>
      <a href="our-success.html" class="nav-item">Success Stories</a>
      <a href="clients.html" class="nav-item">Clients</a>
      <a href="contact.html" class="nav-item">Contact</a>
      <a href="apply.html" class="btn-primary nav-cta">Apply Now</a>
    </nav>
    <button class="nav-toggle" id="navToggle" aria-label="Toggle navigation">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>
```

### R6. Canonical Navbar CSS (replaces existing navbar styles)

```css
.site-header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border-subtle);
  transition: var(--transition);
}
.nav-container { display: flex; align-items: center; justify-content: space-between; height: 72px; }

.logo { font-family: 'Open Sans', sans-serif; font-weight: 800; font-size: 22px; letter-spacing: -0.02em; display: inline-block; }
.logo-mark { color: var(--green-primary); }
.logo-text { color: var(--text-main); }

.main-nav { display: flex; align-items: center; gap: 32px; }
.main-nav .nav-item {
  position: relative;
  font-size: 15px;
  font-weight: 500;
  color: var(--text-main);
  cursor: pointer;
  padding: 8px 0;
  transition: var(--transition);
}
.main-nav .nav-item:hover { color: var(--green-primary); }
.main-nav .nav-item::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--green-primary);
  transition: width 0.3s;
}
.main-nav .nav-item:hover::after { width: 100%; }
.main-nav .nav-cta::after { display: none; }
.main-nav .nav-cta { color: var(--text-on-dark); padding: 12px 22px; font-size: 14px; }

.has-dropdown .dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  min-width: 220px;
  background: var(--white-pure);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  box-shadow: var(--shadow-md);
  padding: 12px;
  opacity: 0;
  visibility: hidden;
  transform: translateY(8px);
  transition: var(--transition);
}
.has-dropdown:hover .dropdown { opacity: 1; visibility: visible; transform: translateY(0); }
.has-dropdown .dropdown a {
  display: block;
  padding: 10px 14px;
  border-radius: var(--radius-sm);
  font-size: 14px;
  color: var(--text-secondary);
  transition: var(--transition);
}
.has-dropdown .dropdown a:hover { background: var(--green-light); color: var(--green-primary); }

.nav-toggle { display: none; background: none; border: none; flex-direction: column; gap: 5px; cursor: pointer; padding: 8px; }
.nav-toggle span { display: block; width: 24px; height: 2px; background: var(--text-main); border-radius: 2px; transition: var(--transition); }

@media (max-width: 1023px) {
  .main-nav {
    position: fixed;
    inset: 72px 0 0 0;
    background: var(--white-pure);
    flex-direction: column;
    align-items: flex-start;
    padding: 32px 24px;
    gap: 20px;
    transform: translateX(100%);
    transition: var(--transition);
    overflow-y: auto;
  }
  .main-nav.open { transform: translateX(0); }
  .nav-toggle { display: flex; }
  .has-dropdown .dropdown { position: static; opacity: 1; visibility: visible; transform: none; box-shadow: none; padding: 0 0 0 16px; border: none; }
}
```

### R7. Canonical Footer HTML

```html
<footer class="site-footer">
  <div class="footer-accent"></div>
  <div class="container">
    <div class="footer-grid">
      <div class="footer-brand">
        <a href="index.html" class="logo">
          <span class="logo-mark">Your</span><span class="logo-text" style="color:var(--text-on-dark);">Logo</span>
        </a>
        <p class="footer-tagline">Your tagline here — a premium business solution helping companies and people connect across borders.</p>
        <div class="footer-social">
          <a href="#" aria-label="Facebook"><svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/></svg></a>
          <a href="#" aria-label="LinkedIn"><svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-4 0v7h-4v-7a6 6 0 0 1 6-6zM2 9h4v12H2zM4 4a2 2 0 1 1 0 4 2 2 0 0 1 0-4z"/></svg></a>
          <a href="#" aria-label="Twitter"><svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M22 5.8a8.5 8.5 0 0 1-2.4.7 4.2 4.2 0 0 0 1.8-2.3 8.4 8.4 0 0 1-2.6 1A4.2 4.2 0 0 0 11.5 9 11.9 11.9 0 0 1 3 4.5a4.2 4.2 0 0 0 1.3 5.6 4 4 0 0 1-1.9-.5v.1a4.2 4.2 0 0 0 3.4 4.1 4.2 4.2 0 0 1-1.9.1 4.2 4.2 0 0 0 3.9 2.9A8.4 8.4 0 0 1 2 18.4 11.9 11.9 0 0 0 8.5 20c7.7 0 11.9-6.4 11.9-11.9v-.5A8.5 8.5 0 0 0 22 5.8z"/></svg></a>
          <a href="#" aria-label="Instagram"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor"/></svg></a>
        </div>
      </div>
      <div class="footer-col">
        <h4>Company</h4>
        <a href="about.html">About</a>
        <a href="ceo-message.html">CEO Message</a>
        <a href="management.html">Management</a>
        <a href="mission-vision.html">Mission &amp; Vision</a>
        <a href="licences.html">Certifications</a>
      </div>
      <div class="footer-col">
        <h4>Services</h4>
        <a href="for-clients.html">For Clients</a>
        <a href="for-applicants.html">For Applicants</a>
        <a href="job-categories.html">Job Categories</a>
        <a href="our-success.html">Success Stories</a>
        <a href="gallery.html">Gallery</a>
      </div>
      <div class="footer-col">
        <h4>Contact</h4>
        <a href="mailto:your@email.com">your@email.com</a>
        <a href="tel:+8801XXXXXXXXX">+880 1XXX-XXXXXX</a>
        <p class="footer-address">Your Address Here<br>City, Country</p>
        <a href="contact.html" class="btn-outline" style="margin-top:12px;color:var(--text-on-dark);border-color:var(--text-muted-dark);">Get in Touch</a>
      </div>
    </div>
    <div class="footer-bottom">
      <span>© <span id="footYear">2026</span> Your Company Name Here. All rights reserved.</span>
      <div class="footer-legal">
        <a href="#">Privacy Policy</a>
        <a href="#">Terms of Service</a>
      </div>
    </div>
  </div>
</footer>
```

### R8. Canonical Footer CSS

```css
.site-footer { position: relative; background: var(--black-main); color: var(--text-muted-dark); padding: 80px 0 32px; }
.footer-accent { position: absolute; top: 0; left: 0; right: 0; height: 4px; background: linear-gradient(90deg, var(--green-primary), var(--green-dark)); }
.footer-grid { display: grid; grid-template-columns: 2fr 1fr 1fr 1.5fr; gap: 48px; margin-bottom: 48px; }
.footer-brand .logo { display: inline-block; margin-bottom: 16px; }
.footer-tagline { font-size: 15px; line-height: 1.7; color: var(--text-muted-dark); margin-bottom: 20px; max-width: 360px; }
.footer-social { display: flex; gap: 12px; }
.footer-social a {
  width: 40px; height: 40px;
  display: inline-flex; align-items: center; justify-content: center;
  background: var(--black-accent); color: var(--text-muted-dark);
  border-radius: 50%; transition: var(--transition);
}
.footer-social a:hover { background: var(--green-primary); color: var(--text-on-dark); transform: translateY(-2px); }

.footer-col h4 { color: var(--text-on-dark); font-size: 16px; font-weight: 700; margin-bottom: 18px; }
.footer-col a { display: block; color: var(--text-muted-dark); font-size: 14px; margin-bottom: 10px; transition: var(--transition); }
.footer-col a:hover { color: var(--green-primary); }
.footer-address { color: var(--text-muted-dark); font-size: 14px; line-height: 1.7; margin-top: 4px; }

.footer-bottom { padding-top: 28px; border-top: 1px solid var(--black-accent); display: flex; justify-content: space-between; flex-wrap: wrap; gap: 16px; font-size: 13px; }
.footer-legal { display: flex; gap: 24px; }
.footer-legal a { color: var(--text-muted-dark); transition: var(--transition); }
.footer-legal a:hover { color: var(--green-primary); }

@media (max-width: 1023px) { .footer-grid { grid-template-columns: 1fr 1fr; gap: 40px; } }
@media (max-width: 640px)  { .footer-grid { grid-template-columns: 1fr; gap: 32px; } .footer-bottom { flex-direction: column; align-items: flex-start; } }
```

### R9. IntersectionObserver scroll-fade JS (place at end of `<body>`)

```html
<script>
  // Mobile nav toggle
  (function () {
    var t = document.getElementById('navToggle');
    var n = document.getElementById('mainNav');
    if (t && n) t.addEventListener('click', function () { n.classList.toggle('open'); });
  })();

  // Scroll-fade for section header blocks
  (function () {
    if (!('IntersectionObserver' in window)) return;
    var obs = new IntersectionObserver(function (entries) {
      entries.forEach(function (e) {
        if (e.isIntersecting) {
          e.target.classList.add('animate-fade-up');
          obs.unobserve(e.target);
        }
      });
    }, { threshold: 0.15, rootMargin: '0px 0px -40px 0px' });
    document.querySelectorAll('[data-fade]').forEach(function (el) { obs.observe(el); });
  })();

  // Footer year
  (function () {
    var y = document.getElementById('footYear');
    if (y) y.textContent = new Date().getFullYear();
  })();

  // Sticky-header style on scroll
  (function () {
    var h = document.getElementById('mainHeader');
    if (!h) return;
    function update() { h.style.boxShadow = window.scrollY > 8 ? '0 4px 20px rgba(11,18,15,0.06)' : 'none'; }
    window.addEventListener('scroll', update, { passive: true }); update();
  })();
</script>
```

### R10. Hero decorative SVG snippet (used in Phase 1 Task 1.5)

```html
<div class="hero-decor" aria-hidden="true">
  <svg class="hero-decor-blob" width="520" height="520" viewBox="0 0 520 520" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <radialGradient id="hg1" cx="50%" cy="50%" r="50%">
        <stop offset="0%" stop-color="#155E45" stop-opacity="0.18"/>
        <stop offset="100%" stop-color="#155E45" stop-opacity="0"/>
      </radialGradient>
    </defs>
    <circle cx="260" cy="260" r="260" fill="url(#hg1)"/>
  </svg>
  <svg class="hero-decor-grid" width="240" height="240" viewBox="0 0 240 240" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <pattern id="dots" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse">
        <circle cx="2" cy="2" r="1.5" fill="#155E45" fill-opacity="0.18"/>
      </pattern>
    </defs>
    <rect width="240" height="240" fill="url(#dots)"/>
  </svg>
</div>
```

```css
.hero-decor { position: absolute; inset: 0; pointer-events: none; overflow: hidden; }
.hero-decor-blob { position: absolute; top: -120px; right: -120px; }
.hero-decor-grid { position: absolute; bottom: -40px; left: -40px; opacity: 0.6; }
```

### R11. Placeholder Dictionary (apply consistently across all 14 pages)

| Find (regex-friendly substring) | Replace with |
|---|---|
| `Najifa International` | `Your Company Name Here` |
| `Najifa Int'l` (header logo span) | `Your Logo` (use R5 logo markup instead) |
| `Najifa` (standalone) | `Your Company` |
| `Work Abroad. No Hassle. Full Support.` | `Your Tagline Here — A Premium Business Solution` |
| `Work Abroad — No Hassle` (and similar tagline variants) | `Premium Business Solution` |
| `contact@najifa.com`, `info@najifa.com`, etc. | `your@email.com` |
| `+8801` followed by digits (any phone) | `+880 1XXX-XXXXXX` |
| `Banani, Dhaka` and other specific addresses | `Your Address Here, City, Country` |
| `© 2012` (or any © YYYY) | `© <span id="footYear">2026</span>` (script auto-updates) |
| `Bangladesh` (when used as origin country) | leave as is (recruitment template flavor) |
| `Saudi Arabia`, `Qatar`, `UAE`, etc. (destinations) | leave as is (recruitment template flavor) |
| Specific person names (CEO, manager, applicant) | `Your Name` |
| Specific designations (e.g., "Founder & MD") | `Your Designation` |
| Specific testimonial quotes | replace with: `"Working with this company has been transformative — clear communication, full support, and real results from start to finish."` |
| Testimonial author names | `Your Client Name` |
| `Nahdi Pharmacy` and other specific employer names | `International Employer` |
| Specific license numbers (e.g., `RL-1234`) | `License No. XXX-XXXX` |
| `12+ years`, `25+ clients`, etc. (specific stats) | round to generic: `10+ Years`, `20+ Clients`, `5,000+ Placements` |
| `<title>...</title>` | `<title>Your Company Name — Tagline Here</title>` (per page, append page name e.g. `— About`) |
| `<meta name="description" content="...">` | `<meta name="description" content="Your business description goes here. Replace with your company's value proposition.">` |

### R12. Acceptance Criteria Checklist (used at end of every transformation task)

After modifying a file, verify in a browser at widths 375, 768, 1024, 1280:

- [ ] No "Najifa" or "najifa" string remains anywhere on the page (Ctrl+F check)
- [ ] No real personal name, real email, or real phone number remains
- [ ] Page background is white (`var(--white-pure)`), not dark
- [ ] All text uses Open Sans (no Syne, no DM Sans residue)
- [ ] Headings render in `--text-main` (#0B120F), body in `--text-secondary` (#4A5852)
- [ ] Primary buttons are green (`#155E45`), white text
- [ ] Navbar is sticky white with `Your Logo` (green "Your" + dark "Logo")
- [ ] Footer is dark (`#0B120F`) with green accent bar at top, white headings, muted gray body
- [ ] At ≤1023px, navbar collapses to hamburger; menu opens/closes when tapped
- [ ] No horizontal scroll at any width (no overflow)
- [ ] No broken images (template is icon-only — but verify SVGs render)
- [ ] All `<a>` tags have valid href (relative `.html` or `#`)

---

## Phase 1 — Transform `index.html` (Gold Standard)

> **Note for the engineer:** Open the Vercel preview URL in a browser tab pinned next to the editor. After each commit + push, Vercel auto-deploys in ~30s and you can refresh to verify visually.

### Task 1.1: Replace Google Fonts link

**Files:**
- Modify: `Najifa Clone/index.html` (around line 7, the `<link>` for fonts)

- [ ] **Step 1: Open `Najifa Clone/index.html`. Find the existing fonts link:**

```html
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap" rel="stylesheet"/>
```

- [ ] **Step 2: Replace it with the canonical Open Sans link from R1:**

```html
<link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet"/>
```

- [ ] **Step 3: Verify** — search the file for `Syne` and `DM+Sans` — both should return zero results.

- [ ] **Step 4: Commit**

```bash
cd "c:/Databrandix HQ/Najifa Clone"
git add "Najifa Clone/index.html"
git commit -m "index.html: switch to Open Sans Google Font"
```

---

### Task 1.2: Replace `:root` CSS variables block

**Files:**
- Modify: `Najifa Clone/index.html` (the existing `:root { ... }` block, around lines 10–27)

- [ ] **Step 1: Find the existing `:root` block** (starts with `:root {` and ends with the matching `}` before the `/* ===== RESET ===== */` comment).

- [ ] **Step 2: Replace the entire block with the canonical R2 block** (full token set including legacy aliases — see Reference Snippets section R2).

- [ ] **Step 3: Verify** — search for any hex color outside the `:root` block in inline `style="..."` attributes. Note them mentally; they'll be cleaned in Phase 3.

- [ ] **Step 4: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: install new green/black/white light-theme tokens with legacy aliases"
```

---

### Task 1.3: Replace reset + base body styles

**Files:**
- Modify: `Najifa Clone/index.html`

- [ ] **Step 1: Find the existing reset block** — it starts with `/* ===== RESET ===== */` and includes `*, *::before, *::after`, `html`, `body`, `a`, `img`, plus `::-webkit-scrollbar` rules.

- [ ] **Step 2: Replace the entire reset+body+scrollbar section with R3** (Reset + base body — see Reference Snippets).

- [ ] **Step 3: Verify in browser** — page background should now be white, default text dark. Headings still styled by their own classes (untouched at this point).

- [ ] **Step 4: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: convert base body to light theme + Open Sans + reduced-motion support"
```

---

### Task 1.4: Replace utility classes (`.container`, `.section-tag`, `.section-title`, `.section-sub`, buttons, cards, forms)

**Files:**
- Modify: `Najifa Clone/index.html`

- [ ] **Step 1: Find the existing utility section** — around the `/* ===== UTILITY ===== */` comment. It contains `.container`, `.section-tag`, `.section-title`, `.section-sub`, `.btn-primary`, `.btn-outline`, etc.

- [ ] **Step 2: Replace with R4** (Utility classes — see Reference Snippets). This redefines:
  - `.container`, `.section-tag`, `.section-title`, `.section-sub`
  - `.btn-primary`, `.btn-outline`, `.btn-dark`
  - `.card`
  - Form input styles (`.form-input`, `input`, `textarea`, `select`)
  - Section padding rhythm (`section { padding: var(--space-2xl) 0; }`)
  - `.animate-fade-up` keyframe

- [ ] **Step 3: Verify in browser** — section titles render in dark color with green spans, buttons are green, cards (if any visible at this point) are white with soft shadow.

- [ ] **Step 4: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: rewrite utility classes for light theme component system"
```

---

### Task 1.5: Replace navbar HTML + CSS

**Files:**
- Modify: `Najifa Clone/index.html` (header block around line 1097)

- [ ] **Step 1: Locate the existing `<header id="mainHeader">...</header>` block.** It contains a logo, nav items (Company dropdown, Service dropdown, Our Success, Our Clients, Contact, Apply Now), and possibly a mobile toggle.

- [ ] **Step 2: Replace the entire `<header>` block with R5** (Canonical Navbar HTML — see Reference Snippets).

- [ ] **Step 3: Find the existing navbar CSS** — selectors like `#mainHeader`, `.nav-container`, `.logo`, `.nav-item`, `.dropdown`, `.nav-toggle`, `.main-nav`. Some may be split across multiple `@media` blocks.

- [ ] **Step 4: Delete all old navbar CSS and replace with R6** (Canonical Navbar CSS).

- [ ] **Step 5: Verify in browser:**
  - Navbar is white with subtle bottom border
  - Logo reads "Your" (green) + "Logo" (dark), 22px, Open Sans 800
  - Nav items dark, hover → green with underline
  - "Apply Now" is a green pill button
  - At ≤ 1023px width: hamburger appears, menu slides in from right when tapped
  - Dropdown menus appear on hover with white card + soft shadow

- [ ] **Step 6: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: replace navbar with light theme + 'Your Logo' branding + responsive hamburger"
```

---

### Task 1.6: Update hero section (light bg + decorative SVGs + new typography)

**Files:**
- Modify: `Najifa Clone/index.html` (around line 1133, `<section id="hero">`)

- [ ] **Step 1: Locate `<section id="hero">...</section>`** (ends before `<section id="why-choose">`).

- [ ] **Step 2: Replace the hero markup with this light-theme version** (preserves the hero-stats and CTAs but inserts decorative SVGs and removes the dark-photo grid):**

```html
<section id="hero">
  <!-- Decorative background -->
  <div class="hero-decor" aria-hidden="true">
    <svg class="hero-decor-blob" width="520" height="520" viewBox="0 0 520 520" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <radialGradient id="hg1" cx="50%" cy="50%" r="50%">
          <stop offset="0%" stop-color="#155E45" stop-opacity="0.18"/>
          <stop offset="100%" stop-color="#155E45" stop-opacity="0"/>
        </radialGradient>
      </defs>
      <circle cx="260" cy="260" r="260" fill="url(#hg1)"/>
    </svg>
    <svg class="hero-decor-grid" width="240" height="240" viewBox="0 0 240 240" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <pattern id="dots" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse">
          <circle cx="2" cy="2" r="1.5" fill="#155E45" fill-opacity="0.18"/>
        </pattern>
      </defs>
      <rect width="240" height="240" fill="url(#dots)"/>
    </svg>
  </div>

  <div class="container">
    <div class="hero-content" data-fade>
      <div class="hero-badge">
        <span class="hero-badge-dot"></span>
        <span>Premium Business Solution</span>
      </div>
      <h1 class="hero-title">
        Connecting <span>opportunity</span><br>
        with the right people.
      </h1>
      <p class="hero-sub">
        Your Company Name Here helps companies and individuals connect — with step-by-step guidance, training, and proven experience.
      </p>
      <div class="hero-ctas">
        <a href="apply.html" class="btn-primary">Get Started <span>&rarr;</span></a>
        <a href="for-clients.html" class="btn-outline">For Clients</a>
      </div>
      <div class="hero-stats">
        <div class="hero-stat-item">
          <div class="hero-stat-num"><span class="counter" data-target="20">0</span><span>+</span></div>
          <div class="hero-stat-label">Global Clients</div>
        </div>
        <div class="hero-stat-item">
          <div class="hero-stat-num"><span class="counter" data-target="10">0</span><span>+</span></div>
          <div class="hero-stat-label">Countries</div>
        </div>
        <div class="hero-stat-item">
          <div class="hero-stat-num"><span class="counter" data-target="10">0</span><span>+</span></div>
          <div class="hero-stat-label">Years Experience</div>
        </div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Find the existing hero CSS** (selectors `#hero`, `.hero-bg`, `.hero-grid`, `.hero-content`, `.hero-left`, `.hero-right`, `.hero-photo`, `.hero-photo-placeholder`, `.hero-title`, `.hero-sub`, `.hero-badge`, `.hero-stats`, etc.) and **replace with this light-theme hero CSS:**

```css
#hero {
  position: relative;
  background: var(--white-soft);
  padding: var(--space-3xl) 0 var(--space-2xl);
  overflow: hidden;
}
.hero-decor { position: absolute; inset: 0; pointer-events: none; overflow: hidden; }
.hero-decor-blob { position: absolute; top: -120px; right: -120px; }
.hero-decor-grid { position: absolute; bottom: -40px; left: -40px; opacity: 0.6; }

.hero-content { position: relative; max-width: 880px; margin: 0 auto; text-align: center; }
.hero-badge {
  display: inline-flex; align-items: center; gap: 8px;
  background: var(--green-light);
  border: 1px solid var(--border-light);
  padding: 6px 14px;
  border-radius: 30px;
  font-size: 13px; font-weight: 600;
  color: var(--green-primary);
  margin-bottom: 24px;
}
.hero-badge-dot { width: 6px; height: 6px; background: var(--green-primary); border-radius: 50%; animation: pulse 2s infinite; }
@keyframes pulse { 0%,100% { opacity: 1; } 50% { opacity: 0.4; } }

.hero-title {
  font-family: 'Open Sans', sans-serif;
  font-weight: 800;
  font-size: clamp(32px, 6vw, 56px);
  line-height: 1.1;
  letter-spacing: -0.02em;
  color: var(--text-main);
  margin-bottom: 20px;
}
.hero-title span { color: var(--green-primary); }

.hero-sub {
  font-size: clamp(15px, 1.5vw, 18px);
  line-height: 1.7;
  color: var(--text-secondary);
  max-width: 680px;
  margin: 0 auto 36px;
}

.hero-ctas { display: flex; gap: 16px; justify-content: center; flex-wrap: wrap; margin-bottom: 60px; }

.hero-stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  max-width: 720px;
  margin: 0 auto;
  padding: 32px;
  background: var(--white-pure);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  box-shadow: var(--shadow-soft);
}
.hero-stat-item { text-align: center; }
.hero-stat-num {
  font-size: clamp(28px, 3.5vw, 40px);
  font-weight: 800;
  color: var(--green-primary);
  line-height: 1;
  letter-spacing: -0.02em;
}
.hero-stat-label { font-size: 13px; font-weight: 500; color: var(--text-muted); margin-top: 8px; text-transform: uppercase; letter-spacing: 1px; }

@media (max-width: 640px) {
  .hero-stats { grid-template-columns: 1fr; gap: 20px; padding: 24px; }
  .hero-ctas { flex-direction: column; align-items: stretch; }
}
```

- [ ] **Step 4: Verify in browser:**
  - White-soft hero background with subtle green blob top-right + dotted grid bottom-left
  - Centered headline "Connecting opportunity with the right people." (with "opportunity" green)
  - Green pill badge "Premium Business Solution" above headline
  - Two CTAs: green primary + outline secondary
  - White stats card below with 3 columns at desktop, stacks at mobile

- [ ] **Step 5: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: rebuild hero with light theme + decorative SVGs + centered layout"
```

---

### Task 1.7: Update remaining content sections to light theme (`#why-choose`, `#stats-bar`, `#how-it-works`, `#clients-marquee`, `#faq`, `#success-preview`, testimonials)

**Files:**
- Modify: `Najifa Clone/index.html`

> **Engineer note:** index.html has many sections (around 8). Rather than rewriting each from scratch, this task uses a **find-replace recipe** since the legacy aliases in R2 already redirect old `--dark`, `--card-bg`, `--text` etc. to new light tokens. The result is mostly automatic.

- [ ] **Step 1: Find each remaining section's CSS block.** Look for these selectors:
  - `#why-choose`, `.why-card`, `.why-icon`
  - `#stats-bar`, `.stat-item`
  - `#how-it-works`, `.step-card`, `.step-number`
  - `#clients-marquee`, `.marquee-track`, `.client-item`
  - `#faq`, `.faq-item`, `.faq-question`, `.faq-answer`
  - `#success-preview`, `.success-card`
  - `.testimonial`, `.testimonial-card`, `.testimonial-text`, `.testimonial-author`
  - `.cta-banner`, `.cta-banner-overlay`, `.cta-banner-content`, `.cta-banner-text`, `.cta-banner-btns`

- [ ] **Step 2: Apply these targeted CSS overrides at the END of the existing `<style>` block** (don't delete old rules — just override with these later-cascading rules):

```css
/* === Light-theme overrides for content sections === */

/* Card-like blocks: any element previously dark gets white surface */
.why-card, .step-card, .success-card, .testimonial-card, .faq-item {
  background: var(--white-pure);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  box-shadow: var(--shadow-soft);
  color: var(--text-main);
  transition: var(--transition);
}
.why-card:hover, .step-card:hover, .success-card:hover, .testimonial-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--green-light);
}

/* Card body text */
.why-card p, .step-card p, .success-card p, .testimonial-text { color: var(--text-secondary); }

/* Section backgrounds — alternate white-pure ↔ white-soft */
#why-choose, #how-it-works, #clients-marquee, #success-preview { background: var(--white-pure); }
#stats-bar, #faq { background: var(--white-soft); }

/* Stats bar (was dark, now subtle white-soft band) */
#stats-bar .stat-item { background: var(--white-pure); border: 1px solid var(--border-subtle); border-radius: var(--radius); padding: 28px 20px; text-align: center; box-shadow: var(--shadow-soft); }
#stats-bar .stat-item h3, #stats-bar .stat-num { color: var(--green-primary); font-weight: 800; font-size: clamp(28px, 3vw, 40px); }
#stats-bar .stat-item p, #stats-bar .stat-label { color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; font-size: 13px; font-weight: 500; }

/* Why-choose icon tile */
.why-icon { width: 56px; height: 56px; border-radius: var(--radius-sm); background: var(--green-light); color: var(--green-primary); display: inline-flex; align-items: center; justify-content: center; margin-bottom: 20px; }
.why-icon svg { width: 28px; height: 28px; }
.why-card h3 { color: var(--text-main); font-size: 20px; font-weight: 700; margin-bottom: 10px; }

/* How-it-works step number */
.step-number { background: var(--green-primary); color: var(--text-on-dark); width: 44px; height: 44px; border-radius: 50%; display: inline-flex; align-items: center; justify-content: center; font-weight: 700; margin-bottom: 16px; }
.step-card h3 { color: var(--text-main); font-size: 20px; font-weight: 700; margin-bottom: 10px; }

/* Clients marquee — make logos appear as muted gray badges (they're text/SVG icons) */
.client-item { background: var(--white-soft); color: var(--text-secondary); border: 1px solid var(--border-subtle); padding: 16px 24px; border-radius: var(--radius-sm); font-weight: 600; }

/* FAQ */
.faq-item { padding: 20px 24px; margin-bottom: 12px; cursor: pointer; }
.faq-question { color: var(--text-main); font-weight: 600; font-size: 17px; display: flex; justify-content: space-between; align-items: center; }
.faq-answer { color: var(--text-secondary); margin-top: 12px; line-height: 1.7; max-height: 0; overflow: hidden; transition: max-height 0.3s; }
.faq-item.open .faq-answer { max-height: 500px; }
.faq-toggle { color: var(--green-primary); font-size: 22px; font-weight: 700; }

/* Testimonials */
.testimonial-card { padding: 32px 28px; }
.testimonial-text { font-style: italic; font-size: 16px; line-height: 1.8; color: var(--text-secondary); margin-bottom: 20px; }
.testimonial-text::before { content: '\201C'; display: block; font-family: Georgia, serif; font-size: 48px; color: var(--green-primary); line-height: 0.6; margin-bottom: 12px; }
.testimonial-author { display: flex; align-items: center; gap: 12px; }
.testimonial-avatar { width: 44px; height: 44px; border-radius: 50%; background: var(--green-light); color: var(--green-primary); display: inline-flex; align-items: center; justify-content: center; font-weight: 700; font-size: 16px; }
.testimonial-name { color: var(--text-main); font-weight: 600; font-size: 15px; }
.testimonial-role { color: var(--text-muted); font-size: 13px; }

/* CTA banner — green band */
.cta-banner { background: var(--green-primary); color: var(--text-on-dark); padding: var(--space-2xl) 0; position: relative; overflow: hidden; }
.cta-banner-overlay { display: none; }  /* old dark overlay removed */
.cta-banner-content { display: flex; align-items: center; justify-content: space-between; gap: 32px; flex-wrap: wrap; max-width: 1200px; margin: 0 auto; padding: 0 24px; }
.cta-banner-text h2 { font-size: clamp(24px, 3vw, 36px); font-weight: 700; line-height: 1.2; color: var(--text-on-dark); }
.cta-banner-text p { font-size: 16px; color: var(--text-muted-dark); margin-top: 8px; }
.cta-banner-btns { display: flex; gap: 12px; flex-wrap: wrap; }
.cta-banner .btn-primary { background: var(--white-pure); color: var(--green-primary); }
.cta-banner .btn-primary:hover { background: var(--white-soft); transform: translateY(-2px); }
.cta-banner .btn-outline { color: var(--text-on-dark); border-color: rgba(255,255,255,0.4); }
.cta-banner .btn-outline:hover { background: rgba(255,255,255,0.1); border-color: var(--text-on-dark); color: var(--text-on-dark); }
```

- [ ] **Step 3: Verify in browser:**
  - All sections now have white or white-soft backgrounds
  - Cards (why-choose, steps, success, testimonials) are white with soft shadow, hover-lift
  - Stats bar shows green numbers on white-soft band
  - CTA banner is solid green with white text and white CTA buttons
  - Testimonials show green quote mark and avatar circle with initial letter

- [ ] **Step 4: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: convert all content sections to light theme overrides"
```

---

### Task 1.8: Replace footer HTML + CSS

**Files:**
- Modify: `Najifa Clone/index.html`

- [ ] **Step 1: Locate the existing `<footer>...</footer>` block.** It comes after the last `<section>`.

- [ ] **Step 2: Replace the entire `<footer>` block with R7** (Canonical Footer HTML).

- [ ] **Step 3: Find the existing footer CSS** (selectors `.site-footer`, `.footer-grid`, `.footer-col`, `.footer-bottom`, `.footer-social`, etc.).

- [ ] **Step 4: Delete all old footer CSS and replace with R8** (Canonical Footer CSS).

- [ ] **Step 5: Verify in browser:**
  - Footer is dark `#0B120F` with 4px green-gradient accent line at top
  - "Your Logo" with "Logo" in white (dark-context override)
  - 4-column grid at desktop, 2-col at tablet, 1-col at mobile
  - Social icons are dark circles, hover → green
  - Footer bottom shows "© 2026 Your Company Name Here. All rights reserved." (year auto-updates via R9 script)

- [ ] **Step 6: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: replace footer with dark inverted layout + Your Logo + green accent bar"
```

---

### Task 1.9: Add interactive JS (mobile nav, scroll-fade, footer year, sticky-header shadow)

**Files:**
- Modify: `Najifa Clone/index.html`

- [ ] **Step 1: Find the existing `<script>` block(s)** at the end of `<body>`. Note any custom logic (counters, marquee, FAQ toggle) — these need to be preserved.

- [ ] **Step 2: Add R9 script** (Mobile nav + IntersectionObserver + footer year + sticky header) immediately before `</body>`. **Keep existing custom scripts** (counter animation, marquee, FAQ toggle) in place above this — R9 is additive.

- [ ] **Step 3: For section header animations to work, add `data-fade` attribute to each section's heading wrapper.** For example:

```html
<!-- BEFORE -->
<div class="section-header">
  <span class="section-tag">Why Choose</span>
  <h2 class="section-title">...</h2>
  <p class="section-sub">...</p>
</div>

<!-- AFTER -->
<div class="section-header" data-fade>
  ...
</div>
```

Apply `data-fade` to: hero-content (already added in Task 1.6), and each section's `.section-header` div (or equivalent wrapper). Do NOT add `data-fade` to grid/card containers.

- [ ] **Step 4: Verify in browser:**
  - Hamburger toggle works at mobile width
  - Scroll-fade triggers as section headers enter viewport (subtle, not distracting)
  - Footer copyright shows current year (2026)
  - Header gains subtle shadow once scrolled past 8px

- [ ] **Step 5: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: add mobile nav toggle, scroll-fade observer, year auto-update, sticky shadow"
```

---

### Task 1.10: Apply Placeholder Dictionary (replace all branding & personal info)

**Files:**
- Modify: `Najifa Clone/index.html`

- [ ] **Step 1: Replace `<title>`** at top of file with:

```html
<title>Your Company Name — Premium Business Solution</title>
```

- [ ] **Step 2: Replace `<meta name="description">`:**

```html
<meta name="description" content="Your business description goes here. Replace with your company's value proposition."/>
```

- [ ] **Step 3: Apply each row of the R11 placeholder dictionary** to the file. Use editor's find-and-replace. Specific replacements to make in `index.html`:
  - `Najifa International` → `Your Company Name Here` (5+ occurrences in body, footer)
  - `Najifa Int'l` → remove (already replaced by R5 navbar logo markup)
  - Any standalone `Najifa` or `najifa` → `Your Company`
  - `contact@najifa.com` → `your@email.com`
  - `Banani, Dhaka` → `Your Address Here, City, Country`
  - `+8801XXXXXXXX` and any phone → `+880 1XXX-XXXXXX`
  - `© 2012` → `©` (year auto-injected by R9 script via `<span id="footYear">2026</span>` already in R7 footer)
  - All testimonial text bodies → R11 generic quote
  - All testimonial author names → `Your Client Name`
  - Specific employer names (e.g., `Nahdi Pharmacy`) → `International Employer`
  - Specific stats `12+ years` / `25+ countries` → keep numbers but generic context (`10+ Years Experience`)

- [ ] **Step 4: Verify** — Ctrl+F in editor for case-insensitive `najifa` — must return zero matches.

- [ ] **Step 5: Verify in browser** — page reads as a plausible generic recruitment company website with no Najifa-specific references.

- [ ] **Step 6: Commit**

```bash
git add "Najifa Clone/index.html"
git commit -m "index.html: apply placeholder dictionary (Najifa → Your Company; remove personal data)"
```

---

### Task 1.11: Responsive verification + push (Vercel deploys)

**Files:** none (verification only)

- [ ] **Step 1: Open `index.html` in browser** — desktop width (≥ 1280px). Run through R12 acceptance criteria checklist. Fix any deviations inline (small CSS tweaks); commit fixes as separate commits with message `index.html: fix <specific issue>`.

- [ ] **Step 2: Test at 1024px width** — navbar should still be full (not hamburger yet at exactly 1024). Grid layouts should fit gracefully.

- [ ] **Step 3: Test at 768px width** — section padding should drop to 64px, grids start to collapse.

- [ ] **Step 4: Test at 375px width (iPhone SE)** — hamburger menu visible, hero stats stack 1-column, all CTAs are full-width-ish, no horizontal overflow (drag horizontally — should not move).

- [ ] **Step 5: Push to GitHub** — Vercel auto-deploys.

```bash
$token = "<your-PAT>"
$auth = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes("x-access-token:$token"))
git -c http.extraHeader="Authorization: Basic $auth" push origin main
```

- [ ] **Step 6: Wait ~30s for Vercel deploy.** Open the live preview URL on a phone OR use Chrome DevTools device emulator. Re-run R12 checklist on the live URL.

---

### 🛑 PHASE 1 APPROVAL GATE — Show user `index.html` for review

**Stop and wait for user feedback** on the live `index.html` before proceeding to Phase 2. If user requests changes:
- Make changes only to `index.html` (do not touch other pages yet)
- Re-run Task 1.11 verification
- Get re-approval

Only after explicit approval, proceed to Phase 2.

---

## Phase 2 — Propagate to Remaining 13 Pages

> **Engineer note:** Each page in Phase 2 follows the **same recipe**. The recipe is defined once below; each task is a one-line dispatch with page-specific notes.

### Phase 2 Recipe (apply to every page)

For a target file `Najifa Clone/<page>.html`:

- [ ] **R-Step 1: Replace fonts link** with R1 (one-line swap).

- [ ] **R-Step 2: Replace `:root` block** with R2 (legacy aliases included).

- [ ] **R-Step 3: Replace reset + body block** with R3.

- [ ] **R-Step 4: Replace utility classes** (`.container`, `.section-tag`, `.section-title`, `.section-sub`, `.btn-primary`, `.btn-outline`, `.card`, form input styles, section padding) with R4. Keep page-specific section CSS intact for now.

- [ ] **R-Step 5: Replace `<header>` block** with R5; replace navbar CSS with R6.

- [ ] **R-Step 6: Append the light-theme content overrides** from Task 1.7 Step 2 (the override block) at the **end of the page's `<style>` block**. This single override block covers most card/button/section conversions.

- [ ] **R-Step 7: Replace `<footer>` block** with R7; replace footer CSS with R8.

- [ ] **R-Step 8: Add R9 script block** before `</body>` (preserve any page-specific scripts above).

- [ ] **R-Step 9: Apply R11 placeholder dictionary** — find all "Najifa" / personal info / specific employer names and replace.

- [ ] **R-Step 10: Page-specific polish** (see per-task notes below) — e.g., update `<title>`, address page-unique sections.

  **Sub-pages also typically have a top page-banner with `<h1>` page title.** If present, ensure:
  - Background: `var(--white-soft)`
  - Heading color: `var(--text-main)` with optional green `<span>` accent
  - Padding: `padding: 80px 0 60px;` (smaller than full hero)
  - Optional breadcrumbs: `Home > Page Name` in `--text-muted` 14px
  - Apply this CSS at the end of the page's `<style>` block:
  ```css
  .page-banner, .page-header, .page-hero, [class*="-banner"]:not(.cta-banner) {
    background: var(--white-soft);
    padding: 80px 0 60px;
    text-align: center;
  }
  .page-banner h1, .page-header h1, .page-hero h1 {
    font-family: 'Open Sans', sans-serif;
    font-weight: 800;
    font-size: clamp(32px, 5vw, 48px);
    color: var(--text-main);
    line-height: 1.1;
  }
  .page-banner h1 span, .page-header h1 span, .page-hero h1 span { color: var(--green-primary); }
  ```
  - Adjust selector names if the page uses different classes for its top banner.

- [ ] **R-Step 11: Run R12 acceptance criteria** in browser. Fix issues.

- [ ] **R-Step 12: Commit + push.**

```bash
git add "Najifa Clone/<page>.html"
git commit -m "<page>.html: convert to light theme template + apply placeholders"
git -c http.extraHeader="Authorization: Basic $auth" push origin main
```

---

### Task 2.1: Transform `about.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — About Us`
- This page has team/history sections; ensure all member photos/cards become generic
- Specific company history dates (founding year, milestones) → keep generic ("Established YYYY")

- [ ] Run Phase 2 Recipe steps R-Step 1 through R-Step 12 on `Najifa Clone/about.html`.

---

### Task 2.2: Transform `ceo-message.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — CEO Message`
- CEO name → `Your Name`; CEO designation → `Your Designation` (e.g., "CEO & Founder")
- CEO photo placeholder (if dark photo div exists) → keep a clean placeholder with initial letter on green-light circle, OR remove entirely
- Long message body → keep paragraph structure, replace specific company history references with generic phrasing

- [ ] Run Phase 2 Recipe on `Najifa Clone/ceo-message.html`.

---

### Task 2.3: Transform `management.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Management Team`
- Each team member: name → `Your Name`, role → `Your Designation`, bio → 2-line generic placeholder
- Use icon-style avatars (initial letter on green-light circle) — no real photos

- [ ] Run Phase 2 Recipe on `Najifa Clone/management.html`.

---

### Task 2.4: Transform `mission-vision.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Mission & Vision`
- Two main blocks: Mission, Vision — keep narrative concept, replace specific company-history-tied phrasing with generic "premium service / global reach / customer focus"

- [ ] Run Phase 2 Recipe on `Najifa Clone/mission-vision.html`.

---

### Task 2.5: Transform `for-clients.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — For Clients`
- Service tiers / process / pricing-style cards → keep structure, replace specific deliverable claims with generic ones
- Any CTA forms → static `action="#"`

- [ ] Run Phase 2 Recipe on `Najifa Clone/for-clients.html`.

---

### Task 2.6: Transform `for-applicants.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — For Applicants`
- Application steps / requirements list → keep structure, replace specific country requirements with generic "destination requirements"
- Specific destination countries → keep regional grouping (Middle East, Europe, etc.) without country-specific imagery

- [ ] Run Phase 2 Recipe on `Najifa Clone/for-applicants.html`.

---

### Task 2.7: Transform `our-success.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Success Stories`
- Each success story / case-study card: name → `Your Client Name`, story → R11 generic testimonial
- Numbers / metrics → round to generic stats

- [ ] Run Phase 2 Recipe on `Najifa Clone/our-success.html`.

---

### Task 2.8: Transform `clients.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Our Clients`
- Client logo grid → since template has no real images, replace each logo cell with a text-based "Client Name" badge using `.client-item` style (same as homepage marquee items)

- [ ] Run Phase 2 Recipe on `Najifa Clone/clients.html`.

---

### Task 2.9: Transform `job-categories.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Job Categories`
- Category cards (Construction, Hospitality, Healthcare, Manufacturing, Hospitality, etc.) → keep, but ensure no specific employer names or country flags inside; use generic SVG icons (already there)
- Each category description → generic 2-line placeholder

- [ ] Run Phase 2 Recipe on `Najifa Clone/job-categories.html`.

---

### Task 2.10: Transform `gallery.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Gallery`
- This page is photo-heavy in original — but since template is icon-only, **replace each gallery image cell** with a placeholder block:

```html
<div class="gallery-placeholder">
  <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <rect x="3" y="3" width="18" height="18" rx="2"/>
    <circle cx="8.5" cy="8.5" r="1.5"/>
    <polyline points="21 15 16 10 5 21"/>
  </svg>
  <span>Replace with your image</span>
</div>
```

```css
.gallery-placeholder {
  aspect-ratio: 4 / 3;
  background: var(--white-soft);
  border: 1.5px dashed var(--border-light);
  border-radius: var(--radius);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  color: var(--text-muted);
  font-size: 13px;
  font-weight: 500;
}
.gallery-placeholder svg { color: var(--green-primary); opacity: 0.5; }
```

- [ ] Run Phase 2 Recipe on `Najifa Clone/gallery.html`.

---

### Task 2.11: Transform `licences.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Certifications`
- Each license/certificate card → keep structure; replace specific license number with `License No. XXX-XXXX`; replace issuing authority specifics with `Authority Name`
- If certificate images are referenced → replace with text-based card layout (no photos)

- [ ] Run Phase 2 Recipe on `Najifa Clone/licences.html`.

---

### Task 2.12: Transform `contact.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Contact Us`
- Contact form → keep markup but `action="#"`. Add HTML comment above form: `<!-- replace action with your endpoint -->`
- Office address blocks → `Your Address Here, City, Country` for each
- **PRESERVE** the Google Maps `<iframe>` (it's a generic Banani/Dhaka pin already — engineer can swap coords later)
- Phone, email, working hours blocks → placeholder values

- [ ] Run Phase 2 Recipe on `Najifa Clone/contact.html`.

---

### Task 2.13: Transform `apply.html`

**Page-specific notes:**
- `<title>` → `Your Company Name — Apply Now`
- Application form → keep multi-step structure but `action="#"`. Add HTML comment: `<!-- replace action with your endpoint -->`
- All form labels (name, email, phone, address, experience, preferred destination) → keep generic labels, no example values
- **PRESERVE** the Google Maps iframe at end (same as contact.html)

- [ ] Run Phase 2 Recipe on `Najifa Clone/apply.html`.

---

## Phase 3 — Final QA Pass

### Task 3.1: Color token usage audit (remove orphan hex values)

**Files:** all 14 HTML files

- [ ] **Step 1: Search the project for old hex values** that should no longer appear:

```bash
cd "c:/Databrandix HQ/Najifa Clone"
grep -rEn "(#1d9e75|#157a5a|#e8f7f2|#0b1a14|#112318|#1a3028|#e8f0ec|#8aab9a)" "Najifa Clone/"
```

- [ ] **Step 2: For each match found**, replace with the corresponding new token from R2:
  - `#1d9e75` → `var(--green-primary)`
  - `#157a5a` → `var(--green-dark)`
  - `#e8f7f2` → `var(--green-light)`
  - `#0b1a14` → `var(--white-pure)` (was old dark bg) OR `var(--black-main)` (if intentional dark band)
  - `#112318`, `#1a3028` → `var(--white-soft)` or `var(--black-accent)` per context
  - `#e8f0ec` → `var(--text-main)` or `var(--text-on-dark)` per context
  - `#8aab9a` → `var(--text-muted)`

- [ ] **Step 3: Re-run grep** — should return zero matches (or only intentional ones in inline `style=` attributes that need explicit hex).

- [ ] **Step 4: Now check for legacy aliases that are still being used** — these can be removed from R2 once all consumers use the new names:

```bash
grep -rEn "var\(--green\)|var\(--dark\)|var\(--text\)|var\(--white\)|var\(--card-bg\)|var\(--card-border\)|var\(--shadow\)" "Najifa Clone/"
```

For each occurrence, replace with the canonical new token name (e.g., `var(--green)` → `var(--green-primary)`).

- [ ] **Step 5: After all consumers updated, remove the LEGACY ALIASES section from R2 in every file's `:root` block.**

- [ ] **Step 6: Verify all 14 pages still render correctly** in browser.

- [ ] **Step 7: Commit**

```bash
git add "Najifa Clone/"
git commit -m "audit: replace orphan hex values + remove legacy CSS variable aliases"
```

---

### Task 3.2: Cross-page navbar/footer consistency check

**Files:** all 14 HTML files

- [ ] **Step 1: Compare navbar markup across files.** They should all use the R5 markup verbatim. Run:

```bash
grep -A 2 'class="logo"' "Najifa Clone/"*.html | head -100
```

All files should show identical `<span class="logo-mark">Your</span><span class="logo-text">Logo</span>` (with footer-context override for color).

- [ ] **Step 2: Compare footer markup across files.** All footers should match R7. Visual spot-check at least 4 pages: home, about, contact, apply.

- [ ] **Step 3: If any drift found**, copy the canonical block from `index.html` and replace in the drifting page.

- [ ] **Step 4: Commit** any fixes.

```bash
git add "Najifa Clone/"
git commit -m "audit: enforce navbar/footer consistency across all pages"
```

---

### Task 3.3: Responsive verification across all pages

**Files:** none (verification only)

- [ ] **Step 1: Open each of the 14 pages on desktop (1280px+).** Walk through R12 checklist for each. Note any defects.

- [ ] **Step 2: Open each page in Chrome DevTools device emulator at 768px (tablet).** Verify navbar is hamburger, grids are 2-col, no overflow.

- [ ] **Step 3: Open each page at 375px (iPhone SE).** Verify hamburger works, 1-col grids, no overflow, all text legible.

- [ ] **Step 4: Fix any responsive defects found.** Commit each fix separately with descriptive message.

- [ ] **Step 5: Push final state to GitHub.** Verify Vercel deploy succeeds.

---

### Task 3.4: Dead code removal

**Files:** all 14 HTML files

- [ ] **Step 1: For each page, search for clearly-dead CSS rules** — selectors that no longer have matching HTML elements after the conversion. Common candidates:
  - `.hero-photo`, `.hero-photo-placeholder`, `.hero-bg`, `.hero-grid`, `.hero-left`, `.hero-right` (replaced by centered hero in Task 1.6)
  - `.cta-banner-overlay` (we set `display: none` but the rule itself can be removed)
  - Any `--green-glow`-only-using selectors
  - Old dark-mode-specific gradient or shadow rules

- [ ] **Step 2: Remove identified dead rules.** Be cautious: if a selector still has matching HTML but the rule was replaced by an override, leave it (the override still wins).

- [ ] **Step 3: Verify each modified page still renders correctly.**

- [ ] **Step 4: Commit**

```bash
git add "Najifa Clone/"
git commit -m "cleanup: remove dead CSS rules from old dark theme"
```

---

### Task 3.5: Final commit + push + tag

**Files:** none (git operations only)

- [ ] **Step 1: Verify git status is clean.**

```bash
cd "c:/Databrandix HQ/Najifa Clone"
git status
```

- [ ] **Step 2: Push final state.**

```bash
$token = "<your-PAT>"
$auth = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes("x-access-token:$token"))
git -c http.extraHeader="Authorization: Basic $auth" push origin main
```

- [ ] **Step 3: Create a release tag.**

```bash
git tag -a v1.0.0 -m "v1.0.0 — Premium recruitment template (light theme, Open Sans, generic branding)"
git -c http.extraHeader="Authorization: Basic $auth" push origin v1.0.0
```

- [ ] **Step 4: Verify Vercel final deploy succeeds.** Open the production URL — all 14 pages should be browsable, consistent, responsive.

- [ ] **Step 5: ⚠️ Revoke the GitHub Personal Access Token** at https://github.com/settings/tokens — the token has been used in transcript and shouldn't be retained.

---

## Definition of Done

- [ ] All 14 HTML files use Open Sans + new green/black/white light-theme tokens
- [ ] Every "Najifa" / personal info replaced with R11 placeholder dictionary entries (zero `najifa` matches in grep)
- [ ] Every page passes R12 visual consistency check (navbar, footer, button styles, card styles, section rhythm)
- [ ] Responsive at 4 breakpoints (375 / 768 / 1024 / 1280)
- [ ] No dead CSS rules from old dark theme
- [ ] No orphan hex values from old palette
- [ ] Template feels like a premium, ThemeForest-quality recruitment-industry website ready for any company to drop in their content
- [ ] Vercel production deploy is green
- [ ] v1.0.0 tag created and pushed

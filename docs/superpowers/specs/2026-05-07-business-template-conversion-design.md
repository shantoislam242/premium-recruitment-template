# Business Website Template Conversion — Design Spec

**Date:** 2026-05-07
**Project:** Najifa Clone → Generic Premium Recruitment Template
**Working dir:** `c:\Databrandix HQ\Najifa Clone\Najifa Clone\` (14 HTML files, inline CSS)
**Status:** Design approved by user, ready for implementation plan

---

## 1. Goal

Convert the existing 14-page Najifa International recruitment website into a reusable **premium recruitment-industry website template** with:
- Generic placeholder branding (any company can drop in their content)
- Unified Open Sans typography
- New green + black + white light-theme color palette
- Premium minimal SaaS-style visual language (Linear / Stripe / Vercel feel)
- Icon-driven UI (no photographic images)
- Cross-page consistency (navbar, footer, components, spacing rhythm)
- ThemeForest-quality polish, fully responsive

## 2. Foundational Decisions (User-Confirmed)

| # | Decision | Choice | Rationale |
|---|---|---|---|
| 1 | Theme direction | **Light primary** | Matches premium SaaS / corporate template feel |
| 2 | Image strategy | **Icon-driven only, no photos** | Zero external dependencies, modern minimal aesthetic |
| 3 | Content strategy | **Keep recruitment flavor, rebrand only** | Template = "Premium Recruitment Agency Template" with generic branding |
| 4 | File structure | **Keep as-is** | All 14 HTML files in single folder, CSS stays inline per file |
| 5 | Typography | **Open Sans only** | Replaces Syne + DM Sans |

## 3. Color Token System

```css
:root {
  /* === BRAND GREENS === */
  --green-primary: #155E45;     /* primary CTA, links, accents */
  --green-dark:    #0D402F;     /* hover state, deep accents */
  --green-light:   #E8F5F1;     /* tinted backgrounds, badge bg */

  /* === SURFACES === */
  --white-pure:    #FFFFFF;     /* primary page background */
  --white-soft:    #F8FAF9;     /* alternating sections, card bg */
  --black-main:    #0B120F;     /* dark CTA / footer background */
  --black-accent:  #1E2924;     /* darker variants */

  /* === TEXT === */
  --text-main:       #0B120F;   /* headings on light surfaces */
  --text-secondary:  #4A5852;   /* body text on light surfaces */
  --text-muted:      #8A9B94;   /* small / meta text */
  --text-on-dark:    #FFFFFF;   /* text on dark/green surfaces */
  --text-muted-dark: #B8C5C0;   /* muted text on dark surfaces */

  /* === BORDERS === */
  --border-light:  rgba(21, 94, 69, 0.15);
  --border-subtle: rgba(11, 18, 15, 0.08);

  /* === ELEVATION === */
  --shadow-soft:   0 4px 20px rgba(11, 18, 15, 0.06);
  --shadow-md:     0 8px 32px rgba(11, 18, 15, 0.08);
  --shadow-lg:     0 16px 48px rgba(21, 94, 69, 0.12);

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
}
```

**Surface usage rules:**
- Default page background: `--white-pure`
- Alternating sections: `--white-soft`
- Cards: `--white-pure` on `--white-soft` sections (with `--shadow-soft` + `--border-subtle`)
- Hero / CTA / Footer (inverted): `--black-main` or `--green-primary` background, `--text-on-dark` text

## 4. Typography System

**Font import (replaces existing Syne + DM Sans link in every file):**
```html
<link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
```

**Family declaration:**
```css
font-family: 'Open Sans', system-ui, -apple-system, sans-serif;
```

**Type scale:**

| Element | Desktop | Mobile | Weight | Line-height | Letter-spacing |
|---|---|---|---|---|---|
| `h1` / hero title | 48px | 32px | 800 | 1.1 | -0.02em |
| `h2` / section title | 40px | 28px | 700 | 1.15 | -0.015em |
| `h3` / card title | 32px | 24px | 700 | 1.2 | -0.01em |
| `h4` / sub-card | 24px | 20px | 600 | 1.3 | normal |
| `h5` | 20px | 18px | 600 | 1.4 | normal |
| `p` / body | 16–18px | 15–16px | 400 | 1.7 | normal |
| `.section-tag` (eyebrow) | 12px | 11px | 600 | 1.4 | 2.5px (UPPERCASE) |
| `.btn` text | 15px | 14px | 600 | 1 | 0.2px |
| `small` / meta | 14px | 13px | 400 | 1.5 | normal |

**Implemented utility classes:**
```css
.section-title {
  font-size: clamp(28px, 4vw, 40px);
  font-weight: 700;
  line-height: 1.15;
  letter-spacing: -0.015em;
  color: var(--text-main);
}
.section-title span { color: var(--green-primary); }

.section-sub {
  font-size: clamp(15px, 1.4vw, 18px);
  font-weight: 400;
  line-height: 1.7;
  color: var(--text-secondary);
}

.section-tag {
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 2.5px;
  text-transform: uppercase;
  color: var(--green-primary);
  background: var(--green-light);
  padding: 6px 14px;
  border-radius: 30px;
}
```

## 5. Component Design Language

### Buttons
```css
.btn-primary {
  background: var(--green-primary);
  color: var(--text-on-dark);
  font-weight: 600; font-size: 15px;
  padding: 14px 28px;
  border-radius: var(--radius-sm);
  transition: var(--transition);
}
.btn-primary:hover {
  background: var(--green-dark);
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(21, 94, 69, 0.25);
}

.btn-outline {
  background: transparent;
  color: var(--text-main);
  border: 1.5px solid var(--border-light);
  font-weight: 600;
}
.btn-outline:hover {
  border-color: var(--green-primary);
  color: var(--green-primary);
  background: var(--green-light);
}

.btn-dark { background: var(--black-main); color: var(--text-on-dark); }
```

### Cards
```css
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
```

### Navbar (Sticky, Light)
- Background: `--white-pure` with `backdrop-filter: blur(12px)` on scroll, `rgba(255,255,255,0.85)`
- Border-bottom: `1px solid var(--border-subtle)`
- Logo: text-based — `<span class="logo-mark">Your</span><span class="logo-text">Logo</span>`, Open Sans 800, 22px
- Nav links: `--text-main`, hover → `--green-primary` with subtle underline animation
- Mobile (`< 1024px`): hamburger toggle → full-screen white overlay menu

### Footer (Dark Inverted)
- Background: `--black-main`
- Headings: `--text-on-dark`; body/links: `--text-muted-dark`
- Top accent: 4px gradient bar `--green-primary` → `--green-dark`
- Logo override in dark context: `.logo-text { color: var(--text-on-dark); }`
- Social icons: hover → `--green-primary`

### Forms / Inputs
```css
.form-input {
  background: var(--white-soft);
  border: 1.5px solid var(--border-subtle);
  border-radius: var(--radius-sm);
  padding: 14px 18px;
  font-size: 15px;
  color: var(--text-main);
  transition: var(--transition);
}
.form-input:focus {
  border-color: var(--green-primary);
  background: var(--white-pure);
  box-shadow: 0 0 0 4px rgba(21, 94, 69, 0.08);
}
.form-input::placeholder { color: var(--text-muted); }
```

### Hero Section (light, consistent across all pages)
- Background: `--white-soft` + decorative SVG green geometric shapes (circles, dotted grids, soft blurred gradient blobs) at corners
- Headings: `--text-main` with optional `<span>` accent in `--green-primary`
- Same light treatment on every page for consistency (no per-page dark hero variant)

### Testimonial Card
- White card, soft shadow
- Green SVG quote-mark top-left
- Italic body
- Author: green-light circle with initial letter, name (semibold), designation (muted)

### CTA Section
- Full-width band: `--green-primary` OR `--black-main` background
- Large heading + 1 button, white text

### FAQ
- Light cards with green plus/minus toggle icon, smooth height transition

### Logo Markup (Standardized)
```html
<a href="index.html" class="logo">
  <span class="logo-mark">Your</span><span class="logo-text">Logo</span>
</a>
```
```css
.logo { font-family: 'Open Sans'; font-weight: 800; font-size: 22px; letter-spacing: -0.02em; }
.logo-mark { color: var(--green-primary); }
.logo-text { color: var(--text-main); }
```

## 6. Layout & Spacing

### Container
```css
.container { max-width: 1200px; margin: 0 auto; padding: 0 24px; }
```

### Section Padding (consistent across all 14 pages)
- Desktop: `padding: var(--space-2xl) 0;` (96px)
- Mobile: `padding: 64px 0;`
- Hero sections: `padding: var(--space-3xl) 0;` (128px) on desktop

### Grid System
```css
.grid-cards { display: grid; gap: 24px; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); }
.grid-2 { grid-template-columns: repeat(2, 1fr); }
.grid-3 { grid-template-columns: repeat(3, 1fr); }
.grid-4 { grid-template-columns: repeat(4, 1fr); }
```

### Section Rhythm
- Alternate `--white-pure` ↔ `--white-soft` between consecutive sections
- Each section opens with: `.section-tag` → `.section-title` → `.section-sub` (max-width: 680px when centered)
- 48px gap between header block and content grid

### Responsive Breakpoints
- `< 640px` — mobile
- `640–768px` — small tablet
- `768–1024px` — tablet (navbar still hamburger)
- `1024–1280px` — laptop (full navbar)
- `≥ 1280px` — desktop

### Responsive Adjustments
- Navbar collapses to hamburger at `< 1024px`
- Grids: 4 → 2 → 1, 3 → 2 → 1, 2 → 1
- Section padding 96px → 64px at `< 768px`
- All `<img>` / `<iframe>`: `max-width: 100%; height: auto;` (already global)

## 7. Animation Philosophy

**Standard transition (in tokens):**
```css
--transition: all 0.32s cubic-bezier(0.4, 0, 0.2, 1);
```

**Reusable animations:**
```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}
.animate-fade-up { animation: fadeUp 0.6s ease-out both; }
```

**Hover effects** (apply only to interactive elements: cards, buttons, links, icon tiles):
- `translateY(-2px to -4px)`
- Box-shadow elevation increase
- Color/background transition

**Rules:**
- Only on interactive elements, NOT plain text
- All transitions ≤ 350ms
- No autoplay carousels
- `@media (prefers-reduced-motion: reduce)` → disable all transforms/animations
- Scroll-triggered fade-up applied to the **section header block** (`.section-tag` + `.section-title` + `.section-sub` together) via vanilla `IntersectionObserver` (~15 lines JS). Body content (cards, grids) does NOT animate separately to avoid motion overload.

## 8. Branding & Placeholder Dictionary

### Brand Identity Replacements

| Current (Najifa) | Replace With |
|---|---|
| "Najifa International" | **Your Company Name Here** |
| "Najifa" (standalone) | **Your Company** |
| Tagline ("Work Abroad. No Hassle...") | **Your Tagline Here — A Premium Business Solution** |
| Logo | **Your Logo** (text-based, see §5) |
| Favicon | Generic green "Y" SVG circle |
| `<title>` | "Your Company Name — Tagline Here" |
| `<meta name="description">` | "Your business description goes here. Replace with your company's value proposition." |

### Personal/Contact Info Replacements

| Pattern | Replace With |
|---|---|
| Person names (CEO, manager, applicant) | **Your Name** |
| Designation | **Your Designation** (e.g., "CEO & Founder") |
| Emails | **your@email.com** |
| Phone | **+880 1XXX-XXXXXX** |
| Address | **Your Address Here, City, Country** |
| Social handles | `href="#"` with generic icons |
| Testimonial quotes | Generic 2–3 line placeholder |
| Testimonial author | **Your Client Name** / **Your Customer** |
| Specific years/dates | **Established YYYY** |

### Stat / Number Placeholders
- Specific recruitment metrics → rounded generic numbers
- "5000+ workers placed" → **5,000+ Clients Served**
- "20+ countries" → **20+ Countries Reached**

### Page Content Strategy (Decision B)

**Keep recruitment-themed structures, just rebrand:**
- Job category cards (Construction, Hospitality, Healthcare, etc.) — keep
- Application step flow — keep
- License/credential cards — keep structure, replace specific license numbers with `License No. XXX-XXXX`
- CEO message / Mission-Vision narrative — keep, replace person + dates
- Country destinations — keep as regional categories (Middle East, Europe, etc.); no specific country flags as logos
- Google Maps embed (apply.html, contact.html) — leave coordinates as generic Banani/Dhaka pin

### Form Behavior
- All forms: `action="#"` `method="post"` (no real backend)
- **Fully static** — no JS submit feedback (cleaner template; user wires their own backend)
- HTML comment near form: `<!-- replace action with your endpoint -->`

## 9. Implementation Approach

### Phase 1: Reference Page (`index.html`) — Gold Standard
1. Replace fonts link (Syne + DM Sans → Open Sans)
2. Replace `:root` block with new token set (§3)
3. Apply new typography classes globally (§4)
4. Rebuild navbar + footer with light theme + new logo (§5)
5. Convert each section in order: hero → features → about → CTA → testimonials → footer
6. Replace all "Najifa" / personal info using placeholder dictionary (§8)
7. Add scroll-fade-in `IntersectionObserver` script (vanilla, ~15 lines)
8. Test responsive at 1280 / 1024 / 768 / 375 widths
9. **🛑 PAUSE — Show user `index.html` for approval before proceeding**

### Phase 2: Propagate to Remaining 13 Pages

**Group A — About cluster:** `about.html`, `ceo-message.html`, `management.html`, `mission-vision.html`
**Group B — Service/audience pages:** `for-clients.html`, `for-applicants.html`, `our-success.html`, `clients.html`
**Group C — Listing pages:** `job-categories.html`, `gallery.html`, `licences.html`
**Group D — Form/contact pages:** `contact.html`, `apply.html`

For each page:
1. Swap font link → Open Sans
2. Replace entire `:root` block with new tokens
3. Re-tune shared utility classes (same names, new values)
4. Convert dark surfaces → light surfaces
5. Copy navbar + footer markup verbatim from `index.html`
6. Replace branding/personal placeholders via dictionary
7. Polish section-specific layout per spec
8. Verify responsive

### Phase 3: Final QA Pass
- Cross-page navbar consistency
- Cross-page footer consistency
- Color token usage audit (no orphan hex values from old palette)
- Open every page in browser at desktop + mobile widths
- Remove dead CSS rules from old dark theme
- Final commit (skipped — no git repo)

## 10. Risk Mitigation

- **Inline CSS duplication across 14 files**: same `:root` + reset + utility classes appear in every file. Authoritative copy lives in `index.html`; propagated verbatim. Future edits must touch all 14 (acceptable — user explicitly chose this).
- **Approval gate after `index.html`**: catches design issues before they multiply 14×.
- **No external image dependencies**: zero broken-image risk.
- **No new files created**: pure in-place edits, structure untouched.

## 11. Out of Scope (Explicit YAGNI)

- Build tools, bundlers, CSS preprocessor — stays vanilla HTML/CSS
- External CSS extraction
- JS framework / interactivity beyond fade-in observer
- Adding/removing pages
- Backend / form submission handling
- Renaming files
- Photographic image assets
- Renaming recruitment terminology

## 12. Definition of Done

- All 14 HTML files use Open Sans + new green/black/white light-theme tokens
- Every "Najifa" / personal info replaced with placeholder dictionary entries
- Every page passes visual consistency check (navbar, footer, button styles, card styles, section rhythm)
- Responsive at 4 breakpoints (375 / 768 / 1024 / 1280)
- No dead CSS rules from old dark theme
- Template feels like a premium, ThemeForest-quality recruitment-industry website ready for any company to drop in their content

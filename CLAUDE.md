# CLAUDE.md — Govena Website

This file provides full project context for every Claude Code session. Read it before making any changes.

---

## 1. Project overview and purpose

This repository contains the **Govena website** — a static HTML/CSS/JS site deployed on Netlify at `govena.com.au`. It is the customer-facing frontend for Govena's AI Governance Pack product.

The site handles:
- Marketing and conversion (homepage, pricing, how it works, FAQ)
- The multi-step intake questionnaire customers fill in before purchasing
- A document preview page showing a tailored excerpt before purchase
- Post-purchase success page
- A resource hub with 20+ SEO-targeted articles on AI governance

There is **no build step and no framework** — every page is a standalone `.html` file with all styles inlined in `<style>` blocks. No Sass, no bundler, no package.json.

### Relationship to govena-backend

The website is purely frontend. All API calls (saving answers, creating Stripe checkout sessions, generating previews) are proxied to the Vercel-hosted backend via Netlify's `_redirects` file:

```
/api/*  →  https://govena-backend.vercel.app/api/:splat  (200 proxy)
```

This means from the browser's perspective, all API calls hit `govena.com.au/api/*` — CORS is not an issue. The backend lives at a separate repo: **govena-backend** (deployed on Vercel).

---

## 2. Brand identity

All pages use a consistent design system defined via CSS custom properties at the top of each file.

### Colours

| Variable | Hex | Usage |
|----------|-----|-------|
| `--p` | `#6C5CE7` | Primary purple — CTAs, links, accents, badges |
| `--v` | `#A29BFE` | Lavender — secondary accents, eyebrow text |
| `--b` | `#00B4D8` | Cyan — decorative accent (logo underline, occasional highlights) |
| `--dark` | `#0F1729` | Near-black — primary text, dark backgrounds |
| `--gray` | `#F1F2F6` | Light grey — page background for most pages |
| `--white` | `#ffffff` | Card backgrounds, nav |
| `--muted` | `#666` | Secondary body text |
| `--dim` | `#8B8FA8` | Footer text, meta info |
| `--bdr` | `rgba(0,0,0,0.08)` | Default border colour |

Dark sections (nav header on questionnaire, email headers, preview banner) use a gradient: `linear-gradient(135deg, #1E1E2F 0%, #2D2B55 100%)`.

### Typography

All pages load from Google Fonts:
```
DM Sans — 400, 500, 600, 700, 800 — primary body and UI font
Open Sans — 700 — used for specific headings (italic style)
DM Mono — 400, 500 — code/mono elements
```

Font stack fallback: `'DM Sans', sans-serif`

### Logo

The Govena logo is an inline SVG embedded directly in HTML `<nav>` elements — there is no external logo image file used on the live site. Two SVG variants exist in `govena-branding/` (separate directory, not in this repo):
- `govena_new_darktextfinal.svg` — dark text, for use on light backgrounds (nav, footer)
- `govena_new_whitetext_final.svg` — white text, for use on dark backgrounds

The logo in the nav is rendered as inline SVG so it scales and colours correctly without an extra HTTP request.

### Logo mark (text-only version)

Some pages render the logo as a styled text mark:
```html
<div class="wm">
  <span class="wm-name">Govena</span>
  <div class="wm-line">
    <span></span>  <!-- purple bar -->
    <span></span>  <!-- cyan bar -->
  </div>
</div>
```
Font: 700, 48px, colour `#1E1E2F`, with a purple + cyan decorative underline.

### Favicon and app icons

All favicon files are in the repo root:
- `favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png`
- `apple-touch-icon.png`
- `android-chrome-192x192.png`, `android-chrome-512x512.png`
- `site.webmanifest`

### Design patterns

- **Cards**: white background, `border-radius: 12–16px`, `border: 1px solid rgba(0,0,0,0.08)`, subtle box-shadow on hover
- **CTAs**: `background: #6C5CE7`, white text, `border-radius: 8px`, `font-weight: 600`
- **Section alternation**: white sections (`.sw`) alternate with light grey sections (`.sg`) for visual rhythm
- **Badges/pills**: `background: rgba(162,155,254,0.15)`, `border: 1px solid rgba(162,155,254,0.3)`, `color: #A29BFE`
- **Nav**: fixed, 96px height, frosted glass (`backdrop-filter: blur(16px)`), white background at 97% opacity

---

## 3. File-by-file reference

### Core pages

| File | Title | Purpose |
|------|-------|---------|
| `index.html` | Govena — AI Governance for Australian SMEs | Homepage — hero, value proposition, how it works summary, social proof, pricing teaser, FAQ, footer |
| `pricing.html` | Pricing — Govena | Pricing page — two tiers ($499 pack / $1,199 consult), feature comparison, FAQ |
| `how-it-works.html` | How It Works — Govena | Three-step explainer: questionnaire → generation → delivery |
| `questionnaire.html` | AI Governance Pack — Intake Questionnaire | The multi-step form customers fill in; 5 sections (business basics, AI use, risk/compliance, vendors, governance preferences); dark-themed; submits to `/api/save-answers` then redirects to Stripe |
| `preview.html` | Your Pack Preview — Govena | Shows a generated excerpt of the Acceptable Use Policy after `/api/generate-preview` call; has a "Buy now" CTA that calls `/api/create-checkout` with the `previewId` |
| `success.html` | Payment Successful — Govena | Post-payment confirmation page; reached via Stripe `success_url` |
| `faq.html` | FAQ — Govena | Standalone FAQ page with accordion UI |
| `contact.html` | Contact — Govena | Contact page |
| `before-we-begin.html` | Before We Begin — Govena | Landing page shown before the questionnaire; explains what information is needed |
| `privacy-policy.html` | Privacy Policy — Govena | Privacy policy |
| `terms-of-service.html` | Terms of Service — Govena | Terms of service |

### Resource/article pages

All article pages follow the same template: nav + article header + body content + related articles section + footer.

| File | Topic |
|------|-------|
| `resources.html` | Resource hub — index of all articles with search/filter |
| `govena-resources.html` | Alternative/older resources landing page |
| `article-acceptable-use-policy.html` | What is an AI Acceptable Use Policy? |
| `article-adm-deadline.html` | The December 2026 ADM disclosure deadline |
| `article-adm-transparency.html` | Automated decision-making and transparency |
| `article-ai-culture.html` | Building an AI-positive workplace culture |
| `article-ai-ethics-principles.html` | Australia's AI Ethics Principles explained |
| `article-ai-hallucinations.html` | AI hallucinations and governance implications |
| `article-ai-register.html` | What is an AI Register and why you need one |
| `article-health-ai-governance.html` | AI governance for healthcare businesses |
| `article-incident-response.html` | AI incident response planning |
| `article-law-firm-ai-governance.html` | AI governance for law firms |
| `article-ndb-scheme.html` | The Notifiable Data Breaches scheme |
| `article-offshore-vendors.html` | Offshore AI vendors and APP 8 obligations |
| `article-privacy-5-questions.html` | Five privacy questions before using AI |
| `article-public-vs-enterprise-ai.html` | Public vs enterprise AI — what's the difference? |
| `article-shadow-ai.html` | Shadow AI in the workplace |
| `article-staff-training.html` | AI staff training and governance |
| `article-vendor-assessment.html` | How to assess an AI vendor |
| `article-vendor-data-training.html` | Vendor data use for model training |
| `article-vendor-exits.html` | What happens to your data when you leave an AI vendor |
| `ai-opportunity-report.html` | AI opportunity report (lead magnet / gated content) |
| `sample-preview.html` | Static sample preview — shows what the document preview looks like without calling the API |

### Infrastructure / config files

| File | Purpose |
|------|---------|
| `_redirects` | Netlify redirect rules. The critical line: `/api/*  https://govena-backend.vercel.app/api/:splat  200` — this proxies all API calls to the Vercel backend |
| `site.webmanifest` | PWA manifest for Android home screen icons |
| `og-image.png` | 1200×630 Open Graph image used in social previews |

### Image/icon files (root level)

`favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png`, `apple-touch-icon.png`, `android-chrome-192x192.png`, `android-chrome-512x512.png` — all standard favicon/PWA icon sizes.

---

## 4. The questionnaire in detail

`questionnaire.html` is the most complex page. It is a dark-themed, multi-step form with 5 sections:

1. **Business basics** — business name (text), industry (select dropdown), employee count (radio), business description (textarea)
2. **AI use** — AI tools used (checkbox group, `data-group="aitools"`, with free-text "other"), use cases (checkbox group, `data-group="aiuses"`), client-facing outputs (radio `name="clientai"`), ADM use (radio `name="aiad"`)
3. **Risk and compliance** — personal info types (checkbox group, `data-group="piTypes"`), personal data as AI inputs (radio `name="aidata"`), Privacy Act subject (radio `name="privacyact"`), regulatory frameworks (checkbox group, `data-group="regFrameworks"`, with free-text "other"), existing privacy policy (radio `name="privpolicy"`)
4. **Vendors** — vendor acquisition (checkbox group, `data-group="vendoracq"`), vendor review process (radio `name="vend"`), vendor location (radio `name="vendloc"`)
5. **Governance preferences** — governance owner (radio `name="owner"`), additional context (textarea)

The form is driven by the JavaScript in `questionnaire-integration.js` (maintained in `govena-backend`, copied here). The key function `collectAnswers()` scrapes the DOM and builds the `answers` object. `proceedToPayment()` POSTs to `/api/save-answers` and then uses Stripe.js `redirectToCheckout`.

**DOM conventions in questionnaire.html:**
- Sections are `<div id="s1">`, `<div id="s2">`, etc.
- Radio options use class `.radio-opt` with `.selected` added on click
- Checkbox options use class `.check-opt` with `.selected` added on click
- The visible label text is inside `.radio-text` → first `div` child (or the element itself)
- `data-group` attributes on container elements identify checkbox groups

---

## 5. The preview page in detail

`preview.html` calls `POST /api/generate-preview` on load (loading answers and tier from `sessionStorage` keys `govena_answers` and `govena_tier`). The API returns structured JSON with a `sections` array; the preview page renders this into styled HTML mimicking the document style. The document is truncated mid-sentence to tease the full pack.

A "Buy now" / "Get your full pack" CTA calls `POST /api/create-checkout` passing the `previewId` returned by the preview API, which retrieves the answers from KV and creates a Stripe checkout session.

---

## 6. The `_redirects` file — critical

```
/api/*  https://govena-backend.vercel.app/api/:splat  200
```

This single line is what connects the frontend to the backend. The `200` status code means Netlify proxies the request transparently — to the browser, the API is on the same origin. **Never delete or modify this without updating the Vercel backend CORS settings accordingly.**

---

## 7. SEO and meta conventions

Every public page has:
- `<title>` — format: `Page Name — Govena`
- `<meta name="description">` — 1–2 sentences
- `<meta name="robots" content="index, follow">`
- `<link rel="canonical">` — absolute URL at `govena.com.au`
- Full Open Graph block (`og:type`, `og:title`, `og:description`, `og:url`, `og:image`, `og:image:width`, `og:image:height`, `og:site_name`, `og:locale`)
- Twitter Card block (`twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`)

The homepage and pricing page include `application/ld+json` structured data (WebSite + Product schemas).

Article pages should follow the same meta pattern. When adding new articles, ensure canonical URL points to `https://govena.com.au/{filename}`.

---

## 8. Deployment

**Hosting:** Netlify, custom domain `govena.com.au`.

**Current workflow:** Manual drag-and-drop of the folder into Netlify's deploy interface.

**New workflow (after GitHub setup):** Netlify connected to the `rhyle-s/govena-website` GitHub repository. Every push to `main` triggers an automatic deploy. No build command needed — publish directory is the repo root (`.`).

**Netlify settings to configure:**
- Build command: *(leave blank — no build step)*
- Publish directory: `.` (repo root)
- Branch to deploy: `main`

---

## 9. Conventions and rules

### No build tooling
There is no build step. No npm, no bundler, no Sass. All styles are in `<style>` blocks in each HTML file. Keep it this way — do not introduce a build dependency.

### Inline styles only
All CSS is inside `<style>` tags in the `<head>`. There are no external `.css` files. This avoids an extra HTTP request and keeps each page self-contained.

### Inline SVG for the logo
The logo in nav is embedded as inline SVG. Do not replace with an `<img>` tag — it would lose colour control and add an HTTP request.

### Consistent CSS variable usage
Use the CSS custom properties (`--p`, `--v`, `--b`, `--dark`, etc.) defined at `:root` for all colours. Do not hardcode hex values that correspond to these variables.

### questionnaire-integration.js lives in govena-backend
The JavaScript that drives the questionnaire form (`proceedToPayment`, `collectAnswers`, etc.) is maintained in the `govena-backend` repo as `questionnaire-integration.js` and copied into `questionnaire.html`. When updating the form logic, update `govena-backend/questionnaire-integration.js` first, then copy it across.

### Article pages are SEO content
Articles exist to drive organic search traffic. They should be written in plain English, target specific search queries, and link to the pricing page or questionnaire as a CTA. Do not change the URL structure (filenames) of existing articles — it will break any inbound links.

### _redirects must not change
The `_redirects` file proxies API calls to Vercel. If the Vercel backend URL changes, update it here. Never remove it.

### No JavaScript frameworks
The site uses vanilla JavaScript only — no React, Vue, or jQuery. Keep it that way.

### Mobile responsiveness
Every page includes `@media (max-width: 900px)` breakpoints. When adding new sections or pages, always include mobile styles. Nav collapses to a hamburger menu at 900px on most pages.

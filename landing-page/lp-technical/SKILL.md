---
name: lp-technical
description: >
  Technical build standards for high-performance, accessible, SEO/AIO-ready landing pages.
  Use this skill whenever writing HTML, CSS, or JS for a landing page, marketing page, or
  any conversion-focused web page. Triggers on: writing page markup, setting up meta tags,
  implementing images or fonts, building a responsive layout, adding analytics, optimizing
  page speed, setting up schema markup, or reviewing landing page code for quality. Apply
  these standards to EVERY file you generate — treat them as non-negotiable build rules,
  not optional enhancements. Pair with lp-strategy and lp-conversion skills.
---

# Landing Page Technical Standards

Non-negotiable build rules. Apply to every file generated.

## 1. Document Shell

```html
<!DOCTYPE html>
<html lang="[ISO-639 code, e.g. en]">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="[Max 155 chars. Outcome-focused, not feature list.]">
  
  <!-- OG / Social -->
  <meta property="og:title" content="[Same as H1 or tighter]">
  <meta property="og:description" content="[Same as meta description]">
  <meta property="og:image" content="[Absolute URL, 1200×630px]">
  <meta property="og:type" content="website">
  
  <!-- Canonical -->
  <link rel="canonical" href="[Full page URL]">
  
  <title>[Primary Keyword — Brand Name] (max 60 chars)</title>
  
  <!-- Preconnect for external fonts only -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
</head>
```

## 2. Mobile-First CSS

Write all base styles for mobile (≥320px). Add complexity upward.

```css
/* ── Design Tokens ───────────────────────────── */
:root {
  --color-bg:       #[value];
  --color-surface:  #[value];
  --color-text:     #[value];
  --color-muted:    #[value];
  --color-accent:   #[value];
  --color-accent-h: #[value]; /* hover state */

  --font-display:   '[Display font]', serif;
  --font-body:      '[Body font]', sans-serif;

  --space-xs: 0.5rem;
  --space-sm: 1rem;
  --space-md: 2rem;
  --space-lg: 4rem;
  --space-xl: 8rem;

  --radius-sm: 4px;
  --radius-md: 12px;
  --radius-lg: 24px;

  --transition: 0.2s ease;
}

/* ── Reset ───────────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
img, video { max-width: 100%; display: block; }
a { color: inherit; text-decoration: none; }

/* ── Base ────────────────────────────────────── */
body {
  font-family: var(--font-body);
  color: var(--color-text);
  background: var(--color-bg);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

/* ── Container ───────────────────────────────── */
.container {
  width: 100%;
  max-width: 1200px;
  margin-inline: auto;
  padding-inline: var(--space-sm); /* mobile */
}
@media (min-width: 768px) {
  .container { padding-inline: var(--space-md); }
}
```

## 3. Core Web Vitals Rules

Target scores: LCP <2.5s, CLS <0.1, FID/INP <200ms.

**LCP (Largest Contentful Paint — speed)**
```html
<!-- Hero image: preload + correct sizing -->
<link rel="preload" as="image" href="hero.webp" fetchpriority="high">
<img src="hero.webp" alt="[descriptive]" width="1200" height="630"
     fetchpriority="high" decoding="async">

<!-- ALL other images: lazy load -->
<img src="photo.webp" alt="[descriptive]" width="800" height="500"
     loading="lazy" decoding="async">
```

**CLS (Cumulative Layout Shift — visual stability)**
```html
<!-- Always declare width + height on every img and video -->
<!-- Always reserve space for fonts with font-display: swap -->
<style>
@font-face {
  font-family: 'DisplayFont';
  src: url('font.woff2') format('woff2');
  font-display: swap; /* prevents invisible text flash */
}
</style>
```

**INP (Interactivity)**
- No JavaScript that blocks the main thread on load
- Defer all non-critical scripts: `<script src="analytics.js" defer></script>`
- No layout-triggering CSS animations (avoid `width`, `height`, `top`, `left`; use `transform` and `opacity` only)

## 4. Image Optimization

```
Format priority: AVIF > WebP > JPEG/PNG (fallback)
Hero: WebP, compressed to <150KB
Body images: WebP, compressed to <80KB each
Use <picture> for format fallback:
```

```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="[descriptive alt]" width="800" height="500" loading="lazy">
</picture>
```

## 5. Accessibility (a11y)

75% of organizations report accessibility contributes to revenue. It's also a legal requirement in many jurisdictions.

```html
<!-- Semantic structure (required, not optional) -->
<header role="banner">
  <nav aria-label="Main navigation">...</nav>
</header>
<main id="main-content">
  <section aria-labelledby="hero-heading">
    <h1 id="hero-heading">...</h1>
  </section>
</main>
<footer role="contentinfo">...</footer>

<!-- Skip link (keyboard users) -->
<a href="#main-content" class="skip-link">Skip to main content</a>

<!-- Interactive elements -->
<!-- Min tap target: 44×44px -->
<!-- Color contrast: 4.5:1 normal text, 3:1 large text (WCAG AA) -->
<!-- All icons that convey meaning need aria-label -->
<button aria-label="Open mobile menu">
  <svg aria-hidden="true">...</svg>
</button>
```

```css
/* Skip link */
.skip-link {
  position: absolute; top: -100%; left: var(--space-sm);
  background: var(--color-accent); color: #fff; padding: 0.5rem 1rem;
}
.skip-link:focus { top: var(--space-sm); }

/* Visible focus ring (never remove outline without replacement) */
:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 3px;
}
```

## 6. SEO + AIO (AI Optimization)

Bake in from day one. Cannot be retrofitted.

```html
<!-- Schema: LocalBusiness or Service (choose relevant type) -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "[Service name]",
  "provider": {
    "@type": "Organization",
    "name": "[Brand]",
    "url": "[URL]"
  },
  "description": "[Outcome-focused description]",
  "areaServed": "[City or Country]",
  "hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "[Offer name]"
  }
}
</script>

<!-- FAQ Schema (boosts AI Overview inclusion) -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "[Question text]",
      "acceptedAnswer": { "@type": "Answer", "text": "[Answer]" }
    }
  ]
}
</script>
```

**AIO Content Rules** (so AI Overviews cite your page):
- Use clear `<h2>` and `<h3>` for every question the visitor might ask
- Answer questions in the first sentence of each paragraph (inverted pyramid)
- Include entity-rich language: specific names, places, numbers, dates
- Use `<article>` wrapper around main content with `itemprop="mainContentOfPage"`

## 7. Analytics Integration

```html
<!-- Google Analytics 4 — defer, non-blocking -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>

<!-- CTA conversion tracking -->
<button class="cta-primary" 
        onclick="gtag('event','cta_click',{event_category:'conversion',event_label:'hero_cta'})">
  Get My Free Audit
</button>
```

**Minimum tracking events to implement:**
- `cta_click` — every CTA button, labeled by section
- `form_submit` — every form submission
- `scroll_depth` — 25%, 50%, 75%, 100% (use GA4 enhanced measurement)

## 8. Performance Checklist

Before shipping, verify:
- [ ] Lighthouse Performance score ≥ 90 (mobile)
- [ ] Lighthouse Accessibility score ≥ 95
- [ ] Lighthouse SEO score = 100
- [ ] No render-blocking resources (check Network tab)
- [ ] All images have explicit `width` + `height`
- [ ] `font-display: swap` on all custom fonts
- [ ] Schema markup validates at schema.org/validator
- [ ] Meta description between 120–155 characters
- [ ] H1 appears exactly once per page
- [ ] All CTAs tracked in analytics

## 9. Client Handover Requirements

Empowered clients keep content fresh (which signals "aliveness" to search engines).

- Use a CMS or clearly commented HTML sections with `<!-- EDITABLE: headline -->` markers
- Provide a Loom/screen-recorded walkthrough of how to update content
- Ensure all font/image/icon licenses allow client ownership
- Document the analytics dashboard and what each metric means
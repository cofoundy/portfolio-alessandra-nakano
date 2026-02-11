# QA Report: Alessandra Nakano Herrera

**Date:** 2026-02-11
**URL:** https://cofoundy.github.io/portfolio-alessandra-nakano/
**Status:** FAIL

## Data Validation
- [x] Name matches source: "Alessandra Nakano Herrera" matches Sheet and config.ts
- [x] Email matches source: "a.nakano@pucp.edu.pe" matches Sheet exactly
- [x] Title consistent with source: "Psicóloga Clínica · Educación Emocional" — consistent with CV/profession
- [x] Companies listed exist in config.ts: La Tarumba, Nido Crayolas, Ensayando la Paz, La Casa Amarilla
- [x] Education institutions match config.ts: PUCP, FAI Chile, FLACSO Argentina
- [x] Dates match config.ts exactly
- [x] No hallucinated data detected

## Clean Deploy
- [x] No "Powered by" / "Made with" / "Built with" visible text
- [x] No "View source" / "View on GitHub" / "Fork this" template links
- [x] No "Lorem ipsum" / "Your name here" / "[placeholder]" text
- [x] No Astro logo, Vercel badge, or template watermarks visible to users
- [x] No "undefined" or "null" visible in content
- [x] No broken links showing "#" or "javascript:void(0)" as visible text

## Technical
- [x] Page loads — HTTP 200
- [x] CSS loads — HTTP 200 (`_astro/index.z47Fn8Iq.css`)
- [x] Profile image exists — HTTP 200 (`profile.jpg`)
- [x] Favicon loads — HTTP 200 (`favicon.svg`)
- [ ] Console errors — UNABLE TO CHECK (Chrome MCP server not running)
- [x] astro.config.mjs has both `site` and `base` correctly set

## Issues Found

### ISSUE 1 — CRITICAL: Programming symbols background pattern inappropriate for a Psychologist
**Severity:** High
**Location:** Hero.astro (lines 31-119), deployed HTML background SVG
**Details:** The Hero section background contains a repeating SVG pattern of programming symbols: `</>`, `{}`, `=>`, `[]`, `<>`, `()`, `::`, `==`, `++`, `;`. These are code/developer-oriented symbols that are completely inappropriate for a Clinical Psychologist specializing in Early Childhood Emotional Education. This is a template artifact from the `minimal-mono` developer-focused template that was not customized for the client's profession.
**Fix:** Remove the `programming-symbols` SVG pattern entirely, or replace with profession-appropriate decorative elements (e.g., soft organic shapes, leaf patterns, or abstract waves).

### ISSUE 2 — CRITICAL: Footer renders empty Twitter and GitHub icon links
**Severity:** High
**Location:** Footer.astro (lines 73-120), visible in deployed HTML
**Details:** The Footer component does NOT use conditional rendering for Twitter and GitHub social links. Alessandra's config.ts only defines `email` and `linkedin` in `social: {}` — no `twitter` or `github`. However, Footer.astro hardcodes `<a href={siteConfig.social.twitter}>` and `<a href={siteConfig.social.github}>` without optional chaining guards. The result in deployed HTML is two `<a>` tags with no `href` attribute, rendering as clickable Twitter (X) and GitHub icons that link to nothing. This is a known template bug (documented in MEMORY.md).
**Fix:** Add conditional rendering in Footer.astro: `{siteConfig.social?.twitter && ( ... )}` and `{siteConfig.social?.github && ( ... )}`, matching the pattern already used in Hero.astro.

### ISSUE 3 — MEDIUM: No mobile navigation (hamburger menu missing)
**Severity:** Medium
**Location:** Header.astro (line 11)
**Details:** The header uses `class="hidden md:block"` which completely hides the navigation on mobile viewports (< 768px). There is no hamburger menu or any alternative mobile navigation. On mobile, users have no way to navigate to specific sections except by scrolling. This is a known limitation of the minimal-mono template.
**Fix:** Add a mobile hamburger menu component, or at minimum add a sticky bottom nav bar for mobile.

### ISSUE 4 — LOW: Profile photo exists but is not displayed anywhere
**Severity:** Low
**Location:** `public/profile.jpg` exists (HTTP 200) but is not referenced in any component
**Details:** The client submitted a professional photo (per Google Sheet form). The file `profile.jpg` exists in the public folder and loads correctly via HTTP, but the Hero component and no other component references or displays it. The client specifically provided a photo and requested colors that "combine with the photo colors — green, pink, beige."
**Fix:** Add the profile photo to the Hero section (e.g., circular photo alongside the name, or in the About section).

### ISSUE 5 — LOW: HTML has unclosed `<section>` nesting issue
**Severity:** Low
**Location:** `index.astro` (lines 30-36)
**Details:** In the deployed HTML, the `<section>` wrapping Hero through Education contains the Hero which itself opens with `<section>` (the `<div id="hero">`), and the inner sections (About, Projects, Experience, Education) each use `<section id="...">`. The outer `<section>` in index.astro wraps everything including the Hero div and the other section tags. While not a visual bug, it creates semantically odd nested sections.
**Fix:** Minor — change the outer wrapper from `<section>` to `<div>` or `<main>`.

## Summary

| Check | Result |
|-------|--------|
| Data accuracy | PASS |
| Anti-hallucination | PASS |
| No watermarks/placeholders | PASS |
| CSS/assets loading | PASS |
| Template artifact (code symbols) | FAIL |
| Broken social links in footer | FAIL |
| Mobile navigation | FAIL |
| Profile photo displayed | FAIL |

## Evidence
- Screenshots could not be captured (Chrome MCP server was not running during QA)
- All checks performed via curl HTTP requests and full HTML source analysis

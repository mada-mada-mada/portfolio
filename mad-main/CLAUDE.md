# Project: Magdalena Szymaniec — Portfolio

## Overview
Static portfolio site for Magdalena Szymaniec, a senior product designer based in Berlin. Built with Astro (static site generator, no framework). Deployed on Netlify — **do not push to git unless Magda explicitly asks**, Netlify credits are limited.

## Stack
- **Astro 6.x** — static output, no JS framework
- **Vanilla CSS** — all styles are inline `<style>` blocks inside each `.astro` page; no shared stylesheets
- **Google Fonts** — Lora (serif headings, weight 600), DM Sans (body, weight 400/600), Gentium Plus (weight 700, not currently used)
- No build-time components, no shared layouts — each page is fully self-contained

## Running locally
```bash
cd /Users/magdalenaszymaniec/portfolio/mad-main
npm run dev      # starts dev server
npm run build    # production build
```

---

## File structure

```
src/pages/
  index.astro               # Homepage — two case study cards
  about.astro               # About page
  blindmate-onboarding.astro  # Case study — Blindmate Onboarding (longest page)
  blindmate-premium.astro   # Case study — Blindmate Premium

public/
  icon_magda.svg            # Nav avatar / favicon
  CV_magdalena.pdf
  onboarding/               # All images for blindmate-onboarding.astro
  blindmate/                # Images for index.astro onboarding card
    welcome/                # Animated welcome screen PNGs (5 variants)
  premium/                  # SVGs for index.astro premium card
  about/                    # Photos for about.astro
  logos/                    # Company logos for about.astro carousel
```

---

## Design tokens

### Colours
| Token | Value | Usage |
|-------|-------|-------|
| Page bg | `#f7f6fa` | Body background |
| Primary text | `#302b36` | All headings and body copy |
| Muted text | `#827d87` | Captions, dates, secondary labels |
| Purple accent | `#9b90cc` | Buttons, active nav pill, borders, arrows |
| Purple hover | `#8778bc` | Button hover state |
| Frame bg | `#f3f1fe` | `img-frame`, `img-strip`, premium section bg |
| Frame border | `#d8d2f0` | All frame borders |
| Tag bg | `#eae5fc` | Tag pills, footer bg |

### Typography
| Element | Font | Size (mobile → desktop) | Weight |
|---------|------|--------------------------|--------|
| `h1` (about) | Lora | 1.75rem → 2.5rem | 600 |
| `cs-title` (case study h1) | Lora | 2rem → 3rem | 600 |
| `cs-section-title` (h2) | Lora | 1.25rem → 1.5rem | 600 |
| `cs-subsection-title` (h3) | Lora | 1.0625rem → 1.25rem | 600 |
| `body-text` | DM Sans | 0.9375rem → 1rem | 400 |
| `img-caption` | DM Sans | 0.8125rem | 400, muted |
| Nav links | DM Sans | 0.875rem | 600 |
| Tags / pills | DM Sans | 0.8125rem | 600 |

### Spacing rhythm
- Mobile page padding: `24px` horizontal
- Desktop page padding: `clamp(48px, 8vw, 160px)` horizontal
- Desktop breakpoint: `min-width: 640px`
- Section gap (cs-body): `40px` mobile, `48px` desktop
- Frame padding: `24px` mobile, `32px` desktop

### Shadows
**Currently: no shadows on any images or UI elements.** Magda removed all shadows in May 2026 and decided not to add them back.

---

## CSS architecture

Every page is a single `.astro` file with all CSS in a `<style>` block. There is **no shared CSS**. If you add a new page, copy patterns manually.

### Important quirk — Astro CSS scoping
Astro scopes class selectors automatically. **`#id` selectors must be wrapped in `:global()`** to work, e.g. `:global(#lightbox) { ... }`. Class selectors work normally and do NOT need `:global()`.

### Mobile-first
All base styles are mobile. Desktop overrides live inside `@media (min-width: 640px)` at the bottom of the `<style>` block.

---

## Shared patterns across pages

### Nav
Sticky top nav. On mobile: shows only nav links (home / about). On desktop: also shows avatar + name on the left. Active link gets `background-color: #9b90cc; color: #fff` pill.

### Page fade in/out
```css
@keyframes page-fade-in { from { opacity: 0; } to { opacity: 1; } }
body { animation: page-fade-in 0.35s ease both; }
```
Internal links fade the page out over 210ms before navigating (JS in each page).

### Footer
`background-color: #eae5fc`, contains LinkedIn / Illustration Portfolio / Instagram links + copyright.

---

## blindmate-onboarding.astro — detailed breakdown

This is the most complex page. Full case study with many image sections, interactive lightbox, tooltips, and a before/after animation.

### Page sections (in order)
1. **Header** — year `2024–2026`, title, tags (B2C, App Onboarding, `10 min read`)
2. **TL;DR box** — lilac background, two-column layout on desktop (text left, goals/role right)
3. **Stats grid** — 4 stats, 2-col mobile → 4-col desktop
4. **The challenge** — body text + `image1.png` (all roads lead to the Wall)
5. **Why this matters** — body text only
6. **What I worked on** — body text + bullet list
7. **Role selection intro** — body text (with glossary tooltips)
8. **Old onboarding strip** — `old_onboarding1–4.png`, arrows between images, caption
9. **Conversational onboarding intro** — body text (with glossary tooltips)
10. **Before/After comparison** — `role_selection1.png` → arrow → `quiz1.svg` + `quiz2.svg`
11. **Conversational aspect** — 3 body paragraphs (last one has "the Wall" tooltip)
12. **Full flow diagram** — `image4.png` (landscape), caption
13. **Simplified user flow** — `user-flow.svg`, caption
14. **User testing** — h2 + 4 body paragraphs
15. **3 user quotes** — `quote-group--three` (3-col on desktop)
16. **Key design decisions** — h2 with subsections:
    - *The Humour* — body text
    - *Shorter = Better (?)* — body text + `shorter1.svg` / `shorter2.svg` (img-pair, 420px desktop)
    - Image caption
    - *The wall* — body text + Josh quote (hug width) + body text
17. **Wall iterations** — `wall1–3.png` with lilac arrows, caption
18. **Wall body text** — 2 paragraphs + *Early user invite* subsection
19. **Early wall** — `earlywall1.svg` / `earlywall2.svg` (img-pair), caption
20. **Smaller changes** — h3 + bullet list (max-width 68ch)
21. **AB Tests** — h2 + 3 body paragraphs
22. **Welcome screen variants** — `welcome1–5.png` (`img-strip--welcome`, 320px desktop)
23. **New user in discovery phase** — h2 + body text
24. **Checklist images** — `checklist1–3.png` in `img-hierarchy` (natural size hierarchy), caption
25. **Learnings** — h2 + 3 body paragraphs
26. **Before/After animation** — `before1–3.png` ↔ `after1–3.png` crossfade (10s loop), "then"/"now" label

### Image display classes

| Class | Description | Mobile | Desktop |
|-------|-------------|--------|---------|
| `.wide-img` | Full-width image inside `img-frame` | 100% width | 100% width |
| `.phone-shot` | Portrait phone screenshot | `flex: 1 1 0; width: 0` (proportional) | `height: 420px; width: auto` |
| `.comparison-phone` | Left phone in before/after comparison | proportional | `height: 380px` |
| `.comparison-card` | Card(s) on right of comparison | proportional | `height: 380px` |
| `.hierarchy-img` | Checklist images (natural size hierarchy) | `flex: 1 1 0; width: 0` | `width: 220px; height: auto` |

**Special overrides:**
- `.img-strip--welcome .phone-shot` → `height: 320px` (5 welcome screens, smaller)
- `.img-pair .phone-shot` → `width: 40%; height: auto` mobile, `height: 420px` desktop

### Frame / container classes

| Class | Description |
|-------|-------------|
| `.img-frame` | Full-bleed frame (margin: 0 -24px mobile), lilac bg + border |
| `.img-strip` | Same as img-frame but for scrollable strips (now always `overflow-x: visible`) |
| `.img-strip--captioned` | Strip variant with caption below; sets outer `padding: 0`, inner scroll-row provides padding |
| `.img-strip--welcome` | Modifier for 5-image welcome strip |
| `.img-pair` | Flex row of 2 SVG phone screens, centered, 64px gap |
| `.img-hierarchy` | Flex row with natural-size images (fixed width on desktop = height hierarchy) |
| `.comparison-layout` | Before/after flex row: phone + arrow + cards |

**Critical CSS bug to avoid:** The desktop `@media` rule `.img-strip { padding: 32px }` will override `.img-strip--captioned { padding: 0 }` because it appears later in the stylesheet. The fix is to explicitly re-add `.img-strip--captioned { padding: 0 }` inside the desktop media query. This is already done — don't remove it.

### Caption behaviour
- `.img-caption` base: `font-size: 0.8125rem; color: #827d87; margin-top: 20px; max-width: 52ch; margin: auto`
- `.img-caption--center`: `text-align: center`
- Inside `img-strip--captioned`: `margin-top: 0; padding: 12px 24px 24px` (mobile), `padding: 12px 32px 32px` (desktop)

### Lightbox
All clickable images open a full-screen lightbox with prev/next navigation. Grouped by closest `.img-frame` or `.img-strip` ancestor. Supports keyboard (←/→/Esc) and touch swipe.

**Recognised image classes** (must be in this list or lightbox ignores them):
`.wide-img`, `.phone-shot`, `.comparison-phone`, `.comparison-card`, `.hierarchy-img`

**Before/after slots** have their own click handler on `.ba-slot` that opens all 6 images.

### Glossary tooltips
Two types — both are bottom-sheet popups on mobile:
- `.wall-term` — always shows "The Wall" definition
- `.glossary-term` — reads `data-term` and `data-def` attributes

### Before/After animation
```
.ba-before / .ba-after — stacked in CSS grid (grid-area: 1/1)
Animation: 10s cycle
  0–35%:  "then" / before visible
  35–50%: 1.5s crossfade
  50–85%: "now" / after visible
  85–100%: 1.5s crossfade back
```

### Quote styles
| Class | Description |
|-------|-------------|
| `.quote-block` | Purple left border, `#f3f1fe` bg |
| `.quote-block--hug` | `align-self: flex-start` — hugs content width |
| `.quote-group` | Flex column of quote blocks |
| `.quote-group--three` | 3-col row on desktop |

### Arrows between images
Small `#9b90cc` SVG arrows (`width="20" height="20"`, `opacity: 0.5`) used between images in strips. Class `.strip-arrow`. Currently used in: old onboarding strip and wall iterations strip.

---

## index.astro — detailed breakdown

### Sections
1. **Nav**
2. **Hero** — name, title, intro text, CV download button
3. **Case study card: Blindmate Onboarding** — meta, description, "Read Case Study" button, then preview images
4. **Case study card: Blindmate Premium** — meta, description, "Read Case Study" button, then animated premium reel
5. **Footer**

### Onboarding card preview
Three `screen-wrap` elements in a horizontal scroll strip:
- Animated welcome screen (`screen-slot` with 5 images cycling via JS, 1.2s opacity transition)
- `conversational-onboarding.png`
- `invites-sent.png`

Cursor shows a label pill on hover (`data-label` attribute).

### Premium card preview
Infinite auto-scrolling reel of 5 premium feature SVGs, duplicated for seamless loop. Pauses on hover.

---

## about.astro — detailed breakdown

### Sections
1. **Intro** — bio text, CV download + LinkedIn buttons
2. **Photo strip** — 6 photos in infinite scroll animation (55s), duplicated for loop. Tap on mobile opens lightbox. Cursor label on desktop.
3. **Motto section** — quote + body text + company logo carousel
4. **Testimonials** — one testimonial card (Laurenz Reichl, Blindmate CEO)
5. **Footer**

---

## blindmate-premium.astro

Simple static text case study — no images, no interactive elements. Just nav, header with tags, body sections, footer. Not complex.

---

## Key decisions & conventions

- **No shadows** — removed entirely in May 2026 by Magda's request
- **No emojis** unless Magda asks
- **No comments in code** unless the why is genuinely non-obvious
- **Button style** — `background: #9b90cc`, no border-radius (square corners), `padding: 8px 16px`, DM Sans 600
- **Links in body text** — `color: #302b36`, `text-decoration-color: #c8c5d0`, hover to `#9b90cc`
- **Git** — never push unless Magda explicitly says so (Netlify deploy credits)
- **New images** — Magda drops files in `/Users/magdalenaszymaniec/Desktop/screenshots for onboarding/` then asks to copy them over. Use `cp` or `sips` as needed.
- **PNG transparent padding** — Figma device mockup exports embed phone content (1170×2532) inside larger PNG (1440×2802) with ~135px transparent border. Fix with: `sips -c 2532 1170 filename.png`
- **`text-wrap: balance`** — avoid on body paragraphs; it shrinks line width visually. Use `max-width` instead.
- **Astro `#id` CSS** — must use `:global(#id)` syntax or styles won't apply

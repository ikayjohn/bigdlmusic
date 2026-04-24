# BIG DL — Website Build Instructions

## For Claude Code / Codex Terminal

**Domain:** www.bigdlmusic.com
**Stack:** Static HTML / CSS / Vanilla JS — single-page scroll site
**Font:** Inter (Google Fonts)
**Approach:** Section-by-section build. Dark, minimal, intentional. No frameworks, no dependencies beyond Google Fonts.

---

## 1. PROJECT SETUP

Create the following file structure:

```
bigdl-site/
├── index.html
├── css/
│   └── style.css
├── js/
│   └── main.js
├── assets/
│   ├── images/          ← photoshoot images go here
│   └── logo/            ← Big DL logo files go here
└── favicon.ico
```

### Base HTML Shell

- Standard HTML5 boilerplate
- `<meta charset="UTF-8">`
- `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
- `<title>Big DL — Beat-driven rap for deep thinkers</title>`
- Meta description: `"Big DL is a beat maker and rapper creating beat-driven rap for deep thinkers. Stream the Flex EP now."`
- Open Graph tags for social sharing (og:title, og:description, og:image, og:url)
- Link to Google Fonts: Inter (weights 400, 500, 600, 700)
- Link to `css/style.css`
- Defer load `js/main.js`
- Favicon link

---

## 2. DESIGN SYSTEM (CSS Custom Properties)

Define all design tokens as CSS custom properties on `:root`. This is the single source of truth for the entire site. Follow these values exactly — do not deviate.

### Color Palette

```css
:root {
  /* === PRIMARY COLOURS === */
  --color-black:        #0B0B0B;   /* Primary base — backgrounds, main sections */
  --color-charcoal:     #1A1A1A;   /* Secondary backgrounds, cards, containers */
  --color-white:        #EDEDED;   /* Main text, headings */
  --color-grey:         #9A9A9A;   /* Secondary text, captions, labels */

  /* === ACCENT === */
  --color-amber:        #C89B3C;   /* CTA buttons, hover states, active elements, subtle highlights */
  /* Usage rule: accent should feel DISCOVERED, not obvious */

  /* === SUPPORT (optional, use sparingly) === */
  --color-steel:        #3A6EA5;   /* Secondary highlights, alternative mood */
}
```

**Color Ratio (strictly enforce):**
- 80% dark base (`--color-black`, `--color-charcoal`)
- 15% neutral text (`--color-white`, `--color-grey`)
- 5% accent (`--color-amber`)

**Color Rules — hard restrictions:**
- NO gradients anywhere
- NO bright or neon colours
- NO multiple accents competing
- NO colourful backgrounds
- The amber accent must feel like a subtle discovery, not a loud highlight

### Typography

```css
:root {
  /* === FONT === */
  --font-primary:       'Inter', sans-serif;

  /* === TYPE SCALE === */
  --text-h1:            clamp(48px, 5vw, 64px);    /* Hero / main title */
  --text-h2:            clamp(28px, 3vw, 36px);     /* Section titles */
  --text-h3:            clamp(20px, 2.5vw, 24px);   /* Subsections */
  --text-body:          clamp(16px, 1.2vw, 18px);   /* Body text */
  --text-small:         clamp(12px, 1vw, 14px);     /* Captions, labels */

  /* === FONT WEIGHTS === */
  --weight-h1:          700;
  --weight-h2:          600;
  --weight-h3:          600;
  --weight-body:        400;
  --weight-small:       400;

  /* === LINE HEIGHTS === */
  --lh-tight:           1.1;      /* H1 */
  --lh-snug:            1.3;      /* H2, H3 */
  --lh-body:            1.6;      /* Body text */

  /* === LETTER SPACING === */
  --ls-heading:         0.02em;   /* +2% on headings */
  --ls-body:            0;
}
```

**Typography Rules — hard restrictions:**
- Only use Inter. No secondary fonts, no decorative fonts, no script fonts.
- Headings: clean, no text shadows, no outlines, no effects
- Body text: short paragraphs, high readability, no dense blocks
- Emphasis: use font-weight (bold) OR accent colour sparingly — never both simultaneously
- Do NOT use all-caps everywhere. Reserve uppercase for very small labels only.
- Slight letter-spacing on headings (+1% to +2%) is encouraged

### Spacing & Layout

```css
:root {
  /* === SPACING === */
  --space-xs:           8px;
  --space-sm:           16px;
  --space-md:           32px;
  --space-lg:           64px;
  --space-xl:           96px;
  --space-2xl:          128px;

  /* === LAYOUT === */
  --max-width:          1200px;
  --section-padding:    var(--space-xl) var(--space-md);
}
```

### Global Resets & Base Styles

```css
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: var(--font-primary);
  font-size: var(--text-body);
  font-weight: var(--weight-body);
  line-height: var(--lh-body);
  color: var(--color-white);
  background-color: var(--color-black);
  overflow-x: hidden;
}

img {
  max-width: 100%;
  height: auto;
  display: block;
}

a {
  color: inherit;
  text-decoration: none;
}
```

---

## 3. SECTION-BY-SECTION BUILD

The page is a single continuous scroll with 5 sections. Each section is a `<section>` element with its own `id`.

---

### SECTION 1 — HERO

**ID:** `#hero`
**Purpose:** Immediately communicate: "This is a real artist. This is intentional."

#### Layout

- Full viewport height (`100vh`, `100dvh` as fallback for mobile)
- Full-screen dark background image (the shadow-face hooded photo from the photoshoot)
- Background image: `background-size: cover; background-position: center top;`
- Dark overlay on top of the image using `::after` pseudo-element with `background: rgba(11, 11, 11, 0.55)` — enough to keep text legible but image visible
- Content positioned center-left on desktop, centered on mobile
- Content sits above the overlay (z-index layering)

#### Content (in order, top to bottom within the content block)

1. **Big DL Logo** — the wordmark/logo image from `assets/logo/`. Display at a reasonable width (around 280–360px on desktop, scaled down on mobile). Use an `<img>` tag with proper alt text: `alt="Big DL"`. Do NOT use text as a substitute — use the actual logo image file.

2. **Sub-headline** — `<p>` tag:
   ```
   Beat-driven rap for deep thinkers.
   ```
   Style: `var(--text-h3)`, `var(--weight-h3)`, `var(--color-white)`, `var(--ls-heading)`

3. **Optional line** — `<p>` tag, smaller text:
   ```
   Flex EP — now streaming
   ```
   Style: `var(--text-small)`, `var(--color-grey)`, uppercase, `letter-spacing: 0.1em`

4. **CTA Buttons** — two buttons side by side (flex row, gap 16px):

   - **"Listen on Spotify"** — `<a>` styled as primary button
     - Background: `var(--color-amber)`
     - Text: `var(--color-black)`, `var(--weight-h3)`
     - Padding: `14px 28px`
     - Border-radius: `0` (sharp corners, no rounding — intentional)
     - Hover: slight brightness increase, subtle transform scale(1.02)
     - `href="#"` placeholder (client will replace with actual Spotify link)

   - **"Watch on YouTube"** — `<a>` styled as secondary/outline button
     - Background: transparent
     - Border: `1px solid var(--color-white)`
     - Text: `var(--color-white)`, `var(--weight-h3)`
     - Same padding and radius as primary
     - Hover: border colour shifts to `var(--color-amber)`, text shifts to `var(--color-amber)`
     - `href="#"` placeholder

#### Animation

- On page load, fade in the content block with a slight upward translate (transform: translateY(20px) → translateY(0), opacity: 0 → 1)
- Duration: 0.8s, ease-out
- Stagger: logo first, then sub-headline (0.15s delay), then optional line (0.3s delay), then buttons (0.45s delay)
- Use CSS `@keyframes` and `animation-delay` — no JS needed

#### Responsive

- On screens below 768px: center all text, reduce logo width to ~200px, stack buttons vertically (flex-direction: column), full-width buttons
- On screens below 480px: further reduce spacing

---

### SECTION 2 — MUSIC (FLEX EP)

**ID:** `#music`
**Purpose:** Make it easy to listen immediately.

#### Layout

- Background: `var(--color-charcoal)` — this section uses the secondary dark to create visual separation from the hero
- Padding: `var(--section-padding)`
- Max-width container centered
- Two-column layout on desktop (flex or CSS grid):
  - **Left column (40–45%):** EP artwork image (placeholder: a square image container, 400×400px max, with `background: var(--color-black)` as fallback). The client will supply the Flex EP cover art — place an `<img>` tag with `src="assets/images/flex-ep-cover.jpg"` and `alt="Flex EP Cover Art"`.
  - **Right column (55–60%):** Text content and embed/button

#### Right Column Content

1. **Section title:**
   ```
   Flex EP
   ```
   Style: `var(--text-h2)`, `var(--weight-h2)`, `var(--color-white)`, `var(--ls-heading)`

2. **Supporting line:**
   ```
   Built for drive-time listening. Designed for those who connect the dots.
   ```
   Style: `var(--text-body)`, `var(--color-grey)`, max-width ~480px

3. **Spotify Embed or Button:**
   - Include a Spotify embed `<iframe>` placeholder. Use this format:
     ```html
     <iframe
       style="border-radius: 0;"
       src="https://open.spotify.com/embed/album/ALBUM_ID_HERE?utm_source=generator&theme=0"
       width="100%"
       height="352"
       frameBorder="0"
       allowfullscreen=""
       allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
       loading="lazy"
     ></iframe>
     ```
   - The `theme=0` parameter forces dark mode on the Spotify embed
   - Below the embed, also include a fallback "Listen Now" button (same style as the primary CTA button from the hero) in case the embed doesn't load or the client prefers a button instead. Wrap it in a `<noscript>` or simply place it below the embed as a secondary option.

#### Animation

- Fade-in-up on scroll (use IntersectionObserver in `main.js`). When the section enters the viewport (threshold ~0.15), add a class like `.visible` that triggers opacity/transform transition.
- EP artwork: fade in from left (translateX(-30px) → 0)
- Right column: fade in from right (translateX(30px) → 0)
- Duration: 0.6s, ease-out

#### Responsive

- Below 768px: stack vertically. EP artwork on top (full width, max 360px, centered), text content below.
- Reduce spacing between elements

---

### SECTION 3 — ABOUT

**ID:** `#about`
**Purpose:** Build credibility and reinforce positioning.

#### Layout

- Background: `var(--color-black)` — back to primary dark
- Padding: `var(--section-padding)`
- Max-width container, but narrower than other sections — cap at `720px` for a clean text-block feel
- Centered alignment

#### Content

1. **Section label (optional, small):**
   ```
   About
   ```
   Style: `var(--text-small)`, `var(--color-grey)`, uppercase, `letter-spacing: 0.15em`, margin-bottom `var(--space-md)`

2. **About copy — three paragraphs:**

   Paragraph 1:
   ```
   Big DL is a beat maker and rapper creating beat-driven rap for deep thinkers.
   ```
   Style: `var(--text-h3)`, `var(--weight-h3)`, `var(--color-white)` — this first line reads as a bold intro statement

   Paragraph 2:
   ```
   His music blends Afro-leaning rhythm with layered metaphors, playful storytelling, and calm reflection — records designed for drive-time listening and listeners who enjoy connecting the dots.
   ```
   Style: `var(--text-body)`, `var(--color-grey)`, line-height `var(--lh-body)`

   Paragraph 3:
   ```
   Every record is built with intention — from the beat to the final delivery.
   ```
   Style: `var(--text-body)`, `var(--color-white)` — slightly brighter than paragraph 2 for emphasis on the closing statement

3. **Spacing between paragraphs:** `var(--space-md)` (32px)

#### Animation

- Simple fade-in on scroll. No directional movement — just opacity 0 → 1 over 0.6s.
- Stagger the three paragraphs slightly (0s, 0.1s, 0.2s delay)

#### Responsive

- Already single-column, so minimal changes needed
- Ensure padding adjusts: `var(--space-lg) var(--space-sm)` on mobile

---

### SECTION 4 — VISUAL STRIP (PROOF)

**ID:** `#visuals`
**Purpose:** Show presence without noise. Proof-of-presence through photoshoot imagery.

#### Layout

- Background: `var(--color-charcoal)`
- Padding: `var(--space-lg) 0` — no horizontal padding, images go edge-to-edge
- NO section title — the images speak for themselves

#### Image Grid

- 9 images in a grid
- Desktop: 3 columns × 3 rows
- Use CSS Grid: `grid-template-columns: repeat(3, 1fr);`
- Gap: `4px` (very tight gaps — the images should feel cohesive, almost like a contact sheet)
- Each image cell: `aspect-ratio: 1 / 1;` (square crops)
- Images use `object-fit: cover;` to fill the square
- Placeholder: use `<img src="assets/images/photo-01.jpg" alt="Big DL photoshoot" />` through `photo-09.jpg`
- If actual images aren't available yet, render placeholder divs with `background: var(--color-black);` and a subtle border: `1px solid rgba(237, 237, 237, 0.05)`

#### Hover Effect

- On hover over any image: subtle scale (transform: scale(1.03)), slight brightness increase (filter: brightness(1.1)), and reduce opacity of ALL other images to 0.5
- Transition: 0.3s ease
- This effect requires a CSS approach: on `.visual-grid:hover img { opacity: 0.5; }` then `.visual-grid:hover img:hover { opacity: 1; transform: scale(1.03); filter: brightness(1.1); }`

#### Animation

- Images fade in on scroll with a staggered delay (each image 0.05s after the previous)
- Use IntersectionObserver to trigger

#### Responsive

- Tablet (below 768px): 3 columns still, but with 3px gap
- Mobile (below 480px): 3 columns still (keep the grid tight), 2px gap

---

### SECTION 5 — CONTACT

**ID:** `#contact`
**Purpose:** Provide a clean communication channel.

#### Layout

- Background: `var(--color-black)`
- Padding: `var(--section-padding)`, with extra top padding `var(--space-2xl)` to create breathing room
- Max-width container, centered text
- This section should feel spacious and final

#### Content

1. **Section title:**
   ```
   Contact
   ```
   Style: `var(--text-h2)`, `var(--weight-h2)`, `var(--color-white)`, `var(--ls-heading)`

2. **Body text:**
   ```
   For inquiries, collaborations, or bookings:
   ```
   Style: `var(--text-body)`, `var(--color-grey)`, margin-top `var(--space-sm)`

3. **Email link:**
   ```html
   <a href="mailto:bigdl.studio@gmail.com">bigdl.studio@gmail.com</a>
   ```
   Style: `var(--text-h3)`, `var(--color-amber)`, hover: underline with `text-underline-offset: 4px`

4. **Social links** — horizontal row (flex, centered, gap 32px), margin-top `var(--space-lg)`:
   - Instagram — `<a href="#" target="_blank" rel="noopener">Instagram</a>`
   - TikTok — `<a href="#" target="_blank" rel="noopener">TikTok</a>`
   - YouTube — `<a href="#" target="_blank" rel="noopener">YouTube</a>`
   - Style: `var(--text-small)`, `var(--color-grey)`, uppercase, `letter-spacing: 0.1em`
   - Hover: color shifts to `var(--color-white)`
   - Optionally use simple SVG icons (inline SVGs for Instagram, TikTok, YouTube) next to or instead of text. Keep them monochrome (`currentColor`), 20×20px.

5. **Footer line** (bottom of page, small):
   ```
   © 2026 Big DL. All rights reserved.
   ```
   Style: `var(--text-small)`, `var(--color-grey)`, margin-top `var(--space-2xl)`, opacity 0.5

#### Animation

- Fade in on scroll, same as About section

---

## 4. JAVASCRIPT (`main.js`)

Keep JS minimal. Only two responsibilities:

### Scroll-triggered Animations (IntersectionObserver)

```js
// Select all elements that should animate on scroll
const animateElements = document.querySelectorAll('.animate-on-scroll');

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target); // animate once only
    }
  });
}, {
  threshold: 0.15,
  rootMargin: '0px 0px -50px 0px'
});

animateElements.forEach(el => observer.observe(el));
```

Every section and animated element should have the class `animate-on-scroll`. The corresponding CSS:

```css
.animate-on-scroll {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.animate-on-scroll.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Directional variants */
.animate-on-scroll.from-left {
  transform: translateX(-30px);
}
.animate-on-scroll.from-left.visible {
  transform: translateX(0);
}

.animate-on-scroll.from-right {
  transform: translateX(30px);
}
.animate-on-scroll.from-right.visible {
  transform: translateX(0);
}
```

### Smooth Scroll (if using anchor navigation)

The site currently has no fixed nav, but if a nav is added later, ensure anchor links scroll smoothly. The `scroll-behavior: smooth` on `html` handles this — no JS needed.

---

## 5. RESPONSIVE BREAKPOINTS

```css
/* Desktop-first approach */

/* Tablet */
@media (max-width: 768px) {
  /* Stack two-column layouts */
  /* Reduce heading sizes via clamp (already handled) */
  /* Adjust section padding */
}

/* Mobile */
@media (max-width: 480px) {
  /* Further spacing reduction */
  /* Full-width buttons */
  /* Tighter image grid gaps */
}
```

Specific responsive rules are noted in each section above. The `clamp()` values on typography handle most scaling automatically.

---

## 6. PERFORMANCE & BEST PRACTICES

- All images: use `loading="lazy"` on every `<img>` except the hero background (that loads immediately)
- Hero background: set via CSS `background-image` on the `#hero` element — this loads with the page
- Preload the hero background image in `<head>`: `<link rel="preload" as="image" href="assets/images/hero-bg.jpg">`
- Preconnect to Google Fonts: `<link rel="preconnect" href="https://fonts.googleapis.com">`
- Image formats: prefer `.webp` with `.jpg` fallback via `<picture>` element where possible
- Minify CSS and JS for production (can be done manually or via build step later)
- Test on: Chrome, Safari, Firefox, and mobile Safari/Chrome

---

## 7. FILES THE CLIENT NEEDS TO SUPPLY

Before the site can go live, the following assets are needed:

1. **Big DL logo file** — SVG preferred, PNG fallback. Place in `assets/logo/`
2. **Hero background photo** — the shadow-face hooded image from the photoshoot. High resolution (at least 1920px wide). Place as `assets/images/hero-bg.jpg`
3. **Flex EP cover art** — square format (at least 800×800px). Place as `assets/images/flex-ep-cover.jpg`
4. **9 photoshoot images** — for the visual strip grid. Square crops preferred. Place as `assets/images/photo-01.jpg` through `photo-09.jpg`
5. **Spotify album/EP link** — the embed URL for the Flex EP
6. **YouTube link** — for the "Watch on YouTube" CTA
7. **Social media URLs** — Instagram, TikTok, YouTube profile links
8. **Favicon** — 32×32 and 180×180 (apple-touch-icon)

---

## 8. WHAT NOT TO DO

This list is as important as the build instructions. Enforce strictly:

- **NO gradients** — anywhere, ever, on this site
- **NO rounded corners on buttons** — sharp, intentional edges
- **NO generic stock imagery** — only real photoshoot content
- **NO bright or neon colours** — everything stays muted and controlled
- **NO decorative fonts** — Inter only
- **NO parallax scrolling** — keep it clean, not gimmicky
- **NO hamburger menu animations that draw attention** — if a mobile nav is added later, keep it dead simple
- **NO multiple accent colours competing** — amber is the only accent, used at 5%
- **NO heavy JavaScript libraries** — no jQuery, no GSAP, no AOS.js. Vanilla only.
- **NO scroll-jacking** — natural browser scroll behaviour
- **NO autoplaying audio or video**
- **NO cookie banners or popups** (unless legally required later)
- **NO dense text blocks** — keep paragraphs short and breathable

---

## 9. BUILD ORDER

Execute in this sequence:

1. **Phase 1:** Project setup, file structure, HTML shell, CSS reset, design tokens
2. **Phase 2:** Hero section (HTML + CSS + animation)
3. **Phase 3:** Music section (HTML + CSS + Spotify embed placeholder)
4. **Phase 4:** About section (HTML + CSS)
5. **Phase 5:** Visual strip (HTML + CSS grid + hover effects)
6. **Phase 6:** Contact section + footer (HTML + CSS)
7. **Phase 7:** JavaScript (IntersectionObserver scroll animations)
8. **Phase 8:** Responsive testing and adjustments
9. **Phase 9:** Performance pass (lazy loading, preloads, image optimization notes)

---

## 10. FINAL CHECKLIST

Before considering the build complete, verify:

- [ ] All 5 sections render correctly on desktop (1440px, 1280px, 1024px)
- [ ] All 5 sections render correctly on tablet (768px)
- [ ] All 5 sections render correctly on mobile (375px, 390px)
- [ ] Hero animation plays on page load
- [ ] Scroll animations trigger correctly on each section
- [ ] Visual strip hover effect works (dim siblings, brighten hovered image)
- [ ] All links have proper `href` placeholders
- [ ] Email link opens mailto
- [ ] Spotify embed placeholder is in place
- [ ] Colour usage follows the 80/15/5 ratio
- [ ] No gradients anywhere
- [ ] No rounded button corners
- [ ] Typography matches the type scale exactly
- [ ] Page loads fast with no render-blocking resources
- [ ] All images have alt text
- [ ] HTML validates (no errors)

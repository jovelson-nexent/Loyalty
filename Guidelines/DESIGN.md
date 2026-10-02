---
name: Nexent Fusion Style
description: The ember briefing-room showcasing system, grounded in the real Card Avantaj (by Nexent Bank) logo and Geologica type. Warm ember on cream, Georgia editorial headlines over Geologica data chrome, one ember gradient signature per composition.
colors:
  ember-red: "#F21D2F"
  ember-red-alt: "#F23E2E"
  ember-orange-red: "#F25D27"
  ember-orange: "#F2811D"
  ember-peach: "#F2BC8D"
  cream-bg: "oklch(0.97 0.008 42)"
  cream-surface: "oklch(0.995 0.003 42)"
  cream-subtle: "oklch(0.93 0.012 42)"
  cream-border: "oklch(0.89 0.016 42)"
  cream-border-med: "oklch(0.82 0.022 42)"
  ink-primary: "oklch(0.17 0.016 42)"
  ink-secondary: "oklch(0.38 0.016 42)"
  ink-muted: "oklch(0.56 0.010 42)"
  header-bg: "oklch(0.13 0.022 38)"
  header-text: "oklch(0.98 0.003 42)"
  header-muted: "oklch(0.68 0.012 38)"
  brand-navy: "#162632"
  semantic-green: "oklch(0.40 0.12 148)"
  semantic-green-bg: "oklch(0.95 0.05 148 / 0.50)"
  semantic-red-bg: "oklch(0.97 0.05 25 / 0.50)"
  chart-muted: "#7a6860"
  chart-grid: "rgba(160, 110, 90, 0.07)"
  chart-total: "#5C3D2E"
  chart-primary: "#F21D2F"
  chart-other: "#1F6B52"
  chart-secondary: "#4A3D8F"
  scenario-pessimistic: "#8B9299"
  scenario-moderate: "#3B6FB6"
  scenario-optimistic: "#2D8B57"
  gradient-ember: "linear-gradient(135deg, #F21D2F 0%, #F25D27 52%, #F2811D 100%)"
typography:
  display:
    fontFamily: "Georgia, \"Times New Roman\", serif"
    fontSize: "clamp(2.6rem, 5.5vw, 4.2rem)"
    fontWeight: 400
    lineHeight: 1.08
    letterSpacing: "-0.03em"
  headline:
    fontFamily: "Georgia, \"Times New Roman\", serif"
    fontSize: "clamp(1.65rem, 3vw, 2.35rem)"
    fontWeight: 400
    lineHeight: 1.15
    letterSpacing: "-0.02em"
  title:
    fontFamily: "Geologica, Arial, \"Segoe UI\", system-ui, sans-serif"
    fontVariationSettings: "\"CRSV\" 0"
    fontSize: "0.9375rem"
    fontWeight: 600
    lineHeight: 1.35
    letterSpacing: "0"
  body:
    fontFamily: "Geologica, Arial, \"Segoe UI\", system-ui, sans-serif"
    fontVariationSettings: "\"CRSV\" 0"
    fontSize: "15px"
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: "0"
  label:
    fontFamily: "Geologica, Arial, \"Segoe UI\", system-ui, sans-serif"
    fontVariationSettings: "\"CRSV\" 0"
    fontSize: "0.67rem"
    fontWeight: 700
    lineHeight: 1.4
    letterSpacing: "0.12em"
    textTransform: "uppercase"
rounded:
  sm: "6px"
  md: "8px"
  lg: "10px"
  pill: "999px"
spacing:
  xs: "8px"
  sm: "14px"
  md: "18px"
  lg: "24px"
  xl: "36px"
  page-pad: "clamp(24px, 4vw, 56px)"
  page-max: "min(96vw, 1680px)"
components:
  card-surface:
    backgroundColor: "{colors.cream-surface}"
    textColor: "{colors.ink-primary}"
    rounded: "{rounded.lg}"
    padding: "22px 26px"
  insight-box:
    backgroundColor: "{colors.cream-subtle}"
    textColor: "{colors.ink-muted}"
    rounded: "{rounded.lg}"
    padding: "20px 24px"
  deck-toggle-active:
    backgroundColor: "{colors.ember-red}"
    textColor: "{colors.cream-surface}"
    rounded: "{rounded.md}"
    padding: "8px 14px"
  cta-banner:
    backgroundColor: "{colors.header-bg}"
    textColor: "{colors.header-text}"
    rounded: "{rounded.lg}"
    padding: "28px 32px"
  featured-stat-value:
    backgroundColor: "{colors.cream-bg}"
    textColor: "{colors.ember-orange-red}"
    rounded: "{rounded.lg}"
    padding: "20px 22px"
  gradient-hero:
    backgroundColor: "{colors.gradient-ember}"
    textColor: "{colors.header-text}"
    rounded: "0"
    padding: "clamp(48px, 8vh, 96px) clamp(24px, 4vw, 56px)"
---

# Design System: Nexent Fusion Style

## 1. Overview

**Creative North Star: "The Ember Briefing Room" — for a card that moves with you.**

Nexent Fusion Style is the visual language for CEB x Metyis executive analytics presentations, built for Card Avantaj (by Nexent Bank). It keeps the current system's warm scroll-deck showcasing intact and grounds it in the real Card Avantaj brand: the official diamond-ribbon logo, the Geologica typeface, and one gradient signature moment per page.

The canonical implementation is the scroll-deck pattern in `Visuals/Quasi-Cash/Quasi-Cash Betting.html`: a long-form cream canvas with fixed section navigation, Georgia serif headlines, Geologica data chrome, and ember-red wayfinding. Each page is a single self-contained HTML file built with Bootstrap 5.3 utilities, Chart.js 4.4, and vanilla JavaScript. Depth comes from tonal layering and restrained shadows, not glass effects. The one place the system reaches for a gradient is a deliberate signature: a hero band or a single shape device abstracted from the logo, always in the ember palette.

An alternate **dashboard layout** exists in campaign-focused files (e.g. `Visuals/Campaign Clash/Campaign Clash.html`): centered dark header band, 8px cards, sans-only headlines. Use that variant only when the brief explicitly targets campaign-clash or single-screen dashboard density. Default to the scroll-deck.

The system explicitly rejects corporate navy-and-gold dashboard tropes, side-stripe callouts, undifferentiated card grids, and gradient text. Ember color carries meaning: primary action, active toggles, metric emphasis, clash severity, and section wayfinding.

**What the fusion changed from the base Current system (and why):**

| Aspect | Base (Current) | Fusion | Rationale |
|---|---|---|---|
| Color grading | Ember on cream | **Unchanged** | The grading the team preferred; retained exactly. |
| Body / data / UI type | Segoe UI | **Geologica** (Arial fallback) | The real brand typeface; it also matches the logo wordmark. |
| Headline type | Georgia serif | **Unchanged** | Editorial contrast is core to the "showcasing" look. |
| Logo | None (system had no real mark) | **Real Card Avantaj logo package** | The headline reason for the fusion. |
| Gradient | Mostly flat ember | **One ember gradient signature** per page | Brings the brand's "in-motion" energy in the ember palette. |
| Section eyebrows | Uppercase tracked labels | **Unchanged** (one sanctioned all-caps use) | Wayfinding the team relies on; see §3. |

**Key Characteristics:**

- Scroll-deck sections with fixed dot navigation and optional toolbar toggles
- Georgia serif for page title and section headlines; Geologica for labels, tables, charts, body
- Real Card Avantaj diamond-ribbon logo anchoring the header and CTA/hero surfaces
- Full-palette ember spectrum (`#F21D2F` through `#F2BC8D`) for campaign identity, single-series ramps, and the one gradient moment
- Cream-tinted OKLCH neutrals (hue ~42) instead of cold gray or pure white
- 10px corner radius on card surfaces, insight boxes, and table wraps
- Uppercase tracked section eyebrow labels in ember red (the one sanctioned all-caps use)
- Tabular numerals on every metric
- Interactive overview tables with orange-red cell highlight on hover/active

## 2. Colors

Warm ember on cream: saturated accents on tinted neutrals, never pure black or white. **This palette is retained verbatim from the Current system; it is the color grading the fusion is built to preserve.** The Card Avantaj logo's warm orange gradient (`#FE5000 → #FF8B1D`) sits almost exactly on the ember spectrum's orange end (`#F25D27 → #F2811D`), so the real mark reads as part of this family rather than an imported accent.

### Primary

- **Ember Red** (#F21D2F / oklch(0.58 0.24 25)): Section eyebrow labels, active toggles, deck dots, sim cohort labels, opt card numbers, clash highlights, gradient start.
- **Ember Orange-Red** (#F25D27): Featured stat values, overview table active cells, opt card headlines, chart emphasis, gradient midpoint.
- **Ember Orange** (#F2811D): CTA banner strong text, commission stack segments, gradient end, dashboard layout eyebrow (alternate variant). This is the ember tone closest to the Card Avantaj logo orange.
- **Ember Peach** (#F2BC8D): Low-intensity series, elite tier, persistence scale low end.

### Secondary

- **Ember Red Alt** (#F23E2E): Campaign-specific series (e.g. CHURN node), chart gradient midpoints.

### Neutral (surface — warm cream)

- **Cream Background** (oklch(0.97 0.008 42)): Page canvas, featured-stat cell fill.
- **Cream Surface** (oklch(0.995 0.003 42)): Cards, tables, briefing blocks, chart containers.
- **Cream Subtle** (oklch(0.93 0.012 42)): Table headers, insight boxes, sim panels, bar tracks.
- **Cream Border** (oklch(0.89 0.016 42)): Default 1px borders, deck section dividers.
- **Cream Border Medium** (oklch(0.82 0.022 42)): Stronger dividers, toggle borders, insight box borders, form focus context.

### Neutral (ink)

- **Ink Primary** (oklch(0.17 0.016 42)): Body text, briefing strong text, table row labels.
- **Ink Secondary** (oklch(0.38 0.016 42)): Chart titles, table headers, briefing list text, toggle default text.
- **Ink Muted** (oklch(0.56 0.010 42)): Dates, chart captions, supporting copy, sim labels.

### Header / inverted surfaces

- **Header Background** (oklch(0.13 0.022 38)): CTA banner fill; full-width header band in dashboard layout variant. A warm near-black (hue 38), consistent with the ember grading.
- **Header Text** (oklch(0.98 0.003 42)): CTA banner prose; dashboard layout H1; reversed text on the gradient hero.
- **Header Muted** (oklch(0.68 0.012 38)): Dashboard layout lead subtitle only.

### Brand-locked

- **Brand Navy** (#162632): The Card Avantaj wordmark color, from the official logo files. It appears only inside the logo asset; **never recolor the wordmark** and do not adopt this cool navy as a UI ink (the system's inks are the warm cream-tinted tones above). It is documented here only so it is not mistaken for a stray color. The color logo's orange sits happily on cream; the mono-dark (navy) logo is the fallback for low-contrast light backgrounds.

### Semantic

- **Semantic Green** (oklch(0.40 0.12 148)): Positive or ambassador-tier signals, chart comparison series.
- **Semantic Green Background** (oklch(0.95 0.05 148 / 0.50)): `.insight-box.ok` positive tint; border `oklch(0.82 0.04 148)`.
- **Semantic Red Background** (oklch(0.97 0.05 25 / 0.50)): Active clash row highlight (dashboard variant).

### Chart series

- **Chart Muted** (#7a6860): Default axis ticks, legend labels, chart subtitle tone.
- **Chart Grid** (rgba(160, 110, 90, 0.07)): Chart.js grid lines.
- **Chart Total / baseline** (#5C3D2E): Warm ink brown for totals or context lines.
- **Chart Primary** (#F21D2F): Betting, Revolut, or primary comparison series.
- **Chart Other** (#1F6B52): Comparison cohort (non-revolving, other category).
- **Chart Secondary** (#4A3D8F): Interest / secondary metric; sim opt stack `.int` segment.
- **Chart fills:** `rgba(242, 29, 47, 0.10)`, `rgba(31, 107, 82, 0.10)`, `rgba(74, 61, 143, 0.10)` for area under lines.

### Scenario (pricing simulation)

- **Pessimistic** (#8B9299 / fill rgba(139, 146, 153, 0.18))
- **Moderate** (#3B6FB6 / fill rgba(59, 111, 182, 0.18))
- **Optimistic** (#2D8B57 / fill rgba(45, 139, 87, 0.18))

### Gradient (the one signature)

The ember analog of the Card Avantaj brand gradient. Use it for exactly one moment per composition (§5 and §7 Gradient hero), never on text.

```css
--gradient-ember: linear-gradient(135deg, #F21D2F 0%, #F25D27 52%, #F2811D 100%);
/* softer variant for large fields that carry dark text: extend into peach */
--gradient-ember-soft: linear-gradient(135deg, #F25D27 0%, #F2811D 60%, #F2BC8D 100%);
```

The diamond-ribbon shape device is built from three overlapping ember-tuned panels (mirroring how the real logo symbol is constructed from three gradient panels):

```css
--grad-device-1: linear-gradient(135deg, #F21D2F 0%, #F2811D 100%);            /* red -> orange, opaque */
--grad-device-2: linear-gradient(135deg, rgba(242,129,29,0) 0%, #F2811D 100%); /* orange, transparent -> opaque */
--grad-device-3: linear-gradient(135deg, rgba(242,29,47,0) 0%, #F21D2F 100%);  /* red, transparent -> opaque */
```

### CSS variable map (canonical `:root` block)

Use these exact names in new HTML files:

```css
:root {
  --red: #F21D2F;
  --red-alt: #F23E2E;
  --orange-red: #F25D27;
  --orange: #F2811D;
  --peach: #F2BC8D;
  --bg: oklch(0.97 0.008 42);
  --surface: oklch(0.995 0.003 42);
  --subtle: oklch(0.93 0.012 42);
  --border: oklch(0.89 0.016 42);
  --border-med: oklch(0.82 0.022 42);
  --ink: oklch(0.17 0.016 42);
  --ink-2: oklch(0.38 0.016 42);
  --ink-muted: oklch(0.56 0.010 42);
  --green: oklch(0.40 0.12 148);
  --green-bg: oklch(0.95 0.05 148 / 0.50);
  --red-bg: oklch(0.97 0.05 25 / 0.50);
  --chart-muted: #7a6860;
  --chart-grid: rgba(160, 110, 90, 0.07);
  --gradient-ember: linear-gradient(135deg, #F21D2F 0%, #F25D27 52%, #F2811D 100%);
  --page-pad: clamp(24px, 4vw, 56px);
  --page-max: min(96vw, 1680px);
}
```

### Named Rules

**The Tinted Neutral Rule.** Never use `#000`, `#fff`, or untinted `#f8f9fa`. All neutrals carry warm hue ~42 at low chroma. (The single exception is the logo asset itself, which ships with its own specified white and navy; never restyle the asset.)

**The Ember Spectrum Rule.** Campaign nodes and single-brand ramps use the five-step ember sequence: red, red-alt, orange-red, orange, peach. For dual- or multi-series comparison charts on cream, use the chart-series palette (warm ink, ember red, semantic green, muted violet) so lines remain distinguishable at a glance.

**The One-Gradient Rule.** The ember gradient is a signature moment, not a texture. One gradient surface or shape device per composition (see §5). Never apply the gradient to text.

**The No Side-Stripe Rule.** Callouts and insight boxes use full borders and background tints. Never use `border-left` greater than 1px as a colored accent.

**The Logo-Color Discipline Rule.** The logo is a fixed asset. Do not recolor the orange gradient or the navy wordmark, and do not sample Brand Navy into the UI ink scale. When the logo needs to sit on a dark or gradient surface, switch to the mono-white logo file, don't restyle the color one.

## 3. Typography

**Display / Headline Font:** Georgia, Times New Roman (serif)
**Body / Data / Label / UI Font:** Geologica (variable), Arial fallback, then Segoe UI / system-ui

**Character:** Editorial serif headlines on real-brand operational chrome. Georgia carries narrative weight at page and section scale; Geologica, the actual Card Avantaj typeface and the font of the logo wordmark, keeps tables, charts, toggles, captions, and body crisp and unmistakably on-brand. This is the fusion's defining type decision: the Current system's serif/sans editorial contrast is preserved, but the sans half is upgraded from generic Segoe UI to the real brand face.

### Loading Geologica

Geologica is a variable font shipped in `assets/fonts/Geologica-Variable.ttf`. Load it with `@font-face` and always pin the Cursive axis off.

```css
@font-face {
  font-family: "Geologica";
  src: url("assets/fonts/Geologica-Variable.ttf") format("truetype-variations");
  font-weight: 100 900;
  font-display: swap;
}
body, h3, h4, p, span, td, th, button, label, input, select {
  font-family: "Geologica", Arial, "Segoe UI", system-ui, sans-serif;
  font-variation-settings: "CRSV" 0; /* mandatory: non-cursive lowercase "a" */
}
h1.display, h1, h2, .section-title {
  font-family: Georgia, "Times New Roman", serif;
}
```

For a **single-file shareable artifact**, base64-embed the TTF in the `@font-face` `src` (`url("data:font/ttf;base64,...")`). The variable TTF is ~340 KB (~460 KB base64); embed it once. Georgia needs no import (system font). See §9.

### Hierarchy

- **Display** (Georgia, 400, clamp(2.6rem, 5.5vw, 4.2rem), 1.08): Page title in `.page-header h1` only.
- **Headline / Section Title** (Georgia, 400, clamp(1.65rem, 3vw, 2.35rem), 1.15): `.section-title` below section eyebrow labels; max-width ~30ch (34ch in featured sections).
- **Chart Title** (Geologica, 600–700, 0.8rem): Above chart canvases inside card surfaces. Sentence case.
- **Title** (Geologica, 600, 0.9375rem): Briefing block `h3`, sim opt card titles.
- **Body** (Geologica, 400, 15px, 1.5): Default prose, briefing lists, chart captions (0.72rem for captions).
- **Eyebrow label** (Geologica, 700, 0.67rem, 0.12em tracking, uppercase): Section labels, table headers, sim labels, featured stat labels. The one sanctioned uppercase use.
- **Metric** (Geologica, 700, 1.05–1.75rem, tabular-nums): `.featured-stat .n`, `.opt-headline`, overview table nums.

### Named Rules

**The Two-Family Rule.** Georgia serif for page title and section titles; Geologica for everything else. Do not collapse into one family (loses the editorial contrast), and do not introduce a third face.

**The Non-Cursive-Axis Rule.** `font-variation-settings: "CRSV" 0` is mandatory wherever Geologica loads. The cursive single-story "a" is off-brand; the Brand Guidelines flag the cursive default as misuse.

**The Sentence-Case Rule (with one exception).** All copy, titles, chart labels, table headers, and buttons are sentence case. The single exception is the eyebrow section label (`.section-label`), which stays uppercase + tracked as wayfinding. Do not extend uppercase anywhere else, and never all-caps the brand name.

**The One Eyebrow Rule.** A single uppercase tracked label per deck section. Do not repeat kicker grammar on every card.

**The Tabular Numbers Rule.** All counts, percentages, and currency amounts use `font-variant-numeric: tabular-nums` via a `.tabular` class or inline.

**The Serif Headline Rule.** Page title and section titles use Georgia. Card interiors, tables, and charts stay Geologica.

**The Caption Floor.** Geologica captions read small; floor them at ~12–13px for legibility even where a proportional rule would go smaller.

## 4. Logo

The real Card Avantaj logo package ships in `assets/logo/`. These rules come from the official 2025 Card Avantaj Brand Guidelines and apply as-is; they are not per-deck style choices.

**Symbol.** A folded ribbon/diamond shape suggesting an "N" (for Nexent) in motion, in a diagonal gradient from Deep Orange (#FE5000) to Blossom Orange (#FF8B1D), fading toward transparent at the trailing edge. It is built from three overlapping gradient panels (the shape device in §5 mirrors this construction in ember tones).

**Wordmark.** "card avantaj", always two lines, always lowercase. This is the one approved lowercase use; everywhere else in copy it is "Card Avantaj" (see PRODUCT.md Writing rules). Set in Geologica ExtraBold, color Brand Navy (#162632). Never recolor the wordmark.

**Lockup.** Symbol + wordmark + "by nexent bank" beneath. Use the lockup only when the Nexent Bank link must be explicit: website footers, legal/regulatory copy, co-branded materials. Default to the plain logo (symbol + wordmark) everywhere else.

**Polychromatic vs. monochromatic.**
- **Color logo** (`card-avantaj-logo-color.png`) is primary. Its warm orange harmonizes with the cream canvas: use it in the page header and anywhere the cream/surface background gives it contrast.
- **Mono dark / navy** (`card-avantaj-logo-mono-dark.png`): fallback for low-contrast light backgrounds where the orange would wash out.
- **Mono white** (`card-avantaj-logo-mono-white.png`): for dark surfaces, the CTA banner (`header-bg`), and the ember gradient hero.
- **Symbol only, color** (`card-avantaj-symbol-color.png`): favicon, compact nav mark, and the source silhouette for the §5 shape device.

**Placement in the scroll-deck.** Put the plain color logo in the `.page-header`, top-left or aligned to the page title, at 110–160px wide. On the CTA banner and gradient hero, use the mono-white logo. Anchor the logo to the page margin; do not float it at an arbitrary offset.

**Safezone.** Clearspace on every side must be at least the width of the "C" in the wordmark. No graphic, text, chart, or page edge may enter that zone.

**Minimum sizes.**

| Version | Print | Digital |
|---|---|---|
| Full lockup (with "by nexent bank") | 45 mm | 160 px |
| Logo (symbol + wordmark) | 25 mm | 110 px |
| Symbol alone | 10 mm | 30 px |

**Co-branding.** Alongside another logo (partner, retailer, Mastercard/Visa), the gap must be at least the width of the symbol. Scale logos proportionally and align through the vertical center.

### Named Rules

**The Lowercase Exception Rule.** Lowercase "card avantaj" exists only inside the logotype graphic. Every other mention uses "Card Avantaj".

**The No-New-Lockups Rule.** Never rearrange, re-pair, or invent a lockup. Use only the approved files in `assets/logo/`.

### Don't (Brand Manual logo misuse)

- Don't rearrange the symbol and wordmark
- Don't rotate the symbol or the logo
- Don't create new lockups
- Don't place the color logo on a clashing background
- Don't recolor the wordmark
- Don't distort or warp the logo
- Don't outline the logo
- Don't add effects (drop shadow, glow, bevel)

## 5. Graphic Elements (the signature moment)

Card Avantaj's abstract graphic language expands and distorts the logo symbol. In the fusion it is re-tuned to the ember palette and used sparingly, one device per composition.

**Shape / gradient device.** A large, soft-edged "blob" or ribbon form, an abstracted fragment of the diamond-ribbon symbol, filled with `--gradient-ember` (or the three `--grad-device-*` panels for a construction faithful to the logo). Use it as a hero background, a section divider, or a mask bleeding in from one edge behind reversed white headline text. Start from the symbol silhouette in `assets/logo/card-avantaj-symbol-color.png` rather than inventing an arbitrary blob.

**Line / gradient device.** A thin diagonal ember gradient stroke or a single wave-line echoing the symbol's curve, laid over a solid gradient field. A restrained secondary accent, never the sole device on a page.

### Named Rules

**The One-Blob Rule.** One gradient blob or line device per composition. It is a signature accent, not a repeating pattern; do not tile or scatter.

**The Symbol-Derived Rule.** New shapes should read as fragments or expansions of the actual logo symbol's curve, not arbitrary blobs.

**The Ember-Not-Orange Rule.** The shape device uses the ember gradient (`#F21D2F → #F2811D`), which meets the logo's orange at its warm end so the two read as one family. Do not reproduce the experimental Cherry Red → Blossom Orange grading here; that is the palette the fusion deliberately moved away from.

## 6. Elevation

Hybrid: mostly flat cream surfaces with light ambient shadow on card surfaces and toolbar buttons. Featured deck sections use surface background bands, not heavier shadow. CTA banners use flat charcoal fill; the gradient hero uses flat gradient fill.

### Shadow Vocabulary

- **Card surface** (`0 1px 3px oklch(0.13 0.022 38 / 0.05)`): `.card-surface` default lift.
- **Toolbar button** (`0 2px 8px oklch(0.13 0.022 38 / 0.08)`): Fixed deck toolbar.
- **Overview cell active** (`0 0 0 2px var(--orange-red), 0 4px 16px oklch(0.13 0.022 38 / 0.22)`): Interactive table cell emphasis.
- **Focus ring** (`0 0 0 3px oklch(0.97 0.05 25 / 0.30)`): Form select and sim input focus.
- **Dashboard card ambient** (`0 1px 3px oklch(0.13 0.022 38 / 0.06), 0 6px 20px oklch(0.13 0.022 38 / 0.04)`): Alternate layout only.

### Named Rules

**The Flat Interior Rule.** Card interiors, offer items, matrix cells, and featured-stat cells stay flat. Shadow belongs on outer containers only.

**The Unshadowed Logo Rule.** Never add a shadow, glow, or bevel to the logo, even where surrounding cards carry ambient shadow (Brand Manual misuse).

## 7. Components

### Scroll-deck shell (canonical)

- **Page wrap:** `max-width: var(--page-max); margin: 0 auto; padding: 0 var(--page-pad)`.
- **Page header:** Cream background, bottom border 1px cream-border, generous vertical padding `clamp(56px, 8vh, 96px)` top. Color logo top-left (110–160px). H1 in Georgia display; dates line 0.78rem ink-muted with tabular nums.
- **Deck section:** Vertical padding `clamp(40px, 6vh, 72px)`, bottom border 1px, `scroll-margin-top: 16px`.
- **Deck section featured:** Surface background, negative horizontal margin to full bleed within page pad, top+bottom borders.
- **Deck nav:** Fixed right column of 8px dots; inactive `border-med`, active/hover ember red; hidden below 992px. Dots carry `title` attributes.
- **Deck toolbar:** Fixed top-left; toggle buttons with surface fill, border-med, active state ember-red fill; `aria-pressed`.

### Section header

- **Eyebrow label:** `.section-label` in ember-red, uppercase, 0.67rem, 0.12em tracking, Geologica 700, margin-bottom 10px. The one sanctioned all-caps use.
- **Title:** `.section-title` in Georgia headline scale, sentence case, margin-bottom 28px.

### Gradient hero (signature)

Use once, typically as the opening or closing full-bleed band.

- **Background:** `var(--gradient-ember)`, full bleed (negative margin to page edges), radius 0.
- **Logo:** mono-white, top-left.
- **Text:** Header Text (near-white); title in Georgia display, reversed. Optional pill CTA: cream-surface fill, `rounded.pill`, ink-primary or ember-red text.
- **Shape device (optional):** one ribbon fragment bleeding from a corner, per §5, behind the headline. Not in addition to another blob elsewhere on the page.
- Respect `prefers-reduced-motion` for any entrance/parallax on the hero.

### Card surface

- **Corner Style:** 10px radius (`rounded.lg`).
- **Background:** Cream Surface. **Border:** 1px cream-border. **Shadow:** Card surface ambient.
- **Padding:** 22px 26px; chart canvases inside use `.chart-box` heights (300–520px).

### CTA banner

- **Shape:** 10px radius, charcoal (`header-bg`) background, 28px 32px padding.
- **Typography:** Georgia clamp(1.15rem, 2.2vw, 1.45rem); `<strong>` in ember-orange (`#F2811D`).
- **Logo:** mono-white, if the banner carries brand sign-off.

### Featured stats grid

- **Layout:** 4-column grid (3-column variant `.cols-3`), 2px gap with border color as grid lines, 10px outer radius.
- **Cells:** Cream-bg fill; value `.n` at 1.75rem 700 orange-red (Geologica, tabular); label `.l` uppercase 0.68rem ink-muted.

### Insight box

- **Style:** Full 1px cream-border-med, cream-subtle background, 10px radius, 20px 24px padding.
- **Heading:** Eyebrow-label style 0.72rem ink-secondary.
- **Body:** 0.9rem ink-muted, max-width ~90ch. **OK variant:** Green-bg fill, green-tinted border.

### Toggle buttons (year filter, toolbar, sim)

- **Default:** Surface background, 1px border-med, 8px radius, 0.78rem 600 ink-secondary (Geologica), sentence case.
- **Hover:** Border shifts to ink-muted. **Active:** Ember-red fill and border, surface text color.
- A pill-shaped variant (`rounded.pill`, ember-red active fill) echoes the Card Avantaj "Apply now" button and is fine for brand-forward surfaces (hero CTA).

### Overview table

- **Wrap:** Surface background, 1px border, 10px radius, overflow hidden.
- **Header:** Subtle background, uppercase 0.65rem ink-secondary, right-aligned year columns.
- **Interactive nums:** Hover/active fills orange-red with surface text; sibling nums dim to 0.38 opacity during hover (`num-hovering`).

### Data tables

- **Header:** Sticky in `.data-table-wrap`, cream-subtle, uppercase 0.68rem.
- **Readable variant:** Zebra odd rows `oklch(0.985 0.004 42)`; year separator 2px border-med top.
- **Cells:** 10–12px padding; `.num` right-aligned tabular.

### Session briefing

- **Grid:** Two-column briefing blocks with surface cards, Georgia `h3` at 1.15rem.
- **Lists:** 0.9rem ink-secondary (Geologica) with strong ink-primary emphasis.

### Simulation panel (Part 4 pattern)

- **Sim panel:** Subtle background, bordered, 24px 28px padding.
- **Controls:** Uppercase sim-labels; form-select and sim-days-input with ember focus ring.
- **Heatmap:** Subtle header row; cell hover/active `inset 0 0 0 2px var(--red)`.
- **Opt ladder cards:** Surface cards, active state red border + 1px outline; opt headline 1.5rem orange-red; stack bar orange + violet.

### Charts (Chart.js)

- **Defaults object:** Geologica 11–13px; muted tick color; grid from `--chart-grid`; tooltip `rgba(28, 14, 12, 0.94)`, 8px corner radius, 14px padding. Set `Chart.defaults.font.family = "Geologica, Arial, sans-serif"`.
- **Registry + Y-axis toggle:** Charts register in `chartRegistry`; toolbar forces `min: 0` on Y axes when active.
- **Multi-series:** Use the chart-series palette; pair `borderDash` on secondary series when needed. Sentence-case axis and legend labels.

### Dashboard layout (alternate)

Use only when brief targets campaign-clash style:

- **Header band:** Full-width charcoal, centered, mono-white logo, ember eyebrow, 1.75rem Geologica H1 (or Georgia title for editorial weight), 3px ember bottom rule.
- **Cards:** 8px radius, heavier ambient shadow, featured 3px top rule.
- **Campaign chips, offer rows, clash matrix:** As documented in the Campaign Clash reference.

## 8. Photography (optional)

When a deck uses photography (hero or divider), follow the Card Avantaj direction: authentic, candid, multigenerational moments of people using the card in everyday life, warm golden-hour natural light, genuine not posed.

**Signature overlay technique.** An ember-gradient blob bleeds in from a bottom or side edge of the photo, masking part of it; headline and CTA sit in reversed white type inside that gradient area. This is the fusion's "text over photo" solution, not a flat dark scrim. It counts as the page's one gradient moment (do not also place a separate hero band). Logo: mono-white over the gradient area, color logo over a clear light region.

## 9. Asset Index

`assets/` is a lean, portable subset. Copy it alongside `PRODUCT.md` and this file and every HTML/PPT deliverable below works with zero other dependencies.

### Portable set (`assets/` — use this for HTML/web decks)

| Need | File |
|---|---|
| Logo, default (no lockup), color | `assets/logo/card-avantaj-logo-color.png` |
| Logo, with "by nexent bank" lockup (footers, legal copy) | `assets/logo/card-avantaj-logo-color-lockup.png` |
| Logo, monochrome dark navy (low-contrast light backgrounds) | `assets/logo/card-avantaj-logo-mono-dark.png` |
| Logo, monochrome white (dark surfaces, CTA banner, gradient hero, photography) | `assets/logo/card-avantaj-logo-mono-white.png` |
| Symbol only, full color (favicon, compact nav, shape-device source) | `assets/logo/card-avantaj-symbol-color.png` |
| Variable font, full axis control | `assets/fonts/Geologica-Variable.ttf` — always set `"CRSV" 0` |

Reference with paths relative to wherever `assets/` sits alongside the HTML file:

```html
<img src="assets/logo/card-avantaj-logo-color.png" alt="Card Avantaj" style="width:140px">
```

For a **single-file shareable deliverable**, base64-embed the Geologica TTF and the one or two logo PNGs actually used, so the file travels as one attachment (the Current system's "shareable as-is" principle). Keep `assets/` in this design-rules folder as the source of truth.

### Named Rules

**The Reuse-Before-Recreate Rule.** Use the shipped logo and font files rather than redrawing the logo or approximating the wordmark in CSS/SVG; a hand-rebuilt logo drifts off-model and violates the misuse rules.

**The Portable-Set Rule.** If a new asset becomes essential to HTML deliverables, add it to `assets/` so the folder stays self-sufficient.

## 10. Do's and Don'ts

### Do

- **Do** default to the scroll-deck pattern from `Quasi-Cash Betting.html` for new long-form analyses.
- **Do** use Georgia for page title and section titles; Geologica for everything else, with `"CRSV" 0`.
- **Do** copy the canonical `:root` CSS variable block verbatim; the ember/cream grading is retained from the Current system.
- **Do** place the real Card Avantaj logo in the page header (color on cream, mono-white on dark/gradient).
- **Do** use exactly one ember gradient moment per composition (hero band or one shape device).
- **Do** keep the gradient in ember tones (`#F21D2F → #F2811D`).
- **Do** use 10px radius on card-surface, insight-box, table wraps, and CTA banners.
- **Do** keep copy sentence case; reserve uppercase for the single eyebrow label per section.
- **Do** write the brand name as "Card Avantaj" (capital C and A, with the space) everywhere except the logo graphic.
- **Do** keep HTML self-contained: Bootstrap 5.3 + Chart.js 4.4 + vanilla JS, with Geologica and logo embedded or linked from `assets/`.
- **Do** apply `font-variant-numeric: tabular-nums` on every numeric metric.
- **Do** include deck-dot navigation for analyses with more than 4 sections.

### Don't

- **Don't** default to the dark centered header band unless the brief is explicitly dashboard/campaign-clash.
- **Don't** collapse to one type family (loses the editorial contrast) or introduce a third face.
- **Don't** ship Geologica with the cursive axis on; `"CRSV" 0` is mandatory.
- **Don't** recolor, rotate, distort, outline, shadow, or re-lockup the logo; don't sample Brand Navy into the UI ink scale.
- **Don't** use navy gradient headers, gold amber accents, or Bootstrap primary blue.
- **Don't** use side-stripe borders on insight boxes, cards, or list items.
- **Don't** apply the gradient to text, or use more than one gradient/shape device per composition.
- **Don't** use pure `#000` or `#fff`, or switch to cool white/blue-grey neutrals; the fusion keeps warm cream.
- **Don't** use ALL CAPS for titles, buttons, or chart labels; sentence case, with the eyebrow label the only exception.
- **Don't** use gradient text, glassmorphism, or hero-metric SaaS templates.
- **Don't** use identical icon-plus-heading card grids for unrelated content blocks.

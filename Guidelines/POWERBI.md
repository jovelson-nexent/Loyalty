# Nexent Fusion Style → Power BI: Directions for the Developer

**Source:** Adapted from `PRODUCT.md` and `DESIGN.md` (the Nexent Fusion Style system: the ember briefing-room showcasing style, grounded in the real Card Avantaj logo and Geologica type). Canonical reference deck: `Quasi-Cash Betting.html`.

**Goal:** Power BI reports for CEB Card Avantaj stakeholders should feel like the same "ember briefing room" the HTML decks do: warm cream canvas, ember-red emphasis, editorial serif headlines over clean data chrome, the real Card Avantaj logo anchoring the report, even though Power BI is a different medium (fixed-canvas report pages, not a scrolling HTML deck). This doc translates every rule into its nearest Power BI equivalent and flags what doesn't carry over.

**What the fusion changes vs. the Current Power BI guidance:**

- **Color grading is unchanged.** The ember-on-cream palette is retained exactly, so the same Nexent ember theme JSON still applies (see below). No re-theming needed.
- **Body font moves from Segoe UI to the brand font.** Geologica if IT can deploy it to stakeholder machines; otherwise **Arial**, the Card Avantaj brand's own sanctioned web-safe fallback (not Segoe UI). See §4, this is the single most important thing to get right and confirm up front.
- **The real Card Avantaj logo goes on the report.** Insert the logo PNGs from `assets/logo/` as Image visuals; follow the placement, mono-vs-color, safezone, and minimum-size rules in DESIGN.md §4. See §8 below.

> **Theme JSON.** The palette is identical to the Current Nexent system, so any existing `Nexent-PowerBI-Theme.json` (ember colors pre-mapped) imports and works as-is via **View → Themes → Browse for themes**. If you don't have that file, it can be generated on request from DESIGN.md §2, the color values below are the source. Everything in this doc is what to do *on top of* that theme.

---

## 1. What carries over directly

| Fusion concept | Power BI equivalent |
|---|---|
| Cream canvas, ink text, ember accents | **Report theme JSON** (ember palette) — sets background, foreground, dataColors |
| Ember red for emphasis / active states | `dataColors[0]` and accent color in the theme; also KPI conditional formatting |
| Cream Surface cards, 1px cream border | Visual containers with **Visual border** on + rounded corners + subtle shadow (§3) |
| Insight boxes (cream-subtle bg) | Text boxes / Card visuals with `--subtle` fill, no left border |
| Tabular numerals on every metric | Power BI number formatting is already aligned in tables/matrices, keep **Display units** and **decimal places** consistent per metric across visuals |
| Uppercase tracked eyebrow labels | Static text boxes above each visual group: the **one** sanctioned uppercase use; bold, uppercase, letter-spacing where the text box allows. Nothing else goes all-caps |
| Chart series palette (red / green / violet / warm ink) | `dataColors` array in the theme, in that order |
| Real Card Avantaj logo | **Image visual** on each page header (§8) |
| No pure black/white, no navy/gold, no gradient text | Theme enforces this, do not override with default Power BI blue on individual visuals |

## 2. What doesn't carry over (and the workaround)

- **Scroll-deck with fixed dot navigation** doesn't exist in Power BI. Use **Page navigation** (a horizontal tab strip at the top, or a left-hand vertical bookmark/buttons nav pane) styled with ember-red active state and cream-border-medium inactive state. Each "deck section" becomes its own **report page**.
- **Georgia serif for headlines** — Power BI text visuals can only use fonts installed on the *viewer's* machine (no font embedding). Georgia is bundled with Windows/Office, so it renders for essentially all corporate users, but confirm with IT. If Georgia is ever unavailable, the safe fallback is **Times New Roman**, not a sans, keep the serif/sans contrast.
- **Geologica for body** — same machine-install constraint, but Geologica is *not* a bundled Windows font. See §4 for the deployment-vs-Arial decision; this is the fusion's key Power BI risk.
- **The ember gradient signature** — Power BI has no clean native gradient fill for shapes. Approximate with **one** exported ember-gradient banner image placed as a hero strip on the landing/summary page only (matches the One-Gradient rule, one per report page at most). Don't tile it, don't fake it with default Power BI gradient presets on every visual.
- **`prefers-reduced-motion`** — no Power BI equivalent. Skip; note it doesn't apply.
- **Full self-contained single file** — not applicable; a `.pbix`/published report is the deliverable. Drop the "no build step" requirement.
- **Bootstrap / Chart.js / vanilla JS** — not applicable; use native Power BI visuals (§5).

## 3. Report canvas & visual containers

- **Page background:** cream, not white — set via the theme (`--bg` equivalent) and again per-page under **Format page → Canvas background** if a page needs a manual override.
- **Visual containers ("cards"):**
  - Fill: cream-surface (near-white, warm-tinted).
  - Border: on, 1px, cream-border color.
  - Corner radius: **10px** on every visual (Format → General → Effects → Visual border → Rounded corners = 10). This single detail most signals "on-brand" vs. "default Power BI."
  - Shadow: keep it light — Power BI's default drop-shadow preset is close enough; no heavy shadow, no glow.
- **Padding:** generous internal breathing room; increase table/matrix row height and column padding rather than letting text touch borders.
- **Never** apply a colored left-border accent to a card or text box (the "no side-stripe" rule; a common Power BI habit via the border color picker that must be avoided).

## 4. Typography (the fusion's key Power BI decision)

Two families, matching the HTML system: Georgia serif titles + the brand sans for everything else.

- **Page/section titles:** Georgia, regular weight (not bold), large (~28–32pt on a 16:9 page), ink-primary color, tight letter-spacing. Bundled with Windows/Office; fallback Times New Roman.
- **Body / labels / table & chart text / KPI numbers:** **Geologica** if available, else **Arial**.
  - **Geologica is not a default Windows font.** To use it in Power BI, IT must deploy the Geologica family (from `assets/fonts/`, or the full static-weight set) to every stakeholder machine. If that's in place, set body/label/KPI text to Geologica everywhere.
  - **If Geologica can't be deployed, fall back to Arial** — the Card Avantaj Brand Guidelines' own explicit web-safe fallback. Do **not** fall back to Segoe UI; Arial is the brand-sanctioned substitute and closer to Geologica's proportions.
  - Whichever you land on, use it consistently across the whole report; don't mix Geologica on some pages and Arial on others.
- **KPI / big numbers:** brand sans, bold, ember orange-red, sized up (20–28pt) — the equivalent of the HTML "featured-stat-value."
- **Case:** sentence case everywhere except the eyebrow labels (§1). Do not all-caps titles, buttons, or axis labels.
- Do **not** let the whole report default to sans-only. Every page needs at least one Georgia moment (the page title) to keep the editorial contrast.

## 5. Chart & visual mapping

| HTML/Chart.js pattern | Power BI visual |
|---|---|
| Line/area charts (trend over time) | Native **Line/Area chart**, colors from theme `dataColors` |
| Bar comparisons (cohort vs. cohort) | Native **Clustered/Stacked bar chart** |
| Featured stats grid (4-up cream cells, big orange-red numbers) | **Multi-row card**, or 4 **Card** visuals in a grid, formatted per §3–4 |
| Overview table with orange-red hover highlight | **Table**/**Matrix** with **conditional formatting** (background/font color). True hover isn't native, approximate with a selected-state bookmark or slicer-driven highlight |
| Insight box (narrative callout) | Text box, cream-subtle fill, no border-left, rounded corners (or a rounded rectangle shape behind the text) |
| CTA banner (dark charcoal, ember-orange strong text) | Text box with `header-bg` (warm charcoal) fill, header-text body copy, ember-orange emphasis phrase, mono-white logo alongside |
| Gradient hero (one per report) | One exported ember-gradient **image** as a page-top banner on the summary page, mono-white logo + reversed white title over it |
| Toggle buttons (active = ember-red) | **Slicer** (button style): selected item ember-red fill + cream text; unselected cream-surface + border-med outline |
| Scenario ladder (pessimistic/moderate/optimistic) | Small multiples or a 3-series chart using the exact scenario colors (`#8B9299` / `#3B6FB6` / `#2D8B57`), don't substitute the default palette |

## 6. Color reference (for anything the theme JSON doesn't auto-apply)

Identical to the Current/HTML system, ember grading retained.

**Ember (primary/accent):**
- Ember Red `#F21D2F` — eyebrow labels, active toggle/slicer, primary emphasis, gradient start
- Ember Red Alt `#F23E2E` — secondary series
- Ember Orange-Red `#F25D27` — KPI/featured numbers, active table cell, gradient midpoint
- Ember Orange `#F2811D` — CTA emphasis text, gradient end
- Ember Peach `#F2BC8D` — low-intensity series / tier-5 shading

**Cream (neutrals — warm, never pure white or cool grey):**
- Background `oklch(0.97 0.008 42)` · Surface `oklch(0.995 0.003 42)` · Subtle `oklch(0.93 0.012 42)` · Border `oklch(0.89 0.016 42)` · Border Medium `oklch(0.82 0.022 42)`

**Ink (text):**
- Primary `oklch(0.17 0.016 42)` · Secondary `oklch(0.38 0.016 42)` · Muted `oklch(0.56 0.010 42)`

**Header/inverted (CTA banners, hero):**
- Header background `oklch(0.13 0.022 38)` · Header text `oklch(0.98 0.003 42)`

**Semantic:** Positive `oklch(0.40 0.12 148)` on green-tint `oklch(0.95 0.05 148 / 0.50)`; negative background `oklch(0.97 0.05 25 / 0.50)`

**Chart series (priority order):** `#F21D2F` → `#1F6B52` → `#4A3D8F` → `#5C3D2E` → axis/legend `#7a6860`

**Scenario:** Pessimistic `#8B9299` · Moderate `#3B6FB6` · Optimistic `#2D8B57`

> OKLCH values won't paste into Power BI's picker (hex/RGB only). Converted hex equivalents are baked into the theme JSON, use those directly. The logo's own colors (Deep Orange `#FE5000` / Blossom Orange `#FF8B1D` gradient, Brand Navy `#162632` wordmark) live only inside the logo image, don't add them to the report palette.

## 7. Do's and Don'ts (Power BI–specific)

**Do:**
- Import the ember theme JSON first, before any manual formatting.
- Apply 10px rounded corners to every visual container, table, and card.
- Use Georgia for page titles; the brand sans (Geologica, else Arial) everywhere else.
- Place the real Card Avantaj logo on every page header (§8).
- Keep number formatting consistent (decimals, separators, % vs. raw) across every visual showing the same metric.
- Use ember-red as the *only* "active/selected" color across slicers, buttons, and bookmarked nav states.
- Keep to one ember-gradient image per report page, at most.

**Don't:**
- Don't leave any visual on Power BI's default blue/gray theme, that reintroduces the corporate look the brand rejects.
- Don't use a colored left border on any card, text box, or table row.
- Don't use pure white (`#FFFFFF`) or pure black (`#000000`); use the cream/ink equivalents.
- Don't fall back to Segoe UI for body, use Arial if Geologica isn't deployed.
- Don't recolor, rotate, distort, or add effects to the logo (see §8 and DESIGN.md §4).
- Don't apply drop shadows, glow, or gradient fills to visual backgrounds; don't all-caps titles or axis labels.
- Don't let report pages default to sans-only, every page needs a Georgia title.

## 8. Logo on the report

Insert the logo as an **Image visual** (Insert → Image) from `assets/logo/`.

- **Page headers on cream:** `card-avantaj-logo-color.png` (the color logo's warm orange harmonizes with cream). If a page header is a very pale/low-contrast wash, use `card-avantaj-logo-mono-dark.png` (navy).
- **Dark CTA/charcoal bands and the gradient hero image:** `card-avantaj-logo-mono-white.png`.
- **Favicon / compact nav mark:** `card-avantaj-symbol-color.png`.
- **Lockup** (`...-lockup.png`, "by nexent bank"): only where the Nexent Bank relationship must be explicit (report footer, legal/regulatory pages). Default to the plain logo.
- **Size & clearspace:** keep the logo ≥110px wide (≥160px for the lockup); leave clearspace of at least the "C" width on all sides; anchor to the page margin. Never stretch to fit, lock the aspect ratio.
- **Never** recolor, rotate, outline, or add a shadow/glow to the image (Brand Manual misuse rules).

---

**Open questions for the dev to confirm before build:**
1. **Font deployment:** can IT install the Geologica family on all stakeholder machines? If not, we build on the Arial fallback, confirm which before starting so the report doesn't get re-fonted late.
2. Is Georgia confirmed available on all stakeholder machines (Windows + Office), or should we test the Times New Roman fallback up front?
3. Multi-page navigation: tab strip across the top, or a left rail of buttons? (Determines how we mimic the deck's fixed-dot nav.)
4. Any hover-state interactivity Power BI can't natively replicate (e.g. the sibling-dimming effect on the overview table), flag these to scope a bookmark-based workaround rather than promising native hover.
5. Do we have (or need generated) the `Nexent-PowerBI-Theme.json` ember theme file, and one exported ember-gradient banner image for the summary-page hero?

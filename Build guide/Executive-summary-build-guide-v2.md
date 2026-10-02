# Executive summary: step-by-step build guide (v2)

Card Avantaj loyalty dashboard · Power BI Report Server (January 2025) · Nexent Fusion design system

**What changed in v2**
- Page size is now **1600 × 900 (16:9)**. See section 1.
- Every background, container, title and text block is an **HTML Content** visual. Native visuals are used only where they must be: slicers, the page navigator, the chart and the logo.
- The sidebar uses the native **Page navigator** on top of an HTML sidebar background.
- All HTML visuals now **scale with their size**, so they fit at any zoom level.
- Every size and position is written as **H · W · X · Y** (height, width, horizontal, vertical).

> In Power BI: select the visual → Format → General → Properties → Size (**Height**, **Width**) and Position (**Horizontal**, **Vertical**). Type the numbers exactly.

---

## 1. Page size: use 1600 × 900

**Why the visuals did not fit before.** The page was 1440 × 1080 (4:3). On a widescreen monitor Power BI shrinks a 4:3 page to fit it. Native visuals shrink with the page, but the old HTML visuals kept their text at a fixed size, so it overflowed the smaller boxes.

**What fixes it**
1. **16:9 page (1600 × 900).** Same shape as your monitor and the Report Server browser window, so there are no empty side bands and little shrinking.
2. **Scaling HTML.** Each HTML measure is designed for one visual size (written in its description, e.g. *design size 308 × 1352*). All text and spacing is proportional to the visual's width, so the content always fills the box exactly, at any zoom.

**Set it up**
1. Select the Executive summary page → Format page → **Canvas settings** → Type **Custom** → Height **900**, Width **1600**. Vertical alignment **Top**.
2. **Canvas background**: colour `#FAF3F1`, transparency 0 %. **Wallpaper**: `#FAF3F1`.
3. View → **Page view → Fit to page** (desktop and Report Server). Use **Actual size** only when you want to check pixel detail.

---

## 2. One-time report setup

1. **Theme**: View → Themes → Browse for themes → `Downloads\Guidelines\Nexent-PowerBI-Theme.json`.
2. **HTML visual**: Visualizations → `…` → Get more visuals → **HTML Content (lite)** by Daniel Marsh-Patrick (certified edition).
3. **Pages**: create the empty pages, in this order (page order = navigator order): *Executive summary*, *Insights*, *Financial impact*, *Coins & redemption*, *Challenges*, *Badges*, *Trends*, *Period comparison*, *Tier progression*, *Wizz reconciliation*, *Glossary*, *Data health*. Give every page the 1600 × 900 settings from section 1.
4. **Save the PBIX.** The new measures are only kept after you save.

### Standard settings for every HTML Content visual ("HTML standard")
Apply these each time a step says **HTML standard**:
- Values: the measure named in the step. Granularity: empty.
- Format → General → **Title off**, **Background off**, **Border off**, **Shadow off**. If you see a **Padding** option, set it to **0**.
- Format → General → **Header icons off** (so no icons appear on hover).
- Visual → Content formatting → *Show raw HTML* **off**.

> If a scrollbar appears inside an HTML visual, re-check the H and W values. The content is built to fill the box exactly. If it persists, send me a screenshot.

---

## 3. Build order and layer order

Build in this order. Later items go **on top** of earlier ones.

| # | Object | Type | H · W · X · Y |
|---|---|---|---|
| 1 | Sidebar background | HTML `HTML Sidebar` | 836 · 200 · 0 · 64 |
| 2 | Header band | HTML `HTML Header` | 64 · 1600 · 0 · 0 |
| 3 | Logo | native Image | 46 · 111 · 24 · 9 |
| 4 | Page navigators (×4) | native Page navigator | see section 5 |
| 5 | Slicers (×4) + Reset | native | see section 6 |
| 6 | Page title block | HTML `HTML Exec Title` | 76 · 1352 · 224 · 144 |
| 7 | Key KPIs | HTML `HTML Exec KPIs` | 264 · 1352 · 224 · 228 |
| 8 | Members by tier chart | native clustered column | 372 · 780 · 224 · 512 |
| 9 | Profit around upgrade chart | native line chart | 372 · 552 · 1024 · 512 |
| 10 | Upgrade step slicer | native slicer (dropdown) | 30 · 180 · 1384 · 524 |

```
X: 0      200 224                                                     1576 1600
Y 0   ┌─────────────────────── HEADER (HTML) + logo ─────────────────────────┐
Y 64  ├────────┬──────────────────────────────────────────────────────────────┤
      │SIDEBAR │ slicers (native)                              Y 84  H 48     │
      │ (HTML) │ TITLE BLOCK (HTML)                            Y 144 H 76     │
      │  +     │ KPIs + Time to progress (HTML)                Y 228 H 264    │
      │ page   │ MEMBERS BY TIER (native)     │ PROFIT PER MEMBER (native)    │
      │ nav    │ Y 512 H 372                  │ + step slicer  Y 512 H 372    │
Y 900 └────────┴──────────────────────────────────────────────────────────────┘
```

The content column runs from X 224 to X 1576 (W 1352) with 24 px margins.

---

## 4. Header

### 4.1 Header band
- Add **HTML Content** → Values `_Measures[HTML Header]` → **HTML standard**.
- **H 64 · W 1600 · X 0 · Y 0**.
- Shows "Loyalty program / Performance dashboard", the last available date, a pill with the active period, and the 3 px ember gradient line (the page's one gradient moment).

### 4.2 Logo
- Insert → Image → `Downloads\Guidelines\logo\card-avantaj-logo-mono-white.png`.
- **H 46 · W 111 · X 24 · Y 9**. Image → Scaling **Fit**.
- Background off, border off, shadow off, header icons off.

---

## 5. Sidebar: HTML background + native Page navigator

### 5.1 Sidebar background
- Add **HTML Content** → Values `_Measures[HTML Sidebar]` → **HTML standard**.
- **H 836 · W 200 · X 0 · Y 64**.
- It draws the panel, the right border, the group labels (OVERVIEW, ENGAGEMENT, STRUCTURE, REFERENCE) and a footer ("Data to Aug 2026 · Figures in RON").

### 5.2 Page navigators
Insert → Buttons → **Navigator → Page navigator**. You create **four** navigators, one per group, so the group labels sit between them.

| Navigator | Pages shown | H · W · X · Y |
|---|---|---|
| Overview | Executive summary, Insights, Financial impact | 98 · 176 · 12 · 98 |
| Engagement | Coins & redemption, Challenges, Badges, Trends, Period comparison | 166 · 176 · 12 · 230 |
| Structure | Tier progression, Wizz reconciliation | 64 · 176 · 12 · 430 |
| Reference | Glossary, Data health | 64 · 176 · 12 · 528 |

Each button is 30 high with 4 px between buttons, so a navigator is 34 × pages − 4 high. The group labels in `HTML Sidebar` sit 82 px above each navigator (label tops 16 / 148 / 348 / 446 inside the sidebar).

For each navigator, in the Format pane:

**Pages**
- *Show hidden pages* **off**, *Show tooltip pages* **off**.
- Switch off every page that does not belong to this group (the Pages card lists each page with a toggle).

**Grid layout**
- Orientation **Vertical**. Padding between buttons **4 px**.

**Style** (use *Apply settings to state* to switch between states)

| Setting | Default | On hover | On press | Selected |
|---|---|---|---|---|
| Text: font | Arial 10 | Arial 10 | Arial 10 | Arial 10 **bold** |
| Text: colour | `#4A403C` | `#160D0A` | `#160D0A` | `#FAF8F7` |
| Text: alignment | Left | Left | Left | Left |
| Fill | on, `#FFFDFC` | `#EFE5E2` | `#E5D8D3` | `#F21D2F` |
| Border | off | off | off | off |

- Text padding left **12** (Style → Text → Padding, if shown).
- **Shape**: Rounded rectangle, rounded corners **8**.
- General: Background off, Title off, Header icons off.

The **Selected** state highlights the current page automatically. You can copy the four navigators to every page unchanged.

> **If your Pages card has no per-page toggles**, use **one** navigator with all 12 pages at **H 404 · W 176 · X 12 · Y 98**, and set `VAR _Grouped = FALSE ()` in `HTML Sidebar`. The sidebar then shows a single "PAGES" label instead of the four group labels.

---

## 6. Filters (native slicers, no container)

The slicers sit directly on the page background. There's no HTML filter bar.

### 6.1 Slicers

| Slicer | Field | Header text | Style | H · W · X · Y | Selection |
|---|---|---|---|---|---|
| Period | `Period Preset[Period]` | (header off) | **Tile** | 48 · 300 · 224 · 84 | Single select on. Select **MTD**. |
| End month | `Period End[End Month]` | END MONTH | **Dropdown** | 48 · 200 · 560 · 84 | Single select on. Select **Latest month**. |
| Tier | `Dim_Tier[Tier]` | TIER | **Dropdown** | 48 · 200 · 796 · 84 | Multi-select, "Select all" on. |
| Product | `Dim_Wizz[Product]` | PRODUCT | **Dropdown** | 48 · 200 · 1032 · 84 | Multi-select, "Select all" on. |

Formatting for all four:
- **Background off, border off, shadow off, header icons off**.
- **Slicer header** on (dropdowns only): Arial 8, **bold**, `#7A7370`. If the header shows its own small chevron, turn off the header's icon or collapse option, so only the dropdown box has a chevron.
- **Values**: Arial 10, `#160D0A`. Dropdown box: background `#FFFDFC`, border `#E5D8D3` 1 px, radius 6.
- Period tiles:
  - Unselected: background `#FFFDFC`, border `#E5D8D3`, font `#4A403C` Arial 10 bold.
  - Selected: background `#160D0A` (or `#F21D2F`), font `#FAF8F7`.
  - Align the tiles' vertical centre with the dropdown boxes, not with the header labels.

### 6.2 Reset button (native)
- Insert → Buttons → **Reset** (or Blank + Action *Bookmark* → a "Defaults" bookmark).
- **H 32 · W 88 · X 1488 · Y 100** (aligned with the dropdown boxes).
- Text "Reset", Arial 10, `#4A403C`. Fill `#FFFDFC` (hover `#EFE5E2`). Border `#D1C0B9` 1 px. Rounded corners **16** (pill).

### 6.3 Sync slicers
View → **Sync slicers** → sync all four slicers to all pages (visible), so filters follow navigation.

---

## 7. Page title block
- **HTML Content** → `_Measures[HTML Exec Title]` → **HTML standard**.
- **H 76 · W 1352 · X 224 · Y 144**.
- Eyebrow **OVERVIEW** (ember red), title **Executive summary** (Georgia), and on the line below it the "Showing" chips with the active period, tiers and products, e.g. *YTD · Jan – Aug 2026 · Explorer, Elite · All products*.

---

## 8. Key KPIs
- **HTML Content** → `_Measures[HTML Exec KPIs]` → **HTML standard**.
- **H 264 · W 1352 · X 224 · Y 228**.
- Contains 8 KPI cards (4 × 2) and the full-width "Time to progress" row. There's no section heading.

| Card | Measure | Value colour | Caption |
|---|---|---|---|
| Members in program | `[Members]` | ink `#160D0A` | As of <end date> |
| Engaged members | `[Engaged Members]`, `[Engaged Members %]` | `#F25D27` | x % of members · As of … |
| Program cost | `[Program Cost (RON)]` | `#F2811D` | period |
| Redemption rate | `[Redemption Rate]` | green `#00581D` | period |
| Coins collected | `[Coins Collected]` | `#F25D27` | period |
| Coins redeemed | `[Coins Redeemed]` | `#F25D27` | period |
| ROI — turnover ratio | `[ROI Turnover Ratio]` as "95.4×" | `#F2811D` | Spend per coin · period |
| ROI — isolated uplift | not available | pending tint | Not period-driven |

Engaged means a Card Avantaj customer collected or redeemed Avantaj Coins within the 180 days before month-end. The measure uses the source flag (`ENGAGED_CUST_CNT`); Wizz has no engagement flag and is excluded from the percentage denominator.

**Time to progress row** (same visual, no extra work):

| Element | Measure | Behaviour |
|---|---|---|
| Journey Starter → Ambassador | `[Time to Progress Starter to Ambassador (Months)]` | Sum of the average months per step. Shows "—" plus an "N steps without data" pill while any step has no upgrades. |
| 4 step chips | `[Avg Months to Progress (Step)]`, `[Upgrades (Step, LTD)]` over table `Progression Step` | Orange accent when data exists, grey "no data yet" otherwise |
| Tag | — | "LTD · not filtered" next to the label |

All progression measures use `REMOVEFILTERS()`, so slicers never change this row, even though the rest of the visual follows them.

---

## 9. Charts: shared card style

Both charts are native visuals with no HTML card behind them. To make them match the KPI cards, give each chart its own card frame:
- General → **Background on**, `#FFFDFC`, 0 % transparency.
- General → **Border on**, `#E5D8D3`, 1 px, **rounded corners 10**.
- General → **Padding** 12 (all sides). Shadow off. Header icons off.
- **Title on**: Arial 12 **bold**, `#160D0A`, left-aligned. Don't use Georgia here; Georgia is only for the page title.
- **Subtitle on** (if available in your version): Arial 9, `#7A7370`.

#### 9.1 Members by tier (Program snapshot)
- Insert **Clustered column chart**. **H 372 · W 780 · X 224 · Y 512**.
- X-axis `Dim_Tier[Tier]`. Y-axis `[Members]`, then `[Engaged Members]`.
- Sort: `…` → Sort axis → **Tier**, **Ascending** (Starter → Ambassador). Keep the same order as the Time to progress steps.
- Title: **Members and engaged members by tier**. Subtitle: **Point-in-time at the end month**.
- Legend: **on**, top-left, Arial 9, `#4A403C`, marker circle.
- Columns: Members `#D8CCC3`, Engaged Members `#F21D2F`. Space between categories 30 %, space between series 2 px.
- Data labels on: Arial 8, `#4A403C`, **display units None**, format `#,0`, position Outside end. This shows Ambassador as 362, not "0K".
- X-axis: Arial 9, `#4A403C`, title off. **Y-axis off**, because the data labels carry the values. Gridlines off.

#### 9.2 Profit per member around the upgrade (Profitability evolution)

This is an event study: the average monthly profit of the memberships that made one upgrade step, from 6 months before to 6 months after the upgrade month (month 0). It joins `Fact_TierMigrations` (upgrade month) to `Fact_CustomerView` (monthly profit) on `CUST_ID` + `WIZZ_FLG`. It follows the **Product** slicer and ignores the period and tier filters.

- Insert **Line chart**. **H 372 · W 552 · X 1024 · Y 512**.
- X-axis `Relative Month[Month]` (−6 … −1, **Upgrade**, +1 … +6; already sorted by `Offset`). Y-axis `[Profit per Member (Relative to Upgrade)]`. Tooltips `[Upgrade Cohort Customers (Relative to Upgrade)]`.
- X-axis type **Categorical**. Sort axis → **Month**, **Ascending**.
- Field well → right-click `Month` → **Show items with no data**: on, so all 13 months appear at equal spacing.
- Title: **Avg. profit per member around the upgrade**. Subtitle: **RON / month · month 0 = upgrade · product filter only**.
- Legend off.
- Line: `#F25D27`, 2.5 px, interpolation **Linear**. Don't use smooth interpolation; it overshoots on sparse data. Shade area on, 88 % transparency.
- Markers: on, circle, size 5, `#F25D27`.
- X-axis: Arial 9, `#4A403C`, title off. Y-axis: Arial 8, `#7A7370`, display units None, 0 decimals, title off. Gridlines `#EFE5E2`, 1 px.
- Leave space for the step slicer: set the title's right padding, or keep the title short so it ends before X 1376.

### 9.3 Step slicer (native)
- Insert **Slicer**, field `Progression Step[Step]`. **H 30 · W 180 · X 1384 · Y 524** (top-right, inside the chart card).
- Style **Dropdown**, **Single select on**. Header off. Values Arial 9, `#160D0A`. Background `#FFFDFC`. Border `#E5D8D3`, radius 6.
- Select **Starter → Explorer** as the default before saving. With nothing selected, the measures also fall back to Starter → Explorer.
- Don't sync this slicer to other pages.

### 9.4 Interactions
- Format → **Edit interactions**:
  - **Product** slicer → line chart: **Filter**. The line follows the product. Wizz always shows no line, because Wizz has no tiers by business design (no upgrades). With Product = All the line equals Card Avantaj.
  - **Period tiles, End month, Tier** → line chart: **None**. The measures ignore these filters anyway, and this avoids needless queries.
- The step slicer must stay **Filter** on the line chart. Set it to **None** on every other visual.

---

## 10. Finishing
1. **Selection pane** (top → bottom): logo, navigators, slicers, Reset, step slicer, line chart, column chart, header, title, KPIs, sidebar.
2. **Tab order**: navigators → slicers → KPIs → chart.
3. **Alt text** for the chart and the HTML KPI visual.
4. View → **Lock objects**.
5. Save → publish to Report Server.

**Reuse on other pages.** Copy these to every page: header, logo, sidebar, the four navigators, slicers and Reset. Positions stay the same when pasted. Each page then gets its own title, KPI and card measures.

## 11. Check before publishing
- [ ] Nothing is cut off or scrolls at **Fit to page** or at **Actual size**.
- [ ] MTD + Latest month → Members **166,813**, Engaged **145,806** (88.2 %, Wizz excluded from the denominator), Program cost **926K RON**, Redemption rate **38.6 %**, Coins collected **2.40M**, ROI **95.4×**. Header pill "MTD · Aug 2026".
- [ ] YTD → "YTD · Jan – Aug 2026", Coins collected **14.23M**, Redemption rate **48.3 %**, ROI **108.8×**.
- [ ] Tier = Explorer + Elite → title chips show "Explorer, Elite", Members **42,082**, Redemption rate **71.7 %** (YTD).
- [ ] The Executive summary navigator button is red. Clicking other buttons opens the right page and highlights it there.
- [ ] Time to progress chips (current sample): Starter → Explorer **4.3 mo / 12 upgrades**, Explorer → Insider **4.6 mo / 5 upgrades**, two steps "no data yet". Journey "—" with "2 steps without data". These values don't change with any slicer.
- [ ] Profitability evolution follows only the step slicer; the four filter slicers don't change it.

## 12. Known limitations
| Topic | Note |
|---|---|
| Geologica font | The certified HTML visual blocks embedded fonts. HTML text asks for Geologica and falls back to Arial (matches the native visuals). |
| Navigation in HTML | The HTML visual cannot switch pages, so navigation stays native (Page navigator). |
| ROI turnover ratio | Card spend ÷ coins collected, shown as "95.4×", not the prototype's "16 %". |
| Source tables | `Fact_TierMigrations` and `Fact_CustomerView` now hold the full base (1,632,960 customer-months, equal to the summary's member count; checked live on the Data health page). |
| Time to progress | Estimated as the sum of per-step averages (`PREV_TIER_MONTH_CNT`), not tracked per customer across the whole journey. Blank until all 4 steps have upgrades. |
| Profitability evolution | Customer-level join done in DAX. It can be slow on full data; if it is, pre-compute it in SQL (the prototype's `TIER_PROGRESSION_IMPACT`). |
| ROI uplift | Still not available. |
| Period "Latest month" | Follows the newest `Fact_Overview` month (Aug 2026). |

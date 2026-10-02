# Collection & redemption: step-by-step build guide

Builds on the Executive summary guide (v2) and uses the same 1600 × 900 canvas, shared shell, and Nexent Fusion styling.

- Object dimensions are listed as **H · W · X · Y**.
- Use **HTML Content** for the page title and KPI strip; use native visuals for the chart views and bookmark navigator.
- For native charts, use the chart card style from section 9 of the Executive summary guide. Do not put a separate HTML background behind them.
- The sample figures in the HTML prototype are illustrative only and are not reference values.
- The measures named in this guide have now been added to the currently open Loyalty Dashboard model. Save the PBIX to persist them.

---

## 1. Page content

| Block | Content |
|---|---|
| Title | *ENGAGEMENT* eyebrow, **Collection & redemption** title and dynamic Showing chips |
| KPI strip | Coins collected, coin redemption value and redemption rate |
| Collection view | Monthly coins collected by tier; monthly coins collected per engaged member in tier small multiples |
| Redemption view | Monthly coins redeemed by tier; redemption rate; monthly redemption value by transaction type |

The Period, End month, Tier and Product slicers are copied from Executive summary and remain interactive. A two-state bookmark navigator switches between the Collection and Redemption chart layouts without changing slicers.

---

## 2. Duplicate the Executive summary page

1. Right-click the **Executive summary** tab and choose **Duplicate page**.
2. Rename the copy **Collection & redemption**.
3. Keep the shared header, logo, sidebar and page navigator, Period tiles, End month/Tier/Product slicers, and Reset button.
4. Delete the Executive summary KPI visual, both charts and the step slicer.
5. Keep the title visual in place. Replace its measure with `_Measures[HTML Coins Title]`. It already uses the same filter chips and formatting as the other report page titles.
6. Set the Collection & redemption page as the selected destination for its entry in the sidebar page navigator. The navigator's active-page styling should then follow the current page.

Do not replace the title with a native text box.

### Improve the unselected Period tiles

The selected tile is already distinct in the screenshot, but the unselected tiles blend into the page background. Select the **Period** tile slicer and adjust its unselected/default item style only:

- Fill: `#FFFDFC` (surface white).
- Text: `#4A403C`.
- Outline: `#D1C0B9`, 1 px; corner radius 6 if available.
- Hover: `#F8DCC7` if the visual exposes a hover state.
- Keep the selected tile dark (`#160D0A`) with light text (`#FAF8F7`) so the active period remains unmistakable.

Apply this to the Period slicer itself, not the report theme's global slicer defaults; that avoids changing End Month, Tier or Product slicers. If the existing slicer format pane does not expose separate unselected/selected item states, keep its existing selected state and set the common/default fill and text to the values above.

---

### Collection / redemption view selector

Use a **Bookmark navigator**, rather than trying to squeeze all five charts into one view. The two screenshots show two coherent chart sets. A two-state navigator lets each set use the available canvas at a readable size.

- Place the native **Bookmark navigator** at **36 · 320 · 224 · 352**, just below the KPI strip and aligned with the content's left edge.
- Configure it horizontally with two buttons: **Collection** and **Redemption**.
- Selected: fill `#160D0A`, text `#FAF8F7`.
- Unselected: fill `#FFFDFC`, text `#4A403C`, outline `#D1C0B9`.
- Hover: `#F8DCC7`; use a subtle 1 px outline and 6 px corner radius if supported.

**Create the view bookmarks**
1. Open **View → Selection** and **View → Bookmarks**.
2. Group the Collection visuals as `Collection view`; group the Redemption visuals as `Redemption view`.
3. Show the Collection group and hide the Redemption group. Add a bookmark named **Collection**.
4. Show the Redemption group and hide the Collection group. Add a bookmark named **Redemption**.
5. In each bookmark's `…` menu, set **Data off**, **Display on**, **Current page on**. Use **Selected visuals** and select both visual groups in the Selection pane before adding/updating each bookmark, so only their visibility is captured and slicer values remain unchanged.
6. Insert → **Buttons → Bookmark navigator** and select the bookmark group containing only `Collection` and `Redemption`. If its style options are insufficient, use two blank buttons, each with Action → Bookmark set to the corresponding bookmark.

The selected navigator button should indicate which chart view is visible. Keep a separate **Reset filters** button if needed; don't make the view bookmarks double as filter-reset bookmarks.

---

## 3. Layout

```
Y 0    ┌──────────────────────── Header 64 ─────────────────────────────┐
Y 64   │Sidebar│ Period tiles │ End month │ Tier │ Product │ Reset     │
Y 144  │       │ Title  76                                            │
Y 228  │       │ KPI strip 112                                        │
Y 352  │       │ [ Collection ] [ Redemption ]                        │
Y 400  │       ├──────────────────────────┬───────────────────────────┤
       │       │       view-specific charts, bottom Y 884             │
Y 884  └───────┴──────────────────────────┴───────────────────────────┘
               X 224                    X 908                     X 1576
```

| # | Object | Type | H · W · X · Y |
|---|---|---|---|
| 1–7 | Shared shell | Copy from Executive summary | unchanged |
| 8 | Page title | HTML `HTML Coins Title` | **76 · 1352 · 224 · 144** |
| 9 | KPI strip | HTML `HTML Coins KPI Strip` | **112 · 1352 · 224 · 228** |
| 10 | Bookmark navigator | Native bookmark navigator | **36 · 320 · 224 · 352** |
| 11a | Collection view: collected by tier | Native stacked column chart | **484 · 668 · 224 · 400** |
| 11b | Collection view: per-engaged-member small multiples | Native line chart | **484 · 668 · 908 · 400** |
| 12a | Redemption view: redeemed by tier | Native stacked column chart | **484 · 668 · 224 · 400** |
| 12b | Redemption view: redemption rate | Native line chart | **220 · 668 · 908 · 400** |
| 12c | Redemption view: redemption by type | Native line chart | **248 · 668 · 908 · 636** |

The two bookmark views reuse the same positions where possible. The chart area ends at Y 884. Hide the inactive visual group so the views do not overlap.

---

## 4. Page title

- **HTML Content** visual → `_Measures[HTML Coins Title]` → **HTML standard** from the Executive summary guide.
- Position: **76 · 1352 · 224 · 144**.
- The title should read **Collection & redemption**, with the `ENGAGEMENT` eyebrow and the same period/tier/product chips as the other pages.
- The new `_Measures[HTML Coins Title]` already matches the existing page-title styling and keeps the dynamic chips.

---

## 5. KPI strip

- **HTML Content** visual → `_Measures[HTML Coins KPI Strip]` → **HTML standard**.
- Position: **112 · 1352 · 224 · 228**.
- Make one visual containing three equal joined cells:
  1. **Coins collected** — `[Coins Collected]`
  2. **Coins redeemed** — `[Coins Redeemed]`
  3. **Redemption rate** — `[Redemption Rate]`
- Coin values use compact `K`/`M` suffixes with no currency symbol (these are coin counts); format the rate as `0.0%`. Use `[Period Label]` as the small caption below each value.
- Use the same card surface `#FFFDFC`, separator/border `#E5D8D3`, ink `#160D0A`, orange `#F25D27` and green `#00581D` as the Financial impact KPI strip. Avoid native Card visuals for these three text-led KPIs.

The measure is already in `_Measures` under `_Report\HTML`. It reuses `[Coins Collected]`, `[Coins Redeemed]`, `[Redemption Rate]` and `[Period Label]`. The coin values use compact `K`/`M` suffixes and are coin counts, not RON amounts.

---

## 6. Tier palette and chart formatting

### 6.1 Apply tier colors

The model measure `Dim_Tier[Tier_Selected]` returns the color for the tier in context:

| Tier | Color |
|---|---|
| Ambassador | `#F21D2F` |
| Elite | `#4A3D8F` |
| Insider | `#1F6B52` |
| Explorer | `#F2811D` |
| Starter | `#5C3D2E` |

For each chart with Tier as a series/category:
1. Format visual → **Data colors** (or the relevant series color section) → **fx**.
2. Format style: **Field value**; What field should we base this on: **`[Tier_Selected]`**.
3. Apply and verify each color against the legend.

The measure returns muted grey when there isn't one tier in context. For tiered categories the measure is evaluated for each tier; use the tier column from `Dim_Tier`, not a text label from the fact.

You can also set a report theme's `dataColors` palette in the same order as the `Dim_Tier[Tier]` sort order:

```json
"dataColors": ["#5C3D2E", "#F2811D", "#1F6B52", "#4A3D8F", "#F21D2F"]
```

That gives a convenient default palette, but it is an ordered palette, not a permanent Tier-to-color rule. The color assigned can shift when series order or visual fields change, and the palette also affects other visuals. Use the `[Tier_Selected]` field-value conditional format where exact tier-color identity matters.

### 6.2 Shared native chart defaults

Apply to the five charts in their relevant bookmark view:

- General → Background on, `#FFFDFC`, 0% transparency.
- General → Border on, `#E5D8D3`, 1 px, rounded corners 10.
- Padding 12; shadow and header icons off.
- Title on, Arial 12 bold, `#160D0A`, left aligned. Subtitle, if available, Arial 9, `#7A7370`.
- Keep axis titles off unless the measure/unit is not clear from the chart title or subtitle. Use categorical X-axes for month-by-month trends and sort `Dim__Calendar[YearMonth]` ascending by `YearMonth Seq`.
- Axis labels are a separate setting from axis titles. Keep categorical month labels on when there is room; hide Y-axis labels when direct data labels already provide the values or when the chart is a small multiple and each point is labeled. Do not assume Power BI will automatically change label visibility based on categorical vs continuous axis type.

Power BI themes can set per-visual-type defaults for axis title/label formatting, but they cannot express a conditional rule such as “hide values only when X-axis is categorical.” Set that choice in each visual. The screenshot uses categorical monthly axes; use the displayed `YYYY-MMM` labels vertically where necessary.

### 6.3 Collection view: monthly coins collected by tier

- Visual: **Stacked column chart**.
- Position: **484 · 668 · 224 · 400**.
- X-axis: `Dim__Calendar[YearMonth]`; Y-axis: `[Coins Collected]`.
- Legend: `Dim_Tier[Tier]`, ordered Starter → Explorer → Insider → Elite → Ambassador.
- Title: **Coins Collected by Tier**.
- Apply `[Tier_Selected]` through Data colors → **fx** → Field value.
- Y-axis title off; format `#,0`; gridlines `#EFE5E2`. Use white data labels inside segments only if enough room; otherwise disable labels and use tooltips.

`[Coins Collected]` is from `Fact_Overview`, which has both tier and month grain. Do not use `[Wizz Coins Collected]` from the Wizz transaction table for this tier breakdown.

### 6.4 Collection view: average coins per engaged member

- Visual: **Line chart** with small multiples by `Dim_Tier[Tier]`.
- Position: **484 · 668 · 908 · 400**.
- X-axis: `Dim__Calendar[YearMonth]`; Y-axis: `[Coins Collected per Engaged Member per Month - Card Avantaj]`; Small multiples: `Dim_Tier[Tier]`.
- Title: **Average coins collected per engaged member**. Subtitle: **Card Avantaj · coins per engaged member-month · 180-day coin activity**.
- Use a restrained neutral line `#7A7370` so tiers are distinguished by their small-multiple titles, not repeated colors. Markers on, data labels only if legible, Y-axis title off.
- Keep the same Y scale across all five multiples if the main purpose is comparing absolute levels. Use independent scales only if the purpose is showing within-tier trend shape, and make that choice explicit in a subtitle or tooltip.

The existing measure filters both collected coins and engaged member-months to Card Avantaj (`WIZZ_FLG = 0`). It returns blank for Wizz-only selection. “Engaged” means the customer collected or redeemed Avantaj Coins within the 180 days before month-end; the measure uses the source flag for this definition.

### 6.5 Redemption view: monthly coins redeemed by tier

- Visual: **Stacked column chart**.
- Position: **484 · 668 · 224 · 400**.
- X-axis: `Dim__Calendar[YearMonth]`; Y-axis: `[Coins Redeemed]`; Legend: `Dim_Tier[Tier]`, Starter → Ambassador.
- Title: **Coins Redeemed by Tier**.
- Apply `[Tier_Selected]` through Data colors → **fx** → Field value.
- Format the Y-axis `#,0`; title off; gridlines `#EFE5E2`. Labels inside segments only when they fit; otherwise use tooltips.

`[Coins Redeemed]` is from `Fact_Overview`, so this chart is in coins and can be split by tier. Do not use detailed redemption amount-in-RON measures here.

### 6.6 Redemption view: redemption rate

- Visual: **Line chart**.
- Position: **220 · 668 · 908 · 400**.
- X-axis: `Dim__Calendar[YearMonth]`; Y-axis: `[Redemption Rate]`.
- Title: **Redemption Rate**. Line `#160D0A`, 2 px; small markers; data labels on as percentages if they remain readable.
- Format Y-axis `0%`; axis titles off; gridlines off or `#EFE5E2`.

The measure is redeemed coins ÷ collected coins; it is a ratio of monthly totals, not a matched-customer redemption cohort.

### 6.7 Redemption view: value by transaction type

- Visual: **Line chart** with one series for each type.
- Position: **248 · 668 · 908 · 636**.
- X-axis: `Dim__Calendar[YearMonth]`; Y-axis values:
  - `[Bonus Conversion Value (RON)]`
  - `[Pay Transactions Value (RON)]`
  - `[Vouchers & Experiences Value (RON)]`
  - `[Wizz Air Transfer Value (RON)]`
- Title: **Redemption by Transaction Type**. Y-axis format `#,0`; axis titles off.
- Apply colors in measure order: `#F21D2F`, `#1F6B52`, `#4A3D8F`, `#5C3D2E`. This is not a Tier legend, so do not apply `[Tier_Selected]` to these series.
- Use small markers and direct value labels only when they don't collide. Keep legend visible.

The transfer remains a separate type and is included in total redemption value. Do not subtract Wizz reversals from collected coins. The displayed categories may not sum to total redemptions; don't imply reconciliation unless verified.

---

## 7. Interactions and validation

1. Keep Period, End month, Tier and Product slicers synced and visible as on Executive summary.
2. Confirm the bookmark navigator changes only which chart group is visible. Confirm all slicer selections stay unchanged as you switch between Collection and Redemption.
3. Confirm all visible charts respond to Period, End month, Tier and Product as appropriate.
4. Test one tier, then all tiers; test Card Avantaj, Wizz and All products.
5. For Wizz-only selection, the Card Avantaj per-engaged-member chart should be blank (not a fabricated zero).
6. Check that transaction-type values are not presented as reconciling to total redemptions unless they actually match `[Coin Redemption Value (RON)]`.
7. Check the KPI and chart numbers against the selected filters in a table visual or DAX query. Prototype numbers are mock values; don't use them as validation targets.

---

## 8. Known limitations

- `Fact_WizzCoinTransactions` is Wizz-only and has no tier. The tiered collection and redemption charts use `Fact_Overview` measures instead; do not substitute Wizz transaction measures for those tier splits.
- The redemption type series are selected categories. The total measure includes all redemption activities, including the separately displayed Wizz Air transfer; other categories may mean the visible series do not sum to the total.
- Wizz reversals are reported separately and are not subtracted from collected coins.
- “Engaged” is the current model proxy for “active”; the definition remains pending. Label it transparently.
- The source tables have different date coverage. Use the calendar relationship and selected end month, and do not invent values for months with no fact data.
- The prototype's values (including its standalone redemption-rate series) are illustrative, not data truth. The report should use model measures only.

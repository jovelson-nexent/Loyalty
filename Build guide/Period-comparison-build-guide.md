# Period comparison: step-by-step build guide

This guide builds on the Trends guide. The page uses the same shell, grid and styling:

- Canvas: 1600 × 900, with the shared header, sidebar and slicers.
- Style: Nexent Fusion.

Conventions:
- Positions are given as **H · W · X · Y**.
- HTML Content is used for the title and the detail table. Native visuals are used for the slicers and the chart.
- All model objects in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

The prototype's Period comparison tab:
- a KPI dropdown;
- Period A and Period B, each with a start and an end month, and a **Compare** button;
- a bar chart by tier (A grey, B red);
- a detail table: Tier | A | B | Δ | Δ %.

| Block | Content |
|---|---|
| Title | *ENGAGEMENT* eyebrow, the title **Period comparison**, and "Showing" chips: KPI (dark), A · months (grey), B · months (red), tiers, products |
| KPI selector | Tile slicer, the same 10 KPIs as Trends, single select |
| Period A / Period B | Two dropdown slicers of months, multi-select |
| Chart | Native clustered column chart: Period A vs Period B by tier |
| Detail table | HTML: Tier | Period A | Period B | Change | Change %, an All tiers row, and a data note when needed |

Differences from the prototype:
- **10 KPIs instead of 4.** The slicer reuses Trends' `'Trend KPI'` calculation group, so both pages offer the same KPIs. The prototype's "coins / engaged member", "challenges completed / person", "badges / person" and "redemption rate" are all included.
- **Months are picked, not ranges.** Power BI has no "start month / end month" pair that acts as one filter. Each period is a multi-select month list: pick one month, or Ctrl+click several (consecutive or not).
- **No Compare button.** The visuals update as soon as a month is clicked.
- **Change in pp for rates.** The prototype shows Δ as a plain difference for all KPIs. For rates the table shows percentage points, so "+8.7 pp" isn't confused with "+8.7%".
- **Data note.** The table warns when a comparison is distorted by a known data effect (challenges before Jul 2026, the badge in-progress stock).

---

## 2. How it works (read first)

**Two period tables, not linked to the model.** `'Compare A'` and `'Compare B'` are calculated tables with one row per month. They have no relationships, so the Period A slicer filters nothing by itself. The measures read the selected months and calculate the KPI for them.

New model objects:

| Object | Where | What it does |
|---|---|---|
| Table `'Compare A'` | New table | Columns `[Month]` ("Aug 2026", sorted by `[Month Seq]`), `[Month End]` and `[Month Seq]` (both hidden) |
| Table `'Compare B'` | New table | Same as `'Compare A'` |
| `[Period A]` / `[Period B]` | `_Report\Period Comparison` | Average of the KPI's monthly values over the months selected in A / B |
| `[Period A Label]` / `[Period B Label]` | `_Report\Period Comparison` | Text for the months: "Aug 2026", "Jun – Aug 2026", or "2 months · Sep 25 – Nov 25" when the months aren't consecutive |
| `[Period Chart Title]` | `_Report\Period Comparison` | Chart title: "*KPI* by tier" |
| `[Period Chart Subtitle]` | `_Report\Period Comparison` | Chart subtitle with the real months: `[Period A Label] & "    vs   " & [Period B Label]` → "Jul 2026    vs   Aug 2026" |
| `[HTML Period Compare Title]` | `_Report\HTML` | Page title with the chips |
| `[HTML Period Compare Table]` | `_Report\HTML` | Detail table |
| `'Trend KPI'` (existing) | | Each item now also sets the format of `[Period A]` and `[Period B]` (0% or 0.00) |

**Rules:**
- **Defaults.** With nothing selected, Period A is the month **before** the latest month and Period B is the **latest** month (today Jul 2026 vs Aug 2026). Clearing a slicer returns to its default.
- **Period value = average of the monthly values.** For a rate over Jun–Aug, that's (Jun rate + Jul rate + Aug rate) ÷ 3. It's the same as the prototype and puts one month and three months on the same scale (per month). Months with no value (for example Ambassador in Sep 2025) are skipped.
- **The Period tiles (MTD / PM / YTD / LTD) and the End month slicer are ignored.** The months come only from the Period A and B slicers. The Tier and Product slicers **do** apply.
- The KPIs are the same measures as on the other pages, so one month here equals that month on Trends.

---

## 3. Duplicate the page

1. Right-click **Trends** and choose **Duplicate page**. Rename the copy **Period comparison**.
2. Keep the shared shell: header, logo, sidebar page navigator, Tier / Product slicers and the Reset button.
3. **Period tiles and End month slicer:** they do nothing on this page. Choose one:
   - Recommended: hide them on this page (Selection pane → eye icon). In View → Sync slicers keep them synced but not *visible* on this page, so the choice still carries over to the other pages.
   - Or leave them and accept that clicking them changes nothing here.
4. Keep the KPI tile slicer (it's the same field). Delete the line chart and the side panel.
5. In the title visual, replace the measure with `_Measures[HTML Period Compare Title]`.
6. In the sidebar page navigator, make sure the Period comparison entry points to this page.

**KPI slicer sync.** The copied KPI slicer is a separate slicer. If you'd rather keep the chosen KPI when moving between Trends and Period comparison, sync the two in View → Sync slicers (tick *Sync* on both pages). Either way works.

---

## 4. Layout

```
Y 144  │ Title 76                                                          │
Y 228  │ KPI selector (tile slicer, 10 tiles in one row) 52                │
Y 296  │ Period A ▾ 56 │ Period B ▾ 56 │                                   │
Y 368  ├──────────── Chart (native) ──────────────┬──── Detail table ─────┤
       │ 516 · 820 · 224 · 368                    │ 516 · 516 · 1060 · 368 │
Y 884  └──────────────────────────────────────────┴───────────────────────┘
```

| # | Visual | Type | H · W · X · Y |
|---|---|---|---|
| 8 | Title | HTML Content | 76 · 1352 · 224 · 144 |
| 9 | KPI selector | Slicer (Tile) | 52 · 1352 · 224 · 228 |
| 10 | Period A | Slicer (Dropdown) | 56 · 320 · 224 · 296 |
| 11 | Period B | Slicer (Dropdown) | 56 · 320 · 560 · 296 |
| 12 | Chart | Clustered column chart | 516 · 820 · 224 · 368 |
| 13 | Detail table | HTML Content | 516 · 516 · 1060 · 368 |

The table HTML scales to the visual: the unit is the smaller of width ÷ 516 and height ÷ 516. If you resize it, keep it roughly square.

---

## 5. Title

HTML Content, HTML standard, **76 · 1352 · 224 · 144**: `_Measures[HTML Period Compare Title]`.

- Eyebrow *ENGAGEMENT*, title **Period comparison** (Georgia 28).
- Chips:
  - **KPI** (dark chip);
  - **A · months** (grey `#D8CCC3`, the chart's Period A colour);
  - **B · months** (red `#F21D2F`, white text, the chart's Period B colour);
  - tiers;
  - products.
- No period chip, because the Period tiles don't apply here.

---

## 6. KPI selector

Same as Trends §6: `'Trend KPI'[KPI]`, Tile, single select, header off, **52 · 1352 · 224 · 228**. Select **Redemption rate** as the saved default.

---

## 7. Period A and Period B slicers

1. Insert → Slicer. Field: `'Compare A'[Month]`. **56 · 320 · 224 · 296**.
2. Slicer settings → Style: **Dropdown**. Selection:
   - Single select **off** (so Ctrl+click picks several months);
   - Multi-select with Ctrl **on**;
   - Show "Select all" **off**.
3. Slicer header **on**, text **Period A**, Arial 9 bold, `#7A7370`. Values: Arial 10, `#160D0A`.
4. Add a small grey marker so the slicer matches the chart: Format → Visual border **on**, colour `#D8CCC3`, 2 px, radius 6.
5. Sort the slicer by `[Month]` (it follows Month Seq, so oldest first). If you prefer the latest month first, sort descending.
6. **Leave it empty** when saving, so the page opens on the defaults (Jul 2026 vs Aug 2026 today, and it moves forward each month by itself).
7. Copy the slicer to **56 · 320 · 560 · 296**, change the field to `'Compare B'[Month]`, header **Period B**, border colour `#F21D2F`.
8. Sync slicers: these two slicers exist only on this page.

Tip: when nothing is selected the dropdown shows "All". That is fine: the title chips and the table headers show which months are actually used ("Jul 2026", "Aug 2026"). If "All" might confuse readers, add a slicer subtitle "Empty = previous month" (A) and "Empty = latest month" (B).

---

## 8. Chart (native clustered column chart)

1. Insert → Clustered column chart, **516 · 820 · 224 · 368**.
2. Fields:
   - X-axis: `Dim_Tier[Tier]`.
   - Y-axis: `_Measures[Period A]`, then `_Measures[Period B]` (in that order, so A is on the left).
   - No legend field.
3. Rename the fields in the visual (double-click in the Y-axis well): **Period A**, **Period B**.
4. Sort: … → Sort axis → **Tier**, **descending** (Ambassador first, as on the other pages).
5. Apply the shared chart-card defaults (Collection & redemption guide §6.2): background `#FFFDFC`, border `#E5D8D3` 1 px radius 10, padding 12, gridlines `#EFE5E2`, axis titles off.
6. Title: fx → Field value → `_Measures[Period Chart Title]` ("Redemption rate by tier"). Georgia 18 pt, `#160D0A`.
   - Subtitle **on**: fx → Field value → `_Measures[Period Chart Subtitle]`. Arial 10, `#7A7370`.
   - It shows the months actually used, for example "Jul 2026    vs   Aug 2026", or "2 months · Sep 25 – Nov 25    vs   Jun – Aug 2026".
7. Columns → Colours: Period A **`#D8CCC3`**, Period B **`#F21D2F`**. Spacing: inner padding about 10%.
   - Optional, colour by tier: Period B → fx → Field value → `_Measures[Tier_Selected]`; Period A → fx → Field value → `_Measures[Tier Color Light]` (a light version of the same tier colour). Then turn the legend **off**: it would still show grey and red. In the subtitle, describe the shades instead: light = Period A, dark = Period B. If the fx button doesn't appear for a series, your Report Server version doesn't support per-series conditional colours with two measures; keep the grey and red.
   - Axis labels can't take a colour per tier (the colour applies to all labels at once).
8. Legend: **on**, top left, Arial 9, `#4A403C`. It shows Period A / Period B; the months are in the chart subtitle, the title chips and the table headers.
9. Y-axis: start **0**, end **Auto**. Display units: **None**. The value format (0% or 0.00) comes from the calculation group.
10. Data labels: **on**, Arial 9, `#4A403C`, display units None. With 5 tiers × 2 bars they are readable.
11. Tooltips show the KPI in its own format.

Tier colours aren't used in this chart: the colours mean the **period**, not the tier. The tier colours are in the table's dots.

---

## 9. Detail table (HTML)

HTML Content, HTML standard, **516 · 516 · 1060 · 368**: `_Measures[HTML Period Compare Table]`.

Content, top to bottom:
1. Card header, same pattern as the chart next to it (title = what the card shows, subtitle = the real months):
   - Title **Change by tier** (Georgia 24, the same size as the chart's 18 pt title). It doesn't repeat the KPI name, which is already in the chart title, the chip and the KPI slicer.
   - Subtitle, 13 px grey like the chart subtitle: "Jul 2026 → Aug 2026", plus "| change in pp" for rates. The arrow reads A → B, the direction of the change.
2. Table:

   | Column | Content |
   |---|---|
   | Tier | Colour dot and tier name, Ambassador first |
   | Period A | Grey marker and the months in the header; the value |
   | Period B | Red marker and the months in the header; the value in bold |
   | Change | B − A. Rates in **pp**, other KPIs in the KPI's units. Green `#00581D` up, red `#F21D2F` down |
   | Change % | (B − A) ÷ A, same colours. "—" when A is 0 or blank |

   - **All tiers** row when more than one tier is visible. It is the KPI for all visible tiers together, not the average of the rows.
   - "—" when a tier has no value in a period.
3. Footnote: how the period values and the change are calculated.
4. **Data note** (orange accent), only when it applies:
   - a challenge KPI with any selected month before Jul 2026;
   - Badge completion rate, Badges enrolled / member or Badge enrolment rate (the in-progress stock and the ~100% enrolment).

With no KPI selected the table says "Pick one KPI above to compare."

---

## 10. Validation

1. Defaults (nothing selected in A or B), **Redemption rate**, all tiers:

   | Tier | A: Jul 2026 | B: Aug 2026 | Change | Change % |
   |---|---|---|---|---|
   | Ambassador | 80% | 88% | +7.4 pp | +9% |
   | Elite | 74% | 83% | +8.7 pp | +12% |
   | Insider | 46% | 55% | +8.3 pp | +18% |
   | Explorer | 20% | 27% | +7.3 pp | +37% |
   | Starter | 13% | 22% | +8.9 pp | +69% |
   | All tiers | 31% | 39% | +7.4 pp | +24% |

2. Click **MTD**, **YTD** or **LTD** (if the tiles are visible): nothing changes on this page.
3. **Challenge completion rate**, A = Mar + Apr + May 2026, B = Jun + Jul + Aug 2026:
   - chips "A · Mar – May 2026" and "B · Jun – Aug 2026";
   - Elite 29% → 34% (+4.5 pp); Insider 100% → 59%; All tiers 57% → 50% (−6.2 pp);
   - the challenge data note appears.
4. A = Sep 2025 + Nov 2025 (Ctrl+click): the A label is "2 months · Sep 25 – Nov 25".
5. Select one tier in the Tier slicer: one tier in the chart and table, no All tiers row.
6. Click each KPI: the chart title, Y-axis format, chips and table all change. Rates show pp; per-member KPIs show 0.00 (badges enrolled 0.0, coins 0.0 in the table).
7. Clear A and B: back to Jul 2026 vs Aug 2026.

---

## 11. Known limitations and open questions

- **Averages are not weighted.** A three-month period is the plain average of three monthly values. A month with few members counts as much as a busy month. This matches the prototype; a weighted version (total coins redeemed ÷ total coins collected over the period) would give slightly different numbers. Ask if you need it.
- **All tiers vs the tier rows.** All tiers is the KPI for all members together each month, then averaged. It isn't the average of the five tier rows (Starter has far more members), so it can sit outside the range you'd expect from the rows. For example Challenge completion rate in Mar–May 2026 is 100% for three tiers but 57% for All tiers.
- **Challenges before Jul 2026** cover few members (enrolment 2–14% vs 82–89% after). The data note uses a fixed date (1 Jul 2026). Update the date in `[HTML Period Compare Table]` (`DATE ( 2026, 7, 1 )`) if the programme team explains the change differently.
- **Badge in-progress** only accumulates, so badge completion rate falls and badges enrolled / member rises between any two periods. The data note says so.
- **A and B can overlap** (for example A = Jun–Aug, B = Aug). That is allowed; the visuals don't warn about it.
- **The months list** comes from the months in `Fact_Overview[AS_OF_MONTH]`. When a new month is loaded, refresh the model and it appears in both slicers; the defaults move forward automatically.
- The prototype's figures are mock values. Don't use them as validation targets.

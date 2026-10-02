# Trends: step-by-step build guide

This guide builds on the Challenges and Badges guides. The page uses the same shell, grid and styling:

- Canvas: 1600 × 900, with the shared header, sidebar and slicers.
- Style: Nexent Fusion.

Conventions:
- Positions are given as **H · W · X · Y**.
- HTML Content is used for the title and the side panel. Native visuals are used for the slicers and the chart.
- All model objects in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

The prototype's Trends tab is a single chart ("trimmed to a single chart per your manager's note") with a KPI dropdown. Every tier is a line.

| Block | Content |
|---|---|
| Title | *ENGAGEMENT* eyebrow, the title **Trends**, and "Showing" chips: selected KPI (dark chip), period, tiers, products |
| KPI selector | Tile slicer with 10 KPIs, single select |
| Trend chart | Native line chart: selected KPI by month, one line per tier |
| Side panel | HTML: KPI definition, first vs last month by tier with the change, and a "How to read" note |

Differences from the prototype:
- The prototype has 7 KPIs. The page adds the **badge** enrolled / member, enrolment rate and completion rate, so badges mirror challenges (as on the Badges page). "Badges collected / person" is *Badges completed / member*.
- The side panel is new. Several KPIs have known data effects (the challenge step-up in Jul 2026, the growing badge in-progress stock). The panel explains them next to the chart, so the reader doesn't misread a line.

---

## 2. How the KPI selector works (read first)

**Why not a field parameter.** Field parameters are not supported on Power BI Report Server January 2025. They can appear in Desktop but don't work after upload. The page uses a calculation group instead; calculation groups already work on your server (`Period Preset`).

New model objects:

| Object | Where | What it does |
|---|---|---|
| Calculation group `'Trend KPI'`, column `[KPI]` | New table | 10 items, one per KPI (list below). Precedence **5**, below `Period Preset` (10). Each item only sets the **format string** of `[Trend Value]`; every other measure keeps its own value and format |
| `[Trend Value]` | `_Report\Trends` | `SWITCH ( SELECTEDVALUE ( 'Trend KPI'[KPI] ), … )` returns the selected KPI. Blank when no single KPI is selected |
| `[Trend Chart Title]` | `_Report\Trends` | Chart title: "*KPI* by tier, month by month" |
| `[HTML Trends Title]` | `_Report\HTML` | Page title with the chips |
| `[HTML Trend Panel]` | `_Report\HTML` | Side panel |

The 10 KPIs (slicer order) and the measure behind each:

| # | KPI (slicer text) | Measure | Format |
|---|---|---|---|
| 0 | Coins collected / engaged member | `[Coins Collected per Engaged Member per Month]` | #,0 |
| 1 | Redemption rate | `[Redemption Rate]` | 0% |
| 2 | Challenges enrolled / member | `[Challenges Enrolled per Member]` | 0.00 |
| 3 | Challenges completed / member | `[Challenges per Member per Month]` | 0.00 |
| 4 | Challenge enrolment rate | `[Challenge Enrolment Rate]` | 0% |
| 5 | Challenge completion rate | `[Challenge Completion Rate]` | 0% |
| 6 | Badges enrolled / member | `[Badges Enrolled per Member]` | 0.0 |
| 7 | Badges completed / member | `[Badges per Member per Month]` | 0.00 |
| 8 | Badge enrolment rate | `[Badge Enrolment Rate]` | 0% |
| 9 | Badge completion rate | `[Badge Completion Rate]` | 0% |

These are the same measures as on the Collection & redemption, Challenges and Badges pages. On a month axis each point is one month, so the chart shows the same values as those pages' monthly charts.

**Why the format matters.** Compatibility level 1600 doesn't allow dynamic format strings on measures, but calculation items can set one. That is what lets the Y-axis and tooltips switch between 0% and 0.00 when the KPI changes.

**Adding a KPI later.** Add a calculation item to `'Trend KPI'` (expression `SELECTEDMEASURE ()`, format string expression as in the other items) and add the same name to the `SWITCH` in `[Trend Value]`. Use the same text in both, or the line will be blank. Then add its definition and note to `[HTML Trend Panel]`.

---

## 3. Duplicate the page

1. Right-click **Badges** and choose **Duplicate page**. Rename the copy **Trends**.
2. Keep the shared shell: header, logo, sidebar page navigator, Period tiles, End month / Tier / Product slicers, and the Reset button.
3. Delete the KPI strip, the tier table, the two line charts, the Top Badges list and the bookmark navigator. Delete the bookmarks' visibility groups if they came along. This page has no bookmarks.
4. In the title visual, replace the measure with `_Measures[HTML Trends Title]`.
5. In the sidebar page navigator, make sure the Trends entry points to this page.

---

## 4. Layout

```
Y 144  │ Title 76                                                          │
Y 228  │ KPI selector (tile slicer, 10 tiles in one row) 52                │
Y 296  ├──────────────── Trend chart (native) ─────────────┬── Side panel ─┤
       │                                                  │  (HTML)       │
       │ 588 · 1000 · 224 · 296                           │  588 · 336    │
       │                                                  │  · 1240 · 296 │
Y 884  └──────────────────────────────────────────────────┴───────────────┘
```

| # | Visual | Type | H · W · X · Y |
|---|---|---|---|
| 8 | Title | HTML Content | 76 · 1352 · 224 · 144 |
| 9 | KPI selector | Slicer (Tile) | 52 · 1352 · 224 · 228 |
| 10 | Trend chart | Line chart | 588 · 1000 · 224 · 296 |
| 11 | Side panel | HTML Content | 588 · 336 · 1240 · 296 |

The panel HTML scales to the visual: the unit is the smaller of width ÷ 336 and height ÷ 584. If you resize it, keep roughly the same proportions.

---

## 5. Title

HTML Content, HTML standard, **76 · 1352 · 224 · 144**: `_Measures[HTML Trends Title]`.

- Eyebrow *ENGAGEMENT*, title **Trends** (Georgia 28).
- Chips: **selected KPI** (dark `#160D0A` chip, so it reads as the active choice), then period, tiers and products.
- With no KPI selected the first chip says "Pick one KPI".

---

## 6. KPI selector (tile slicer)

1. Insert → Slicer. Field: `'Trend KPI'[KPI]`. **52 · 1352 · 224 · 228**.
2. Slicer settings → Style: **Tile**. Selection: **Single select** on. Turn the slicer header **off**.
3. Select **Redemption rate** (or the KPI you want people to see first). The selection is saved with the report.
4. Style it like the Period tiles (Collection & redemption guide, "Improve the unselected Period tiles"):
   - Values: Arial 9, text `#4A403C`, wrap text on. Long names such as "Coins collected / engaged member" go onto two lines, which is why the slicer is 52 high.
   - Default fill `#FFFDFC`, border `#D1C0B9` 1 px; selected fill `#160D0A` with text `#FAF8F7`.
5. The items keep the calculation item order (0–9). Don't sort the slicer alphabetically.
6. **Sync slicers:** the `'Trend KPI'` slicer only exists on this page. Don't add it to other pages.

**Don't** use the `'Trend KPI'` column anywhere else (a page filter, a table, a matrix). It only changes the format of `[Trend Value]`, so it's harmless, but it has no meaning elsewhere.

---

## 7. Trend chart (native line chart)

1. Insert → Line chart, **588 · 1000 · 224 · 296**.
2. Fields:
   - X-axis: `Dim__Calendar[YearMonth]`, categorical, sorted by `YearMonth Seq` ascending.
   - Y-axis: `_Measures[Trend Value]`.
   - Legend: `Dim_Tier[Tier]`.
3. Apply the shared chart-card defaults (Collection & redemption guide §6.2): background `#FFFDFC`, border `#E5D8D3` 1 px radius 10, padding 12, gridlines `#EFE5E2`, axis titles off.
4. Title: fx → Field value → `_Measures[Trend Chart Title]`. Georgia 18 pt, `#160D0A`. Subtitle off (the panel holds the definition).
5. Legend: bottom, Arial 9, `#4A403C`.
6. Line colours (Format → Lines → Colours): Ambassador `#F21D2F`, Elite `#4A3D8F`, Insider `#1F6B52`, Explorer `#F2811D`, Starter `#5C3D2E`. Lines 2 px, small markers.
   - Tip: copy one of the Badges line charts and paste it here, then change the Y-axis to `[Trend Value]`. The colours carry over.
7. Y-axis: start **0**, end **Auto**. Display units: **None** (otherwise 0% values can show as "0K"). The value format comes from the calculation group.
8. Data labels: **off**. 12 months × 5 tiers of labels can't be read; the side panel gives the first and last month values.
9. Tooltips show the KPI with its own format, for example 37% or 1.29.

**Period on this page.**
- The chart follows the Period tiles like every other page: **LTD** shows Sep 2025 – end month (12 months), **YTD** shows Jan – end month, **MTD** and **PM** show a single point.
- Recommended: on this page, select **LTD**. If the Period slicer is synced across pages, either accept that the choice carries over, or open View → Sync slicers and untick *Sync* for Trends so this page can stay on LTD.

---

## 8. Side panel (HTML)

HTML Content, HTML standard, **588 · 336 · 1240 · 296**: `_Measures[HTML Trend Panel]`.

Content, top to bottom:
1. *SELECTED KPI* eyebrow and the KPI name (Georgia 22).
2. Definition (one sentence).
3. Table: Tier (colour dot) | first month | last month (bold) | Change.
   - First and last month are the first and last months with data in the selected period (LTD: Sep 25 and Aug 26).
   - Change: rates in **percentage points** (pp); counts and per-member values in **%**.
   - "—" when a tier has no value that month (for example Ambassador in Sep 2025).
   - **All tiers** row when more than one tier is visible.
4. "How to read" note (orange accent) for the selected KPI.

With no KPI selected, the panel shows "Pick one KPI above to see its trend."

The definitions and notes:

| KPI | Definition | How to read |
|---|---|---|
| Coins collected / engaged member | Coins collected in the month ÷ engaged members in the month | Averages over engaged members only, so it moves with both spend and the size of the engaged base |
| Redemption rate | Coins redeemed ÷ coins collected in the month | Monthly flows: members redeem coins collected earlier, so one month can be high or low |
| Challenges enrolled / member | (Completed + in progress at month end + expired) ÷ members at month end | Steps up from Jul 2026; check whether challenges became auto-enrolled |
| Challenges completed / member | Challenges completed ÷ members | Follows the Jul 2026 step-up |
| Challenge enrolment rate | % of members with ≥ 1 challenge in the month | Steps up from Jul 2026 |
| Challenge completion rate | Completed ÷ enrolled | High before Jul 2026 on a small base; compare from Jul 2026 only |
| Badges enrolled / member | (Completed + in progress at month end + expired) ÷ members at month end | In-progress badges only accumulate, so it rises every month |
| Badges completed / member | Badges completed ÷ members | Sep 2025 is the first month of history |
| Badge enrolment rate | % of members with ≥ 1 badge in the month | About 100% everywhere; doesn't separate tiers |
| Badge completion rate | Completed ÷ enrolled | Falls as the in-progress stock grows, not because members complete less |

The notes are fixed text. They describe what the data shows today (Sep 2025 – Aug 2026). Review them when new months arrive or the programme team answers the open questions (§10).

---

## 9. Validation

1. Click each KPI tile: the chart title, Y-axis format, title chip and panel all change.
2. Clear the KPI selection (Ctrl+click the selected tile): the chart is empty and the panel says "Pick one KPI".
3. Select one tier: one line, no "All tiers" row in the panel.
4. Check the **all tiers** monthly values with Period = LTD (deselect the Tier slicer, then use a card or tooltip):

| KPI | Sep 25 | Dec 25 | Mar 26 | Jun 26 | Jul 26 | Aug 26 |
|---|---|---|---|---|---|---|
| Coins collected / engaged member | 12 | 7 | 8 | 13 | 15 | 16 |
| Redemption rate | 18% | 37% | 37% | 29% | 31% | 39% |
| Challenges enrolled / member | 0.10 | 0.04 | 0.05 | 0.18 | 1.39 | 1.31 |
| Challenges completed / member | 0.09 | 0.01 | 0.03 | 0.15 | 0.54 | 0.39 |
| Challenge enrolment rate | 9% | 2% | 4% | 14% | 82% | 89% |
| Challenge completion rate | 90% | 39% | 52% | 83% | 39% | 30% |
| Badges enrolled / member | 15.8 | 18.4 | 19.5 | 21.0 | 21.5 | 21.7 |
| Badges completed / member | 2.08 | 1.19 | 0.82 | 0.88 | 0.90 | 0.84 |
| Badge enrolment rate | 99% | 100% | 100% | 100% | 100% | 100% |
| Badge completion rate | 13% | 7% | 4% | 4% | 4% | 4% |

5. Panel check, Challenge completion rate, LTD, all tiers: Ambassador — → 52%; Elite 24% → 37% (+12 pp); Insider 100% → 32%; Explorer 100% → 32%; Starter 100% → 27%; All tiers 90% → 30% (−60 pp).
6. Elite, Redemption rate: Sep 25 36% → Aug 26 83%.

---

## 10. Known limitations and open questions

- **MTD / PM** show a single point. Use LTD (or YTD) on this page.
- **History** starts Sep 2025, so the chart has at most 12 points. Ambassador has no value in Sep 2025 and only 362 members, so its line jumps.
- **Challenges change in Jul 2026.** Enrolment rate goes from 14% to 82%, and enrolled per member from 0.18 to 1.39. Ask the programme team whether challenges became auto-enrolled. Until then, compare challenge KPIs from Jul 2026 onward.
- **Badge in-progress** only accumulates, so badges enrolled / member rises and badge completion rate falls every month. Ask whether "in progress" means real progress or just visible.
- **Coins collected / engaged member** in Sep 2025 (12) is about double Oct 2025 (6). It is the first month of history; worth checking with the data team.
- The KPI selector can't be a field parameter on Report Server January 2025 (§2). If the server is upgraded to a version with field-parameter support, the calculation group still works, so there's no need to change it.
- The prototype's figures are mock values. Don't use them as validation targets.

# Challenges: step-by-step build guide

This guide builds on the Executive summary guide (v2) and the Collection & redemption guide. The page uses the same layout and styling:

- Canvas: 1600 × 900, with the shared header, sidebar and slicers.
- Style: Nexent Fusion.

Conventions:
- Positions are given as **H · W · X · Y**.
- HTML Content is used for the title, the KPI strip and the tables. Native visuals are used for the slicers and the trend charts.
- All measures in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

| Block | Content |
|---|---|
| Title | *ENGAGEMENT* eyebrow, the title **Challenges**, and the dynamic "Showing" chips |
| KPI strip | Challenges enrolled, enrolment rate, enrolled per member, challenges completed, completion rate, and completed per member |
| Tier table | One row per tier, plus a Total row, built with HTML |
| Trend charts | Completed per member per month by tier, and completion rate by tier (native line charts) |
| Most completed challenges | Ranked HTML list: completions, share, % of members, completions by tier, months active. Scrollable, top 50. Shown in the "By challenge" view |

The prototype's KPI dropdown for the monthly evolution chart is replaced by **two fixed trend charts**. A dropdown would need a single SWITCH measure, and the model cannot format such a measure correctly for both counts and percentages: it is at compatibility level 1600, which has no dynamic format strings. The two most useful views are therefore shown side by side.

---

## 2. Data definitions (read first)

The source has **no enrolment field**. It provides the following:

| Field | Behaviour | Measure |
|---|---|---|
| Completed challenges | Monthly flow (summed over the period) | `[Challenges Completed]` |
| In-progress challenges | Month-end snapshot (period end month only) | `[Challenges In Progress]` |
| Expired challenges | Monthly flow (currently 0 in every month) | `[Challenges Expired]` |
| Per-challenge completions | Monthly flow per challenge name; **completions only** | `[Challenge Completions]` |

New measures created (folder `_Badges & Challenges\Challenges`):

| Measure | Definition |
|---|---|
| `[Challenges Enrolled]` | Completed in the period + in progress at the period end + expired in the period. This is a **proxy** for enrolments. |
| `[Members with Active Challenge]` | Members at the period end minus members with no challenge in progress |
| `[Members with Active Challenge %]` | The count above ÷ `[Completion Members]` |
| `[Challenge Completion Rate]` *(updated)* | Completed ÷ Enrolled. The denominator now includes expired challenges. The values are unchanged while expired = 0. |
| `[Challenges Enrolled per Member]` | `[Challenges Enrolled]` ÷ `[Completion Members]` (members at the period end) |
| `[Challenges Completed per Member]` | `[Challenges Completed]` ÷ `[Completion Members]` |
| `[Challenge Enrolment Rate]` | % of customers at the period end with ≥ 1 challenge (completed, in progress or expired) in any month of the period. **Customer-level, from `Fact_Completion`.** Counted across tiers, so a customer who changed tier still counts. |
| `[Never Enrolled in Challenge %]` | % of customers at the period end with no challenge activity in any month up to the period end. **Customer-level, from `Fact_Completion`.** |

**Why the enrolment rate is defined as a share of people (and not as in the prototype).** The prototype divides enrolled *challenges* by *members*. That mixes two units. With the real data the result is about 209% for YTD (348K ÷ 167K), which no one can read as a rate. That ratio is kept, but labelled honestly as **Enrolled / member** (2.09). The real "enrolment rate" is a share of people, which needs customer-level data.

What this means for the page:
- **Completion rate depends on the length of the period.** Completions add up over the period, while in-progress is a single month-end snapshot. For example: MTD Aug 2026 is 29.7%, YTD is 56.0% and LTD is 59.3%. Compare tiers or months within the same period setting, not MTD against YTD.
- **The per-challenge list can show completions only.** The data has no enrolments or in-progress counts per challenge, so it cannot show an enrolment rate or completion rate per challenge.
- **Two sources for completions.** `[Challenge Completions]` (per challenge) and `[Challenges Completed]` (overview) come from different tables. They agree closely but not exactly: for example, Aug 2026 is 64,317 vs 64,729. The list's share % uses its own total.
- **Enrolment rate and never enrolled come from `Fact_Completion`** (customer level, full load: 1.63M rows, 174K customers, Sep 2025 – Aug 2026). The overview table can't give these measures: it only has "no completed" and "no in-progress" counts per month, and their overlap is unknown.
  - Reconciliation with the overview: members and completed match exactly in every month. In-progress is slightly lower in `Fact_Completion` (for example Aug 2026: 153,040 vs 153,117).
  - `Fact_Completion` has one row per customer, month and programme (`WIZZ_FLG`), with no duplicates. In Aug 2026, 356 customers (0.2%) hold both a Card Avantaj and a Wizz membership. For 354 of them the two rows are in different tiers, because Wizz has no tiers by business design and its rows carry a placeholder Starter tier.
    - Enrolment rate and never enrolled count **people** (distinct `CUST_ID`). With Product = All, these customers count once, so the base is 356 lower than `[Members]`, which counts memberships.
    - In the tier rows, a customer counts in each tier where they hold a membership.
  - **History starts in Sep 2025**, so "never enrolled" means "no challenge since Sep 2025".
  - Safeguard: if `Fact_Completion` ever reloads with 1,000 rows or fewer (the old sample SQL), the KPI strip shows a red **Sample** flag and the tier table shows a red footnote.
- **Possible automatic enrolment.** In-progress challenges jump from about 3% of members (Jun 2026) to about 82% (Jul 2026), and the top challenges include "Card Wallet Enrolled" and "Has Wallet Starter". If customers are enrolled automatically, the enrolment rate reflects how the programme is set up rather than member behaviour. Confirm this with the programme team before the rate is used as a target.

Ask the data team for an enrolment field (or an enrolment date per customer-challenge). When it exists, only the expression of `[Challenges Enrolled]` needs to change.

---

## 3. Duplicate the page

1. Right-click **Collection & redemption** and choose **Duplicate page**. Rename the copy **Challenges**.
2. Keep the shared shell: header, logo, sidebar page navigator, Period tiles, End month / Tier / Product slicers, and the Reset button.
3. Delete all the chart visuals. Keep the bookmark navigator, but point it at the new *Challenges view* bookmark group (§8.1). On this page, don't capture the Collection/Redemption bookmarks: they belong to the other page.
4. In the title visual, replace the measure with `_Measures[HTML Challenges Title]`.
5. In the KPI visual, replace the measure with `_Measures[HTML Challenges KPI Strip]`.
6. In the sidebar page navigator, make sure the Challenges entry points to this page.

---

## 4. Layout

The area under the KPI strip (Y 356 → 884) switches between two views using bookmarks (see §8.1).

```
Y 144  │ Title 76                               [ By tier | By challenge ] │
Y 228  │ KPI strip 112 (6 cells)                                           │
Y 356  ├───────────── View "By tier" ─────────────┬───── "By challenge" ────┤
       │ Challenges by tier (HTML) 248            │ Most completed          │
Y 620  │ Completed/member/month │ Completion rate │ challenges (HTML) 528   │
Y 884  └────────────────────────┴─────────────────┴─────────────────────────┘
         X 224                    X 908                       X 1576
```

| # | Object | Type | H · W · X · Y | View |
|---|---|---|---|---|
| 1–7 | Shared shell | Copied | unchanged | both |
| 8 | Page title | HTML `HTML Challenges Title` | **76 · 1352 · 224 · 144** | both |
| 9 | Bookmark navigator | Native | **32 · 260 · 1316 · 166** | both |
| 10 | KPI strip | HTML `HTML Challenges KPI Strip` | **112 · 1352 · 224 · 228** | both |
| 11 | Tier table | HTML `HTML Challenges Tier Table` | **248 · 1352 · 224 · 356** | By tier |
| 12 | Completed per member per month | Native line chart | **264 · 668 · 224 · 620** | By tier |
| 13 | Completion rate by tier | Native line chart | **264 · 668 · 908 · 620** | By tier |
| 14 | Most completed challenges | HTML `HTML Top Challenges` | **528 · 1352 · 224 · 356** | By challenge |

There are 16 px gaps between all blocks, and the content ends at Y 884.

The HTML tables scale to the size of the visual: the unit is the smaller of width ÷ 1352 and height ÷ 248 (tier table) or ÷ 528 (top challenges). Small changes to the size keep the layout intact; the content shrinks rather than overflowing.

---

## 5. Title and KPI strip

**Title (8).** Use an HTML Content visual with the HTML standard settings, at **76 · 1352 · 224 · 144**. It shows:
- the eyebrow *ENGAGEMENT* and the title **Challenges** (Georgia 28);
- the chips for period, tiers and products.

**KPI strip (9).** Use an HTML Content visual at **112 · 1352 · 224 · 228**. It has six joined cells: three for enrolment, then three matching cells for completion.

| Cell | Measure | Format | Caption |
|---|---|---|---|
| Challenges enrolled | `[Challenges Enrolled]` | K/M | "Completed + in progress" |
| Enrolment rate (green) | `[Challenge Enrolment Rate]` | 0% | "Members with ≥ 1 challenge" (shows "**Sample** · ≥ 1 challenge" only if the data is sampled) |
| Enrolled / member | `[Challenges Enrolled per Member]` | 0.00 | Period label |
| Challenges completed | `[Challenges Completed]` | K/M | Period label |
| Completion rate (green) | `[Challenge Completion Rate]` | 0% | "Completed ÷ enrolled" |
| Completed / member | `[Challenges Completed per Member]` | 0.00 | Period label |

Check values (YTD · Jan–Aug 2026, all tiers and products): 348.3K · 90% · 2.09 · 195.2K · 56% · 1.17.

"Members with active challenge" and "completed / member / month" were removed from the strip to make room. Their measures are still in the model, and the monthly measure is still used in chart 11.

---

## 6. Challenges by tier (HTML table)

- Use an HTML Content visual → `_Measures[HTML Challenges Tier Table]` → HTML standard.
- Position: **248 · 1352 · 224 · 356**.
- The h2 title "Challenges by tier" uses Georgia 24 px (≈ 18 pt). Set the native chart titles to **Georgia 18 pt** so all three titles in this view match.
- Rows run from Ambassador down to Starter, matching the Financial impact page. Each tier has a colour dot from `[Tier_Selected]`. There are no bars.
- The **Total** row only appears when more than one tier is visible.
- Columns, in grouped headers:

| Group | Column | Measure |
|---|---|---|
| — | Tier (with colour dot) | `Dim_Tier[Tier]` |
| — | Members (at the period end) | `[Completion Members]` |
| Enrolment | Enrolled | `[Challenges Enrolled]` |
| Enrolment | Per member (orange) | `[Challenges Enrolled per Member]` |
| Enrolment | Enrolment rate | `[Challenge Enrolment Rate]` |
| Completion | Completed | `[Challenges Completed]` |
| Completion | Per member (orange) | `[Challenges Completed per Member]` |
| Completion | Completion rate | `[Challenge Completion Rate]` |
| — | Never enrolled | `[Never Enrolled in Challenge %]` |

- A footnote gives the definitions. It adds a red warning only if `Fact_Completion` is ever sampled again.
- The tier slicer filters the rows. Rows with no members are hidden.

---

## 7. Trend charts (native)

Apply the shared chart-card defaults from the Collection & redemption guide (§6.2):
- background `#FFFDFC`;
- border `#E5D8D3`, 1 px, radius 10;
- padding 12;
- title in Georgia 18 pt, `#160D0A` (matches the HTML table titles);
- axis titles off;
- gridlines `#EFE5E2`.

**X-axis.** Use `Dim__Calendar[YearMonth]` as a categorical axis, sorted by `YearMonth Seq`.

**Legend.** Put `Dim_Tier[Tier]` in the Legend. Power BI **doesn't allow fx (Field value) colours for series that come from a legend**, so set each series colour once in Format → Lines → Colours:

| Tier | Colour |
|---|---|
| Ambassador | `#F21D2F` |
| Elite | `#4A3D8F` |
| Insider | `#1F6B52` |
| Explorer | `#F2811D` |
| Starter | `#5C3D2E` |

Set the legend at the bottom, Arial 9, `#4A403C`. Line width 2 px, small markers.

### 7.1 Completed per member per month (12)
- Position: **264 · 668 · 224 · 620**.
- Y-axis: `[Challenges per Member per Month]`, format `0.00`. Hide the Y-axis values and turn on data labels only for the last point, or rely on tooltips.
- To hide labels for values below 0.1: Data labels → Values → Color → **fx** → Field value → `[Challenges per Member Label Color]`. It returns `#FFFFFF00` (transparent) when the value is below 0.1, and `#4A403C` otherwise.
- Title: **Challenges completed per member per month**.
- Optional subtitle: **Completions ÷ member-months**.

### 7.2 Completion rate by tier (13)
- Position: **264 · 668 · 908 · 620**.
- Y-axis: `[Challenge Completion Rate]`, format `0%`, from 0 to 1 (fixed, so the tiers are easy to compare).
- Title: **Challenge completion rate**.
- Subtitle: **Completed in month ÷ (completed + in progress at month end)**.
- To hide labels at 100%: Data labels → Values → Color → **fx** → Field value → `[Challenge Completion Rate Label Color]`. It returns `#FFFFFF00` (transparent) when the rate rounds to 100%, and `#4A403C` otherwise.
  - **Why the 100% values appear:** before Jul 2026 the source has **in progress = 0** for Starter, Explorer and Insider, so the rate is completed ÷ completed. These points aren't real performance. Consider starting the chart's X-axis at **Jul 2026**, when in-progress tracking begins for all tiers.

Each point here is a single month, so the rate is the monthly rate. It will be lower than the YTD rate shown in the KPI strip (see §2). The subtitle explains this.

Ambassador has very few members (362), so its lines can jump sharply. That is expected.

---

## 8. Most completed challenges (HTML list)

- Use an HTML Content visual → `_Measures[HTML Top Challenges]` → HTML standard.
- Position: **528 · 1352 · 224 · 356** (view "By challenge", same area as the tier table and charts).
- The title uses Georgia 24 px, like the tier table. There are no bars.
- The list shows the **top 50** challenges by completions, in a scrollable body with a sticky header. The subtitle says "Top 50 of N".
  - Long names are cut off with an ellipsis; hover to see the full name.
  - Rank ties share a number.
- Columns:

| Column | Definition |
|---|---|
| # · Challenge | Rank and name |
| Completed | `[Challenge Completions]` in the period |
| Share | Completions ÷ completions of all challenges in the selection |
| % of members (orange) | Completions ÷ `[Completion Members]` at the period end (the challenge's reach) |
| Ambassador … Starter (tier dots) | Completions by tier. Tiers excluded by the slicer show "—" |
| Months active | Months in the period with ≥ 1 completion |

- **No enrolled, in-progress or completion-rate columns per challenge.** The source (`Fact_EngagementCompletion`) only has `COMPLETED_CUST_CNT`; no table in the model has enrolments or in-progress counts per challenge. To add them, ask the data team for `IN_PROGRESS_CUST_CNT` (and ideally `ENROLLED_CUST_CNT`) by challenge, month, tier and Wizz flag. The footnote states this.
- The visual follows the period, tier and product filters. When nothing matches, it shows "No challenge completions for the current selection".
- **Sorting.** HTML can't sort when a column header is clicked. If click-to-sort is required (as in the prototype), use a native **Table** instead:
  - Columns: `Fact_EngagementCompletion[ENGAGEMENT_NAME]` and `[Challenge Completions]`.
  - Add a visual filter `ENGAGEMENT_TYPE = Challenge`.
  - Style it with the chart-card defaults and a data bar on completions.

The list is capped at 50 rows to keep the HTML text small. Uncapped, the all-months list (185 challenges) produced about 44K characters.

### 8.1 Bookmarks: "By tier" and "By challenge"

1. **Group the visuals** (Selection pane, Ctrl+click, then Group):
   - `View · By tier` = tier table (11) + both line charts (12, 13).
   - `View · By challenge` = Most completed challenges (14).
2. **Create the bookmarks** (View → Bookmarks):
   - Show `View · By tier` and hide `View · By challenge`. Add a bookmark and rename it **By tier**.
   - Hide `View · By tier` and show `View · By challenge`. Add a bookmark and rename it **By challenge**.
   - For each bookmark (… menu): turn **Data off**, **Display on**, **Current page on**, and choose **Selected visuals** after selecting only the two groups. Then slicer and period choices are kept when switching views, and the shell isn't affected.
3. **Bookmark group:** select both bookmarks, then … → **Group**, and name it **Challenges view**.
4. **Navigator:** Insert → Buttons → Navigator → **Bookmark navigator**, at **32 · 260 · 1316 · 166**. Bookmarks: *Challenges view*. Style it like the Period tiles (selected `#160D0A` with white text; default `#FFFDFC` with a `#E5D8D3` border and `#4A403C` text; Arial 10 bold).
5. Leave **By tier** active before saving, so the page opens on the tier view.

Don't capture the Collection/Redemption bookmarks on this page.

---

## 9. Validation

1. Switch Period between MTD, PM, YTD and LTD:
   - The KPI strip, the table and the list should change.
   - *Never enrolled* only changes when the **End month** changes, because it always looks at all history up to the end month.
2. Select one tier:
   - The table shows a single row with no Total.
   - The list re-ranks for that tier.
   - The charts show one line.
3. Select Product = Wizz:
   - Completions are small (for example 989 YTD).
   - In months with no Wizz completions, the list shows the empty-state message.
4. Check these YTD values: 195,160 completed, 348,277 enrolled, 56.0% rate, 2.09 enrolled per member, 1.17 completed per member.
   - Completion rate: Starter 51%, Explorer 61%, Insider 57%, Elite 69%, Ambassador 72%.
   - Enrolment rate 89.8% and never enrolled 9.8%.
   - By tier, enrolment rate / never enrolled: Ambassador 100% / 0%, Elite 99.9% / 0.05%, Insider 98.7% / 1.1%, Explorer 95.7% / 4.1%, Starter 86.3% / 13.2%.
5. Make sure the tier colours in the native charts match the dots in the table.

---

## 10. Known limitations

- "Enrolled" is a proxy (completed + in progress + expired). Label it clearly and replace it when the source has a real enrolment field.
- Completion rate depends on the length of the period (see §2).
- There is no per-challenge enrolment, in-progress count or completion rate.
- "Never enrolled" only covers history from Sep 2025 onwards.
- Enrolment rate and never enrolled come from `Fact_Completion`, and the other measures come from the overview table. They reconcile, apart from small in-progress differences (see §2).
- The enrolment rate may reflect automatic enrolment rather than member choice (see §2).
- The completion counts in the overview and per-challenge tables differ slightly. Each visual uses one source consistently.
- The prototype's figures are mock values. Don't use them as validation targets.

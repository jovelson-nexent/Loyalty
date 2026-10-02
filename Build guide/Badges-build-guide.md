# Badges: step-by-step build guide

This guide builds on the Challenges guide. The page uses the same shell, layout grid, styling and two-view bookmark pattern:

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
| Title | *ENGAGEMENT* eyebrow, the title **Badges**, and the dynamic "Showing" chips |
| KPI strip | **Mirrors Challenges:** badges enrolled, enrolment rate, enrolled per member, badges completed, completion rate, completed per member |
| Tier table | **Mirrors Challenges:** one row per tier plus Total, with Enrolment / Completion / Never enrolled columns (view "By tier") |
| Trend charts | Badges completed per member per month by tier, and badge completion rate by tier (native line charts, view "By tier") |
| Most earned badges | Ranked HTML list: earned, share, reach, level split, earned by tier, months. Scrollable, top 50 (view "By badge") |

Differences from the prototype:
- The prototype has 4 KPI cards. The page uses the same six cells and table columns as Challenges, so both pages read the same way. The earlier "no badge / earner rate / level mix" measures stay in the model (§2) for later use.
- The prototype's "Most popular badges" bar + line combo becomes an HTML list, matching the Challenges page. The data has 54 badges, and a 54-bar chart can't be read.
- The prototype's **Wizz badges** chart is not built. See §10.

---

## 2. Data definitions (read first)

| Field | Behaviour | Measure |
|---|---|---|
| Completed badges (overview and `Fact_Completion`) | Monthly flow: badges earned in the month. Summed over the period | `[Badges Completed]` |
| Per-badge completions (`Fact_EngagementCompletion`, type = Badge) | Monthly flow per badge name and **level** (1/2/3, plus Bronze/Silver/Gold for 6 badges) | `[Badge Completions]`, `[Badge Completions L1/L2/L3]` |
| In-progress badges | Month-end snapshot (period end month only) | `[Badges In Progress]` |
| Expired badges | Monthly flow (0 in every month) | `[Badges Expired]` |

Measures that mirror Challenges (folder `_Badges & Challenges\Badges`):

| Measure | Definition (same logic as the Challenges measure) |
|---|---|
| `[Badges Enrolled]` | Completed in the period + in progress at period end + expired in the period (**proxy**, like `[Challenges Enrolled]`) |
| `[Badge Completion Rate]` | `[Badges Completed]` ÷ `[Badges Enrolled]` |
| `[Badges Enrolled per Member]` | `[Badges Enrolled]` ÷ `[Completion Members]` |
| `[Badges Completed per Member]` | `[Badges Completed]` ÷ `[Completion Members]` |
| `[Badge Enrolment Rate]` | % of customers at period end with ≥ 1 badge (completed, in progress or expired) in any month of the period. Customer-level (`Fact_Completion`) |
| `[Never Enrolled in Badge %]` | % of customers at period end with no badge activity up to the period end |
| `[Badge Completion Rate Label Color]` | Transparent label when the rate rounds to 100% (same as Challenges; not needed today, no tier/month reaches 100%) |

**Read this before presenting the badge completion rate.** The formula is identical to Challenges, but the data behaves differently:
- **In progress is large and only grows.** Per customer, `IN_PROGRESS_BADGES` never goes down (for example 6 → 10 → 15 → 17). Totals rise from 1.4M (Sep 2025) to 3.5M (Aug 2026), about 21 per member. Badges appear to be always-on: every badge a member has touched stays "in progress" until completed, and nothing expires.
- **So the rate is low and falls over time.** Monthly rate: about 12–25% in Sep 2025, about 3–6% in Aug 2026. YTD 23%, LTD 33%, MTD Aug 4%. The decline is the in-progress stock growing, not members completing less.
- **Enrolment rate is about 100%** (99.6% YTD; never enrolled 0.4%). Almost every member has at least one badge in progress, which again suggests automatic enrolment. It confirms the page shape but doesn't discriminate between tiers.
- Ask the programme team: is a badge "in progress" when the member has made real progress, or as soon as the badge is visible? If the second, a better denominator is badges with progress > 0.

Other measures (kept in the model, not on the page now):

| Measure | Definition |
|---|---|
| `[Badges Earned per Member]` | `[Badges Completed]` ÷ `[Completion Members]` (members at period end). Same logic as challenges completed per member |
| `[Members with No Badge]` | Members at period end (distinct `CUST_ID` in `Fact_Completion`) who earned **no badge in any month of the period**. Earnings count whatever tier the member held at the time |
| `[Members with No Badge %]` | The count above ÷ members at period end |
| `[Badge Earner Rate]` | % of members at period end who earned ≥ 1 badge in the period (= 1 − no badge %) |
| `[Never Earned Badge %]` | % of members at period end with no badge in any month **up to** the period end (history starts Sep 2025) |
| `[Badge Completions L1]`, `L2`, `L3` | Per-badge completions at level 1/2/3 (Bronze/Silver/Gold included) |
| `[Badge Level 2+ Share]` | Level 2 + 3 completions ÷ all badge completions: how far members progress beyond the entry level |

The existing `[Members with No Badge Completed]` (overview) only looks at the **last month** of the period. For YTD it says 111K members have no badge, which is really "no badge in August". Use the customer-level measures above instead if you need it.

What this means for the page:
- **Completion rate depends on the period length** (as on Challenges): MTD Aug 2026 4%, YTD 23%, LTD 33%.
- **Two sources for completions.**
  - `[Badges Completed]` (overview) and `Fact_Completion` match exactly in every month.
  - `[Badge Completions]` (per badge) is slightly higher: YTD 1,065,886 vs 1,035,309 (+3%).
  - The KPI strip and the tier table use the overview. The list uses the per-badge source, and its share % uses its own total.
- **Customers in both programmes (not duplicates).** `Fact_Completion` is unique on customer + month + `WIZZ_FLG`. In Aug 2026, 356 customers have both a Card Avantaj row and a Wizz row. The customer-level measures (no badge, earner rate, never earned) count **people** (distinct `CUST_ID`). With Product = All, their base is 356 lower than `[Members]`, which counts memberships.
- **Reach** in the list = level-1 completions ÷ members at period end. This assumes a member earns each level of a badge once. If level 1 can be earned again in a later month, reach is overstated. Confirm with the programme team.

---

## 3. Duplicate the page

1. Right-click **Challenges** and choose **Duplicate page**. Rename the copy **Badges**.
2. Keep the shared shell: header, logo, sidebar page navigator, Period tiles, End month / Tier / Product slicers, and the Reset button.
3. Keep all visuals: the positions are the same (§4), only the measures change. Bookmarks are not copied with the page, so the navigator will be empty until you create the *Badges view* group (§8.1). **Don't** point it at *Challenges view*.
4. In the title visual, replace the measure with `_Measures[HTML Badges Title]`.
5. In the KPI visual, replace the measure with `_Measures[HTML Badges KPI Strip]`.
6. Replace the tier table measure with `_Measures[HTML Badges Tier Table]` and the list measure with `_Measures[HTML Top Badges]`.
7. In the sidebar page navigator, make sure the Badges entry points to this page.

---

## 4. Layout

Same grid as Challenges. The area under the KPI strip (Y 356 → 884) switches between two views using bookmarks (§8.1).

```
Y 144  │ Title 76                                  [ By tier | By badge ] │
Y 228  │ KPI strip 112 (6 cells)                                          │
Y 356  ├───────────── View "By tier" ─────────────┬────── "By badge" ─────┤
       │ Badges by tier (HTML) 248                │ Most earned badges    │
Y 620  │ Earned/member/month   │ Earner rate      │ (HTML) 528            │
Y 884  └───────────────────────┴──────────────────┴───────────────────────┘
         X 224                   X 908                       X 1576
```

| # | Object | Type | H · W · X · Y | View |
|---|---|---|---|---|
| 1–7 | Shared shell | Copied | unchanged | both |
| 8 | Page title | HTML `HTML Badges Title` | **76 · 1352 · 224 · 144** | both |
| 9 | Bookmark navigator | Native | **32 · 260 · 1316 · 166** | both |
| 10 | KPI strip | HTML `HTML Badges KPI Strip` | **112 · 1352 · 224 · 228** | both |
| 11 | Tier table | HTML `HTML Badges Tier Table` | **248 · 1352 · 224 · 356** | By tier |
| 12 | Earned per member per month | Native line chart | **264 · 668 · 224 · 620** | By tier |
| 13 | Earner rate by tier | Native line chart | **264 · 668 · 908 · 620** | By tier |
| 14 | Most earned badges | HTML `HTML Top Badges` | **528 · 1352 · 224 · 356** | By badge |

The HTML tables scale to the visual: the unit is the smaller of width ÷ 1352 and height ÷ 248 (tier table) or ÷ 528 (list).

---

## 5. Title and KPI strip

**Title (8).** HTML Content, HTML standard, **76 · 1352 · 224 · 144**. Eyebrow *ENGAGEMENT*, title **Badges** (Georgia 28), and chips for period, tiers and products.

**KPI strip (10).** HTML Content, **112 · 1352 · 224 · 228**. Six joined cells, the same as Challenges:

| Cell | Measure | Format | Caption |
|---|---|---|---|
| Badges enrolled | `[Badges Enrolled]` | K/M | "Completed + in progress" |
| Enrolment rate (green) | `[Badge Enrolment Rate]` | 0% | "Members with ≥ 1 badge" |
| Enrolled / member | `[Badges Enrolled per Member]` | 0.00 | Period label |
| Badges completed | `[Badges Completed]` | K/M | Period label |
| Completion rate (green) | `[Badge Completion Rate]` | 0% | "Completed ÷ enrolled" |
| Completed / member | `[Badges Completed per Member]` | 0.00 | Period label |

Check values (YTD · Jan–Aug 2026, all tiers and products): **4.51M · 100% · 27.04 · 1.04M · 23% · 6.21**.

---

## 6. Badges by tier (HTML table)

- HTML Content → `_Measures[HTML Badges Tier Table]` → HTML standard, **248 · 1352 · 224 · 356**.
- h2 "Badges by tier" in Georgia 24 px (≈ 18 pt); set the native chart titles to **Georgia 18 pt**.
- Rows from Ambassador down to Starter, each with its tier colour dot (`[Tier_Selected]`). No bars. The **Total** row appears only when more than one tier is visible.
- Columns are the same as the Challenges tier table:

| Group | Column | Measure |
|---|---|---|
| — | Tier (dot) | `Dim_Tier[Tier]` |
| — | Members (period end) | `[Completion Members]` |
| Enrolment | Enrolled | `[Badges Enrolled]` |
| Enrolment | Per member (orange) | `[Badges Enrolled per Member]` |
| Enrolment | Enrolment rate | `[Badge Enrolment Rate]` |
| Completion | Completed | `[Badges Completed]` |
| Completion | Per member (orange) | `[Badges Completed per Member]` |
| Completion | Completion rate | `[Badge Completion Rate]` |
| — | Never enrolled | `[Never Enrolled in Badge %]` |

---

## 7. Trend charts (native)

Apply the shared chart-card defaults (Collection & redemption guide §6.2): background `#FFFDFC`, border `#E5D8D3` 1 px radius 10, padding 12, title Georgia 18 pt `#160D0A`, axis titles off, gridlines `#EFE5E2`.

- X-axis: `Dim__Calendar[YearMonth]`, categorical, sorted by `YearMonth Seq`.
- Legend: `Dim_Tier[Tier]`, bottom, Arial 9, `#4A403C`. Set the series colours once in Format → Lines → Colours: Ambassador `#F21D2F`, Elite `#4A3D8F`, Insider `#1F6B52`, Explorer `#F2811D`, Starter `#5C3D2E`. Lines 2 px, small markers.

Tip: on the Challenges page, copy both line charts (Ctrl+C) and paste them here (Ctrl+V). The colours and formatting carry over; just swap the Y-axis measure.

### 7.1 Badges completed per member per month (12)
- **264 · 668 · 224 · 620**.
- Y-axis: `[Badges per Member per Month]` (existing), format `0.00`.
- Title: **Badges completed per member per month**. Subtitle: **Completions ÷ member-months**.
- Typical values: Starter ≈ 0.6, Explorer ≈ 1.3, Insider ≈ 1.5, Elite ≈ 1.6, Ambassador 1.7–4.4.
- No label colour rule is needed (all values are above 0.1).

### 7.2 Badge completion rate by tier (13)
- **264 · 668 · 908 · 620**.
- Y-axis: **replace** `[Challenge Completion Rate]` with `[Badge Completion Rate]`, format `0%`.
- **Don't fix the axis from 0 to 1** as on Challenges: the values are 3–25%, and the lines would sit flat at the bottom. Leave the range on Auto (start 0).
- Title: **Badge completion rate**. Subtitle: **Completed in month ÷ (completed + in progress at month end)**.
- Data label colour (optional, for symmetry): fx → Field value → `[Badge Completion Rate Label Color]`. No point reaches 100% today, so nothing is hidden.
- Values by month: Sep 2025 12% (Starter) – 25% (Elite); Aug 2026 Starter 3.1%, Explorer 4.8%, Insider 4.6%, Elite 5.1%, Ambassador 5.8%. The downward slope comes from the growing in-progress stock (§2). Unlike Challenges, there's no 100% artefact before Jul 2026, because badges have in-progress values in every month.

Ambassador has only 362 members, so its lines jump. That is expected.

---

## 8. Most earned badges (HTML list)

- HTML Content → `_Measures[HTML Top Badges]` → HTML standard, **528 · 1352 · 224 · 356** (view "By badge").
- Top 50 badges by completions, scrollable, with a two-row sticky header (group row + column row). The subtitle says "Top 50 of N".

| Group | Column | Definition |
|---|---|---|
| — | # · Badge | Rank (ties share a number) and name (hover for the full name) |
| Completions | Earned | `[Badge Completions]`, all levels |
| Completions | Share | Earned ÷ all badge completions in the selection |
| Completions | Reach (orange) | Level-1 completions ÷ `[Completion Members]` at period end (see the assumption in §2) |
| By level | Level 1 / 2 / 3 | `[Badge Completions L1/L2/L3]`; "—" when the badge has no such level |
| By tier | Ambassador … Starter (dots) | Earned by tier; tiers excluded by the slicer show "—" |
| — | Months | Months in the period with ≥ 1 completion (seasonal badges such as *Black Friday Deal Hunter* or *Cupid's Shopper* show 1–2) |

- The list follows the period, tier and product filters. With no data it shows "No badge completions for the current selection".
- For click-to-sort, use a native **Table** instead (`ENGAGEMENT_NAME` + `[Badge Completions]`, visual filter `ENGAGEMENT_TYPE = Badge`).

### 8.1 Bookmarks: "By tier" and "By badge"

Same steps as the Challenges guide §8.1:

1. Group the visuals in the Selection pane:
   - `View · By tier` = tier table (11) + both line charts (12, 13).
   - `View · By badge` = Most earned badges (14).
2. Create two bookmarks, **By tier** and **By badge** (show one group, hide the other). For each: **Data off**, **Display on**, **Current page on**, **Selected visuals** (only the two groups).
3. Group both bookmarks as **Badges view**.
4. Point the bookmark navigator (9) at *Badges view*. Same style as on Challenges.
5. Leave **By tier** active before saving.

---

## 9. Validation

1. Switch Period between MTD, PM, YTD and LTD:
   - KPI strip, table and list change.
   - *Never enrolled* only changes when the **End month** changes.
2. Select one tier: one row with no Total; the list re-ranks; the charts show one line.
3. Check the YTD values by tier (all products):

| Tier | Members | Enrolled | Per member | Enrolment rate | Completed | Per member | Completion rate | Never enrolled |
|---|---|---|---|---|---|---|---|---|
| Ambassador | 362 | 12,904 | 35.65 | 100% | 2,841 | 7.85 | 22% | 0% |
| Elite | 3,670 | 154,167 | 42.01 | 100% | 49,554 | 13.50 | 32% | 0% |
| Insider | 13,958 | 551,351 | 39.50 | 100% | 143,252 | 10.26 | 26% | 0% |
| Explorer | 38,412 | 1,357,765 | 35.35 | 100% | 329,586 | 8.58 | 24% | 0% |
| Starter | 110,411 | 2,434,859 | 22.05 | 99% | 510,076 | 4.62 | 21% | 1% |
| **Total** | **166,813** | **4,511,046** | **27.04** | **100%** | **1,035,309** | **6.21** | **23%** | **0%** |

4. Completion rate by period (all tiers): MTD Aug 2026 3.9% · PM 4.2% · YTD 23.0% · LTD 33.3%.
5. Top 3 YTD in the list: Early Bird 97,753 · Weekend Warrior 74,749 · Supermarket Shopper 64,761.

Ambassador completes fewer badges per member than Elite and Insider (7.85 vs 13.50). This is in the data, not an error; worth a question to the programme team.

---

## 10. Known limitations

- "Enrolled" is a proxy (completed + in progress + expired), as on Challenges.
- **In-progress badges** grow every month and never decrease, so the completion rate is low and trends down (see §2). Confirm the definition of "in progress" with the programme team.
- Enrolment rate is about 100%, so it doesn't separate tiers.
- **Wizz badges chart** (prototype: % of Wizz cardholders per Wizz badge by level) is not built. In the per-badge source, `WIZZ_FLG = 1` appears for only 5 badges in a single month (Wizz Addict, Shopping Hunter, Global Spender, Digital Traveler, Duty Free Expert, plus a few Eurotrip rows), about 1K completions in total. With the Product slicer = Wizz, the list already shows them. Revisit when the Wizz flag is reliable.
- Two completion sources differ by about 3% (see §2). Each visual uses one source consistently.
- **Reach** assumes each badge level is earned once per member.
- "Never enrolled" covers history from Sep 2025 only.
- The prototype's figures are mock values. Don't use them as validation targets.

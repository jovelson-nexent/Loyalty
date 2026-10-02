# Wizz reconciliation: step-by-step build guide

This guide builds on the Challenges, Badges, Trends and Tier progression guides. The page uses the same shell, grid and styling:

- Canvas: 1600 × 900, with the shared header, sidebar and slicers.
- Style: Nexent Fusion.

Conventions:
- Positions are given as **H · W · X · Y**.
- All five visuals are HTML Content. This page has no native chart.
- The **Source check** table moved to the **Data health** page (see `Data-health-build-guide.md`).
- All model objects in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

| Block | Content |
|---|---|
| Title | *STRUCTURE* eyebrow, the title **Wizz reconciliation**, and "Showing" chips (period, "Wizz cardholders", "All tiers"). A red chip warns when a slicer is ignored or excludes Wizz |
| KPI strip | Wizz cardholders, Net cashback, Reversed %, Redeemed, Effective cashback rate, Earned at 2% |
| Cashback by month | Collected, reversed and net cashback per month, net split into 1% and 2% rates, and redemptions with % of net. Has a Total row |
| Wizz challenges | Challenge completions by Wizz cardholders: share and reach |
| Wizz badges | Badge completions by Wizz cardholders: share, reach, and Level 1/2/3 (Bronze/Silver/Gold) |

### Differences from the prototype

| Prototype | PBIX | Why |
|---|---|---|
| Cashback rate table with **1% / 2% rows** | One row per month, with **Net 1% / Net 2% columns** | The rate is on cashback only. Redemptions have no rate, so they can't be split into 1% and 2% rows. The months view also shows the trend. |
| The note "cashback rate not available" | The rate is shown | `Fact_WizzCoinTransactions[CASHBACK_RATE_PCT]` is now loaded. |
| Top challenges: participants, completed, **completion rate** | Completed, Share, **Reach**, Months | There is no enrolment per challenge for Wizz. Reach = completions ÷ Wizz members. |
| Badges completion-rate **bar chart** | HTML table with levels | Same reason as challenges (no enrolment), and no bars, matching the other pages. |
| — | **Source check** table, on the **Data health** page | Tests whether the Wizz detail feeds agree with the monthly summary. It is a data-quality check, so it sits with the other checks. |

---

## 2. How the numbers work (read first)

### 2.1 Scope

- **Product slicer.**
  - With All or Wizz selected, every visual shows Wizz cardholders (`Dim_Wizz[WIZZ_FLG] = 1`).
  - With **Card Avantaj** selected, the visuals show "Not applicable" and the title shows a red chip.
  - The page does this through the hidden measure `[Wizz In Scope]`.
- **Tier slicer is ignored.** The cashback feed has no tier column, so tier can't be applied consistently across the page.
  - When a tier is selected, the title shows the red chip **"Tier filter not applied"**.
  - Wizz has no tiers by business design. Its rows in `Fact_CustomerView` carry a placeholder Starter tier, so a tier filter would be meaningless here.
- **Period.** Period tiles and End month work as on the other pages.

### 2.2 The three Wizz sources

| Source | Covers | Used for |
|---|---|---|
| `Fact_WizzCoinTransactions` | Cashback collected and reversed, by rate. **May – Sep 2026** | Cashback by month and the cashback KPIs |
| Redemptions with `WIZZ_FLG = 1` (all "Point transfer to Wizz Air") | May – Sep 2026 | Redeemed and % of net |
| `Fact_Overview` (monthly loyalty summary) with Wizz rows | **Aug 2026 only** (1,470 members) | Cardholders, card spend, and the Reach denominators |

How far the first two feeds agree with the summary is shown on the **Data health** page (Wizz source check).

The End month list follows `Fact_Overview`, which ends in Aug 2026. The Sep 2026 cashback and redemptions are loaded but can't be selected. The Cashback table footnote flags this automatically.

### 2.3 Definitions

| Measure | Definition |
|---|---|
| Net cashback | Collected − reversed (coins) |
| Reversed % | Reversed ÷ collected |
| Net 1% / Net 2% | Net cashback by `CASHBACK_RATE_PCT` |
| Earned at 2% | Net 2% ÷ net cashback |
| Redeemed % of net | Wizz redemptions ÷ net cashback, same months. This can exceed 100% (May 2026 = 263%) because members also redeem coins from other sources |
| Effective cashback rate | Net cashback ÷ Wizz card spend, **only months with Wizz rows in the summary** (today: Aug 2026) |
| Reach (challenges) | Completions ÷ Wizz members at the end of the period |
| Reach (badges) | Level-1 completions ÷ Wizz members at the end of the period |

### 2.4 New model objects

Folder `_Wizz` in `_Measures`:

| Measure | Notes |
|---|---|
| `Wizz In Scope` *(hidden)* | 1 when the Product filter includes Wizz |
| `Wizz Completion Members` *(hidden)* | Reach denominator |
| `Wizz Cashback Collected` / `Reversed` / `Net Cashback` | Coins |
| `Wizz Net Cashback 1%` / `2%` | By rate |
| `Wizz Cashback Reversal %` | |
| `Wizz Cashback Transactions` / `Wizz Cashback Reversal Transactions` | Counts |
| `Wizz Coins Redeemed` / `Wizz Redemption Transactions` / `Wizz Redeemed % of Net Cashback` | |
| `Wizz Cardholders` / `Wizz Card Spend (RON)` | From the monthly summary |
| `Wizz Effective Cashback Rate` | See 2.3 |
| `Wizz Challenge Completions` / `Wizz Badge Completions` | |

Folder `_Report\HTML`: `HTML Wizz Title`, `HTML Wizz KPI Strip`, `HTML Wizz Cashback Table`, `HTML Wizz Challenges`, `HTML Wizz Badges`. (`HTML Wizz Source Check` is used on the Data health page.)

---

## 3. Duplicate the page

1. Right-click **Tier progression** and choose **Duplicate page**. Rename the copy **Wizz reconciliation**.
2. Keep the shared shell: header, logo, sidebar page navigator, Period tiles, End month / Tier / Product slicers, and the Reset button.
3. Delete everything else, including the Monthly moves chart.
4. In the title visual, replace the measure with `_Measures[HTML Wizz Title]`.
5. In the sidebar page navigator, make sure the Wizz reconciliation entry (under *Structure*) points to this page.

---

## 4. Layout

```
Y 144  │ Title 76                                                           │
Y 228  │ KPI strip 96                                                       │
Y 340  ├──── Cashback by month (HTML) ───────────────────────────────────────┤
       │ 264 · 1352 · 224 · 340                                             │
Y 620  ├──── Wizz challenges (HTML) ─────────┬──── Wizz badges (HTML) ───────┤
       │ 264 · 664 · 224 · 620               │ 264 · 672 · 904 · 620         │
Y 884  └─────────────────────────────────────┴───────────────────────────────┘
```

| # | Visual | Type | H · W · X · Y |
|---|---|---|---|
| 8 | Title | HTML Content | 76 · 1352 · 224 · 144 |
| 9 | KPI strip | HTML Content | 96 · 1352 · 224 · 228 |
| 10 | Cashback by month | HTML Content | 264 · **1352** · 224 · 340 |
| 11 | Wizz challenges | HTML Content | 264 · 664 · 224 · 620 |
| 12 | Wizz badges | HTML Content | 264 · 672 · 904 · 620 |

**If you already built the page:** delete the Source check visual (or cut and paste it onto the Data health page), then widen Cashback by month to W 1352. Nothing else moves.

The gap between columns is 16 px (224 + 664 + 16 = 904).

The tables scale to the visual: the unit is the smaller of width ÷ 664 (left) or ÷ 672 (right), and height ÷ 264. At full width, Cashback by month keeps its font size (height limits the unit) and the columns spread across the row. Headers are sticky, and the body scrolls when there are more rows.

For all five visuals, set **Background off, Border off, Shadow off, Padding 0**. Each HTML draws its own card.

---

## 5. Title

HTML Content, **76 · 1352 · 224 · 144**: `_Measures[HTML Wizz Title]`.

- Chips: period, "Wizz cardholders", "All tiers".
- Red chips:
  - **"Tier filter not applied"** when a tier is selected.
  - **"Product = Card Avantaj · excludes Wizz"** when Card Avantaj is selected.

---

## 6. KPI strip

HTML Content, **96 · 1352 · 224 · 228**: `_Measures[HTML Wizz KPI Strip]`.

| Card | Value | Footer |
|---|---|---|
| Wizz cardholders (dark) | Members at period end | "Members at end of …", or the first month with Wizz members when the period is earlier |
| Net cashback (orange) | Net, K format | Collected · reversed |
| Reversed (red) | Reversed % | Reversal transactions |
| Redeemed (orange) | Coins redeemed, K format | % of net cashback |
| Effective cashback rate (green) | 0.00% | Months used |
| Earned at 2% (orange) | % of net | "the rest at 1%" |

With Card Avantaj selected, the strip shows one "Not applicable" banner.

---

## 7. Cashback by month (HTML)

HTML Content, **264 · 1352 · 224 · 340**: `_Measures[HTML Wizz Cashback Table]`.

- **Columns.**
  - Month
  - Collected, Reversed, Rev. %, **Net** (bold)
  - Net 1%, Net 2%
  - **Redeemed** (orange), % of net
- **Rows.** One row per month with cashback or redemptions. A **Total** row appears when more than one month is shown.
- **Footnote.** Explains redemptions and adds "Sep 2026 cashback loaded, after the latest End month" while that is true.

---

## 8. Source check (moved)

The Source check is now on the **Data health** page, as **Wizz source check** (see `Data-health-build-guide.md`). It no longer depends on the Product slicer.

---

## 9. Wizz challenges (HTML)

HTML Content, **264 · 664 · 224 · 620**: `_Measures[HTML Wizz Challenges]`.

- **Columns.** #, Challenge, **Completed**, Share, **Reach** (orange), Months.
- **Sort.** By completions. The table shows up to 50 rows and scrolls.

---

## 10. Wizz badges (HTML)

HTML Content, **264 · 672 · 904 · 620**: `_Measures[HTML Wizz Badges]`.

- **Columns.** #, Badge, **Earned** (all levels), Share, **Reach** (level 1 ÷ members), Level 1, Level 2, Level 3, Months.
- "—" means no completions at that level.

---

## 11. Validation

End month **Aug 2026**, Product = All, Tier = All.

### 11.1 KPI strip

| Card | MTD | YTD / LTD |
|---|---|---|
| Wizz cardholders | 1,470 | 1,470 |
| Net cashback | 54.5K | 138.7K |
| Collected · reversed | 55.3K · 797 | 140.9K · 2.2K |
| Reversed % | 1.4% | 1.6% |
| Redeemed | 37.5K (69% of net) | 77.8K (56% of net) |
| Effective cashback rate | 1.00% | 1.00% (Aug only) |
| Earned at 2% | 24% | 22% |

PM (Jul 2026): cardholders "—", with the footer "First in the loyalty summary: Aug 2026". Net cashback is 43.8K.

### 11.2 Cashback by month (YTD)

| Month | Collected | Reversed | Net | Net 1% | Net 2% | Redeemed | % of net |
|---|---|---|---|---|---|---|---|
| May 2026 | 1,870 | 22 | 1,848 | 1,366 | 483 | 4,853 | 263% |
| Jun 2026 | 38,929 | 408 | 38,520 | 29,792 | 8,728 | 11,440 | 30% |
| Jul 2026 | 44,792 | 1,001 | 43,792 | 35,445 | 8,346 | 23,934 | 55% |
| Aug 2026 | 55,342 | 797 | 54,545 | 41,465 | 13,079 | 37,536 | 69% |
| **Total** | 140,933 | 2,228 | 138,705 | 108,069 | 30,636 | 77,763 | 56% |

### 11.3 Source check

See the Data health guide (§8).

### 11.4 Challenges and badges (Aug 2026)

| Challenge | Completed | Reach |
|---|---|---|
| Wizz LOY Has Wallet | 365 | 24.8% |
| Wizz LOY Challenge First Wizz Purchase | 269 | 18.3% |
| Wizz LOY Challenge First Travel Transaction 1000 | 181 | 12.3% |
| Wizz Challenge Explore Europe | 147 | 10.0% |
| ISS MGM Challenge Wizz 13072026 | 14 | 1.0% |

| Badge | Earned | L1 / L2 / L3 |
|---|---|---|
| Wizz Addict | 272 | 272 / — / — |
| Shopping Hunter | 258 | 258 / — / — |
| Global Spender | 216 | 188 / 26 / 2 |
| Digital Traveler | 212 | 212 / — / — |
| Eurotrip | 106 | 106 / — / — |
| Duty Free Expert | 60 | 54 / 5 / 1 |

### 11.5 Behaviour checks

- **Product = Card Avantaj.** Every visual shows "Not applicable", and the title shows the red chip.
- **Tier = Elite.** The numbers are unchanged, and the title shows "Tier filter not applied".
- **End month before Aug 2026.**
  - Cardholders show "—".
  - Cashback still shows its months.

---

## 12. Known limitations and open questions

The open questions now live on the **Data health** page (table `'Data Questions'`), so there is one list for the whole report. The Wizz ones:

| ID | Question |
|---|---|
| W1 | Coins redeemed: the feed is 46% of the summary (37.5K vs 82K, Aug 2026), while Card Avantaj reconciles at 96%. Are some Wizz redemptions missing the flag or label? |
| W2 | Coins collected: cashback is 26% of the summary (54.5K vs 213K). What are the other coins? |
| W3 | Cashback starts in May 2026, but Wizz rows appear in the summary only from Aug 2026. Where were Wizz members before? |
| W4 | Effective rate 1.00% while 22% of net cashback is earned at 2%. Which spend is eligible? |
| W5 | Sep 2026 cashback and redemptions are loaded, but the summary ends Aug 2026. They appear here once `Fact_Overview` has Sep 2026. |

The small completion gap (feed − summary = −13 challenges, −17 badges in Aug 2026) is in the Wizz source check; Card Avantaj has a similar gap, so it is likely cut-off timing.
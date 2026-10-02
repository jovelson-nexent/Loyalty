# Tier progression: step-by-step build guide

This guide builds on the Challenges, Badges and Trends guides. The page uses the same shell, grid and styling:

- Canvas: 1600 × 900, with the shared header, sidebar and slicers.
- Style: Nexent Fusion.

Conventions:
- Positions are given as **H · W · X · Y**.
- HTML Content is used for the title, KPI strip and both tables. A native chart shows the monthly moves.
- All model objects in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

| Block | Content |
|---|---|
| Title | *STRUCTURE* eyebrow, the title **Tier progression**, and "Showing" chips (period, tiers, products) |
| KPI strip | Members (with net change), Upgrades, Downgrades, Joined, Left programme, Starter → Ambassador journey |
| Tier flow | HTML table per tier: Start + In (joined, up, down) − Out (up, down, left) = End, net change, monthly up and down rates |
| Monthly moves | Native column chart: upgrades above zero, downgrades below zero, by month |
| Progression steps | HTML table per one-tier step: upgrades and monthly rate in the period; months in the previous tier; profit per member before vs after the upgrade, compared with members who stayed |

### Differences from the prototype

**Members and active participants.** The prototype's "Members in program" and "Active participants" KPIs are already on the Executive summary.

- This page replaces them with **Members + net change**, which is the stock.
- The other cards (upgrades, downgrades, joined, left) are the movements that explain the change.

**Tier funnel and migration chart → Tier flow table.** The prototype shows members vs active per tier, plus entries vs exits per tier, in two charts.

- Those charts can't add up: entries and exits ignored members who join or leave the programme.
- The flow table **reconciles**: for every tier, Start + In − Out = End, exactly (checked for LTD, YTD, MTD, each tier and each product).

**Time to progression table → Progression steps table.**

- **Kept:** the same 4 one-tier steps, upgrades and average time.
- **Added: monthly upgrade rate.** It is comparable across periods; raw counts depend on tier size.
- **Added: "in tier at start".** This share shows how much of the average time is a lower bound (§2.3).
- **Profit uplift post-transition** was "not available" in the prototype. It is now calculated, against a comparison group (§2.4).

**Transition timeline → Monthly moves chart.** The prototype had mock data (one line per tier). The page uses a diverging up/down column chart instead: it shows the Jun 2026 upgrade spike and the Jul 2026 start of downgrades at a glance.

**Profit around the upgrade (event study).** The Executive summary already has this chart (Profitability evolution). It isn't repeated here. The steps table gives the before/after numbers.

---

## 2. How the numbers work (read first)

### 2.1 Tier flow (reconciling)

Each month a member has one row in `Fact_TierMigrations`.

A row is **continuing** when the member was also in the programme the month before (`PREV_AS_OF_MONTH` = previous month end). Every member-month is then exactly one of these:

| Column | Meaning | How |
|---|---|---|
| Start | Members in the tier at the end of the month before the period | Rows at start month − 1 |
| Joined | New or returning members | Rows in the period that are **not** continuing (first month, or a gap) |
| In · Up | Moved up into the tier | Continuing rows, `TIER_MIGRATION` = Upgrade, `TIER` = tier |
| In · Down | Moved down into the tier | Same, Downgrade |
| Out · Up | Moved up out of the tier | Continuing rows, Upgrade, `PREV_TIER` = tier |
| Out · Down | Moved down out of the tier | Same, Downgrade |
| Left | In the tier one month, not in the programme the next | Member-months in the tier at m − 1 − continuing rows from the tier at m |
| End | Members in the tier at the end of the period | Rows at end month |

**Start + Joined + In − Out − Left = End**, for every tier and for the total.

**Monthly rate** = moves out ÷ member-months in the tier at the start of each month in the period. It is the average share of the tier that moves each month, so it is comparable between MTD, YTD and LTD.

**First month of history.** Sep 2025 is the first month in the data, so every row there looks "new". The flow therefore always starts from **Oct 2025**:

- **LTD** = Sep 2025 members → Aug 2026, with moves Oct 2025 – Aug 2026.
- **MTD / PM** with end month Sep 2025 has no moves. The table then says so instead of showing zeros.

**The tier slicer** works on the tier rows. In and Out are seen *from* the selected tiers. With one tier selected, the Total row shows that tier only, and In · Up ≠ Out · Up is expected.

### 2.2 Upgrades and downgrades: which count?

The KPI cards and the monthly chart count moves **out of** the tiers in context. With all tiers, out = in, so it doesn't matter. With one tier selected, the cards show that tier's members moving up or down, and the rate below each card is that tier's monthly rate.

### 2.3 Months in the previous tier (lower bounds)

`Avg months` = average of `PREV_TIER_MONTH_CNT` on the upgrade row. This is the existing `[Avg Months to Progress (Step)]`, the same value as the Executive summary chips.

The data only starts in Sep 2025. **46–68 %** of upgraders were already in their previous tier then (column *In tier at start*).

- For them, the months are counted from Sep 2025, so the real time is longer.
- Members who haven't upgraded yet are not in the average at all.
- **Both effects push the average down.** Read 4.1 months as "at least about 4 months".
- The full journey (21.8 months) is the sum of the 4 step averages, so it is also a lower bound.

These columns ignore the period, tier and product slicers. They use all history, as on the Executive summary.

### 2.4 Profit before vs after the upgrade

New calculated table `'Upgrade Profit Study'` (hidden). It is computed **once at refresh**, so the visual is instant.

- **Events.** Upgrades with a full 3 months of profit data on both sides. Today that is upgrades from **Dec 2025 – May 2026**; the window moves automatically as data is added.
- **Membership level.** Each event is a membership (customer + programme, `WIZZ_FLG`). Profit is read from `Fact_CustomerView` for that membership only, so a customer's card in the other programme is not mixed in. Each programme has its own tier.
- **Before** = average profit per member per month in months −3 to −1. **After** = months +1 to +3.
- **The upgrade month (month 0) is excluded.** It is the month the member qualified, so its spending is part of the reason for the upgrade, not a result of it.
- **Stayers** = members who stayed in the same from-tier in the same months, measured over the same windows.
  - Their profit falls 13–22 % between the two windows: seasonality and the general trend.
  - Without them, the upgraders' change would mix the effect of the upgrade with the calendar.
- **Uplift vs stayers** = (After − Before for upgraders) − (After − Before for stayers), in profit per member per month.

This is an **association, not a proven effect**: members who upgrade were already becoming more active. The table says so in its footnote.

### 2.5 New model objects

Folder `_Tier Migration\Flow` (table `_Measures`):

| Measure | Notes |
|---|---|
| `[Tier Flow Opening]`, `[Tier Flow Joined]`, `[Tier Flow Upgraded In]`, `[Tier Flow Downgraded In]`, `[Tier Flow Upgraded Out]`, `[Tier Flow Downgraded Out]`, `[Tier Flow Left]`, `[Tier Flow Closing]` | The flow columns (§2.1) |
| `[Tier Flow Net Change]` | Closing − Opening |
| `[Tier Flow Upgrade Rate (Monthly)]`, `[Tier Flow Downgrade Rate (Monthly)]`, `[Tier Flow Left Rate (Monthly)]` | Moves out ÷ exposure |
| `[Tier Moves Upgrades (Chart)]`, `[Tier Moves Downgrades (Chart)]` | Chart series; downgrades are negative, but the format hides the minus sign |
| `[Tier Flow Start Month]`, `[Tier Flow End Month]`, `[Tier Flow Exposure]` | Hidden helpers |

Folder `_Tier Migration\Progression`:

| Measure | Notes |
|---|---|
| `[Step Upgrades]`, `[Step Upgrade Rate (Monthly)]` | Per `'Progression Step'` row, in the period. These ignore the tier slicer, because the step defines the tiers |
| `[Step Upgraders In Tier Since Start %]` | Share of the step's upgraders already in the from-tier in Sep 2025 (LTD) |
| `[Upgrade Profit Before]`, `[Upgrade Profit After]`, `[Upgrade Profit Change %]`, `[Stayer Profit Change %]`, `[Upgrade Profit Uplift vs Stayers]`, `[Upgrade Profit Study Events]` | Read from `'Upgrade Profit Study'` |
| `[Stayer Profit Before]`, `[Stayer Profit After]` | Hidden |

Folder `_Report\HTML`:
- `[HTML Tier Title]`
- `[HTML Tier KPI Strip]`
- `[HTML Tier Flow Table]`
- `[HTML Tier Steps Table]`

Calculated table: `'Upgrade Profit Study'` (hidden). It has 48 rows: 4 steps × 2 groups × 6 event months.

The existing measures in `_Tier Migration` (`[Upgrades]`, `[Tier Entries]`, `[Net Tier Change]`, …) are unchanged.

- They are still used elsewhere.
- They don't reconcile, because they ignore members who join or leave.
- Use the `Tier Flow` measures for anything that must add up.

---

## 3. Duplicate the page

1. Right-click **Challenges** and choose **Duplicate page**. Rename the copy **Tier progression**.
2. Keep the shared shell: header, logo, sidebar page navigator, Period tiles, End month / Tier / Product slicers, and the Reset button.
3. Delete everything else: KPI strip, tier table, charts, Top Challenges table, bookmark navigator, and their bookmarks and groups.
4. In the title visual, replace the measure with `_Measures[HTML Tier Title]`.
5. In the sidebar page navigator, make sure the Tier progression entry points to this page.

---

## 4. Layout

```
Y 144  │ Title 76                                                           │
Y 228  │ KPI strip 96                                                       │
Y 340  ├──────── Tier flow (HTML) ─────────────┬── Monthly moves (native) ──┤
       │ 264 · 816 · 224 · 340                 │ 264 · 520 · 1056 · 340      │
Y 620  ├───────────────────── Progression steps (HTML) ───────────────────────┤
       │ 264 · 1352 · 224 · 620                                              │
Y 884  └─────────────────────────────────────────────────────────────────────┘
```

| # | Visual | Type | H · W · X · Y |
|---|---|---|---|
| 8 | Title | HTML Content | 76 · 1352 · 224 · 144 |
| 9 | KPI strip | HTML Content | 96 · 1352 · 224 · 228 |
| 10 | Tier flow | HTML Content | 264 · 816 · 224 · 340 |
| 11 | Monthly moves | Stacked column chart | 264 · 520 · 1056 · 340 |
| 12 | Progression steps | HTML Content | 264 · 1352 · 224 · 620 |

The tables scale to the visual: the unit is the smaller of width ÷ 816 (flow) or ÷ 1352 (steps), and height ÷ 264. If you resize them, keep the proportions.

---

## 5. Title

HTML Content, **76 · 1352 · 224 · 144**: `_Measures[HTML Tier Title]`.

- Eyebrow *STRUCTURE*, title **Tier progression** (Georgia 28).
- Chips: period, tiers, products.

---

## 6. KPI strip

HTML Content, **96 · 1352 · 224 · 228**: `_Measures[HTML Tier KPI Strip]`.

| Card | Value | Footer |
|---|---|---|
| Members (dark) | Members at period end | Net change vs the month before the period, green/red |
| Upgrades (green) | Moves up in the period | Monthly upgrade rate |
| Downgrades (red) | Moves down in the period | Monthly downgrade rate |
| Joined | New or returning members | — |
| Left programme | Members who left | Monthly leaving rate |
| Starter → Ambassador | Sum of the 4 step averages, in months | "LTD · lower bound" |

Members uses the "K" format like the other strips. The movements are shown in full, so they can be compared with the tables.

---

## 7. Tier flow (HTML)

HTML Content, **264 · 816 · 224 · 340**: `_Measures[HTML Tier Flow Table]`.

- **Rows.** Tier rows with the colour dots, as in the Challenges table, Ambassador first. A Total row appears when more than one tier is shown.
- **Header.** "Tier flow"; the subtitle gives the start and end months and the months of moves.
- **Columns.** Start | **In**: Joined, Up, Down | **Out**: Up, Down, Left | **End** (bold) | Net (green/red) | **Monthly rate**: Up (green), Down (red).
- **"—"** where a move is impossible:
  - Starter can't be upgraded *into* or downgraded *out of*.
  - Ambassador can't be upgraded *out of* or downgraded *into*.
- The footnote defines the columns.

---

## 8. Monthly moves (native chart)

1. Insert → **Stacked column chart**, **264 · 520 · 1056 · 340**.
2. Fields:
   - X-axis: `Dim__Calendar[YearMonth]`, categorical, sorted by `YearMonth Seq` ascending.
   - Y-axis: `_Measures[Tier Moves Upgrades (Chart)]` and `_Measures[Tier Moves Downgrades (Chart)]`.
3. Apply the shared chart-card defaults (Collection & redemption guide §6.2): background `#FFFDFC`, border `#E5D8D3` 1 px radius 10, padding 12, gridlines `#EFE5E2`, axis titles off.
4. Title **"Upgrades and downgrades by month"**, Georgia 18 pt, `#160D0A`.
   - Subtitle: *"Members moving up or down from the selected tiers · downgrades below zero"*, Arial 10, `#7A7370`.
5. Colours: Upgrades `#00581D`, Downgrades `#F21D2F`, matching the KPI cards.
6. Rename the series in the visual (double-click the field in the Y-axis well): **Upgrades**, **Downgrades**. Legend: top left, Arial 9.
7. Y-axis: Auto/Auto, display units **None**. The downgrades format (`#,0;#,0;0`) shows "4,451", not "−4,451", in labels and tooltips. The axis below zero also shows positive numbers. That is intended: the position shows the direction.
8. **Zero line.** Analytics pane (magnifying glass) → **Y-axis constant line** → Add.
   - Value **0**. Colour `#4A403C`, transparency 0%, style **Solid**, width 1 px.
   - Position **In front**, so the line sits on top of the columns.
   - Data label **off**.
   - Optional: rename the line "Zero" so it's easy to find later.
9. Data labels on, Arial 8, inside end. Downgrades are 0 until Jun 2026. To hide the zero labels, use the label colour fx with the existing transparent-when-small pattern, or simply leave them.
10. **Period.** The chart follows the Period tiles. Select **LTD** to see the whole history (Oct 2025 – Aug 2026). MTD / PM show one month.

---

## 9. Progression steps (HTML)

HTML Content, **264 · 1352 · 224 · 620**: `_Measures[HTML Tier Steps Table]`.

- **Rows.** The 4 one-tier steps with both tier dots (Starter → Explorer, …).
  - A **Full journey · Starter → Ambassador** row gives the sum of the step averages.
  - The journey row has no upgrade count: the steps' sum isn't a journey count, and skip-tier upgrades are excluded.
- **In period: Upgrades, Monthly rate.** These follow the Period and Product slicers; they ignore the Tier slicer.
- **Months in previous tier · LTD: Avg months, In tier at start** (§2.3). These ignore all slicers.
- **Profit / member / month · 3 months before vs after:** Before, After, Change (upgraders), Stayers (their change), **Uplift vs stayers** (bold), Upgrades used (§2.4). These ignore all slicers.
- The two-line footnote gives the definitions and the "association, not a proven effect" caveat.

---

## 10. Validation

**All tiers, YTD (Jan – Aug 2026), all products:**

| Tier | Start | Joined | In up | In down | Out up | Out down | Left | End | Net | Up rate | Down rate |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Ambassador | 10 | 0 | 364 | — | — | 12 | 0 | 362 | +352 | — | 1.2% |
| Elite | 2,821 | 35 | 2,212 | 10 | 363 | 901 | 144 | 3,670 | +849 | 1.3% | 3.3% |
| Insider | 9,337 | 464 | 8,731 | 811 | 2,030 | 2,638 | 717 | 13,958 | +4,621 | 2.2% | 2.9% |
| Explorer | 26,444 | 6,345 | 14,886 | 2,560 | 7,517 | 2,448 | 1,858 | 38,412 | +11,968 | 3.0% | 1.0% |
| Starter | 86,537 | 41,680 | — | 2,618 | 16,283 | — | 4,141 | 110,411 | +23,874 | 2.1% | — |
| Total | 125,149 | 48,524 | 26,193 | 5,999 | 26,193 | 5,999 | 6,860 | 166,813 | +41,664 | 2.3% | 0.5% |

KPI strip, YTD:

| Members | Upgrades | Downgrades | Joined | Left | Journey |
|---|---|---|---|---|---|
| 166.8K (+41,664 vs Dec 2025) | 26,193 (2.3% / month) | 5,999 (0.5%) | 48,524 | 6,860 (0.6%) | 21.8 mo |

Other periods, all tiers:

| Period | Start | Joined | Up | Down | Left | End |
|---|---|---|---|---|---|---|
| LTD | 100,536 | 74,409 | 33,416 | 5,999 | 8,132 | 166,813 |
| MTD (Aug 2026) | 159,843 | 8,076 | 4,230 | 1,557 | 1,106 | 166,813 |

Product split, LTD:
- **Card Avantaj** ends at 165,343 members.
- **Wizz** has 1,470 members, all joined. Wizz has no tiers by business design: the source stores its rows as Starter, and they will never move.

**Monthly moves (LTD):**
- Upgrades: Oct 25 2,204 · Nov 2,625 · Dec 2,394 · Jan 26 2,071 · Feb 2,211 · Mar 2,627 · Apr 2,663 · May 3,030 · **Jun 5,657** · Jul 3,704 · Aug 4,230.
- Downgrades: 0 until Jun 2026, then **Jul 4,442** and Aug 1,557.

**Progression steps:**

| Step | Upgrades YTD | Rate YTD | Avg months | In tier at start | Before | After | Change | Stayers | Uplift | Used |
|---|---|---|---|---|---|---|---|---|---|---|
| Starter → Explorer | 14,886 | 1.9% | 4.1 | 54% | 46.2 | 47.1 | +2% | −22% | +12.2 | 8,697 |
| Explorer → Insider | 7,475 | 3.0% | 4.9 | 55% | 49.9 | 67.0 | +34% | −17% | +28.1 | 4,367 |
| Insider → Elite | 2,030 | 2.2% | 5.5 | 46% | 54.1 | 67.0 | +24% | −13% | +22.4 | 1,173 |
| Elite → Ambassador | 363 | 1.3% | 7.3 | 68% | 67.0 | 83.1 | +24% | −13% | +26.0 | 175 |

**Slicer checks:**
1. Select Elite and Insider, MTD:
   - Flow table: two rows plus Total (Total In · Up 1,762 ≠ Out · Up 318, as expected; §2.1).
   - KPI Upgrades = 318.
   - The steps table's upgrade counts don't change with the tier slicer.
2. End month **Sep 2025** with MTD: the flow table shows the "first month of history" message, and the KPI cards show "—".

---

## 11. Known limitations and open questions

- **Downgrades only from Jul 2026** (4,442 in Jul, 1,557 in Aug; none before). This looks like an annual requalification. Ask the programme team. Until then, downgrade rates over YTD/LTD average a mostly-zero period.
- **Upgrade spike in Jun 2026** (5,657, about double the usual). Ask what caused it (campaign, rule change, requalification run).
- **Time in tier is a lower bound** (§2.3). It becomes more reliable as history grows. Once the data covers a full tier cycle, re-read the 21.8-month journey.
- **Skip-tier upgrades** are not in the steps table. LTD there are 1,491 Starter → Insider, 149 Starter → Elite, 53 Explorer → Elite and 1 Starter → Ambassador. They are in the flow table and the KPIs. Ask whether skipping is a rule or a data effect.
- **Returning members** (414 rows with a gap since their previous month) count as *Joined*, in their current tier.
- **Customers in both programmes.** `Fact_TierMigrations` has one row per customer, month and programme (`CUST_ID` + `AS_OF_MONTH` + `WIZZ_FLG`), with no duplicates. In Aug 2026, 356 customers have a Card Avantaj row and a Wizz row. Wizz has no tiers by business design, so its rows carry a placeholder Starter tier, while the Card Avantaj row keeps its own tier.
  - With Product = All, the 1,470 Wizz members appear as Starter joiners. Choose Product = Card Avantaj for a pure tier view.
  - The flow table and the KPIs count rows, so they count memberships, the same way as `[Members]` (Aug 2026: 166,813 in both). With Product = All, these customers count once per programme, which is correct.
  - The profit study and the relative-to-upgrade measures work at membership level: they join `Fact_CustomerView` on `CUST_ID` + `WIZZ_FLG`. A Card Avantaj upgrader's Wizz card profit is therefore not counted (§2.4).
- **Profit comparison** is not a causal effect.
  - Upgraders differ from stayers before the upgrade (lower profit, but rising).
  - Elite → Ambassador rests on 175 upgrades.
  - The profit study ignores all slicers.
- **Calculated table refresh.** `'Upgrade Profit Study'` recomputes on every dataset refresh (a few seconds). If a refresh fails on it, check that `Fact_CustomerView` and `Fact_TierMigrations` loaded.

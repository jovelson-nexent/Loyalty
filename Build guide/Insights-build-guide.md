# Insights: step-by-step build guide

This page is not in the prototype. It lists the KPIs that **moved most from the previous month**, ranked by how big and how material the move is. It uses the same shell, grid and styling as the other pages:

- Canvas: 1600 × 900, with the shared header, sidebar and slicers.
- Style: Nexent Fusion.

Conventions:
- Positions are given as **H · W · X · Y**.
- Both visuals are HTML Content. This page has no native chart.
- All model objects in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

| Block | Content |
|---|---|
| Title | *OVERVIEW* eyebrow, the title **Insights**, and chips: months compared ("Aug 2026 vs Jul 2026"), tiers, products, "Month over month · YTD and LTD presets do not apply" |
| What moved (left) | Up to 10 numbered cards, highest score first. Each card has a headline ("Downgrades fell by 2,887"), a **Better / Worse / Watch** chip, the value against last month, the tier that drove most of the change, why the KPI matters, the page to open, and a "Reported because …" line with the score |
| All monitored KPIs (right) | All 18 KPIs with this month, last month and the change. Reported KPIs are in bold |

Why this page:
- The other pages answer known questions. This page tells the reader where to look first.
- Each card says why it was reported, so the ranking can be checked and isn't a black box.

---

## 2. How it works (read first)

### 2.1 Which months
- **This month** = the End month slicer. **Last month** = the month before.
- The Period tiles (MTD, PM, YTD, LTD) **do not apply**. The comparison is always month over month. The title chip says so.
- Tier and Product slicers apply to both months.

### 2.2 When a KPI is reported
Each KPI in the hidden table `'Insight KPIs'` has a threshold, a floor and a weight.

| Step | Rule |
|---|---|
| Move | Counts and ratios: % change ÷ threshold. Rates: change in points ÷ threshold. If last month was 0, the move counts as very large |
| Impact | Counts: the absolute change. Rates and per-member KPIs: the change × the base (e.g. 9 pts of completion rate × enrolled challenges = 19,612 challenges) |
| Reported | Move ≥ 1 (the threshold is cleared) **and** impact ≥ floor |
| Score | √(min(move, 10) × impact ÷ floor) × weight. Moves are capped at 10× so a jump from a tiny base can't win on its own |

- **Better / Worse** comes from the KPI's direction (Polarity): for Members left, Coins expired and Downgrades, up is worse. Coins redeemed is **Watch**: more redemption is engagement but also cost.
- **"Mostly Starter"**: for counts, the tier that drove the largest share of the change. For rates and ratios, the tier with the largest contribution, shown as last month → this month.
- Moves of 1,000% or more are shown as a multiple ("57× the previous month").

### 2.3 Special cases

| Case | What the page shows |
|---|---|
| End month = first loaded month (Sep 2025) | "Sep 2025 is the first loaded month, so there is nothing to compare with" |
| Nothing cleared both tests | "No KPI moved beyond its threshold and impact floor this month". The right panel still lists all KPIs |
| Product = Wizz | Wizz has no history before Aug 2026 in the summary, so only **Wizz net cashback** is compared. An orange note explains why |
| Tier filtered | Wizz net cashback is skipped (Wizz has no tiers) |
| Small segment (e.g. Tier = Elite) | Floors are absolute, so small segments seldom report anything. This is by design: a 30% move on 200 members is not material for the programme |

### 2.4 Model objects

| Object | Type | Folder |
|---|---|---|
| `'Insight KPIs'` *(hidden)* | Calculated table, 18 rows: ID, KPI, Topic, Kind, Prefix, Unit, Polarity, Threshold, Floor, Weight, Driver, Page, Why | — |
| `Insight KPI Value` | Measure: the value of the KPI in context | `_Report\Insights` |
| `Insight KPI Base` | Measure: the base used for impact | `_Report\Insights` |
| `HTML Insights Title` | Measure | `_Report\HTML` |
| `HTML Insights` | Measure | `_Report\HTML` |
| `Engaged Members %` | Measure (fixed) | — |

Fix to `Engaged Members %`: the denominator now excludes Wizz, which has no engaged flag. Aug 2026, all products: **87.41% → 88.18%**. Card Avantaj alone is unchanged.

### 2.5 Tuning
To change a threshold, floor or weight, edit the `'Insight KPIs'` DATATABLE (Modeling → select the table → formula bar). Then **Refresh** the table. The current settings are listed on the Glossary page when you filter it to *Insights*.

To add a KPI, add a row and a matching branch in `Insight KPI Value` (and `Insight KPI Base` if it is a rate or per-member KPI).

---

## 3. Create the page

1. Right-click **Executive summary** and choose **Duplicate page**. Rename the copy **Insights** and move it to second place, after Executive summary.
2. Keep the shared shell: header, logo, sidebar, page navigators, End month / Tier / Product slicers and the Reset button.
3. **Delete the Period tiles** (MTD / PM / YTD / LTD). They don't apply here, and leaving them suggests they do. If you prefer to keep the shell identical, keep them; the title chip warns that presets don't apply.
4. Delete everything else.
5. Update the sidebar (§5).

---

## 4. Layout

```
Y 144  │ Title 76                                                          │
Y 228  ├──── Insights (HTML) ──────────────────────────────────────────────┤
       │ What moved (cards, scrolls)              │ All monitored KPIs     │
Y 884  └──────────────────────────────────────────────────────────────────┘
```

| # | Visual | Type | Measure | H · W · X · Y |
|---|---|---|---|---|
| 1 | Title | HTML Content | `_Measures[HTML Insights Title]` | 76 · 1352 · 224 · 144 |
| 2 | Insights | HTML Content | `_Measures[HTML Insights]` | 656 · 1352 · 224 · 228 |

- For both visuals, set **Background off, Border off, Shadow off, Padding 0**. The HTML draws its own cards.
- The card list scrolls inside the visual. About 4 cards are visible at a time.

---

## 5. Sidebar

The `HTML Sidebar` measure is already updated (group labels moved down). Move the four page navigators and add Insights to the Overview one:

| Navigator | Pages shown | H · W · X · Y |
|---|---|---|
| Overview | Executive summary, **Insights**, Financial impact | 98 · 176 · 12 · 98 |
| Engagement | Coins & redemption, Challenges, Badges, Trends, Period comparison | 166 · 176 · 12 · 230 |
| Structure | Tier progression, Wizz reconciliation | 64 · 176 · 12 · 430 |
| Reference | Glossary, **Data health** | 64 · 176 · 12 · 528 |

Fix them on one page, then copy the four navigators and paste them on every page (positions stay the same).

---

## 6. Validate

Clear any slicer selection, End month = **Aug 2026**:

- [ ] Title chips: "Aug 2026 vs Jul 2026", "All tiers", "All products".
- [ ] Subtitle: "10 of 18 KPIs moved enough to report".
- [ ] Order and scores: Coins expired 13.3 · Downgrades 7.1 · Challenge completion rate 5.9 · Challenges completed 4.2 · Members 3.8 · Members joined 3.2 · Redemption rate 2.9 · Coins redeemed 2.9 · Coins collected 2.0 · Wizz net cashback 1.5.
- [ ] Card 1: "Coins expired rose by 244,612", **Worse**, 248,945 from 4,333, "57× the previous month", mostly Starter.
- [ ] Card 2: "Downgrades fell by 2,887", **Better**, 1,564 from 4,451.

Other scenarios:

- [ ] End month **Mar 2026**: 8 cards, Engaged share −4.8 pts first.
- [ ] End month **Sep 2025**: the "first loaded month" message.
- [ ] Product = **Wizz**: only Wizz net cashback, with the orange note.
- [ ] Tier = **Elite**: "0 of 17" and the empty state. Wizz net cashback is not counted.
- [ ] Changing the Period tile (if kept) changes nothing.

# Financial impact: step-by-step build guide

Builds on the [Executive summary guide (v2)](./Executive-summary-build-guide-v2.md). The same conventions apply:
- Page size is **1600 × 900**.
- Sizes are given as **H · W · X · Y**.
- HTML Content visuals use the **"HTML standard"** settings (section 2 of the Executive summary guide).
- Native charts use the **shared card style** (section 9 of the Executive summary guide).

All measures below already exist in the model. **Save the PBIX** before you start.

---

## 1. What the page shows

| Block | Content |
|---|---|
| Title | Eyebrow *Overview*, title **Financial impact** and the "Showing" chips |
| Overall strip | Three joined cards comparing engaged and not-engaged members: member counts and engaged share, profit per member per month with Δ%, and transactions per member per month with Δ% |
| Per-tier table | Ambassador → Starter plus a Total row. Covers members (engaged, not engaged, engaged %), profit per member per month (engaged, not engaged, Δ%) and transactions per member per month (engaged, not engaged, Δ%) |

- Δ% = (Engaged − Not engaged) ÷ Not engaged. It shows "—" when there are no not-engaged members.
- **Engaged** means the customer collected or redeemed Avantaj Coins within the 180 days before month-end. The dashboard uses the source engagement flag for this definition. **Not engaged** means no such activity; it does not mean the customer left or closed their membership. Wizz has no engagement flag and is excluded.

---

## 2. Duplicate the Executive summary page (fastest way)

1. Right-click the **Executive summary** tab → **Duplicate page**. Rename the copy **Financial impact**.
2. **Keep these** (same positions, nothing to change):
   - Header band
   - Logo
   - Sidebar background and page navigators
   - Period tiles
   - End month, Tier and Product dropdowns
   - Reset button
3. **Delete** these:
   - KPIs HTML
   - Column chart
   - Line chart
   - Step slicer
4. Select the title HTML visual. In **Values**, swap `HTML Exec Title` for `HTML Fin Title`.
5. The page navigator marks the current page automatically, so there's nothing to do there.

> If you build the page from scratch instead, copy the shell objects from the Executive summary page with Ctrl+C / Ctrl+V. Pasting keeps the positions.

---

## 3. Layout

```
Y 0    ┌──────────────── Header 64 ────────────────────────────────────┐
Y 64   │Sidebar│ Period tiles │ End month │ Tier │ Product │   Reset   │  Y 84
       │  200  ├──────────────────────────────────────────────────────┤
Y 144  │       │ Title  76                                            │
Y 228  │       │ Overall strip 176 (3 joined cards + note)            │
Y 412  │       ├──────────────────────────────────────────────────────┤
       │       │ Per-tier financials 472 × 1352                       │
Y 884  └───────┴──────────────────────────────────────────────────────┘
               X 224                                              X 1576
```

| # | Object | Type | H · W · X · Y |
|---|---|---|---|
| 1–7 | Shell (from the Executive summary page) | as is | as is |
| 8 | Title | HTML `HTML Fin Title` | **76 · 1352 · 224 · 144** |
| 9 | Overall strip | HTML `HTML Fin Overall` | **176 · 1352 · 224 · 228** |
| 10 | Per-tier financials | HTML `HTML Fin Tier Table` | **472 · 1352 · 224 · 412** |

- The per-tier table now spans the full content width, X 224 to 1576.
- The bottom edge is at 884, the same as the Executive summary page.

---

## 4. Title
- **HTML Content** → `_Measures[HTML Fin Title]` → **HTML standard**.
- **H 76 · W 1352 · X 224 · Y 144**.
- Same look as the Executive summary title: *OVERVIEW*, then **Financial impact**, then the Showing chips (period · tiers · products).

---

## 5. Overall strip (engaged vs not engaged)
- **HTML Content** → `_Measures[HTML Fin Overall]` → **HTML standard**.
- **H 176 · W 1352 · X 224 · Y 228**.

| Card | Measures | Notes |
|---|---|---|
| Members · engaged vs not engaged | `[Engaged Members]`, `[Not Engaged Members]` | Engaged share (large number) and a red split bar. "As of <end date>" because these are point-in-time counts. |
| Profit per member per month | `[Profit per Customer per Month - Engaged (RON)]`, `[… - Not Engaged (RON)]`, `[Profit Uplift Engaged vs Not Engaged %]` | Engaged in orange, not engaged in grey. Δ% is green when ≥ 0 and red when < 0. |
| Transactions per member per month | `[Transactions per Customer per Month - Engaged]`, `[… - Not Engaged]`, `[Transactions Uplift Engaged vs Not Engaged %]` | Same layout |

- The note line under the cards says that the comparison is **observed, not causal**.
- When Wizz is included in the Product filter, the note adds that Wizz has no engagement flag and is excluded.

---

## 6. Per-tier table
- **HTML Content** → `_Measures[HTML Fin Tier Table]` → **HTML standard**.
- **H 472 · W 1352 · X 224 · Y 412**.
- The card frame, title ("Per-tier financials", Georgia 28, same style as the page title), subtitle and footnote are all inside the HTML. Don't add a native background or title.
- **Rows:**
  - Tiers run Ambassador → Starter, then a highlighted Total row.
  - Tiers with no members in the current filters are hidden, so the Tier slicer works as expected.
  - The rows stretch to fill the card height.
- **Column groups:**
  - **Members**: Engaged · Not engaged · Engaged % (mini bar).
  - **Profit / member / month · RON**: Engaged · Not engaged · Δ%.
  - **Transactions / member / month**: Engaged · Not engaged · Δ%.
  - Δ% cells use green or red pills.

---

## 7. Remove the redundant chart

Delete the native **Profit by Engagement and Tier** bar chart. Its per-tier profit values duplicate the table; for Ambassador, Elite and Insider, the very small not-engaged group can make the side-by-side comparison easy to overinterpret.

The full-width table retains the figures and clarifies that **Engaged %** means a coin collection or redemption in the prior 180 days (Wizz excluded). Its footnote warns that comparisons are volatile where not-engaged groups are tiny. The difference is descriptive, not causal.

---

## 8. Check before publishing

With **All months · All tiers · All products**, the current sample data should give:

| Check | Expected |
|---|---|
| Engaged / Not engaged members | 145,806 / 19,537 · 88.2 % engaged |
| Profit per member per month, A / I | 47.7 / 18.4 RON · **+159 %** |
| Transactions per member per month, A / I | 6.3 / 0.2 · **+2,884 %** |
| Ambassador row | 362 engaged, not engaged "—", Δ% "—" |
| Starter row | 91,163 / 17,778 · profit 42.4 / 16.1 (+163 %) |

Then test these filters:
- **Tier = Elite** leaves only Elite plus Total.
- **Product = Wizz** shows only "—", and the note says Wizz is excluded.
- Changing the **Period** updates the profit and transaction figures. Member counts follow the end month.

---

## 9. Known limitations
- **Wizz has no engagement flag.** Wizz members appear in neither the engaged nor not-engaged group, and Wizz rows are excluded from every per-member engagement comparison.
- **Member counts vs profit figures:**
  - Member counts come from `Fact_Overview`, which is point-in-time at the end month and covers the full base.
  - Profit and transaction figures come from `Fact_CustomerView`, averaged over customer-months in the period.
  - `Fact_CustomerView` holds the full base: it reconciles with `Fact_Overview` on members, profit, coins and transactions (see the Data health page).
- **Self-selection.** The difference between engaged and not-engaged members is observed, not caused by the program. Very high transaction Δ% values are expected because not-engaged members barely transact.
- **Rounding.** Profit is averaged per customer-month. It won't match `[Profit per Member per Month (RON)]` on the Executive summary page, which is based on `Fact_Overview`.

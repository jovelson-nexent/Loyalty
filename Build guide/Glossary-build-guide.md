# Glossary: step-by-step build guide

The Glossary explains the numbers in the report: the **business rules** behind every page, and a dictionary of **KPIs and measures**. It departs from the prototype's single list and follows common BI glossary practice: rules first, then terms grouped by topic, each with a plain definition, the calculation, the grain and the pages that use it. A page slicer and a term search narrow it down.

- Canvas: 1600 × 900, with the shared header and sidebar.
- Style: Nexent Fusion.
- Positions are given as **H · W · X · Y**.
- All model objects in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

| Block | Content |
|---|---|
| Title | *REFERENCE* eyebrow, the title **Glossary**, chips: the page shown ("All pages", one page, or "Several pages"), "Business rules, KPIs and measures", "Data Sep 2025 – Aug 2026" |
| Page slicer | Shows the rules and terms used on one page |
| Term search | Finds one term |
| Business rules (left) | 17 numbered rules: membership, monthly data, period presets, stock and flow, per member per month, units, tiers, upgrades and downgrades, Wizz, engaged member, enrolment proxy, badge level 1, association not cause, life-to-date, period comparison, insights, data health |
| KPIs and measures (right) | 50 terms grouped by topic (Members, Financial, Coins, Challenges, Badges, Tiers, Wizz, Insights, Data health). 22 are headline **KPIs** (orange chip). Columns: Term · Definition and "= calculation" · Grain (Stock, Flow, Rate, Ratio) · Pages (codes). The footer lists the page codes |

When the page slicer includes **Insights**, the right panel adds an **Insights settings** table (threshold, floor, weight and direction of each monitored KPI), read live from `'Insight KPIs'`.

---

## 2. Model objects

| Object | Type | Notes |
|---|---|---|
| `'Glossary Page'` | Calculated table: Order, Page, Code | 11 pages. Feeds the page slicer. Page is sorted by Order |
| `'Glossary'` | Calculated table: Order, Section (Rule / KPI / Measure), TopicOrder, Topic, Term, Definition, Calc, Grain, Pages | 17 rules + 50 terms. Pages holds page codes ("EX,IN,CO") or ALL. Term is sorted by Order |
| `HTML Glossary Title` | Measure, `_Report\HTML` | |
| `HTML Glossary Rules` | Measure, `_Report\HTML` | Design size 440 × 596 |
| `HTML Glossary Metrics` | Measure, `_Report\HTML` | Design size 896 × 596 |

There is no relationship between the two tables. The measures match the selected page codes against the Pages column, so a rule marked ALL appears on every page.

**Engaged member** means a customer who collected or redeemed Avantaj Coins within the 180 days before the monthly snapshot date. The dashboard uses the source engagement flag for this definition; Wizz has no engagement flag and is excluded from the engagement rate.

Page codes: **EX** Executive summary · **IN** Insights · **FI** Financial impact · **CO** Coins & redemption · **CH** Challenges · **BA** Badges · **TR** Trends · **PC** Period comparison · **TP** Tier progression · **WZ** Wizz reconciliation · **DH** Data health.

### Editing entries
Edit the DATATABLE of `'Glossary'` (Modeling → select the table → formula bar), then **Refresh** the table. Keep definitions to one sentence and calculations to one line. To list a term on a new page, add its code to Pages.

---

## 3. Create the page

1. Right-click **Data health** and choose **Duplicate page**. Rename the copy **Glossary** and place it before Data health.
2. Keep the header, logo, sidebar, page navigators and the Reset button.
3. **Delete** the Period tiles and the End month / Tier / Product slicers. Nothing on this page depends on them.
4. Delete everything else.
5. Check that the *Reference* navigator lists Glossary and Data health (see `Insights-build-guide.md`, §5).

---

## 4. Layout

```
Y 144  │ Title 76                                                          │
Y 228  │ [Page ▼ 300]  [Search term ▼ 300]                                 │
Y 288  ├──── Business rules ──────┬──── KPIs and measures ─────────────────┤
       │ 596 · 440 · 224 · 288    │ 596 · 896 · 680 · 288                  │
Y 884  └──────────────────────────┴────────────────────────────────────────┘
```

| # | Visual | Type | Field / measure | H · W · X · Y |
|---|---|---|---|---|
| 1 | Title | HTML Content | `_Measures[HTML Glossary Title]` | 76 · 1352 · 224 · 144 |
| 2 | Page | Slicer, Dropdown | `'Glossary Page'[Page]` | 44 · 300 · 224 · 228 |
| 3 | Search term | Slicer, Dropdown | `'Glossary'[Term]` | 44 · 300 · 540 · 228 |
| 4 | Business rules | HTML Content | `_Measures[HTML Glossary Rules]` | 596 · 440 · 224 · 288 |
| 5 | KPIs and measures | HTML Content | `_Measures[HTML Glossary Metrics]` | 596 · 896 · 680 · 288 |

- HTML visuals: **Background off, Border off, Shadow off, Padding 0**. Each draws its own card. Both lists scroll inside the card; the table header stays visible.
- Slicers: copy the style of the End month slicer from another page (Arial 10, `#4A403C`, fill `#FFFDFC`, border `#E5D8D3`, rounded 8).
  - **Page**: single select on, *Select all* off. Header text "Page". Leave empty to show all pages.
  - **Search term**: Selection → **Search on** (slicer ⋯ → Search). Single select on. Header text "Term". The term list contains rules and measures, so a search finds both.
- Add both slicers to the **Reset** bookmark, cleared. Don't sync them to other pages.

---

## 5. Validate

- [ ] No selection: chip "All pages". Rules subtitle "17 rules behind every number". Metrics subtitle "50 entries · 22 headline KPIs …". The topics run Members → Data health.
- [ ] Page = **Insights**: chip "Insights". 12 rules, 22 entries, 14 headline KPIs. The **Insights settings** table appears at the bottom of the right panel with 18 rows (e.g. Engaged share 1 pt, 1,000 members, 1.2, Better).
- [ ] Term = **Redemption rate**: rules panel "0 rules match the term search" and "No rule matches the selection". Metrics show one entry under Coins: "Coins redeemed ÷ coins collected, same period", Rate, EX · IN · CO · TR · PC.
- [ ] Engaged members % reads "(Wizz excluded)" in its calculation.
- [ ] Reset clears both slicers.

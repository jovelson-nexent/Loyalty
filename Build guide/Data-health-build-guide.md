# Data health: step-by-step build guide

This page collects the data-quality checks and the open questions in one place. It builds on the other guides and uses the same shell, grid and styling:

- Canvas: 1600 × 900, with the shared header, sidebar and slicers.
- Style: Nexent Fusion.

Conventions:
- Positions are given as **H · W · X · Y**.
- All four visuals are HTML Content. This page has no native chart.
- All model objects in this guide are already in the open model. **Save the PBIX to keep them.**

---

## 1. Page content

| Block | Content | Follows slicers? |
|---|---|---|
| Title | *GOVERNANCE* eyebrow, the title **Data health**, chips for the loaded months and slicer behaviour | No |
| Data checks | 14 live checks on all loaded data: grain, load coverage, detail-vs-summary reconciliation, business rules. Only **Watch** and **Info** rows are shown (7 today); passing checks are counted in the subtitle and hidden. Rows link to a question ID when one exists | No |
| Wizz source check | Wizz detail feeds vs the monthly loyalty summary, with the Card Avantaj ratio as a benchmark (moved from Wizz reconciliation) | Period only |
| Open questions | The open questions for the programme and data teams, with evidence. Answered questions are hidden | No |

Why this page:
- The other pages show the business. This page shows whether the data can be trusted, and what is still open.
- The checks recompute on every refresh, so a new load that breaks the grain or stops reconciling turns a row orange.

---

## 2. How it works (read first)

### 2.1 Status

| Status | Meaning |
|---|---|
| **Pass** | The rule holds. The row is hidden; the subtitle counts it ("7 of 11 rules pass (hidden)") |
| **Watch** (orange) | The rule does not hold. The ID under the chip (W5, T3 …) points to the open question about it |
| **Info** (grey) | A fact to know when reading the report, not a rule |

### 2.2 The checks

All 14 are computed; the visual shows only the Watch and Info rows (2, 7, 10, 11, 12, 13, 14 today). If every rule passes and there are no Info rows, it shows "All checks pass".

| # | Area | Check | Pass when | Today |
|---|---|---|---|---|
| 1 | Grain | One row per customer, month and programme in `Fact_CustomerView`, `Fact_TierMigrations`, `Fact_Completion` | 0 duplicates | Pass |
| 2 | Coverage | All sources end in the same month as the summary (`Fact_Overview`) | All aligned | **Watch**: Wizz cashback and redemptions end Sep 2026 (W5) |
| 3 | Reconciliation | Customer view rows = summary members (`CUST_CNT`) | Equal | Pass (1,632,960) |
| 4 | Reconciliation | Customer view profit, coins collected, coins spent, transactions, engaged = summary | Largest gap < 0.1% (summary is in thousands) | Pass (0.00%) |
| 5 | Reconciliation | `Fact_Completion` challenges and badges = `Fact_CompletionOverview` | Gap < 0.1% | Pass |
| 6 | Reconciliation | Tier migrations profit = customer view profit | Gap < 0.1% | Pass (0.00%) |
| 7 | Reconciliation | Per-engagement feed (`Fact_EngagementCompletion`) = completion summary | Within ±2% | **Watch**: challenges +0.8%, badges +2.6% (C4) |
| 8 | Business rule | Wizz has no tiers: no Wizz row with a tier other than the Starter placeholder, and no Wizz tier move | 0 | Pass |
| 9 | Business rule | Wizz-flagged redemptions are all "Point transfer to Wizz Air" | 0 others | Pass (W1) |
| 10 | Business rule | Upgrades move one tier at a time | 0 skips | **Watch**: 1,694 of 33,487 (T3) |
| 11 | Business rule | Downgrades from the first month of tier moves | First downgrade = first move month | **Watch**: Oct 2025 vs Jul 2026 (T1) |
| 12 | Info | Wizz rows with an engaged flag | — | 0 (E3) |
| 13 | Info | Returning members: rows with a gap since the previous month | — | 414 |
| 14 | Info | Customers in both programmes, latest month | — | 356 |

### 2.3 Open questions

- The list is the hidden calculated table **`'Data Questions'`** (columns Order, ID, Area, Question, Evidence, Owner, Status).
- The visual shows only rows with Status **Open**. Owner and Status are kept in the table, not shown; the subtitle counts open questions by owner.
- To add a question or close one, edit its DATATABLE (Modeling → select the table → formula bar). Set Status to **Answered** to hide it. If none are open, the visual shows "No open questions".
- IDs: **W** Wizz, **T** tiers, **C** challenges and badges, **E** coins and engagement.
- The evidence text is fixed (as of the Aug 2026 load). The live numbers are in the Data checks table.

### 2.4 New model objects

| Object | Type | Folder |
|---|---|---|
| `'Data Questions'` *(hidden)* | Calculated table | — |
| `HTML Data Health Title` | Measure | `_Report\HTML` |
| `HTML Data Checks` | Measure | `_Report\HTML` |
| `HTML Data Questions` | Measure | `_Report\HTML` |
| `HTML Wizz Source Check` | Measure (changed) | `_Report\HTML` |

Changes to `HTML Wizz Source Check`:
- Title is now **Wizz source check**, and the subtitle starts with the period.
- It no longer depends on the Product slicer. It always compares Wizz, so it works when the report opens on Card Avantaj.

---

## 3. Create the page

1. Right-click **Wizz reconciliation** and choose **Duplicate page**. Rename the copy **Data health**. Move it to the end of the page list.
2. Keep the shared shell: header, logo, sidebar page navigator, Period tiles, End month / Tier / Product slicers, and the Reset button.
3. Delete everything else.
4. In the title visual, replace the measure with `_Measures[HTML Data Health Title]`.
5. Check that the sidebar page navigator lists **Data health**: it is the last page of the *Reference* navigator, after Glossary (see the Executive summary guide, §5.2).

Optional: if the page is for the project team only, right-click the page tab and choose **Hide page**. It stays reachable from the navigator only if you keep it listed there.

---

## 4. Layout

```
Y 144  │ Title 76                                                           │
Y 228  ├──── Data checks (HTML) ─────────────┬──── Open questions (HTML) ───┤
       │ 376 · 664 · 224 · 228               │ 656 · 672 · 904 · 228        │
Y 620  ├──── Wizz source check (HTML) ───────┤                              │
       │ 264 · 664 · 224 · 620               │                              │
Y 884  └─────────────────────────────────────┴──────────────────────────────┘
```

| # | Visual | Type | H · W · X · Y |
|---|---|---|---|
| 8 | Title | HTML Content | 76 · 1352 · 224 · 144 |
| 9 | Data checks | HTML Content | 376 · 664 · 224 · 228 |
| 10 | Wizz source check | HTML Content | 264 · 664 · 224 · 620 |
| 11 | Open questions | HTML Content | 656 · 672 · 904 · 228 |

- Gaps are 16 px (228 + 376 + 16 = 620; 224 + 664 + 16 = 904).
- Each table scales to its visual. Headers are sticky, and the body scrolls if needed: Data checks fits its 7 rows, Open questions shows about 15 of 16.
- For all four visuals, set **Background off, Border off, Shadow off, Padding 0**. Each HTML draws its own card.

If you already built the Wizz reconciliation page with the Source check, cut that visual and paste it here, then resize it to 264 · 664 · 224 · 620.

---

## 5. Title

HTML Content, **76 · 1352 · 224 · 144**: `_Measures[HTML Data Health Title]`.

Chips: "Loaded data Sep 2025 – Aug 2026", "Checks and questions ignore slicers", "Wizz source check follows the period".

---

## 6. Data checks (HTML)

HTML Content, **376 · 664 · 224 · 228**: `_Measures[HTML Data Checks]`.

- **Rows.** Watch and Info only; passing checks are hidden.
- **Columns.** Area, Check (with a grey detail line), Result, Status (chip, plus the question ID).
- **Subtitle.** "4 to watch · 3 info · 7 of 11 rules pass (hidden) · all loaded data, Sep 2025 – Aug 2026".
- **Footer.** "Passing checks are hidden · ignores slicers · recomputed on every refresh · W5, T3 … = related open question".
- It scans the customer-level tables (1.6M rows each), so it takes about 1 second. Slicer clicks on this page re-run it.

---

## 7. Open questions (HTML)

HTML Content, **656 · 672 · 904 · 228**: `_Measures[HTML Data Questions]`.

- **Rows.** Status = Open only.
- **Columns.** ID (with area), Question and evidence.
- **Subtitle.** "16 open · 9 for the programme team · 7 for the data team".
- **Footer.** "Raised while building the report · evidence as of the Aug 2026 load · answered questions are hidden".

---

## 8. Wizz source check (HTML)

HTML Content, **264 · 664 · 224 · 620**: `_Measures[HTML Wizz Source Check]`.

| Row | Detail feed | Summary |
|---|---|---|
| Coins collected | Wizz net cashback | `[Coins Collected]`, Wizz rows |
| Coins redeemed | Wizz redemption value | `[Coins Redeemed]`, Wizz rows |
| Challenge completions | Per-challenge feed | `[Challenges Completed]`, Wizz rows |
| Badge completions | Per-badge feed | `[Badges Completed]`, Wizz rows |

- **Columns.** Detail feed, Summary, Difference, **Feed ÷ sum.** (green within ±2%, otherwise orange), and **Card Avantaj** (the same ratio for Card Avantaj in the same months).
- Only months present in **both** sources are compared. The subtitle lists them (today: Aug 2026).
- When the period has no such month, the table says so and gives the first month available.

---

## 9. Validation

### 9.1 Data checks (any slicer state)

Subtitle: **4 to watch · 3 info · 7 of 11 rules pass (hidden)**. Seven rows, in this order: Coverage 2 differ · Per-engagement 2.6% · Upgrades 1,694 · Downgrades Jul 2026 · Wizz engaged flag 0 · Returning members 414 · Both programmes 356.

### 9.2 Wizz source check (End month Aug 2026, any period that includes Aug)

| Check | Feed | Summary | Feed ÷ sum. | Card Avantaj |
|---|---|---|---|---|
| Coins collected | 54,545 | 213,440 | 25.6% | — |
| Coins redeemed | 37,536 | 82,050 | 45.7% | 95.8% |
| Challenge completions | 976 | 989 | 98.7% | 99.4% |
| Badge completions | 1,124 | 1,141 | 98.5% | 99.2% |

### 9.3 Behaviour checks

- **Product = Card Avantaj.** All four visuals are unchanged.
- **Tier = Elite.** All four visuals are unchanged.
- **End month before Aug 2026.** Only the Wizz source check changes: it reports no common month (first: Aug 2026).

---

## 10. Maintenance

- **When a question is answered:** set its Status to "Answered" in `'Data Questions'`, and update the guide that mentions it.
- **When a Watch turns Pass** after a new load, the row disappears; close the linked question.
- **New check:** add a tuple to `_R` in `[HTML Data Checks]`: ( order, area, check, detail, result text, "P" / "W" / "I", question ID or "" ).

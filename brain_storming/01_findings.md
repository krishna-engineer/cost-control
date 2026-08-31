## 1. On missing vessels from Handover Sheet: 
- 3 sheets (`Vessel_Master`, `Monthly_Budget_Actual` and `Cost_Transactions`) all hold the same 14 vessels. 
- While `Handover_Deferred_Risk` sheet holds only 10 — V1002, V1007, V1012 and V1013 are absent.
- That is a real gap, because "deferred exposure" is the value of work that should have been done but was not.
- And I believe "deferred exposure" is the single most important thing to know about a vessel being handed over.
- The problem is that a missing row can mean two opposite things: 
    - Either nothing has been deferred on that vessel (happy happy case)
    - Or nobody has assessed it yet. 
- And the sheet gives me no way to tell what exactly is the case here.
- But I have a reason for leaning towards the option of "not assessed":
    - 4 vessels (V1004, V1006, V1008, V1009) flagged as "Handover Review" in `Vessel_Master` is present in 
        `Handover_Deferred_Risk`
    - But 4 missing vessels from `Handover_Deferred_Risk` are present as `Active` in `Vessel_Master`
- Hence These four vessels are excluded from any deferred-exposure comparison and reported as not assessed.


## 2. Rounding differences between the two spending sheets
- Adding up `Amount_USD` from `Cost_Transactions` sheet and comparing against `Actual_USD` in `Monthly_Budget_Actual`, all 3,472 rows matched on structure — nothing orphaned on either side.
- But 690 rows differed in amount by more than one cent. The largest single difference was `0.02`, and the total across every row was `10.28` USD.
- The pattern fits rounding and nothing else. If the two sheets genuinely disagreed about spending, the differences would vary in size.


## 3. The curious case of "Off_Budget_USD"
- I step 2 findings, I've considered all transactions irrespective of Budget status being `Budgeted`/`Non-Budgeted`.
- So what this also means: `non-budgeted` spend (from `Cost_Transactions`) also sits inside `Actual_USD`
- Now let's understand the situation with an example of vessel **V1008** for **March 2024** and cost category being **Logistics & Freight** i.e. single row from `Monthly_Budget_Actual`.

- The `Monthly_Budget_Actual` sheet reports:

| Budget | Actual | Off_Budget | Variance |
| --- | --- | --- | --- |
| 3,629.62 | 4,658.79 | 115.07 | 1,029.17 |

- And for same case, two transactions are present in `Cost_Transactions` sheet:

| ID | Date | Theme | Amount | Status |
| --- | --- | --- | --- | --- |
| T005264 | 21 Mar | Sea freight | 1,259.88 | Non-Budgeted |
| T005265 | 13 Mar | Local delivery | 3,398.91 | Budgeted |

- Finding 1: off-budget spending is inside the actuals (we calculated this in point 2)
    - The two transactions add to 4,658.79, which is exactly `Actual_USD`.
    - The non-budgeted transaction is one of the things being summed.
    - And this holds true across all 228 off-budget cells.

- Finding 2:the off-budget amount does not match its own records
    - The `Monthly_Budget_Actual` reports 115.07 of off-budget spending.
    - While the `Cost_Transactions` records show a single non-budgeted transaction of 1,259.88.
    - i.e. 11 times larger
    - And this is not a one-off. Across the fleet, non-budgeted transactions total 711,718.50 while the `Off_Budget_USD` column totals 106,513.80.

- Both sheets flag the same 228 vessel-month-category cells,  so they agree on "where unplanned spending happened" but they highly disagree on how much was spent.

- Also I expected non-budgeted transactions to cluster on unplanned work like "Emergency crew travel". But they do not.

- Below 9 vessels have no unbudgeted spending at all:
    - ['V1001', 'V1003', 'V1004', 'V1006', 'V1007', 'V1009', 'V1010', 'V1012', 'V1014']
    - And both sheets agree on these 9 vessels.
    - So with roughly 730 transactions each vessel and 31 months of total operation, there's not a single entry for `Off_Budget_USD` or `Non-Budgeted` transaction.

- That is the part I do not find credible as a description of real operations. Over two and a half years, every vessel eventually would need something that was not planned for.

- Also of the 9 vessels without non-budgeted spend, 7 are present in the deferred sheet and 2 are not. So the flag says nothing about whether a vessel was assessed for deferred work.

- Decisions:
    - `Off_Budget_USD` is excluded from all analysis. It is a fraction of budget drawn at random
    - `Budget_Status` is not used to measure unplanned spending. If forced to choose between the two, it is the better-grounded figure.


## 4. Fleet overspend has roughly doubled since 2024
- When I compared the same months 6 months (Jan to July) across 3 yars, since the data stops in July 2026 — the fleet's overspend against budget rises steadily:

| Year | Overspend (Jan–Jul) |
| --- | --- |
| 2024 | 1.92% |
| 2025 | 2.79% |
| 2026 | 3.17% |

- Why I compared only January to July?
    - The data ends in July 2026, so that year is 7 months long while the others are complete 12

- This is deterioration that built gradually rather than arriving as an event.
- The fleet did not have one bad quarter, instead it drifted, month after month, for two and a half years.
- Anyone tracking fleet overspend as a percentage of budget would have seen the level moving away from 2% well in time.

## 5. The deterioration is concentrated, and the three worst vessels fail in different ways
- After finding the Overspend increase year-by-year, I focused on looking at vessel level change.
- Below is the table from analysis:

| Vessel_ID | 2024 | 2025 | 2026 | Change |
|-----------|-----:|-----:|-----:|-------:|
| V1008 |  6.6 |  9.3 | 11.5 |  4.9 |
| V1007 | -1.8 | -0.2 |  2.9 |  4.7 |
| V1002 |  3.5 |  8.2 |  7.4 |  3.9 |
| V1013 |  2.2 |  7.3 |  5.9 |  3.7 |
| V1005 |  6.4 |  7.9 |  9.7 |  3.3 |
| V1006 | -2.5 | -1.0 | -0.4 |  2.1 |
| V1011 |  2.9 |  7.2 |  4.3 |  1.4 |
| V1009 | -0.2 |  0.0 |  0.7 |  0.9 |
| V1012 |  0.6 |  3.4 |  1.5 |  0.9 |
| V1003 |  2.4 |  1.3 |  2.5 |  0.1 |
| V1004 |  2.8 | -1.4 |  1.7 | -1.1 |
| V1010 |  1.1 | -2.3 | -0.0 | -1.1 |
| V1014 |  2.3 |  1.6 |  0.5 | -1.8 |
| V1001 |  2.1 | -1.8 | -2.1 | -4.2 |

- Above table says: 5 vessels deteriorated by more than 3 percentage points between 2024 and 2026 while 4 improved.

- After this, I thought of looking at areas (Cost_Category) under which these top vessels are failing, and found this:

| Vessel_ID | Cost_Category | 2024 | 2025 | 2026 | Change |
|-----------|---------------|-----:|-----:|-----:|-------:|
| **V1002** | Spares | 3.1 | 8.7 | 18.3 | 15.2 |
| V1002 | Repairs & Maintenance | 5.2 | 22.9 | 18.3 | 13.1 |
| V1002 | Safety & Compliance | 5.5 | 5.3 | 15.8 | 10.3 |
| V1002 | Logistics & Freight | 8.0 | 5.0 | 14.9 | 6.9 |
| V1002 | Stores | -2.5 | 4.5 | -1.7 | 0.8 |
| V1002 | Crew Travel | -0.1 | 10.8 | -1.9 | -1.8 |
| V1002 | Crew Wages | 4.9 | 2.0 | 0.8 | -4.1 |
| V1002 | Other Operating Cost | 0.8 | 5.0 | -9.6 | -10.4 |
| ─── | ─── | ─── | ─── | ─── | ─── |
| **V1007** | Crew Travel | -2.6 | -4.9 | 10.1 | 12.7 |
| V1007 | Crew Wages | -8.6 | 1.4 | 2.1 | 10.7 |
| V1007 | Other Operating Cost | -2.0 | -6.5 | 4.9 | 6.9 |
| V1007 | Repairs & Maintenance | 2.6 | -2.9 | 6.3 | 3.7 |
| V1007 | Spares | 5.0 | -1.7 | 8.0 | 3.0 |
| V1007 | Stores | -2.8 | 4.5 | -1.9 | 0.9 |
| V1007 | Logistics & Freight | -1.5 | 2.3 | -5.1 | -3.6 |
| V1007 | Safety & Compliance | 0.5 | 5.3 | -6.1 | -6.6 |
| ─── | ─── | ─── | ─── | ─── | ─── |
| **V1008** | Safety & Compliance | 8.3 | 29.2 | 62.9 | 54.6 |
| V1008 | Repairs & Maintenance | 1.3 | 14.6 | 11.5 | 10.2 |
| V1008 | Logistics & Freight | 11.5 | 11.5 | 21.6 | 10.1 |
| V1008 | Spares | 4.9 | 10.9 | 14.5 | 9.6 |
| V1008 | Other Operating Cost | 8.2 | -10.3 | 10.6 | 2.4 |
| V1008 | Stores | 0.2 | -0.6 | -0.4 | -0.6 |
| V1008 | Crew Travel | -1.5 | -2.3 | -2.2 | -0.7 |
| V1008 | Crew Wages | 13.9 | 11.9 | 0.2 | -13.7 |

- Let's take example of category "Safety & Compliance" category:

V1008: 8.3 => 29.2 => 62.9
V1002: 5.5 => 5.3 => 15.8
V1007: 0.5 => 5.3 => -6.1

- Or "Logistics & Freight" category:

V1008: 11.5 => 11.5 => 21.6
V1002: 8.0 => 5.0 => 14.9
V1007: -1.5 => 2.3 => -5.1

- So what it says is **cost categories don't move together at vessel level.**

- From here, I thought of looking at cost category at fleet level instead of vessel level.

| Cost_Category | 2024 | 2025 | 2026 | Change |
|---------------|-----:|-----:|-----:|-------:|
| Safety & Compliance | 1.5 | 4.0 | 9.3 | 7.8 |
| Repairs & Maintenance | 2.5 | 5.9 | 7.1 | 4.6 |
| Spares | 2.6 | 4.9 | 3.7 | 1.1 |
| Logistics & Freight | 4.1 | 3.7 | 4.5 | 0.4 |
| Stores | 0.8 | 0.0 | 0.7 | -0.1 |
| Other Operating Cost | 2.5 | -0.1 | 2.1 | -0.4 |
| Crew Wages | 1.0 | 0.8 | 0.2 | -0.8 |
| Crew Travel | 1.7 | 2.9 | 0.8 | -0.9 |

- 2 categories rise in every year. The other six are flat or falling. 
- Safety & Compliance goes from `1.5%` over budget to `9.3%`, and Repairs & Maintenance from `2.5%` to `7.1%`.

- Next important area to check was: **how many of the 14 vessels got worse in each category between 2024 and 2026.**

| Cost_Category | Vessels | of 14 |
|---------------|--------:|------:|
| Safety & Compliance | 10 | 71% |
| Repairs & Maintenance | 9 | 64% |
| Spares | 9 | 64% |
| Crew Travel | 7 | 50% |
| Crew Wages | 7 | 50% |
| Logistics & Freight | 7 | 50% |
| Stores | 7 | 50% |
| Other Operating Cost | 5 | 36% |

- This does give management a view on fleet-wide control aimed at these two categories: 
    - tighter approval on unplanned maintenance and compliance spend
    - earlier escalation when either runs over



## 6. Relationship between cost variance and deffered exposure details

| Vessel_ID | Estimated_Deferred_Exposure_USD | Handover_Risk | Variance_Pct | Management_Status |
|-----------|--------------------------------:|:-------------:|-------------:|-------------------|
| V1009 | 169,705.52 | Medium | 1.42 | Handover Review |
| V1004 | 168,701.75 | Low | 0.57 | Handover Review |
| V1014 | 167,023.32 | High | 1.15 | Active |
| V1006 | 159,314.94 | Medium | -0.75 | Handover Review |
| V1003 | 159,118.85 | Medium | 0.59 | Active |
| V1010 | 130,521.61 | Medium | 0.05 | Active |
| V1011 | 129,845.28 | Low | 4.25 | Active |
| V1008 | 123,953.70 | High | 7.65 | Handover Review |
| V1005 | 74,399.47 | High | 8.78 | Active |
| V1001 | 53,799.19 | Medium | -1.37 | Active |
| ─── | ─── | ─── | ─── | ─── |
| V1002 | — | — | 5.92 | Active |
| V1007 | — | — | -0.50 | Active |
| V1012 | — | — | 2.15 | Active |
| V1013 | — | — | 4.11 | Active |

- The top 5 vessels by deferred exposure — V1009, V1004, V1014, V1006, V1003 — all sit between −0.75% 
    and +1.42% variance.
- While the 2 worst overspenders, V1005 at 8.78% and V1008 at 7.65%, sit near the bottom of the exposure list. 
- The risk labels contradict the exposure figures.

- Also VVIMP **column `Handover_Risk` doesn't match with exposure figures:
    - E.g. V1004 with 2nd most deferred exposure has risk as `Low` 
    - But contradictly, V1005 with 2nd least deferred exposure has risk as `High`

- So what it means is, something other than the variance report flagged these vessels or they were selected for reasons the workbook does not record.

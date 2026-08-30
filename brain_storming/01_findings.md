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



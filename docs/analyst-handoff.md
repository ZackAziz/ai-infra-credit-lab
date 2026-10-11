# AI Infrastructure Credit Lab — analyst handoff

**Prepared for Zack and Joel · 11 October 2026**  
**Question:** How much of each company's AI infrastructure buildout is funded by operating cash, and which *identified* obligations become more costly if rates stay higher?

## Decision 1: companies

Use **Meta, Oracle, and CoreWeave**. Meta is the large cash-generating benchmark; Oracle is a mature company making a rapid infrastructure expansion; CoreWeave is a specialized provider using several debt and asset-financing structures. This is a comparison of funding models, **not** a claim that the firms have identical businesses or credit risk.

The first release should use **cash funding pressure** as the common comparison. Debt sensitivity should be a separate, instrument-level panel. Do not rank the three on one composite score.

## Decision 2: metric definitions

All money figures in the comparison table are **USD billions**, rounded to three decimal places where the filing reports USD millions. Display each metric with its period and label it **reported**, **calculated**, or **assumed**. Keep full filing precision in the underlying data.

| Metric | Definition and implementation rule |
|---|---|
| Cash from operations (CFO) | Reported net cash provided by operating activities for the selected period. No annualization of a quarter in the main view. |
| Cash capital spending | Cash paid for property and equipment or the company's disclosed capital-expenditure cash outflow. Preserve the company's exact filing label and flag major noncash equipment additions separately. |
| Cash funding gap | Cash capital spending minus CFO. Positive = capex exceeded operating cash. This is **not** a forecast of borrowing needs; cash reserves, equity, leases, prepayments and other investing/financing cash flows matter. |
| Self-funding ratio | CFO ÷ cash capital spending. Show “N/M” if spending is zero or missing. Do not use this as a credit rating. |
| Gross borrowings | Carrying value of current plus noncurrent borrowings. Show recourse and nonrecourse debt separately for CoreWeave. Exclude operating leases from this headline figure. |
| Available liquid resources | Unrestricted cash + marketable securities; show restricted balances and undrawn facilities separately. Undrawn facilities are conditional borrowing capacity, not cash. |

**Lease policy:** Present operating and finance lease liabilities in their own section. Do not silently add them to borrowings or ignore them. A supplementary “debt plus leases” view may be useful, but must identify lease type and should not reuse interest expense as if it included all lease costs.

**Hold EBITDA / interest coverage for phase two.** EBITDA is not defined consistently in the three filings, and CoreWeave capitalizes some interest. Adding these ratios before reconciling definitions would create misleading comparability.

## Source-checked starter figures

Use these as an initial answer key for Joel's ingestion code. Values are reported, except **gross borrowings**, which is the sum of cited balance-sheet lines. Recheck against the filing if a newer report appears before publication.

| Company and period | CFO ($bn) | Cash capital spending ($bn) | Unrestricted cash ($bn) | Gross borrowings ($bn) | Interest expense ($bn) | Source and special note |
|---|---:|---:|---:|---:|---:|---|
| Oracle, 3 months ended Aug. 31, 2026; balance sheet Aug. 31 | 23.103 | 28.499 | 36.369 | 125.337 | 1.428 | [10-Q](https://www.sec.gov/Archives/edgar/data/1341439/000119312526389274/orcl-20260831.htm). Borrowings = 7.625 current + 117.712 noncurrent. CFO includes **11.363 billion** of customer prepayments with a significant financing component; highlight this in the chart tooltip. |
| Meta, 6 months ended Jun. 30, 2026; balance sheet Jun. 30 | 64.088 | 49.113 | 15.462 | 83.664 | 1.345 | [10-Q](https://www.sec.gov/Archives/edgar/data/1326801/000162828026050705/meta-20260630.htm). Marketable securities are **74.798 billion** in addition to cash. A portion of other cash equivalents is restricted for infrastructure agreements; do not count restricted cash as available. |
| CoreWeave, 6 months ended Jun. 30, 2026; balance sheet Jun. 30 | 3.663 | 14.117 | 5.524 | 35.068 | 1.176 net | [10-Q](https://www.sec.gov/Archives/edgar/data/1769628/000176962826000366/crwv-20260630.htm). Borrowings = 31.405 recourse + 3.663 nonrecourse. Cash capex uses the cash-flow statement's exact line. Noncash equipment additions and leases are material. |

**Period warning:** Oracle's row is **three months**, while Meta's and CoreWeave's are **six months**. The app must not place these values on one unlabeled ranking chart. For the first comparison, derive a common trailing-12-month window from each company's filings, or display period-specific panels until that reconciliation is complete.

## Decision 3: scenarios

1. **Reported base:** Show filing values without changing rates or CFO. This is historical, not a forecast.
2. **Higher refinancing cost:** Assume **+200 basis points** only for principal that contractually matures during the selected forward year *and is assumed refinanced*. Separately apply it to outstanding floating-rate principal whose rate actually resets, after checking hedges and floor/cap terms. Formula: incremental annual cash interest = eligible principal × 0.02 × fraction of year outstanding. Fixed-rate debt that stays outstanding gets **zero** immediate rate shock. If principal or reset terms cannot be verified, display **“not estimated.”**
3. **Slower cash generation:** Show CFO at 90% of the reported level while holding cash capex constant. This is an **illustrative sensitivity**, not a prediction that AI revenue declines 10%. The screen must show the unchanged reported base alongside the assumption.

Do not combine scenarios 2 and 3 into a single “default risk” probability. They answer different questions.

## Analyst checks before a public demo

- Trace each number to the filing, reporting period, units and exact line label. Keep a source URL and extraction timestamp in the repository.
- Reconcile cash capex with any noncash equipment financing, especially CoreWeave; show what is excluded.
- Build an instrument register with principal, fixed/floating, maturity, reset benchmark, spread, hedges and citation. The rates scenario stays disabled for an instrument until these fields are checked.
- Note that Oracle's unusually large customer prepayment inflow boosts the selected quarter's CFO. Explain it before calling the quarter “self-funded.”
- Write a two-page conclusion only after the common trailing-12-month comparison and instrument register are validated. A sound early conclusion is that **the three companies fund infrastructure through materially different mixes of operating cash, debt and other financing**; the project must quantify the differences before making a resilience ranking.

## Zack's work plan

**Now (Weeks 1–2):** Send Joel this file as the decisions on companies, definitions and leases. Independently check the table against the linked filings, especially CoreWeave's rounded cash capex. Record each checked line's filing, period, units and exact label. Write a short paragraph explaining why these three companies make a meaningful comparison and where it is imperfect.

**After Joel builds the first data view (Weeks 3–4):** Compare every displayed figure with the checked source lines. Mark it **matches filing / needs correction**. Do not approve a mixed-period ranking. Help choose the common trailing-12-month period and verify the calculation.

**Before scenarios (Weeks 5–6):** Read each filing's debt and lease notes. Fill in the instrument register with principal, rate type, maturity, hedge status and source. Decide which instruments can honestly enter the +200-basis-point scenario. Document any unavailable terms as unknown.

**At the finish (Weeks 7–8):** Write the two-page analyst memo: one defensible conclusion, evidence, a counterargument and limitations. Present the findings with Joel in a short demo. The memo should describe the scenario as a sensitivity, not an investment recommendation or a probability of default.

## Joel's work plan

**Now (next coding session):** Confirm the existing `ai-infra-credit-lab` repository has the README, requirements file and Oracle EDGAR test script committed. Run the script and save its output. Preserve the working code; this handoff supplies the analyst rules for the next commit.

1. Add Meta and CoreWeave to the SEC fetch workflow. Store each raw value with company, filing URL, filing date, period start/end, units and original XBRL tag or filing line label. Do not silently use a prior-year value if the current one is missing.
2. Create a small normalized table for the six agreed metrics. Convert filing values to **USD billions for display**, while retaining full precision in storage. Keep current and noncurrent borrowings separate in the raw data; calculate the displayed sum. Keep CoreWeave's recourse and nonrecourse totals separate.
3. Produce a simple three-company view that clearly says **Oracle: three months; Meta and CoreWeave: six months**. Include a clickable filing source beside every displayed number and a visible note for Oracle's customer prepayments and CoreWeave's noncash equipment financing. Do not add a ranking yet.
4. Send Zack the three rows and the underlying source list for a **matches filing / needs correction** review. Correct mismatches before adding any scenario controls.

**After Zack signs off on the base data:** Build a common trailing-12-month comparison and document how each figure was derived. Then implement the slower-cash-generation sensitivity. Add the higher-rates sensitivity only for instruments in Zack's checked debt register; show “not estimated” for unresolved instruments. Keep assumptions editable and distinct from reported data.

**Completion standard:** A recruiter should be able to open the app, click from a result to the filing, understand the period and assumption, and reproduce the key calculation. Commit work in small, descriptive steps so Joel's engineering contribution is visible on GitHub.

# UROP 2026: BXM Index Replication & Market Microstructure Analysis

**University of Minnesota - Undergraduate Research Opportunities Program (UROP)**
*   **Researcher:** Johnathan Park
*   **Advisor:** Prof. John Dodson
*   **Period:** Summer 2026 (May ~ August)

## Project Overview
This project investigates the market microstructure of the **Invesco S&P 500 BuyWrite ETF (Ticker: PBP)** against its benchmark, the **Cboe S&P 500 BuyWrite Index (BXM)**.

Derivative-based ETFs face structural frictions that plain equity index funds don't: options must be rolled monthly, execution timing affects the premium collected, and SPX option bid-ask spreads are wide relative to equities. This project measures and explains those frictions using high-resolution Cboe SPX options tick data (OPRA feed, full year 2022).

## Completed Work

**Expiry-to-Expiry Tracking Error Baseline.** Naive daily comparison of PBP vs. BXM yields an inflated tracking error (~4.80%) driven by intraday NBBO noise and a timestamp mismatch between the two products' closing conventions. Following Kumiega et al. (2024), switching to expiry-to-expiry (roll-to-roll) returns removes this noise, bringing tracking error down to 0.93% (R² = 0.996), consistent with Bloomberg's reported 1Y Price TE.

**Tracking Error Decomposition.** The remaining expiry-to-expiry gap decomposes exactly into three components: VWAP execution slippage (~55% of the gap), the fund's expense ratio (~11%), and an execution/sampling residual (~34%).

**Dynamic Option Selection.** A switching rule based on the implied-volatility/realized-volatility (IV-RV) spread — selling the ATM call (BXM) when IV > RV, and a 30-delta call (BXMD) otherwise — improves risk-adjusted performance over a static BXM rollover in the 2022 sample.

## Current Direction: Why Does Daily Tracking Error Collapse at Monthly Frequency?

In a July 2026 meeting, Prof. Dodson raised the hypothesis that BXM might be marked using VWAP prices *every day*, which would make it a non-investable benchmark and explain why daily TE (~4.80%) is so much larger than expiry-to-expiry TE (~0.93%).

We checked this against Cboe's official *BuyWrite Indices Methodology* documentation. **The hypothesis as originally stated does not hold**: VWAP is only used once a month, at the roll date, to price the newly sold option. On all other days, BXM's official formula uses a simple last-bid/ask-before-4pm mark, the same convention a real fund would use.

We nonetheless pursued Prof. Dodson's underlying question directly: how low can daily tracking error actually go if we reconstruct returns using Cboe's exact roll-date formula (a three-leg compounded return: previous-close-to-SOQ, SOQ-to-new-option-sale, new-option-sale-to-close) instead of naive same-day differencing?

- Reconstructing a single roll date (Oct 21, 2022) with the official formula matched the actual BXM return to within 0.04 percentage points, versus a 3.7-point error from naive differencing across the same roll.
- Extending this to all 12 roll dates and all non-roll trading days in 2022, with careful handling of stale/illiquid quotes, contract identification, and trade-condition filtering, brings full-year daily tracking error down from 4.80% to approximately 3.47%.
- The remaining gap does not appear to be a VWAP or methodology issue. It traces to a data limitation: our purchased dataset is **trade-only** (Cboe DataShop "Trades" product), not a continuous quote feed. On days with no trades in the held contract, we do not observe the market's actual bid/ask and must approximate it — this approximation is the binding constraint on how low daily TE can go with the current dataset.

## Data Sources
*   **Options Tick Data:** Cboe DataShop, SPX Options Trades + Calcs (OPRA feed), full year 2022 (~14.8 GB).
*   **PBP ETF Prices:** Cboe DataShop, Equity EOD Summary (15:45 ET bid/ask).
*   **BXM / BXMD Index Levels:** Cboe official daily history; Bloomberg (BXMD validation).
*   **SOQ Settlement Values:** Cboe Index Settlement Values page.

## Key References
*   Cboe Global Indices, *BuyWrite Indices Methodology* (official calculation rules).
*   Kumiega, Sterijevski & Wills (2024), "Black–Scholes 50 Years Later," *International Journal of Financial Studies*.
*   Bakshi & Kapadia (2003), *Review of Financial Studies*.
*   Invesco PBP Statutory Prospectus & Statement of Additional Information.
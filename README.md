# UROP 2026: Optimizing BXM Index Replication & Market Microstructure Analysis

**University of Minnesota - Undergraduate Research Opportunities Program (UROP)**
*   **Researcher:** Johnathan Park
*   **Advisor:** Prof. John Dodson
*   **Period:** Summer 2026 (May ~ August)

## Project Overview
This project investigates the performance drag and tracking error of the **Invesco S&P 500 BuyWrite ETF (Ticker: PBP)** against its benchmark, the **Cboe S&P 500 BuyWrite Index (BXM)**. 

While the BXM methodology strictly dictates a mechanical monthly roll-over of SPX call options based on the Volume-Weighted Average Price (VWAP) around 11:00 AM ET on the third Friday of each month, real-world ETFs struggle to perfectly replicate this process. 

This research aims to decompose the tracking error into two main components:
1. **Synchronicity Problem (Time Mismatch):** The discrepancy between standard equity market closing times (4:00 PM ET) and options market closing times (e.g., 4:15 PM or 3:55 PM ET maker horizons), which distorts standard performance metrics.
2. **Qualitative Tracking Error (Smearing & Sampling):** As stated in Invesco PBP's Statutory Prospectus, the fund allows for "sampling" and admits that "liquidity constraints may delay the purchase or sale of securities," preventing exact reproduction and causing implicit friction costs.

We utilize high-resolution tick data (OPRA feed) to simulate microstructural optimizations, proposing a better replication strategy that minimizes these real-world friction costs.

## Project Stages (Methodology)
*   **Stage 1: Baseline Thematic Replication**
    *   Constructing a baseline simulator that sells the most liquid, ATM call option on the expiration Friday based on standard daily closing prices.
    *   Calculating the standard tracking error against the official PBP ETF historical NAV/Market Price.
*   **Stage 2: Microstructure & VWAP Optimization**
    *   Solving the synchronicity problem by synchronizing the ETF pricing dummy to the exact options trading timestamps (e.g., 2:55 PM CT).
    *   Filtering out market maker quote delays and analyzing the BXM's 11:00 AM VWAP roll-over mechanics.
*   **Stage 3: Advanced Dynamics & Evaluation (Optional)**
    *   70/30 Walk-forward validation over the highly volatile 2022 market period.

## Data Sources
*   **Options Tick Data:** Cboe Datashop (OPRA feed for SPX, 2022 Full Year).
*   **ETF Benchmark Data:** Official Invesco ETF Trust historical NAV & Market Prices.
*   **Realized Volatility:** Ken French Data Library.

## Key References
*   Cboe S&P 500 BuyWrite Index (BXM) Methodology [1, 2].
*   Invesco Exchange-Traded Fund Trust Statutory Prospectus & Factsheet (Analysis on full replication limits and liquidity delays) [3-5].
*   Pessina & Whaley (2021), *Financial Analysts Journal*.

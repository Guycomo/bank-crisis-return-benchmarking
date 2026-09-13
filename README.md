# Bank Crisis Return Benchmarking

Benchmarking how Indian bank stocks behave during periods of market stress, compared against the Bank Nifty index.

## Motivation

Bank stocks don't all react the same way during a crisis — some fall because the entire sector is under stress, others fall for reasons specific to that bank (fraud, insolvency, bad exposure to a failing sector). This project tries to separate those two effects: how much of a bank's fall was "the market/sector doing badly" versus "this specific bank doing badly."

Four banks were chosen deliberately to represent different kinds of crisis:

- **Yes Bank** — went through an RBI-enforced moratorium and reconstruction in 2020, a genuine insolvency-style crisis
- **IndusInd Bank** — had meaningful exposure to the NBFC sector during the 2018–19 IL&FS-triggered shadow-banking crisis
- **Punjab National Bank (PNB)** — hit by a large-scale fraud case (the Nirav Modi scandal) uncovered in 2018, a governance/fraud-driven shock rather than a market one
- **Axis Bank** — a large private bank without a major standalone crisis in this period, included as a comparative benchmark

All four are measured against the  **Bank Nifty index**, which represents the sector as a whole.

## What this project does

1. **Pull historical price data** — daily closing prices for all five tickers (four banks + Bank Nifty) from 2005 to present, pulled live via `yfinance`.
2. **Compute 30-day rolling returns and volatility** — rather than looking at daily price changes (too noisy), the project tracks how each stock performed over trailing 30-trading-day windows, and how much its daily returns bounced around (volatility) over the same window.
3. **Visualize return distributions** — histograms of each asset's rolling returns, to see how "normal" behavior looks before any crisis flagging happens, and to sanity-check that thresholds set later are reasonable.
4. **Flag crisis episodes statistically** — instead of picking crisis dates by hand, any 30-day window where the return falls more than 2 (or 3) standard deviations below that asset's own historical mean gets flagged. Consecutive flagged days are grouped into distinct "episodes" (a crisis period has a start date, end date, and a worst point), rather than just picking one single worst day.
5. **Visualize episodes against the benchmark** — each bank's rolling return is plotted over time, with Bank Nifty overlaid for context, and every detected crisis episode shaded so it's visually obvious when and for how long the stock was in a flagged state.
6. **Compute excess return** — subtracting Bank Nifty's rolling return from each bank's rolling return, day by day. A large negative excess return during a crisis suggests the bank fell for its own reasons, not just because the whole sector was down.
7. **Compute normalized relative performance (RP_t)** — anchored to a specific crisis's start date, this tracks how a bank's cumulative growth compares to Bank Nifty's cumulative growth from that point onward, answering: "if I'd invested in this bank vs the index on the day the crisis started, how much worse off would I be by any later date?"

## Tech stack

- Python, Jupyter Notebook
- `yfinance` for data
- `pandas` / `numpy` for computation
- `plotly` for interactive visualizations

## Status

This is a work in progress — steps above reflect the current intended structure and will be updated here as the analysis evolves or changes.

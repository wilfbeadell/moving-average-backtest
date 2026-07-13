# Moving Average Crossover Backtest

A simple 20/60-day moving-average crossover, backtested across several assets and benchmarked against buy-and-hold. The point is to assess a naive trend rule honestly, not to present a working strategy.

## How it works

The strategy computes a 20-day (fast) and 60-day (slow) moving average of adjusted close prices. The signal is +1 (long) when the fast MA is above the slow MA and -1 (short) otherwise. The position is taken from the previous day's signal, so there is no lookahead bias. Daily strategy return = position × daily return, compounded into an equity curve and benchmarked against buy-and-hold over the same window.

## How to run

```
pip install -r requirements.txt
```

Open `Moving_Average_Strategy.ipynb` and run all cells.

## Results

The strategy underperforms buy-and-hold on most assets, and is heavily negative on strongly trending single names because it repeatedly takes short positions against the trend. Tesla is the clearest example: the strategy ends at roughly 0.26x while buy-and-hold ends at roughly 292x.

Final cumulative return (multiple of starting capital), taken from the recorded runs in the notebook:

| Asset | Window used | Strategy | Buy-and-hold |
|---|---|---|---|
| Brent crude (BZ=F) | 2020 – 2024 | 0.65x | 2.92x |
| Gold (GC=F) | 2020 – 2025 | [run to fill] | [run to fill] |
| Glencore (GLEN.L) | 2011 – 2025* | 1.01x | 0.98x |
| Apple (AAPL) | 2001 – 2020 | 8.13x | 206.60x |
| Shell (SHEL) | 2001 – 2020 | 0.38x | 2.70x |
| BP | 2001 – 2020 | 0.34x | 1.81x |
| Tesla (TSLA) | 2010 – 2025* | 0.26x | 291.65x |

Glencore is the only asset where the strategy edges ahead, a name that roughly consolidated over the period — which is consistent with the closing point below.

## Limitations

- Ignores transaction costs entirely. A ±1 crossover flips position frequently, so real costs (spread, commission, slippage) would erode returns further.
- Always long or short, never flat, the strategy is forced to take a view even when the signal carries no information.
- In-sample only. No train/test split, no out-of-sample validation.
- Date windows are inconsistent across assets, so the cross-asset table is indicative, not a controlled comparison.
- Trialling several assets introduces selection bias: any single asset "working" is not evidence of edge.

## Takeaway

A naive trend rule only helps in choppy or mean-reverting regimes and destroys value in strong trends.

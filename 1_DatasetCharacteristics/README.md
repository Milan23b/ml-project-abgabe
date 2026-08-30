# Dataset Characteristics

**[Open the exploratory data analysis notebook](exploratory_data_analysis.ipynb)**

## Dataset Information

### Dataset Source

- **Dataset:** public BTCUSDT spot-market 15-minute candles
- **Primary source:** [Binance Data Collection](https://data.binance.vision/)
- **Recent-candle supplement:** Binance REST market-data endpoints
- **Owner/contact:** not applicable; the dataset is public exchange market data

Each observation represents one completed UTC 15-minute candle containing identifiers, timestamps, OHLC prices, base and quote volume, trade count, taker-buy volume, provenance, and ingestion time.

### Dataset Characteristics

The notebook distinguishes two snapshots because the raw EDA was refreshed after the modeling artifacts had been created.

| Dataset stage | Observations | Columns/features | Time coverage |
|---|---:|---:|---|
| Raw EDA candles | 230,618 | 15 raw columns | 2020-01-01 to 2026-07-31 |
| Valid feature frame | 229,434 | 335 model features | 2020-01-08 to 2026-07-26 |
| Final aligned modeling table | 227,898 | 335 features plus target metadata | 2020-01-08 to 2026-07-25 |

The reduction from raw candles to model-ready rows is caused by the 672-candle feature warm-up and removal of rows without a complete future target window.

### Target Variable

- **Label name:** `target_binary_4`
- **Label type:** binary classification
- **Prediction horizon:** four 15-minute candles, equivalent to one hour
- **Positive class (`1`):** future one-hour log return greater than 0.1%
- **Negative class (`0`):** future one-hour log return less than or equal to 0.1%
- **Distribution:** 87,307 positive observations (38.31%) and 140,591 negative observations (61.69%)

Target values are aligned with `target_end_time_4`. Rows whose future window is incomplete are excluded, and all future-derived target columns are blocked from the feature matrix.

## Feature Description

The modeling table contains 335 numeric, causal features:

| Feature group | Count | Description |
|---|---:|---|
| Trend | 98 | Moving averages, relative price levels, slopes, RSI, MACD, and Bollinger-style indicators |
| Volume and trades | 69 | Rolling volume/trade statistics, z-scores, taker-buy ratios, and imbalance |
| Lags | 50 | Lagged returns, candle statistics, momentum, and indicator values |
| Volatility | 41 | Historical, Parkinson, Garman–Klass, ATR, and rolling volatility estimates |
| Returns and momentum | 28 | Simple/log returns and momentum across multiple horizons |
| Regime | 19 | Trend, volatility, volume, and combined market-regime encodings |
| Candle and gap | 15 | Body, range, wicks, close location, and time-gap indicators |
| Calendar | 13 | UTC hour, weekday, month, weekend, and cyclical encodings |
| Other controls | 2 | Additional validated numeric controls |

All model features passed checks for missing and infinite values, forbidden future columns, duplicate keys, deterministic generation, and prefix invariance.

## Data Quality Findings

- No ordinary null values in the raw 15-column candle table
- No duplicate keys or conflicting duplicate rows
- No invalid OHLC relationships
- 152 absent candles across 15 time gaps; largest gap: 23 candles
- 99.9341% completeness over the observed period
- 13 zero-volume and zero-trade candles
- Seven rows with non-standard or invalid close-time metadata

Time gaps are reported rather than filled. Fabricating candles through mean or forward imputation would distort returns, volatility, and target alignment.

## Exploratory Findings

- Fifteen-minute log returns are centered near zero but strongly heavy-tailed: standard deviation 0.003380 and kurtosis 114.80.
- Volume is strongly right-skewed; its Spearman correlation with absolute return is approximately 0.401.
- Raw-return autocorrelation is weak, while lag-1 absolute-return autocorrelation is 0.376, indicating volatility clustering.
- Maximum recorded drawdown is approximately -77.23%.
- Target prevalence and volatility vary by year and regime, requiring chronological evaluation.
- Selected feature-to-target correlations are weak; the largest recorded absolute value is RSI-14 at approximately 0.097.

## Notebook Coverage

The [EDA notebook](exploratory_data_analysis.ipynb) follows the required template sections:

1. Dataset Overview
2. Handling Missing Values
3. Feature Distributions
4. Possible Biases
5. Correlations

It includes compact recorded findings and configurable code for rerunning the live checks. Set `BTC_DATA_PATH` to the canonical candle CSV or Parquet file before execution.

# Bitcoin 15-Minute Time-Series Prediction

## Description

This project investigates whether historical BTCUSDT 15-minute market data can produce a stable probability estimate for a future upward price move. The primary task predicts whether the log return over the next four candles (one hour) exceeds 0.1%. Additional 4-hour and 24-hour horizons are evaluated to test whether a longer prediction window produces a more robust signal.

The workflow combines data-quality checks, exploratory analysis, causal feature engineering, chronological validation, baseline modeling, gradient-boosted tree models, probability calibration, and a transaction-cost-aware economic gate. Statistical improvement is not treated as sufficient evidence for trading: a model must also remain viable after fees, spread, slippage, and temporal instability.

### Task Type

Binary time-series classification with probability calibration and economic feasibility analysis.

## Dataset and Validation Design

- **Market:** BTCUSDT spot
- **Resolution:** 15-minute candles
- **Raw EDA snapshot:** 230,618 candles from 2020-01-01 through 2026-07-31
- **Modeling dataset:** 227,898 aligned observations with 335 causal features
- **Primary target:** `target_binary_4`
- **Positive class:** future one-hour log return greater than 0.1%
- **Class distribution:** 38.31% positive and 61.69% negative
- **Validation:** five expanding walk-forward folds with target-time purging and a 96-candle embargo
- **Reserved test:** observations from 2025 onward; not opened because no candidate passed the economic gate

## Results Summary

### Best Statistical Model

- **Model:** LightGBM with sigmoid probability calibration at the one-hour horizon
- **Primary metric:** five-fold walk-forward log loss (lower is better)
- **Development performance:** **0.601224 ± 0.035383**
- **Final-test performance:** not reported; the reserved test remained unopened by design

### Model Comparison

| Model | Horizon | Walk-forward log loss | Role |
|---|---:|---:|---|
| Logistic regression | 1 hour | 0.633709 ± 0.014487 | Learnable baseline |
| LightGBM + sigmoid | 1 hour | **0.601224 ± 0.035383** | Best primary-horizon model |
| CatBoost + sigmoid | 4 hours | 0.663146 ± 0.018843 | Best 4-hour model |
| LightGBM, raw probabilities | 24 hours | 0.692885 ± 0.000371 | Selected 24-hour model |

At the primary horizon, calibrated LightGBM reduces log loss by **0.032485**, or **5.13%**, relative to logistic regression. The higher fold variability shows that the improvement is not equally stable in every market period.

### Economic Feasibility

The statistical improvement did not produce a deployable strategy.

| Horizon | Selected model | Decision | Maximum robust round-trip cost | Assumed current cost | Economic gate |
|---:|---|---|---:|---:|---|
| 1 hour | LightGBM + sigmoid | `no_trade` | Not assessed | Not assessed | Failed |
| 4 hours | CatBoost + sigmoid | `no_trade` | 10 bp | 31.9744 bp | Failed |
| 24 hours | LightGBM, raw | `no_trade` | 5 bp | 31.9744 bp | Failed |

The 4-hour model is the strongest exploratory candidate, but its robust cost allowance covers only 31.28% of the assumed round-trip cost. The correct final decision is **no trade**, with no production approval and no final-test access.

### Key Insights

- **Strongest univariate target relationships:** RSI-14, 16-candle historical volatility, volatility regime, four-candle return, and 96-candle historical volatility. These correlations are weak and are not interpreted as causal feature importance.
- **Model strengths:** leakage-aware chronological validation, calibrated probabilities, explicit temporal-stability reporting, and a separate economic gate.
- **Model limitations:** one spot market and interval, predominantly OHLCV-derived features, nonstationary regimes, simplified execution costs, and no order-book, derivatives, on-chain, or sentiment data.
- **Practical impact:** the pipeline prevents a statistically better model from being presented as tradeable when realistic costs and stability requirements are not met.

## Documentation

1. **[Literature Review](0_LiteratureReview/README.md)**
2. **[Dataset Characteristics](1_DatasetCharacteristics/README.md)** — [notebook](1_DatasetCharacteristics/exploratory_data_analysis.ipynb)
3. **[Baseline Model](2_BaselineModel/README.md)** — [notebook](2_BaselineModel/baseline_model.ipynb)
4. **[Model Definition and Evaluation](3_Model/README.md)** — [notebook](3_Model/model_definition_evaluation.ipynb)
5. **[Presentation](4_Presentation/README.md)**

## Running the Notebooks

The notebooks contain compact recorded result tables from the completed analysis. The underlying market data and trained artifacts are not bundled.

Install the core dependencies:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn pyarrow lightgbm catboost
```

Run the notebooks in this order:

1. `1_DatasetCharacteristics/exploratory_data_analysis.ipynb`
2. `2_BaselineModel/baseline_model.ipynb`
3. `3_Model/model_definition_evaluation.ipynb`

Set `BTC_DATA_PATH` to the canonical candle CSV/Parquet file for live EDA checks. Set `BTC_TASK_DATA_PATH` to the prepared task table for modeling. Baseline and final-model training are disabled by default through `RUN_TRAINING = False`; change this only after verifying the data paths and dependencies. `BTC_HORIZON` selects 4, 16, or 96 candles in the final model notebook.

## Scope

This repository documents a research result, not a production trading system or financial recommendation.

## Cover Image

![Project Cover Image](CoverImage/cover_image.png)

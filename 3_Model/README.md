# Model Definition and Evaluation

**[Open the model-definition and evaluation notebook](model_definition_evaluation.ipynb)**

## Model Selection

LightGBM and CatBoost are evaluated as nonlinear alternatives to logistic regression. They can model thresholds and interactions among 335 lagged, rolling, volatility, trend, volume, calendar, and regime features while supporting regularization and stochastic subsampling.

Separate candidates are evaluated at three horizons:

- h4: one hour
- h16: four hours
- h96: 24 hours

Model and calibration selection use five-fold expanding walk-forward cross-validation. The final test is not used for tuning or comparison.

## Feature Engineering

The final models use the same leakage-safe 335-feature set documented in [Dataset Characteristics](../1_DatasetCharacteristics/README.md). A 672-candle warm-up is removed, target end-times are explicit, and future/target columns are excluded from `X`. Feature generation passed prefix-invariance, determinism, missing-value, infinity, duplicate-key, and target-leakage checks.

## Hyperparameter Tuning and Calibration

The tuning process minimizes mean walk-forward log loss with a temporal-stability penalty. The primary h4 search requests 100 trials per model; h16 and h96 request 25. Pruning stops unpromising trials.

Search dimensions include learning rate, tree count, depth/leaves, row and column subsampling, child-size constraints, binning, split gain, and L1/L2 regularization. Raw, sigmoid, and isotonic probability variants are compared. Calibration selection uses chronological out-of-fold predictions only.

### Selected Models

| Horizon | Selected model | Calibration | Walk-forward log loss mean ± SD |
|---:|---|---|---:|
| 1 hour | LightGBM | Sigmoid | **0.601224 ± 0.035383** |
| 4 hours | CatBoost | Sigmoid | **0.663146 ± 0.018843** |
| 24 hours | LightGBM | Raw | **0.692885 ± 0.000371** |

At h4 the tuned LightGBM and CatBoost results are nearly tied before calibration. The final calibrated selection is LightGBM. At h96 CatBoost narrowly wins the raw tuning comparison, but the complete calibration-selection procedure chooses raw LightGBM. The notebook distinguishes tuning winners from final calibrated selections.

## Evaluation Metrics

- **Log loss:** primary probability-quality and model-selection metric
- **Brier score and calibration:** probability reliability before thresholding
- **ROC AUC and PR AUC:** ranking performance, including focus on the less frequent positive class
- **Fold mean and standard deviation:** average performance and temporal stability
- **Economic metrics:** net return, trade count, profit factor, positive-fold ratio, and maximum robust round-trip cost

No final-test metric is reported because the pre-specified economic gate did not pass.

## Comparison with the Baseline

| Model | Horizon | CV log loss | Change from logistic baseline |
|---|---:|---:|---:|
| Logistic regression | 1 hour | 0.633709 | Reference |
| LightGBM + sigmoid | 1 hour | 0.601224 | -0.032485 (-5.13%) |

The nonlinear model improves average probability quality, but its fold standard deviation rises from 0.014487 to 0.035383. The signal is therefore statistically useful but not uniformly stable across market periods.

## Economic Feasibility and Final Decision

The assumed realistic round-trip cost is 31.9744 basis points.

| Horizon | Decision | Economic gate | Maximum robust cost | Current-cost coverage | Final-test access |
|---:|---|---|---:|---:|---|
| 1 hour | `no_trade` | Failed | Not assessed | Not assessed | No |
| 4 hours | `no_trade` | Failed | 10 bp | 31.28% | No |
| 24 hours | `no_trade` | Failed | 5 bp | 15.64% | No |

Every outer fold selects no-trade at every horizon. The four-hour model is the strongest exploratory candidate, but it remains cost-limited. Production approval is not granted, and the reserved final test remains unopened.

This negative result is intentional and methodologically valid: no-trade is an explicit alternative, so the pipeline does not promote the least-bad threshold as a viable strategy.

## Limitations

- BTCUSDT spot only
- One 15-minute market-data feed
- Predominantly OHLCV and aggregated-trade features
- No order-book, futures, funding, open-interest, on-chain, or sentiment inputs
- Simplified transaction-cost and execution assumptions
- No short, portfolio, or live-execution strategy

## Future Work

- Cost-aware long/no-trade targets
- Direct net-return regression or ranking
- Long/short/no-trade multiclass modeling
- Order-book, derivatives, funding, and open-interest features
- Regime-specific models and next-open execution
- A newly pre-specified confirmation protocol before any final-test access

## Notebook Execution

Set `BTC_TASK_DATA_PATH` to the prepared task table and `BTC_HORIZON` to `4`, `16`, or `96`. Training is disabled by default with `RUN_TRAINING = False`. LightGBM and CatBoost are optional until training is enabled.

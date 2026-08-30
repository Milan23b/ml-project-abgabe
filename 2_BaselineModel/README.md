# Baseline Model

**[Open the baseline notebook](baseline_model.ipynb)**

## Baseline Model Results

### Model Selection

- **Primary baseline:** L2-regularized logistic regression
- **Sanity-check references:** majority class, random class prior, and a momentum rule
- **Rationale:** logistic regression is fast, interpretable, produces probabilities, and tests whether the engineered features contain a stable linear signal. The naive references show whether a fitted model improves on class-frequency and market-heuristic behavior.

The saved walk-forward run selects logistic regression as the learnable baseline. Random prior has the lowest log loss on the single 2024 holdout, but logistic regression has the best mean cross-validation score. This disagreement is retained as evidence of temporal instability rather than resolved by tuning to the holdout.

### Feature Selection

The baseline uses all 335 validated numeric features so that later model comparisons isolate model capacity rather than a different input set. Identifiers, timestamps, target columns, target end-times, and every future-derived column are excluded. Standardization and any median imputation are fitted inside each training fold only.

### Evaluation Methodology

| Partition | Rows | Approximate share of the modeling table | Period |
|---|---:|---:|---|
| Training | 137,988 | 60.55% | Through 2023-12-31 |
| Validation | 35,036 | 15.37% | 2024 after embargo |
| Reserved test | 54,674 | 23.99% | 2025 onward |
| Purge/embargo exclusions | 200 | 0.09% | Boundary safety rows |

Cross-validation uses five expanding walk-forward folds. Each fold has an 8,540-row validation window, target-end-time purging, and a 96-candle/one-day embargo. The reserved test is not loaded or evaluated for model selection.

### Model Performance

| Model | Holdout log loss | CV log loss mean ± SD | Holdout balanced accuracy | Holdout ROC AUC | Holdout PR AUC | Holdout Brier score |
|---|---:|---:|---:|---:|---:|---:|
| Logistic regression | 0.677582 | **0.633709 ± 0.014487** | **0.546210** | **0.566781** | **0.448365** | 0.242316 |
| Random prior | **0.669317** | 0.636716 ± 0.015961 | 0.506718 | 0.500000 | 0.391255 | **0.238180** |
| Momentum | 0.870268 | 0.729983 ± 0.040930 | 0.492391 | 0.469760 | 0.381509 | 0.295683 |
| Majority | 13.513459 | 10.808130 ± 1.387648 | 0.500000 | 0.500000 | 0.391255 | 0.391255 |

The comparison reference for subsequent models is logistic regression's five-fold log loss of **0.633709 ± 0.014487**.

## Metric Practical Relevance

- **Log loss:** primary selection metric because downstream decisions use predicted probabilities. Confident wrong predictions receive a large penalty.
- **Brier score:** measures squared probability error and complements log loss with an intuitive calibration view.
- **Balanced accuracy:** gives equal importance to positive and negative classes despite the 38%/62% class distribution.
- **ROC AUC:** measures general ranking quality independent of one classification threshold.
- **PR AUC:** focuses on the less frequent positive class and is more informative than accuracy when positives are not the majority.
- **F1:** summarizes the precision/recall trade-off after choosing a 0.5 threshold, but it is secondary because the trading threshold is selected later.

None of these statistical metrics establishes profitability. A probability model must still overcome fees, spread, slippage, threshold uncertainty, and regime changes.

## Notebook Execution

The notebook contains the recorded result table and a compact standalone implementation. Set `BTC_TASK_DATA_PATH` to the prepared binary-h4 CSV or Parquet table. Training is disabled by default with `RUN_TRAINING = False` to avoid an accidental expensive rerun.

## Next Step

Logistic regression serves as the fixed reference for the [Model Definition and Evaluation](../3_Model/README.md) phase.

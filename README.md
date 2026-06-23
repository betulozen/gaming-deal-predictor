# Gaming Deal Predictor

Multi-class machine learning framework forecasting the maximum discount tier a Steam
game is expected to reach within a future 90-day period, using historical pricing
dynamics collected from SteamSpy, Steam Store, and IsThereAnyDeal (ITAD) APIs.

## Overview

- **Task**: Forecast whether a Steam game will reach a *No/Small (<50%)*,
  *Medium (50–75%)*, or *Big (≥75%)* discount within the next 90 days.
- **Data**: 2,271 unique Steam games, 8,853 prediction windows, built entirely
  from public APIs — no pre-built dataset was used.
- **Models**: Logistic Regression, Random Forest, Gradient Boosting, Naive Bayes.
- **Evaluation**: A combined temporal–game split that simultaneously prevents
  temporal leakage and game-level leakage between training and test sets.
- **Best result**: Random Forest — Accuracy 0.823, weighted F1 0.827, OvR AUC 0.943.

## Repository Structure

**notebooks/**
- `gaming_deal_predictor_v7.ipynb` — Data collection, feature engineering, training pipeline
- `training_evaluation_v7.ipynb` — Model evaluation, hyperparameter tuning, error analysis

**data/**
- `train_v7.csv` — Training split (post feature engineering)
- `test_v7.csv` — Test split (post feature engineering)

**model_meta_v7.json** — Feature list, class names, best model reference

## How to Reproduce

1. **Run the main pipeline notebook** — `gaming_deal_predictor/notebooks/gaming_deal_predictor_v7.ipynb`
   - Collects game data from SteamSpy, Steam Store, and ITAD
   - Builds 90-day rolling prediction windows and engineers 8 temporal features
   - Applies the combined temporal–game split
   - Trains all four models and saves the files in `data/`, `models/`, and `model_meta_v7.json`

2. **Run the evaluation notebook** — `gaming_deal_predictor/notebooks/training_evaluation_v7.ipynb`
   - Loads the artifacts produced in step 1
   - Reproduces all metrics, confusion matrices, ROC curves, and hyperparameter tuning results reported in the paper

You will need your own free ITAD API key to run the data collection step.
Get one at [isthereanydeal.com/apps](https://isthereanydeal.com/apps)
and set it in the `ITAD_KEY` variable in the first notebook.

## Features

| Feature | Description |
|---|---|
| `game_age_days` | Days since release at the start of the prediction window |
| `days_since_last_discount` | Days since the most recent discount of at least 10% |
| `n_itad_records` | Number of ITAD pricing records in the previous 90 days |
| `avg_discount_depth_prev` | Average discount depth in the previous 90 days |
| `n_big_discounts_prev` | Number of discounts ≥50% in the previous 90 days |
| `avg_discount_frequency` | Fraction of historical windows with a discount ≥10% |
| `never_discounted` | Whether the game has ever been discounted |

## License

Code is provided for academic and research purposes. Data is derived from
public APIs (SteamSpy, Steam Store, IsThereAnyDeal) under their respective terms of use.

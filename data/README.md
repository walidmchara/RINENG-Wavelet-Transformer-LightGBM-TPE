# Data and experimental protocol

## Input definition

The forecasting target is **global irradiance**, expressed in **W/m²**. The entry point `train.py` expects a chronologically ordered CSV table containing a numeric `target` column and observed predictor variables.

For meteorological forecasting, typical candidate predictors include temperature, relative humidity, wind, cloud cover, and solar geometry when they are available from the source records. The actual feature set must follow the article's data definition, rather than an automatically inferred list of all numeric columns.

## Format and preparation

A CSV may use the fields `time`, predictor columns, and `target`. The `time` field defines the observation order; predictors are numerical and should be screened for missing readings and inconsistent units.

Example header and rows:

```csv
time,temperature,cloud_cover,target
2024-06-01T10:00:00,23.1,0.2,612.4
2024-06-01T10:15:00,23.4,0.3,588.7
```

Rows are stably sorted by `time` if that column exists; otherwise the input
row order is assumed chronological and a warning is emitted. Use consistent
sampling intervals, finite numeric values, and at least 80 records. Missing
values are rejected rather than silently imputed. The implementation uses
train-only normalization and chronological training, calibration, validation,
and test divisions. Past target observations may be used autoregressively, but
future and same-forecast-horizon targets must not enter the input window.
Inputs are windows of `--lookback` prior rows; labels are the next row's
target, so this is a one-step-ahead forecast.

```bash
python train.py --csv path/to/measurements.csv --target target --time-col time --epochs 40 --out outputs/experiment
```

For other column names, pass `--target` and `--time-col`; use
`--features feature_a,feature_b` to explicitly select predictors. The target
is appended automatically for autoregressive input. If timestamps are absent,
use `--time-col ""` to suppress the missing-time warning only when the file is
already correctly ordered.

## Experimental controls

The LightGBM residual estimator must use an isolated calibration slice, while Optuna-TPE hyperparameters are selected using validation data alone.

The `--smoke` flag exercises the processing and training code using simulated signals; its error metrics are not measurements from the study. Actual dataset provenance, partition boundaries and metric units belong in the experiment log. The repository does not bundle the original measured forecasting dataset.

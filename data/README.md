# Data and experimental protocol

## Input definition

The forecasting target is **global irradiance**, expressed in **W/m²**. The entry point `train.py` expects a chronologically ordered CSV table containing a numeric `target` column and observed predictor variables.

For meteorological forecasting, typical candidate predictors include temperature, relative humidity, wind, cloud cover, and solar geometry when they are available from the source records. The actual feature set must follow the article's data definition, rather than an automatically inferred list of all numeric columns.

## Format and preparation

A CSV may use the fields `time`, predictor columns, and `target`. The `time` field defines the observation order; predictors are numerical and should be screened for missing readings and inconsistent units.

The implementation uses train-only normalization and a chronological training, calibration, validation and test division. Past target observations may be used autoregressively, but future and same-forecast-horizon targets must not enter the input window.

```bash
python train.py --csv path/to/measurements.csv --target target --time-col time --epochs 40 --out outputs/experiment
```

## Experimental controls

The LightGBM residual estimator must use an isolated calibration slice, while Optuna-TPE hyperparameters are selected using validation data alone.

The `--smoke` flag exercises the processing and training code using simulated signals; its error metrics are not measurements from the study. Actual dataset provenance, partition boundaries and metric units belong in the experiment log. The repository does not bundle the original measured forecasting dataset.

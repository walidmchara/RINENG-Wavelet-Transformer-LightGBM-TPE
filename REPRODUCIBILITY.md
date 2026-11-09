# Experimental reproducibility

**Study:** A hybrid wavelet transformer LightGBM model optimized by optuna-TPE for global irradiance forecasting in solar electric vehicle energy management  
**DOI:** https://doi.org/10.1016/j.rineng.2026.109678

## Model and tuning protocol

The implementation combines a wavelet-oriented Transformer forecast with LightGBM residual correction and Optuna-TPE hyperparameter selection. The code provides a configurable experimental pipeline, but publication-level agreement requires the same wavelet design, network settings, optimization budget and training windows as the study.

## Data and validation

The time-series CSV must preserve sampling order, feature units and forecast issuance time. TPE tuning uses a validation partition, while LightGBM residual learning uses a separate chronological calibration slice. Neither stage should be fitted on the final test interval.

The original measured source dataset is not included here. Normalization parameters are estimated from the training period, and predictors must be limited to the information available at forecast time.

## Evaluation and reporting

A repeatable run records source provenance, feature definitions, target units, forecast horizon, train/validation/test dates, seeds, search budgets and all metrics. The `--smoke` command uses simulated signals only to check execution. Numerical replication of the publication has not yet been independently verified; new outputs should be identified as independent experimental results.

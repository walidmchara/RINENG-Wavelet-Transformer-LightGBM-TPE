# Experimental reproducibility

**Study:** A hybrid wavelet transformer LightGBM model optimized by optuna-TPE for global irradiance forecasting in solar electric vehicle energy management  
**DOI:** https://doi.org/10.1016/j.rineng.2026.109678

## Model and tuning protocol

The default configuration selects a four-scale Morlet filter bank followed by
a two-layer Transformer encoder, a LightGBM residual estimator, and Optuna-TPE
by default (`--search paper`; use `--search none` to disable tuning). The residual model is
fit on a chronological calibration slice separate from the neural model's
training rows. Its inputs are the base forecast and summary statistics of the
historical input window. Search currently tunes only learning rate and
dropout, and evaluates each candidate for at most three epochs; it does not
reconstruct an unspecified publication search space or budget.

The repository does not include the manuscript PDF or the original experiment
source. Consequently, the DOI/title and the general model components in this
repository are not enough to validate equation-level or result-level
consistency. Treat this code as a configurable research implementation, not a
verified reproduction. A full comparison requires the article's method,
original data, feature/forecast definitions, exact split, model and optimizer
settings, and reference metrics.

## Data and validation

The time-series CSV must preserve sampling order, feature units and forecast issuance time. TPE tuning uses a validation partition, while LightGBM residual learning uses a separate chronological calibration slice. Neither stage should be fitted on the final test interval.

The original measured source dataset is not included here. Normalization
parameters are estimated from the training period, and predictors must be
limited to the information available at forecast time. This implementation
uses the next row after each historical input window as its label, so it
represents one-step-ahead prediction at the CSV sampling interval. Change the
data construction only after confirming the study's forecast horizon and
issuance convention.

## Evaluation and reporting

A run's metrics record the input SHA-256, feature names, split times and row
counts, configuration, seed, software versions, and metrics. The `--smoke`
command uses simulated signals only to check execution. Numerical replication
of the publication has not been independently verified; new outputs should be
identified as independent experimental results.

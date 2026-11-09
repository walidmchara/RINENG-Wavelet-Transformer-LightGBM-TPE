# Wavelet–Transformer–LightGBM with Optuna-TPE

**Publication:** A hybrid wavelet transformer LightGBM model optimized by optuna-TPE for global irradiance forecasting in solar electric vehicle energy management  
**Journal:** Results in Engineering (2026)  
**DOI:** https://doi.org/10.1016/j.rineng.2026.109678

## Method

A wavelet-oriented Transformer model provides the initial irradiance forecast, followed by a LightGBM residual estimator trained on separate calibration observations. Optuna's Tree-structured Parzen Estimator selects hyperparameters using validation data.

The project includes chronological data preparation, model fitting, optional hyperparameter selection, evaluation metrics and graphical diagnostics. The architecture family and training parameters are configured in [config.json](config.json).

## Input data

The training script accepts a time-ordered multivariate CSV with a numeric `target` column. The target in this study is **global irradiance**. Dataset columns and evaluation assumptions are discussed in [data/README.md](data/README.md). Source data must be obtained from the study's experimental dataset and should be kept distinct from the synthetic software check.

## Installation and execution

```bash
python -m venv .venv
pip install -r requirements.txt
pytest -q
python train.py --smoke --epochs 2 --out outputs/smoke
```

With measured time-series data:

```bash
python train.py --csv path/to/measurements.csv --target target --time-col time --epochs 40 --out outputs/experiment
```

The program saves training history, test predictions, error metrics, and a fitted checkpoint under the chosen output directory. Optional parameter search is supported through `--search paper --trials N`, subject to the method-specific qualifications below.

## Evaluation notes

Calibration data are distinct from the final test period. A publication-level comparison requires the original search budget, data windowing and evaluation protocol.

The test interval is chronological. Normalization statistics must be estimated on training data, and all reported comparisons should specify the time span, forecast horizon, seed, target units and metric definitions. [REPRODUCIBILITY.md](REPRODUCIBILITY.md) records the implementation boundaries and outstanding experimental checks.

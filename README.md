# Wavelet–Transformer–LightGBM with Optuna-TPE

**Publication:** A hybrid wavelet transformer LightGBM model optimized by optuna-TPE for global irradiance forecasting in solar electric vehicle energy management  
**Journal:** Results in Engineering (2026)  
**DOI:** https://doi.org/10.1016/j.rineng.2026.109678

## Method

A wavelet-oriented Transformer model provides the initial irradiance forecast, followed by a LightGBM residual estimator trained on separate calibration observations. Optuna's Tree-structured Parzen Estimator selects hyperparameters using validation data.

The project includes chronological data preparation, model fitting, optional hyperparameter selection, evaluation metrics and graphical diagnostics. The architecture family and training parameters are configured in [config.json](config.json).

## Input data

The training script accepts a time-ordered multivariate CSV with a numeric `target` column. The target in this study is **global irradiance**. Dataset columns and evaluation assumptions are discussed in [data/README.md](data/README.md). Source data must be obtained from the study's experimental dataset and should be kept distinct from the synthetic software check.

## Train from a clean checkout

Python 3.10+ is recommended. From the repository directory:

```bash
git clone https://github.com/walidmchara/RINENG-Wavelet-Transformer-LightGBM-TPE.git
cd RINENG-Wavelet-Transformer-LightGBM-TPE
python -m venv .venv
# Linux/macOS:
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
# Install the CPU build; it runs on machines without a CUDA GPU.
python -m pip install torch --index-url https://download.pytorch.org/whl/cpu
python -m pip install -r requirements.txt
python train.py --smoke --epochs 2 --out outputs/smoke
```

The smoke run uses generated signals to check the full pipeline; it is not a
scientific experiment. For your own chronological CSV, use a numeric target
column, predictor columns, and preferably a sortable time column:

```bash
python train.py --csv path/to/measurements.csv --target target --time-col time \
  --features temperature,humidity,cloud_cover --epochs 40 \
  --search paper --trials 20 --out outputs/experiment
```

`--features` is optional; when omitted, all numeric columns except the time
column are used, and `target` is included for autoregressive input. If there is
no time column, the CSV row order is used and the program warns you. By
default, `--search paper` uses the optimizer named in `config.json`
(Optuna-TPE); pass `--search none` to skip tuning. You can select `--family` and
`--residual` from the CLI or edit `config.json`.

The output directory contains the neural checkpoint, optional native residual
model artifact, training log, holdout predictions, metrics, and a plot. The
metrics record the input file's SHA-256, chronological split times, row counts,
and key software versions. Keep the original data and run configuration with
these outputs. Use `--help` to see all options.

## Evaluation notes

Calibration data are distinct from the final test period. A publication-level comparison requires the original search budget, data windowing and evaluation protocol.

The test interval is chronological. Normalization statistics are estimated on
training data. The implementation and the paper cannot be confirmed equivalent
without the manuscript's full method, original data, and exact experiment
protocol. See [REPRODUCIBILITY.md](REPRODUCIBILITY.md) before interpreting
results as a reproduction.

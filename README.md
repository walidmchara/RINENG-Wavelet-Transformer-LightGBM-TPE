# RINENG-Wavelet-Transformer-LightGBM-TPE

**Article:** A hybrid wavelet transformer LightGBM model optimized by optuna-TPE for global irradiance forecasting in solar electric vehicle energy management  
**Journal:** Results in Engineering (2026)  
**DOI:** https://doi.org/10.1016/j.rineng.2026.109678

## Status: executable research *reconstruction*, not verified original code

The modules implement the named architecture family (`wavelet_transformer`), chronological data handling, training, testing, and diagnostic outputs. This project was newly prepared using the PDF, not extracted from the original authors' source. **Never present its outputs as the paper's exact published results** until all hyperparameters, feature engineering, source datasets, protocol, loss details and random seeds are independently audited.

The paper identifies **optuna** optimization and **lightgbm** residual modeling. Both are exposed in the code, where applicable. `cbo` is currently an explicitly illustrative chaotic-search placeholder, *not* a validated transcription of the full published optimizer. `informer` is a compact sampled-key sparse attention prototype rather than a validated full Informer implementation.

## Quick start

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# Linux: source .venv/bin/activate
pip install -r requirements.txt
pytest -q
python train.py --smoke --epochs 2 --out outputs/smoke
```

## Real data

```bash
python train.py --csv data/your_data.csv --target target --time-col time --epochs 40 --out outputs/real_trial
```

Optional small optimization sanity check:

```bash
python train.py --smoke --epochs 2 --search paper --trials 3 --out outputs/search_smoke
```

**Important:** the default `--search none` avoids falsely implying the global optimizer has run. For full published-protocol reproduction you must first validate `config.json` and the manuscript's original search space, objective function, budget, and seed procedures.

Outputs: checkpoint, training history, independent chronological test predictions, JSON metrics, plot. Neural residuals are calibrated on a separate chronological slice *inside* the train period; independent test data is never used to train the residual learner.

Read `REPRODUCIBILITY.md` and `data/README.md` before claiming reproduction.

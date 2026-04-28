# Air Quality Forecasting Environmental Signals

> Multivariate time-series forecasting project for predicting PM2.5 air pollution 24 hours ahead across multiple Beijing monitoring stations, using historical pollution, meteorology, temporal features, and leakage-safe chronological validation.

---

## Project objective

This project builds a portfolio-grade environmental forecasting workflow around the **Beijing Multi-Site Air Quality Dataset**.

The main objective is to predict future **PM2.5 concentration** from hourly air-quality and meteorological signals, while treating the task as a serious temporal ML problem rather than a generic regression notebook.

Core goals:

- assemble a multi-station hourly air-quality dataset;
- audit temporal continuity, missingness, and station coverage;
- define leakage-safe forecasting targets;
- compare strong temporal baselines against classical ML models;
- engineer lag, rolling, calendar, station, pollutant, and weather features;
- evaluate model behavior during high-pollution periods;
- assess whether forecasts can support an early-warning style interpretation.

---

## Dataset

This project uses the **Beijing Multi-Site Air Quality Dataset** from the **UCI Machine Learning Repository**:

```text
https://archive.ics.uci.edu/dataset/501/beijing%2Bmulti%2Bsite%2Bair%2Bquality%2Bdata
```

The dataset was donated to UCI on **2019-09-19** and contains hourly air pollution and meteorological observations from **12 nationally controlled air-quality monitoring sites in Beijing**. The time period covered is **2013-03-01 to 2017-02-28**. Missing values are denoted as `NA`. :contentReference[oaicite:0]{index=0}

According to UCI, the dataset includes **6 main air pollutants** and **6 relevant meteorological variables** at multiple Beijing sites, with **420,768 instances** and an associated regression task. :contentReference[oaicite:1]{index=1}

### Dataset citation

```text
Chen, S. (2017). Beijing Multi-Site Air Quality [Dataset].
UCI Machine Learning Repository.
https://doi.org/10.24432/C5RK5G
```

The dataset is licensed under **Creative Commons Attribution 4.0 International (CC BY 4.0)** according to the UCI dataset page. :contentReference[oaicite:2]{index=2}

### Raw files used

The canonical raw inputs for this project are the 12 station-level files:

```text
PRSA_Data_Aotizhongxin_20130301-20170228.csv
PRSA_Data_Changping_20130301-20170228.csv
PRSA_Data_Dingling_20130301-20170228.csv
PRSA_Data_Dongsi_20130301-20170228.csv
PRSA_Data_Guanyuan_20130301-20170228.csv
PRSA_Data_Gucheng_20130301-20170228.csv
PRSA_Data_Huairou_20130301-20170228.csv
PRSA_Data_Nongzhanguan_20130301-20170228.csv
PRSA_Data_Shunyi_20130301-20170228.csv
PRSA_Data_Tiantan_20130301-20170228.csv
PRSA_Data_Wanliu_20130301-20170228.csv
PRSA_Data_Wanshouxigong_20130301-20170228.csv
```

Any additional `data.csv` or `test.csv` files included in the download are **not treated as canonical train/test splits**. This project defines its own chronological train/validation/test split to preserve temporal evaluation discipline.

### Expected raw-data location

```text
data/raw/beijing_multi_site_air_quality/
```

---

## Forecasting task

### Main target

```text
PM2.5 concentration
```

### Main forecasting horizon

```text
t + 24h
```

The core task is to predict PM2.5 concentration 24 hours ahead.

### Secondary horizon

```text
t + 1h
```

The 1-hour horizon is used as a compact sanity check and short-horizon contrast. The main portfolio value remains the 24-hour forecasting task.

### Unit of analysis

```text
one row = one station-hour prediction instance
```

Example:

```text
station: Aotizhongxin
timestamp: 2015-07-12 08:00
features: information available up to 2015-07-12 08:00
target_h24: PM2.5 at 2015-07-13 08:00
```

---

## Modeling philosophy

This project prioritizes:

- temporal validation over random splitting;
- strong baselines before model complexity;
- leakage-safe feature engineering;
- interpretable staged feature development;
- honest diagnostics over leaderboard-style score chasing;
- high-pollution error analysis, not only global average metrics.

The project should not become:

- a generic regression notebook;
- a model zoo;
- a deep-learning-first experiment;
- a dashboard project;
- a pseudo-regulatory air-quality alert system.

---

## Planned notebook roadmap

```text
00_dataset_assembly.ipynb
01_temporal_data_audit.ipynb
02_eda_environmental_signals.ipynb
03_forecasting_problem_design.ipynb
04_temporal_baselines.ipynb
05_feature_engineering_lags_rolling.ipynb
06_ml_forecasting_models.ipynb
07_forecast_diagnostics_error_analysis.ipynb
08_interpretability_and_alert_layer.ipynb
09_finalist_holdout_evaluation.ipynb
10_conclusions_packaging.ipynb
11_optional_advanced_followups.ipynb
```

### Notebook roles

| Notebook | Purpose |
|---|---|
| `00_dataset_assembly.ipynb` | Load and combine the 12 raw station files into one long-format hourly dataset |
| `01_temporal_data_audit.ipynb` | Audit timestamp continuity, missingness, gaps, station coverage, and value sanity |
| `02_eda_environmental_signals.ipynb` | Explore PM2.5 distribution, station differences, temporal patterns, weather, pollutants, and high-pollution episodes |
| `03_forecasting_problem_design.ipynb` | Freeze forecasting horizons, prediction-time availability rules, metrics, and chronological splits |
| `04_temporal_baselines.ipynb` | Build persistence, previous-day/week, rolling, and seasonal baselines |
| `05_feature_engineering_lags_rolling.ipynb` | Create leakage-safe lag, rolling, calendar, station, pollutant, weather, and missingness features |
| `06_ml_forecasting_models.ipynb` | Compare bounded ML candidates against temporal baselines |
| `07_forecast_diagnostics_error_analysis.ipynb` | Diagnose finalist behavior by station, season, hour, PM2.5 level, and high-pollution periods |
| `08_interpretability_and_alert_layer.ipynb` | Interpret model signals and optionally evaluate an early-warning style threshold layer |
| `09_finalist_holdout_evaluation.ipynb` | Evaluate selected finalist on untouched chronological hold-out data |
| `10_conclusions_packaging.ipynb` | Produce portfolio-ready conclusions, limitations, and README material |
| `11_optional_advanced_followups.ipynb` | Optional work: multi-horizon, quantile forecasting, spatial features, station-specific models, or deep temporal benchmark |

---

## Repository structure

```text
air-quality-forecasting-environmental-signals/
├── configs/
├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/
├── models/
├── notebooks/
├── reports/
│   └── figures/
├── src/
├── .gitignore
├── pyproject.toml
└── README.md
```

### `configs/`

Project configuration files. Dataset paths, feature decisions, split settings, and preprocessing options should live here when they become stable enough to configure.

### `data/`

- `raw/` — original input data, immutable
- `interim/` — intermediate transformations, audits, and reusable artifacts
- `processed/` — final modeling-ready datasets

Expected raw dataset placement:

```text
data/raw/beijing_multi_site_air_quality/
```

### `models/`

Serialized models, finalist artifacts, model cards, and selected experiment outputs.

### `notebooks/`

Main analytical workflow: assembly, audit, EDA, forecasting design, baselines, feature engineering, modeling, diagnostics, hold-out evaluation, and conclusions.

### `reports/`

Analytical outputs:

- figures;
- tables;
- evaluation summaries;
- final portfolio material.

### `src/`

Reusable project logic. Starts empty. Code should be extracted here only when repeated notebook logic becomes stable enough to justify reuse.

---

## Environment

This project uses a local virtual environment managed via `pyproject.toml`.

```bash
# Create and activate the environment manually if needed
python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # macOS / Linux

pip install -e .
```

The Jupyter kernel is registered as:

```text
air-quality-forecasting-environmental-signals-venv
```

and can be selected directly in VS Code or Jupyter.

---

## Getting started

1. Download the **Beijing Multi-Site Air Quality Dataset** from UCI.
2. Place the 12 `PRSA_Data_*.csv` station files in:

```text
data/raw/beijing_multi_site_air_quality/
```

3. Keep raw files immutable.
4. Update `configs/config.yaml` if needed.
5. Start with:

```text
notebooks/00_dataset_assembly.ipynb
```

Do not start with modeling. This project first requires data assembly and temporal audit.

---

## Current project status

```text
Current stage:
project scaffold created / dataset placement pending

Next step:
place the 12 station-level PRSA_Data_*.csv files in data/raw/beijing_multi_site_air_quality/

First notebook:
00_dataset_assembly.ipynb
```

---

## Final project standard

This project succeeds if it shows that PM2.5 forecasting can be designed, validated, benchmarked, diagnosed, and interpreted under realistic temporal constraints, with special attention to whether the model remains useful during high-pollution episodes.

---

*This README should be updated as the project evolves. Final results, selected model, hold-out performance, limitations, and portfolio-ready conclusions should only be written after real execution and review.*

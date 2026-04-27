# Air Quality Forecasting Environmental Signals

> Brief description of the project.

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
Project configuration files. All decisions about dataset, features, splits, and preprocessing live here — nothing is hardcoded in code.

### `data/`
- `raw/` — original input data, immutable
- `interim/` — intermediate transformations
- `processed/` — final datasets ready for modeling

### `models/`
Serialized models, metrics, and experiment artifacts.

### `notebooks/`
Exploratory analysis, reasoning, experimentation, and interpretation.

### `reports/`
Analytical outputs: visualizations, feature engineering rationale, evaluation summaries.

### `src/`
Reusable project logic. Starts empty — code is extracted here only when it appears more than once.

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

The Jupyter kernel is registered as `air-quality-forecasting-environmental-signals-venv` and can be selected directly in VS Code or Jupyter.

---

## Getting started

1. Place the dataset in `data/raw/`
2. Update `configs/config.yaml`
3. Open `notebooks/01_eda.ipynb` and start exploring

---

*This README should be updated as the project evolves — description, dataset, methodology, results.*

# Contributing to the Demand Forecasting Platform

Thanks for considering a contribution. This project is structured to be easy
to extend — adding a 14th model, a new dashboard page, or a new monitoring
check should each take well under an hour for someone familiar with the
codebase.

## Getting set up

```bash
git clone https://github.com/OWNER/REPO.git
cd Comparative-Study-of-ARIMA-LSTM-and-Prophet-for-Demand-Forecasting
python -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt

# Generate data and run the full pipeline once
python -m src.data.generate_datasets
python -m src.data.prepare
python -m experiments.run_benchmark
python -m src.explainability.shap_analysis
python -m src.reporting.generate_charts
python -m src.reporting.generate_diagrams
python -m src.monitoring.drift_detection
python -m src.reporting.generate_reports

pytest tests/ -v
```

## Project layout cheat-sheet

| You want to... | Edit / add to |
|---|---|
| Add a new forecasting model | `src/models/<family>.py` + register in `src/models/registry.py` |
| Change a metric or add a new one | `src/evaluation/metrics.py` |
| Add a dashboard page | `dashboard/pages/N_Your_Page.py` (Streamlit auto-discovers it) |
| Add an API endpoint | `api/routers/<area>.py`, register in `api/main.py` |
| Change synthetic data generation | `src/data/generate_datasets.py` |
| Add a monitoring check | `src/monitoring/drift_detection.py` |
| Update the research write-up | `research/PAPER.md` (keep numbers in sync with `reports/leaderboard.csv`) |

## Adding a new model — step by step

1. Implement a class inheriting from `src.models.base.ForecastModel` with
   `fit(train_df)` and `predict(horizon, future_df=None)`.
2. Add a factory entry to `get_model_registry()` in `src/models/registry.py`.
3. Add it to `MODEL_FAMILY` in the same file.
4. Add a unit test in `tests/test_models.py` (keep it fast — use
   `n_estimators=50`-style lightweight configs for CI).
5. Re-run `python -m experiments.run_benchmark --models=YourModel` (the
   runner supports incremental/append mode — see its `--models` flag) and
   confirm it appears correctly in `reports/leaderboard.csv`.
6. Regenerate charts: `python -m src.reporting.generate_charts`.

## Code style

- Formatting: `black` (line length 120, configured in `pyproject.toml`).
- Linting: `flake8` (same line length, `E203`/`W503` ignored for black compat).
- Type hints encouraged but not strictly enforced project-wide yet.
- Run `black src api tests && flake8 src api tests` before opening a PR.

## Tests

- `pytest tests/ -v` should complete in well under a minute (heavy deep
  models are intentionally excluded from the default test suite — see
  `tests/test_models.py` for the fast-model-only pattern).
- New features should ship with at least one test.

## Commit / PR conventions

- Use clear, imperative commit messages (`Add LightGBM quantile mode`, not
  `fixed stuff`).
- Fill out `.github/PULL_REQUEST_TEMPLATE.md` checklist items honestly.
- Keep PRs focused — one model / one dashboard page / one bugfix per PR is
  easier to review than a grab-bag.

## Reporting bugs / requesting features

Use the issue templates under `.github/ISSUE_TEMPLATE/`. For new model
proposals specifically, use the "New model proposal" template — it asks for
the reference paper, which keeps `research/PAPER.md`'s citations honest.

## Code of Conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md). Be kind.

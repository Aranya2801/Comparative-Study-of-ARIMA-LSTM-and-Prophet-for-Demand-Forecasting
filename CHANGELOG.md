# Changelog

All notable changes to this project are documented in this file.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and versioning follows [Semantic Versioning](https://semver.org/).

## [1.0.0] — Initial Release

### Added
- Three synthetic enterprise-grade datasets: retail demand, e-commerce sales,
  inventory/supply chain (`src/data/generate_datasets.py`).
- 13-model comparative forecasting benchmark: ARIMA, SARIMA, Prophet,
  XGBoost, LightGBM, Random Forest, LSTM, GRU, Bidirectional-LSTM, N-BEATS,
  TFT, Informer, Autoformer (`src/models/`, `experiments/run_benchmark.py`).
- Full evaluation metric suite: RMSE, MAE, MAPE, sMAPE, R², bias, forecast
  stability, seasonal-fit score (`src/evaluation/metrics.py`).
- Optuna-based hyperparameter optimization with rolling-origin CV
  (`experiments/run_optuna_search.py`).
- SHAP-based explainability + business-impact translation
  (`src/explainability/shap_analysis.py`).
- MLflow experiment tracking (SQLite backend) for all 13 model runs.
- Data drift (KS-test + PSI) and anomaly detection (robust z-score) monitoring
  (`src/monitoring/drift_detection.py`).
- Automated PDF/HTML reporting (`src/reporting/generate_reports.py`).
- FastAPI service with `/forecast`, `/train`, `/retrain`, `/models`,
  `/metrics`, `/download-report`, full Swagger docs (`api/`).
- 6-page Streamlit dashboard: Executive, Forecast, Model Comparison, Error
  Analysis, Inventory, Business KPIs (`dashboard/`).
- Docker, docker-compose (API + dashboard + MLflow + nginx), and Kubernetes
  manifests (`deployment/`).
- GitHub Actions: CI (lint+test), scheduled retraining, Docker publish,
  CodeQL security scanning (`.github/workflows/`).
- DVC pipeline definition for full reproducibility (`dvc.yaml`).
- Full research write-up with abstract, literature review, methodology,
  results, and discussion (`research/PAPER.md`).
- 33 unit/integration tests (`tests/`), all passing.

[1.0.0]: https://github.com/OWNER/REPO/releases/tag/v1.0.0

# Project Architecture — Self-Harm Trends Forecasting

This document summarizes the architecture, components, data flow, and deployment considerations for this repository.

## Overview

A Flask web application that ingests/analyzes mental-signals data, runs ML forecasting, and serves dashboards (authority/admin/public) with visualizations (maps/charts).

## Components

- app.py
  - Flask application and route definitions (serves templates and JSON APIs).
- flask_prediction_routes.py
  - Additional routes focused on prediction endpoints (if present).
- Templates (`templates/`)
  - Jinja2 templates for pages (e.g., `base.html`, `authority_dashboard.html`).
- Static (`static/`)
  - CSS, JS, vendor assets, Leaflet, Chart.js, etc.
- Models & data (`selfharm_models/`)
  - CSVs: `predictions_full.csv`, `daily_risk_data.csv`.
  - Model artifacts: `models/mt_model.pth`.
- Prediction scripts & notebooks
  - `predict_new.py`, `predict_unlabeled_data.py`, `manage_db.py`, and `SelfHarm_Forecasting_ML_Model_Training.ipynb`.
- Utility scripts (`scripts/`)
  - Helpers, data fetchers (e.g., `fetch_ght.py`).
- Virtual environment
  - `penv/` contains the project virtualenv for development.

## Data Flow

1. Data ingest/ETL (offline or scheduled) writes to CSVs or DB.
2. Training scripts/notebooks produce model artifacts and `predictions_full.csv`.
3. Backend endpoints (`/api/ght-prediction`, `/api/heatmap-data`) read CSVs and return JSON.
4. Frontend fetches endpoints and renders charts (Chart.js) and maps (Leaflet).

## Key API Endpoints

- `/api/ght-prediction` — Returns aggregated forecast values. Includes `estimated_from_signals` when fallback estimation is used.
- `/api/heatmap-data` — Returns array of `[lat, lng, intensity]` for heatmap rendering.
- `/api/health` — Health check endpoint.

## Authentication & Authorization

- Uses Flask sessions for simple role-based access (`session.user`, `session.role`).
- API routes check session role for `admin` / `authority` where needed.

## Deployment Notes

- Development: `python app.py` (do not use in production).
- Production: run behind a WSGI server (Gunicorn on Linux, Waitress on Windows) and reverse-proxy (Nginx) with TLS.
- Move secrets into environment variables (`SECRET_KEY`, DB URIs).
- Set secure cookies: `SESSION_COOKIE_SECURE=True`, `SESSION_COOKIE_HTTPONLY=True` and use `SameSite` as appropriate.

## Security & Privacy

- The project processes sensitive signals — ensure data is anonymized and access-restricted.
- Add security headers (HSTS, CSP, X-Frame-Options, X-Content-Type-Options).
- Use server-side session storage for sensitive sessions if needed.

## Scaling & Reliability

- Consider moving from CSV files to a database (Postgres, managed DB) for scale and concurrent access.
- Use background workers (Celery/RQ) for long-running prediction tasks.
- Add logging, monitoring, and backups.

## Diagram (Mermaid)

```mermaid
flowchart LR
  subgraph Ingest
    A[Data Sources] --> B(ETL/Scripts)
    B --> D[(CSV / DB)]
  end

  subgraph ML
    D --> E[Training / Notebook]
    E --> F[Model artifact (.pth)]
    E --> G[predictions_full.csv]
  end

  subgraph Backend
    F --> H[Flask app (app.py)]
    G --> H
    H --> I[/api/ght-prediction]
    H --> J[/api/heatmap-data]
  end

  subgraph Frontend
    I --> K[Templates + JS (Leaflet, Chart.js)]
    J --> K
    K --> L[Authority/Admin Dashboards]
  end

  subgraph Infra
    H --> N[WSGI (gunicorn / waitress)]
    N --> O[Nginx / TLS]
    O --> P[Users]
  end
```

## Run & Test (quick)

1. Create virtual environment & install deps:

```bash
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
```

2. Start app (dev):

```bash
python app.py
```

3. Test endpoints:

```bash
curl http://localhost:5000/api/health
curl http://localhost:5000/api/ght-prediction
```

## Next actions (recommended)

- Move CSV storage to a database.
- Add CI checks, unit tests, and linting.
- Harden security headers and session handling.
- Add deployment or Docker configuration if you plan to run in production.

---

For details or a different diagram format (PNG/SVG), tell me where to save it and I will generate it.

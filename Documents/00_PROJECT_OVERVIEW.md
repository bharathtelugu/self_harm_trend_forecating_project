# Forecasting National-Level Self-Harm Trends Using Social Network Signals and Machine Learning

## Project Documentation

**Project Name:** Digital Pulse Monitor - Self-Harm Trend Forecasting System

**Version:** 1.0

**Date:** January 2026

**Author:** Development Team

---

## 📋 Table of Contents

1. [Chapter 1: Project Overview](#chapter-1)
2. [Chapter 2: Requirement Analysis and System Specification](#chapter-2)
3. [Chapter 3: System Design](#chapter-3)
4. [Chapter 4: Implementation, Testing, and Maintenance](#chapter-4)
5. [Chapter 5: Results and Discussions](#chapter-5)
6. [Chapter 6: Conclusion and Future Scope](#chapter-6)

---

## Chapter 1: Project Overview <a name="chapter-1"></a>

### 1.1 Executive Summary

The **Digital Pulse Monitor** is a comprehensive machine learning system designed to forecast national-level self-harm trends through analysis of social network signals. This system combines advanced data processing, real-time sentiment analysis, and ensemble machine learning models to provide insights into mental health crises at a population level.

### 1.2 Project Objectives

1. **Primary Objective:** Develop a predictive system that can accurately forecast self-harm trends 30 days in advance using social network signals
2. **Secondary Objectives:**
   - Provide real-time sentiment analysis of mental health signals
   - Create an intuitive dashboard for authorized stakeholders
   - Implement role-based access control for security
   - Generate actionable insights for mental health authorities

### 1.3 Project Scope

**In Scope:**

- Mental signal sentiment analysis (15-day rolling window)
- 30-day trend forecasting using ARIMA, XGBoost, and Prophet
- Geolocation-based heatmapping across India
- User authentication and role-based access control
- Audit logging and monitoring
- Interactive web-based dashboard

**Out of Scope:**

- Real-time social media data collection (data provided in CSV)
- Direct mental health intervention tools
- Mobile application development
- Integration with external mental health services

### 1.4 Project Stakeholders

| Stakeholder | Role                    | Responsibilities                                    |
| ----------- | ----------------------- | --------------------------------------------------- |
| Admin       | System Administrator    | System maintenance, user management, data oversight |
| Authority   | Public Health Authority | Monitor trends, generate reports, policy decisions  |
| User        | Data Analyst            | View insights, analyze trends, generate reports     |
| End User    | General Public          | Access public mental signals analysis               |

### 1.5 Key Features

#### Mental-Signals Analysis

- 100% stacked bar chart showing sentiment breakdown
- 15-day rolling window analysis
- Trend indicators with percentage changes
- Detailed daily breakdown tables

#### Prediction & Forecasting

- 30-day ensemble forecast with confidence intervals
- 4 independent ML models: ARIMA, XGBoost, Prophet, Ensemble
- Confidence scoring by time horizon
- Detailed forecast comparison tables

#### Admin Dashboard

- Interactive geolocation heatmap (25+ Indian cities)
- Real-time system statistics
- High-risk signal detection
- Export functionality (PDF, CSV, JSON)

#### Security & Access Control

- SQLite-based user authentication
- Three role levels: User, Authority, Admin
- Comprehensive audit logging
- Session management with IP tracking

### 1.6 Technology Stack Overview

| Layer                    | Technology                             |
| ------------------------ | -------------------------------------- |
| **Backend**              | Flask 3.1.2 (Python 3.13)              |
| **Database**             | SQLite 3                               |
| **Frontend**             | HTML5, Bootstrap 5.3.0, Chart.js 4.4.0 |
| **Mapping**              | Leaflet.js + Custom Heatmap            |
| **Data Processing**      | Pandas, NumPy                          |
| **ML Models**            | ARIMA, XGBoost, Prophet, Scikit-learn  |
| **Additional Libraries** | Joblib, PyTorch, Statsmodels           |

### 1.7 Project Structure

```
self_harm_trend_forecasting/
├── app.py                              # Main Flask application (576 lines)
├── database.py                         # Database functions & schema (167 lines)
├── manage_db.py                        # CLI database management
├── predict_new.py                      # ML prediction pipeline
├── predict_unlabeled_data.py           # Batch prediction processing
├── flask_prediction_routes.py          # Flask API endpoints
│
├── templates/                          # HTML templates (8 pages)
│   ├── base.html                       # Base template with navbar
│   ├── home.html                       # Landing page
│   ├── mental_signals.html             # 15-day sentiment analysis
│   ├── prediction_analysis.html        # 30-day forecasting
│   ├── login.html                      # Authentication page
│   ├── about.html                      # Project information
│   ├── admin_dashboard.html            # Admin-only dashboard
│   ├── error.html                      # Error pages
│   └── authority_dashboard.html        # Authority dashboard
│
├── static/                             # Static assets
│   ├── style.css                       # Global styling (2000+ lines)
│   ├── favicon.svg                     # Project logo
│   ├── india_states.geojson            # GeoJSON data
│   ├── in.json                         # India location data
│   └── vendor/                         # Third-party libraries
│
├── selfharm_models/                    # Data & trained models
│   ├── daily_risk_data.csv             # Historical sentiment data
│   ├── forecasts_30days.csv            # 30-day forecast data
│   ├── predictions_full.csv            # Full predictions
│   └── models/
│       ├── mt_model.pth                # Trained neural model
│       ├── label_encoders.pkl          # Label encoders
│       └── tokenizer_name.pkl          # NLP tokenizer
│
├── instance/                           # Runtime data (auto-created)
│   └── digital_pulse.db                # SQLite database
│
├── uploads/                            # User uploads directory
├── scripts/                            # Utility scripts
├── documentation/                      # This documentation folder
├── requirements.txt                    # Python dependencies
├── README.md                           # Quick start guide
└── .gitignore                          # Git exclusions
```

### 1.8 Development Timeline

| Phase                 | Duration | Status         |
| --------------------- | -------- | -------------- |
| Requirements Analysis | 2 weeks  | ✓ Complete     |
| System Design         | 3 weeks  | ✓ Complete     |
| Implementation        | 8 weeks  | ✓ Complete     |
| Testing & QA          | 4 weeks  | ✓ Complete     |
| Deployment            | 1 week   | ✓ Complete     |
| Documentation         | Ongoing  | 🔄 In Progress |

---

**Next Chapter:** [Chapter 2: Requirement Analysis and System Specification](./01_REQUIREMENTS_ANALYSIS.md)

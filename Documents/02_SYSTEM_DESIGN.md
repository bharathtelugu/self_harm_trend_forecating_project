# Chapter 3: System Design

## 3.1 Design Approach

### 3.1.1 Design Paradigm: Object-Oriented + Functional

**Rationale for Hybrid Approach:**

- **Object-Oriented** components: User management, database models, ML pipeline classes
- **Functional** components: Data processing, transformations, utility functions
- **Benefits:** Code reusability, maintainability, and clarity

### 3.1.2 Architectural Pattern: MVC (Model-View-Controller)

```
┌─────────────────────────────────────────────────┐
│                  Web Browser                      │
└────────────────────┬────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────┐
│              View Layer (Frontend)                │
│  ┌────────────────────────────────────────────┐ │
│  │ HTML Templates (Jinja2)                    │ │
│  │ - home.html                                │ │
│  │ - mental_signals.html                      │ │
│  │ - prediction_analysis.html                 │ │
│  │ - admin_dashboard.html                     │ │
│  └────────────────────────────────────────────┘ │
└────────────────────┬────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────┐
│          Controller Layer (Flask Routes)         │
│  ┌────────────────────────────────────────────┐ │
│  │ @app.route('/') - home()                   │ │
│  │ @app.route('/login') - login()             │ │
│  │ @app.route('/mental_signals') - signals()  │ │
│  │ @app.route('/prediction') - forecast()     │ │
│  │ @app.route('/api/predict') - api_predict() │ │
│  └────────────────────────────────────────────┘ │
└────────────────────┬────────────────────────────┘
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
    ┌────────┐  ┌────────┐  ┌──────────┐
    │ Models │  │  Data  │  │    ML    │
    │ Layer  │  │Proc.   │  │ Pipeline │
    │        │  │        │  │          │
    └────────┘  └────────┘  └──────────┘
        │            │            │
        └────────────┼────────────┘
                     │
                     ▼
    ┌─────────────────────────────────┐
    │    Database Layer (SQLite)       │
    │  - Users Table                   │
    │  - Audit Log Table               │
    │  - Sessions Table                │
    └─────────────────────────────────┘
```

### 3.1.3 Design Principles

| Principle                       | Implementation                                       |
| ------------------------------- | ---------------------------------------------------- |
| **Single Responsibility**       | Each module has one clear purpose                    |
| **DRY (Don't Repeat Yourself)** | Shared functions in database.py and utility modules  |
| **Separation of Concerns**      | View, Controller, and Model layers separated         |
| **Dependency Injection**        | Functions accept parameters rather than global state |
| **Interface Segregation**       | Minimal API for each component                       |

---

## 3.2 Detailed Design

### 3.2.1 System Components Architecture

```
Digital Pulse Monitor System
│
├── Authentication Module
│   ├── User Login Service
│   ├── Role Verification
│   ├── Session Management
│   └── Audit Logging
│
├── Data Processing Module
│   ├── Sentiment Analysis
│   ├── Time Series Analysis
│   ├── Data Validation
│   └── Data Aggregation
│
├── ML Prediction Module
│   ├── ARIMA Model
│   ├── XGBoost Model
│   ├── Prophet Model
│   ├── Ensemble Model
│   └── Confidence Scoring
│
├── Visualization Module
│   ├── Chart Generation (Chart.js)
│   ├── Geolocation Mapping (Leaflet.js)
│   ├── Heatmap Generation
│   └── Report Generation
│
├── API Module
│   ├── Prediction Endpoints
│   ├── Sentiment Data Endpoints
│   ├── Forecast Endpoints
│   └── Admin Endpoints
│
└── Infrastructure Module
    ├── Database Management
    ├── File Storage
    ├── Logging & Monitoring
    └── Configuration Management
```

### 3.2.2 Module Specifications

#### Module 1: Authentication Module

**File:** `app.py` (Routes section), `database.py`

**Key Functions:**

```python
def verify_user(username, password) → bool
def get_user(username) → dict
def create_user(username, password, role, email) → bool
def log_audit(user_id, action, details) → None
def update_user_password(username, new_password) → None
```

**Responsibilities:**

- User credential verification
- Session management
- Role-based access control
- Audit trail maintenance

#### Module 2: Data Processing Module

**File:** `app.py` (Helper functions section)

**Key Functions:**

```python
def load_sentiment_data() → List[dict]
def generate_mock_sentiment_data() → List[dict]
def load_forecast_data() → List[dict]
def generate_mock_forecasts() → List[dict]
```

**Responsibilities:**

- Load data from CSV files
- Data transformation and normalization
- Mock data generation for fallback
- Data validation

#### Module 3: ML Prediction Module

**File:** `predict_new.py`, `predict_unlabeled_data.py`, `flask_prediction_routes.py`

**Key Classes/Functions:**

```python
def predict_new_tweet(tweet_text) → dict
def load_model_and_encoders() → (model, encoders)
def predict_on_unlabeled_data(texts, model, device) → predictions
def decode_predictions(predictions, encoders) → DataFrame
```

**Responsibilities:**

- Text tokenization and encoding
- Multi-label prediction (MS, ME, RL)
- Random Forest predictions
- Confidence scoring
- Batch processing

#### Module 4: Web Interface Module

**File:** `templates/*.html`, `static/style.css`

**Pages:**

- `home.html` - Landing page
- `login.html` - Authentication
- `mental_signals.html` - Sentiment dashboard
- `prediction_analysis.html` - Forecast dashboard
- `admin_dashboard.html` - Admin controls
- `authority_dashboard.html` - Authority controls
- `about.html` - Project info
- `error.html` - Error handling
- `base.html` - Navigation template

#### Module 5: Database Module

**File:** `database.py`

**Tables:**

```sql
users - User accounts and roles
audit_log - Action history
sessions - Active/historical sessions
```

**Key Operations:**

- CRUD operations for users
- Audit logging
- Session tracking

#### Module 6: API Module

**File:** `flask_prediction_routes.py`, `app.py` (API routes section)

**Endpoints:**

```
POST /api/predict - Single text prediction
POST /api/predict-batch - Multiple text prediction
GET /api/sentiment - 15-day sentiment data
GET /api/forecast - 30-day forecast data
```

---

## 3.3 System Design Using Structured Analysis and Design Tools

### 3.3.1 Data Flow Diagrams (DFD)

#### DFD Level 0: Context Diagram

```
                      ┌─────────────────────┐
                      │   End Users         │
                      │  (Browser Access)   │
                      └──────────┬──────────┘
                                 │
                        ┌────────┴────────┐
                        │                 │
                        ▼                 ▼
                  ┌──────────────┐  ┌──────────────┐
                  │  Manage      │  │  Access      │
                  │  Dashboard   │  │  System      │
                  │  & Reports   │  │              │
                  └──────────────┘  └──────────────┘
                        │                 │
                        └────────┬────────┘
                                 │
                   ┌─────────────┴──────────────┐
                   │                            │
                   ▼                            ▼
        ┌────────────────────────┐  ┌───────────────────┐
        │ Digital Pulse Monitor   │  │  Data Sources     │
        │     System              │  │  - CSV Files      │
        │  (Flask Application)    │  │  - User Input     │
        └────────────────────────┘  │  - Historical Data│
                                     └───────────────────┘
```

#### DFD Level 1: Main System Processes

```
Data Sources          Process Layer              Storage            Output
─────────────────────────────────────────────────────────────────────────────

CSV Files ──────┐
                │
User Input ─────┤──┬──────────────────────┐
                │  │ P1: Data Ingestion   │
Historical Data─┤  │ & Validation         │─────┬──────────► Database
                │  └──────────────────────┘     │
                │         │                      │
                └─────────┼──────────────────────┘
                          │
                          ▼
                  ┌──────────────────────┐
                  │ P2: Sentiment        │
                  │ Analysis             │──────────────► Sentiment Data
                  │ (15-day rolling)     │
                  └──────────────────────┘
                          │
                          ▼
                  ┌──────────────────────┐
                  │ P3: ML Predictions   │
                  │ - ARIMA              │
                  │ - XGBoost            │──────────────► Forecast Data
                  │ - Prophet            │
                  │ - Ensemble           │
                  └──────────────────────┘
                          │
                          ▼
                  ┌──────────────────────┐
                  │ P4: Visualization    │
                  │ - Charts             │
                  │ - Heatmaps           │──────────────► Web Frontend
                  │ - Reports            │
                  └──────────────────────┘
```

#### DFD Level 2: Authentication and Data Processing

```
AUTHENTICATION PROCESS (P1.1)
──────────────────────────────────────

User Credentials ──┐
                   ├──► Verify ──────┐
                   │                  ▼
User Database ─────┘          Access Allowed? ──YES──► Create Session
                                      │
                                      NO
                                      │
                                      ▼
                              Return Error Message


DATA PROCESSING FLOW (P2.1)
──────────────────────────────────────

CSV Data ───────┐
                ├──► Validate ──────┐
Missing Data ───┤     Data          ▼
                │                 Quality Check ──PASS──► Store in DB
                │                      │
System Config ──┘                       FAIL
                                        │
                                        ▼
                                  Use Mock Data


PREDICTION FLOW (P3.1)
──────────────────────────────────────

Historical Data ──┐
                  ├──► Load Models ──────┐
                  │                      ▼
User Tweet ───────┤              Tokenize & Encode
                  │                      │
Model Files ──────┘                      ▼
                              Run Predictions (4 models)
                                        │
                        ┌───────────────┼───────────────┐
                        ▼               ▼               ▼
                      ARIMA          XGBoost         Prophet
                        │               │               │
                        └───────────────┼───────────────┘
                                        │
                                        ▼
                              Ensemble Combination
                                        │
                                        ▼
                              Confidence Scoring
                                        │
                                        ▼
                              Return Predictions
```

### 3.3.2 Data Dictionary

#### D1: Users Table

| Field      | Type      | Size | Key    | Description                      |
| ---------- | --------- | ---- | ------ | -------------------------------- |
| id         | INTEGER   | -    | PK     | Auto-increment user ID           |
| username   | TEXT      | 255  | UNIQUE | Login username                   |
| password   | TEXT      | 255  | -      | Hashed password (bcrypt)         |
| role       | TEXT      | 50   | -      | User role (admin/authority/user) |
| email      | TEXT      | 255  | -      | User email address               |
| created_at | TIMESTAMP | -    | -      | Account creation timestamp       |
| updated_at | TIMESTAMP | -    | -      | Last update timestamp            |
| is_active  | BOOLEAN   | -    | -      | Account active status (1=active) |

#### D2: Audit Log Table

| Field     | Type      | Size | Key | Description                          |
| --------- | --------- | ---- | --- | ------------------------------------ |
| id        | INTEGER   | -    | PK  | Auto-increment log ID                |
| user_id   | INTEGER   | -    | FK  | Reference to users table             |
| action    | TEXT      | 100  | -   | Action performed (LOGIN/LOGOUT/VIEW) |
| timestamp | TIMESTAMP | -    | -   | Action timestamp                     |
| details   | TEXT      | 1000 | -   | Additional action details            |

#### D3: Sessions Table

| Field         | Type      | Size | Key    | Description                    |
| ------------- | --------- | ---- | ------ | ------------------------------ |
| id            | INTEGER   | -    | PK     | Auto-increment session ID      |
| user_id       | INTEGER   | -    | FK     | Reference to users table       |
| session_token | TEXT      | 255  | UNIQUE | Unique session identifier      |
| ip_address    | TEXT      | 45   | -      | User IP address (IPv6 support) |
| user_agent    | TEXT      | 500  | -      | Browser user agent string      |
| login_time    | TIMESTAMP | -    | -      | Session start time             |
| last_activity | TIMESTAMP | -    | -      | Last activity timestamp        |
| logout_time   | TIMESTAMP | -    | -      | Session end time               |

#### D4: Sentiment Data

| Field     | Type    | Description               |
| --------- | ------- | ------------------------- |
| date      | DATE    | Data date (YYYY-MM-DD)    |
| positive  | INTEGER | Positive sentiment count  |
| negative  | INTEGER | Negative sentiment count  |
| neutral   | INTEGER | Neutral sentiment count   |
| ambiguous | INTEGER | Ambiguous sentiment count |

#### D5: Forecast Data

| Field             | Type  | Description                       |
| ----------------- | ----- | --------------------------------- |
| date              | DATE  | Forecast date                     |
| ensemble_forecast | FLOAT | Combined model prediction         |
| arima_forecast    | FLOAT | ARIMA model prediction            |
| arima_lower_ci    | FLOAT | ARIMA lower confidence interval   |
| arima_upper_ci    | FLOAT | ARIMA upper confidence interval   |
| xgboost_forecast  | FLOAT | XGBoost model prediction          |
| prophet_forecast  | FLOAT | Prophet model prediction          |
| prophet_lower_ci  | FLOAT | Prophet lower confidence interval |
| prophet_upper_ci  | FLOAT | Prophet upper confidence interval |

---

### 3.3.3 Structured Charts

#### Control Flow Chart: Login Process

```
                        START
                          │
                          ▼
                  ┌───────────────┐
                  │ Display Login  │
                  │ Form           │
                  └───────┬────────┘
                          │
                          ▼
                  ┌───────────────┐
              ┌──│ User Submits   │
              │  │ Credentials?   │
              │  └───────┬────────┘
              │          │ No
              │          └──────► END (Idle)
              │
              │ Yes
              ▼
        ┌──────────────────┐
        │ Validate Input   │
        │ Format           │
        └─────┬────────────┘
              │
              ▼
        ┌──────────────────┐      No
    ┌──│ Input Valid?     │──────────► Show Error
    │  └─────┬────────────┘            Message
    │        │ Yes
    │        ▼
    │  ┌──────────────────┐
    │  │ Query Database   │
    │  │ verify_user()    │
    │  └─────┬────────────┘
    │        │
    │        ▼
    │  ┌──────────────────┐      No
    └─│ Credentials      │──────────► Show Error
       │ Match?           │            "Invalid"
       └─────┬────────────┘
             │ Yes
             ▼
      ┌──────────────────┐
      │ Create Session   │
      │ Set Cookies      │
      └─────┬────────────┘
            │
            ▼
      ┌──────────────────┐
      │ Log Audit Entry  │
      │ log_audit()      │
      └─────┬────────────┘
            │
            ▼
      ┌──────────────────┐
      │ Redirect to      │
      │ Dashboard        │
      └─────┬────────────┘
            │
            ▼
          END (Success)
```

#### Control Flow Chart: Sentiment Analysis Pipeline

```
                    START
                      │
                      ▼
        ┌─────────────────────────┐
        │ load_sentiment_data()   │
        └────────┬────────────────┘
                 │
                 ▼
        ┌─────────────────────────┐      Exception
    ┌──│ Try to Load CSV File    │──────────┐
    │  │ 'daily_risk_data.csv'   │          │
    │  └────────┬────────────────┘          │
    │           │ Success                   │
    │           ▼                           ▼
    │    ┌──────────────┐        ┌───────────────────┐
    │    │ Parse CSV    │        │ Generate Mock     │
    │    │ Convert Date │        │ Sentiment Data    │
    │    └──────┬───────┘        └────────┬──────────┘
    │           │                         │
    │           ▼                         │
    │    ┌──────────────┐                │
    │    │ Sort by Date │                │
    │    │ Get Last 15  │                │
    │    └──────┬───────┘                │
    │           │                         │
    │           ▼                         │
    │    ┌──────────────┐                │
    │    │ Calculate    │                │
    │    │ Percentages  │                │
    │    └──────┬───────┘                │
    │           │                         │
    └───────────┼─────────────────────────┘
                │
                ▼
        ┌──────────────────┐
        │ Return Sentiment │
        │ Data (15 days)   │
        └────────┬─────────┘
                 │
                 ▼
              END
```

---

### 3.3.4 Flowcharts

#### Process Flowchart: ML Prediction Pipeline

```
                        INPUT: Tweet Text
                              │
                              ▼
                    ┌──────────────────────┐
                    │ Tokenize Text        │
                    │ (AutoTokenizer)      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Encode Tokens        │
                    │ Max Length: 512      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Load MT Model        │
                    │ (mt_model.pth)       │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Forward Pass         │
                    │ Get Output Logits    │
                    └──────────┬───────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
        ▼                      ▼                      ▼
    ┌────────────┐         ┌────────────┐        ┌─────────────┐
    │ Apply      │         │ Apply      │        │ Apply       │
    │ Softmax    │         │ Softmax    │        │ Softmax     │
    │ to MS      │         │ to ME      │        │ to RL       │
    │ Output     │         │ Output     │        │ Output      │
    └────┬───────┘         └────┬───────┘        └────┬────────┘
         │                      │                     │
         ▼                      ▼                     ▼
    ┌────────────┐         ┌────────────┐        ┌─────────────┐
    │ MS-Neg     │         │ ME-Sadness │        │ M-NST       │
    │ Probability│         │ Probability│        │ Probability │
    └────┬───────┘         └────┬───────┘        └────┬────────┘
         │                      │                     │
         └──────────────────────┼─────────────────────┘
                                │
                                ▼
                    ┌──────────────────────┐
                    │ Create Feature Vector│
                    │ (13 features from    │
                    │  NLP outputs)        │
                    └──────────┬───────────┘
                               │
                    ┌──────────┴──────────┐
                    │                     │
                    ▼                     ▼
            ┌────────────────┐    ┌──────────────┐
            │ RF Models      │    │ Heuristic    │
            │ Available?     │    │ Fallback     │
            └─────┬──────────┘    └──────────────┘
                  │                     │
          Yes     │ No                   │
         ┌────────┴────────┐            │
         │                 │            │
         ▼                 └────────────┼─────────┐
    ┌──────────────┐                   │         │
    │ Scale        │                   │         │
    │ Feature      │                   │         │
    │ Vector       │                   │         │
    └──────┬───────┘                   │         │
           │                           │         │
           ▼                           │         │
    ┌──────────────┐                   │         │
    │ Load RF      │                   │         │
    │ Models       │                   │         │
    └──────┬───────┘                   │         │
           │                           │         │
           ▼                           ▼         ▼
    ┌──────────────┐           ┌─────────────────────┐
    │ Predict      │           │ Use Heuristic       │
    │ Death Count  │           │ Calculation         │
    │ Predict      │           │ death_pred =        │
    │ Injury Count │           │ 100 + (ms_neg*300)  │
    └──────┬───────┘           │ injury_pred =       │
           │                   │ 200 + (ms_neg*400)  │
           └───────────┬───────┘                     │
                       │◄────────────────────────────┘
                       │
                       ▼
            ┌──────────────────────┐
            │ Calculate Risk Level │
            │ HIGH: >200           │
            │ MEDIUM: >150         │
            │ LOW: ≤150            │
            └──────────┬───────────┘
                       │
                       ▼
            ┌──────────────────────┐
            │ Return Prediction:   │
            │ {tweet, ms_neg,      │
            │  me_sad, m_nst,      │
            │  gh_death_forecast,  │
            │  gh_injury_forecast, │
            │  risk_level}         │
            └──────────┬───────────┘
                       │
                       ▼
                OUTPUT: Prediction
```

---

## 3.4 UML Diagrams

### 3.4.1 Use Case Diagram

```
                            ┌─────────────────────┐
                            │   Digital Pulse      │
                            │   Monitor System     │
                            └─────────────────────┘
                                      │
                ┌─────────────────────┼─────────────────────┐
                │                     │                     │
               ╱│╲                    │                    ╱│╲
           Admin│ ├─── View Sentiment │ ───────┤ User    │ │
               │ │      Analysis      │        │          │ │
               ╲│╱                    │        ╱│╲         │ │
                │              ┌──────┴───┐   │ │         │ │
                │              │          │   │ │         │ │
                │         ┌────┼──────────┼───┼─┼─────────┘ │
                │         │    │          │   │ │           │
              Use Cases:   │    │          │   │ │          ╱│╲
                │         │    │          │   │ │       Auth.│ │
    ┌───────────┴──────────┴────┴──────────┴───┴─┘           │ │
    │                                                        ╲│╱
    ├─ UC1: Authenticate User
    │   └─ Login with username/password
    │   └─ Validate credentials
    │   └─ Create session
    │
    ├─ UC2: View Mental Signals (Public)
    │   └─ Display 15-day sentiment data
    │   └─ Show stacked bar chart
    │   └─ Display trend indicators
    │
    ├─ UC3: View Predictions (Auth Required)
    │   └─ Display 30-day forecast
    │   └─ Compare model predictions
    │   └─ Show confidence intervals
    │
    ├─ UC4: View Geolocation Heatmap (Auth Required)
    │   └─ Display India map
    │   └─ Color-code risk levels
    │   └─ Show location statistics
    │
    ├─ UC5: Generate Reports (Authority/Admin)
    │   └─ Export data (PDF/CSV/JSON)
    │   └─ Schedule reports
    │   └─ Save report history
    │
    ├─ UC6: Manage Users (Admin Only)
    │   └─ Create new users
    │   └─ Update user roles
    │   └─ Deactivate accounts
    │   └─ Reset passwords
    │
    ├─ UC7: View Audit Logs (Admin Only)
    │   └─ Display user actions
    │   └─ Filter by user/date
    │   └─ Export audit trail
    │
    ├─ UC8: Monitor System Health (Admin Only)
    │   └─ View database statistics
    │   └─ Check model performance
    │   └─ Monitor system resources
    │
    └─ UC9: Logout
        └─ End user session
        └─ Log audit event
```

### 3.4.2 Class Diagram

```
┌─────────────────────────────┐
│         User                │
├─────────────────────────────┤
│ - id: int                   │
│ - username: str             │
│ - password: str (hashed)    │
│ - role: str                 │
│ - email: str                │
│ - created_at: datetime      │
│ - is_active: bool           │
├─────────────────────────────┤
│ + verify_credentials()      │
│ + get_role()                │
│ + update_password()         │
│ + deactivate()              │
└──────────────┬──────────────┘
               │ creates
               │
               ▼
┌─────────────────────────────┐
│    AuditLog                 │
├─────────────────────────────┤
│ - id: int                   │
│ - user_id: int (FK)         │
│ - action: str               │
│ - timestamp: datetime       │
│ - details: str              │
├─────────────────────────────┤
│ + log_action()              │
│ + get_user_logs()           │
│ + get_action_logs()         │
└─────────────────────────────┘

┌─────────────────────────────┐
│    Session                  │
├─────────────────────────────┤
│ - id: int                   │
│ - user_id: int (FK)         │
│ - token: str                │
│ - ip_address: str           │
│ - user_agent: str           │
│ - login_time: datetime      │
│ - logout_time: datetime     │
├─────────────────────────────┤
│ + create_session()          │
│ + validate_session()        │
│ + close_session()           │
│ + extend_session()          │
└─────────────────────────────┘

┌────────────────────────────────────┐
│    SentimentAnalyzer               │
├────────────────────────────────────┤
│ - data_path: str                   │
│ - window_size: int = 15            │
├────────────────────────────────────┤
│ + load_data()                      │
│ + calculate_percentages()          │
│ + get_trend()                      │
│ + generate_chart_data()            │
│ + export_data()                    │
└────────────────────────────────────┘

┌────────────────────────────────────┐
│    MLPredictor                     │
├────────────────────────────────────┤
│ - models: dict                     │
│ - encoders: dict                   │
│ - scaler: StandardScaler           │
├────────────────────────────────────┤
│ + load_models()                    │
│ + predict_arima()                  │
│ + predict_xgboost()                │
│ + predict_prophet()                │
│ + predict_ensemble()               │
│ + calculate_confidence()           │
└────────────────────────────────────┘

┌────────────────────────────────────┐
│    GeolocationMapper               │
├────────────────────────────────────┤
│ - geojson_path: str                │
│ - city_data: dict                  │
├────────────────────────────────────┤
│ + load_geojson()                   │
│ + generate_heatmap()               │
│ + get_risk_levels()                │
│ + get_location_stats()             │
└────────────────────────────────────┘

┌────────────────────────────────────┐
│    ReportGenerator                 │
├────────────────────────────────────┤
│ - sentiment_data: DataFrame        │
│ - forecast_data: DataFrame         │
├────────────────────────────────────┤
│ + generate_pdf()                   │
│ + generate_csv()                   │
│ + generate_json()                  │
│ + schedule_report()                │
└────────────────────────────────────┘
```

### 3.4.3 Sequence Diagram: User Login Flow

```
User         Browser        Flask App      Database     Session Mgr
│             │               │              │              │
│─login───────>│               │              │              │
│              │               │              │              │
│              │─POST login───>│              │              │
│              │               │              │              │
│              │               │─validate───>│              │
│              │               │  input      │              │
│              │               │<─valid──────│              │
│              │               │              │              │
│              │               │─query user─>│              │
│              │               │             │              │
│              │               │<─user data──│              │
│              │               │             │              │
│              │               │─compare─────┐              │
│              │               │  password   │              │
│              │               │<────OK──────┘              │
│              │               │                            │
│              │               │──create session──────────>│
│              │               │                            │
│              │               │<────session token─────────│
│              │               │                            │
│              │               │─log audit entry──────────>│
│              │               │ (LOGIN action)            │
│              │               │                            │
│              │<─redirect────│                            │
│              │              │                            │
│              │─GET ─────────>│                            │
│              │ dashboard    │                            │
│              │              │─validate───────────────────>
│              │              │  session token             │
│              │              │<────valid─────────────────
│              │              │                            │
│              │<─dashboard──│                            │
│              │  HTML       │                            │
│<─render─────│              │                            │
│  page       │              │                            │
│             │              │                            │
```

### 3.4.4 Activity Diagram: Forecast Generation

```
                                START
                                  │
                                  ▼
                        ┌──────────────────┐
                        │ Load Historical  │
                        │ Sentiment Data   │
                        └────────┬─────────┘
                                 │
                                 ▼
                        ┌──────────────────┐
                        │ Validate Data    │
                        │ Quality          │
                        └────────┬─────────┘
                                 │
                                 ▼
                        ┌──────────────────┐
                        │ Load Pre-trained │
                        │ Models           │
                        └────────┬─────────┘
                                 │
        ┌────────────────────────┼────────────────────────┐
        │                        │                        │
        ▼                        ▼                        ▼
    ┌───────────┐        ┌──────────────┐      ┌──────────────┐
    │ ARIMA     │        │ XGBoost      │      │ Prophet      │
    │ Model     │        │ Model        │      │ Model        │
    └─────┬─────┘        └──────┬───────┘      └──────┬───────┘
          │                     │                     │
    [Generate  15-day         [Generate    [Generate
     trend &    forecast]      forecast]    forecast]
          │                     │                     │
          └────────────────────┬─────────────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Combine Predictions  │
                    │ (Ensemble Voting)    │
                    └────────┬─────────────┘
                             │
                             ▼
                    ┌──────────────────────┐
                    │ Calculate Confidence │
                    │ Intervals            │
                    └────────┬─────────────┘
                             │
                             ▼
                    ┌──────────────────────┐
                    │ Assign Risk Levels   │
                    │ - High/Med/Low       │
                    └────────┬─────────────┘
                             │
                             ▼
                    ┌──────────────────────┐
                    │ Store Forecast in    │
                    │ Database             │
                    └────────┬─────────────┘
                             │
                             ▼
                    ┌──────────────────────┐
                    │ Log Audit Event      │
                    │ (FORECAST_GENERATED) │
                    └────────┬─────────────┘
                             │
                             ▼
                    ┌──────────────────────┐
                    │ Return Results to    │
                    │ Frontend             │
                    └────────┬─────────────┘
                             │
                             ▼
                           END
```

---

## 3.5 User Interface Design

### 3.5.1 UI Design Principles

**Consistency:** Use Bootstrap's grid system consistently across all pages
**Accessibility:** WCAG 2.1 AA compliance with keyboard navigation
**Responsiveness:** Mobile-first design adapting to all screen sizes
**Clarity:** Intuitive navigation, clear call-to-action buttons

### 3.5.2 Color Palette

```
Primary: #2E86AB (Blue) - Main actions, headers
Secondary: #A23B72 (Purple) - Secondary actions
Success: #06A77D (Green) - Positive indicators
Warning: #F18F01 (Orange) - Caution indicators
Danger: #D62828 (Red) - Error/high-risk indicators
Neutral: #CCCCCC (Gray) - Inactive, disabled
Background: #FAFAFA (Off-white) - Clean backdrop
Text: #333333 (Dark gray) - Primary text
```

### 3.5.3 Page Wireframes

#### Page 1: Home Page (Public)

```
┌────────────────────────────────────────────────┐
│ Logo | Nav (Home, Signals, About) | Login Btn │
├────────────────────────────────────────────────┤
│                                                  │
│   Digital Pulse Monitor                        │
│   Forecasting Self-Harm Trends                │
│                                                  │
│   [Hero Image/Banner]                         │
│                                                  │
│   ┌─────────────────────────────────────────┐ │
│   │ View Sentiment Analysis    [→]           │ │
│   │ Real-time mental health signals          │ │
│   └─────────────────────────────────────────┘ │
│                                                  │
│   ┌─────────────────────────────────────────┐ │
│   │ About This Project          [→]           │ │
│   │ Understand the forecasting system        │ │
│   └─────────────────────────────────────────┘ │
│                                                  │
├────────────────────────────────────────────────┤
│ Footer: Copyright | Links | Contact            │
└────────────────────────────────────────────────┘
```

#### Page 2: Mental Signals (15-day Analysis)

```
┌────────────────────────────────────────────────┐
│ Logo | Nav | User Menu (if logged in)          │
├────────────────────────────────────────────────┤
│                                                  │
│ MENTAL SIGNALS ANALYSIS                       │
│ 15-Day Sentiment Breakdown                    │
│                                                  │
│ ┌────────────────────────────────────────────┐│
│ │ [100% Stacked Bar Chart]                   ││
│ │ Positive | Negative | Neutral | Ambiguous ││
│ │ (Interactive, legend clickable)            ││
│ └────────────────────────────────────────────┘│
│                                                  │
│ Trend Indicators:                             │
│ ┌──────────────┬──────────────┬──────────────┐ │
│ │ Positive: ↑  │ Negative: ↓  │ Neutral: →  │ │
│ │ +5.2%        │ -2.1%        │ +0.8%       │ │
│ └──────────────┴──────────────┴──────────────┘ │
│                                                  │
│ Daily Breakdown:                              │
│ ┌────────────────────────────────────────────┐ │
│ │ Date     | Pos | Neg | Neut | Amb | Total │ │
│ │ 2026-01-18│ 28  │ 35  │ 32   │ 5   │ 100  │ │
│ │ 2026-01-17│ 26  │ 37  │ 31   │ 6   │ 100  │ │
│ │ ...                                         │ │
│ └────────────────────────────────────────────┘ │
│                                                  │
│ [Export as CSV] [Print]                       │
│                                                  │
├────────────────────────────────────────────────┤
│ Footer                                          │
└────────────────────────────────────────────────┘
```

#### Page 3: Prediction Analysis (30-day Forecast)

```
┌────────────────────────────────────────────────┐
│ Logo | Nav | User Menu                         │
├────────────────────────────────────────────────┤
│                                                  │
│ PREDICTION & FORECASTING                     │
│ 30-Day Ensemble Forecast                     │
│                                                  │
│ ┌────────────────────────────────────────────┐ │
│ │ [Line Chart with Confidence Intervals]    │ │
│ │ Models: ─Ensemble ─ARIMA ─XGBoost─Prophet│ │
│ │ (Interactive legend)                       │ │
│ └────────────────────────────────────────────┘ │
│                                                  │
│ Forecast Table:                               │
│ ┌──────────┬────────┬──────┬──────┬──────────┐│
│ │ Date     │Ensemble│ARIMA │XGBoost│Prophet  ││
│ │ 2026-01-19│ 0.054 │0.052 │ 0.055│ 0.053   ││
│ │ 2026-01-20│ 0.056 │0.054 │ 0.057│ 0.055   ││
│ │ ...                                        ││
│ └──────────┴────────┴──────┴──────┴──────────┘│
│                                                  │
│ Confidence Scoring:                           │
│ 7-day:  ██████░░░░ 85%                        │
│ 14-day: ███████░░░ 72%                        │
│ 30-day: █████░░░░░ 58%                        │
│                                                  │
│ [Export as CSV] [Generate Report]            │
│                                                  │
├────────────────────────────────────────────────┤
│ Footer                                          │
└────────────────────────────────────────────────┘
```

#### Page 4: Admin Dashboard

```
┌────────────────────────────────────────────────┐
│ Logo | Nav | Admin Menu                        │
├────────────────────────────────────────────────┤
│ ┌─────┐                                        │
│ │Menu │  ADMIN DASHBOARD                     │
│ ├─────┤                                        │
│ │Home │  ┌──────────────────────────────────┐ │
│ │Users│  │ System Statistics               │ │
│ │Audit│  │ ┌──────────────────────────────┐ │ │
│ │Logs │  │ │Indexed Posts: 124,500        │ │ │
│ │Rep. │  │ │Active Users: 42              │ │ │
│ │     │  │ │Last Update: 2 min ago        │ │ │
│ └─────┘  │ └──────────────────────────────┘ │ │
│          │                                    │ │
│          │ User Management                   │ │
│          │ ┌──────────────────────────────┐ │ │
│          │ │[Create New User] [Manage]    │ │ │
│          │ │Name │ Role │ Email │ Active │ │ │
│          │ │admin│admin │admin@ │   ✓    │ │ │
│          │ │user │user  │user@  │   ✓    │ │ │
│          │ └──────────────────────────────┘ │ │
│          │                                    │ │
│          │ Recent Audit Log                  │ │
│          │ ┌──────────────────────────────┐ │ │
│          │ │2026-01-18 14:30 │ admin │   │ │ │
│          │ │LOGIN            │ 127.0.0.1 │ │ │
│          │ │2026-01-18 10:15 │ user  │   │ │ │
│          │ │LOGOUT           │ 192.x.x.x │ │ │
│          │ └──────────────────────────────┘ │ │
│          └──────────────────────────────────┘ │
├────────────────────────────────────────────────┤
│ Footer                                          │
└────────────────────────────────────────────────┘
```

---

## 3.6 Database Design

### 3.6.1 ER Diagram

```
┌────────────────────────┐
│        users           │
├────────────────────────┤
│ PK  id                 │ 1
│     username           │───┐
│     password           │   │
│     role               │   │ has many
│     email              │   │
│     created_at         │   │
│     is_active          │   │
└────────────────────────┘   │
           △                 │
           │ 1               │
           │                 │
        owns                 ▼
           │              ┌──────────────────────┐
           │              │   audit_log          │
           │              ├──────────────────────┤
           │              │ PK  id               │
           │              │ FK  user_id          │
           │              │     action           │
           │              │     timestamp        │
           │              │     details          │
           │              └──────────────────────┘
           │
           │
        creates
           │
           └─────────┬────────────────┐
                     │                │
                     ▼                ▼
            ┌──────────────────┐  ┌─────────────────┐
            │   sessions       │  │  sentiment_data │
            ├──────────────────┤  ├─────────────────┤
            │ PK  id           │  │    date         │
            │ FK  user_id      │  │    positive     │
            │     session_token│  │    negative     │
            │     ip_address   │  │    neutral      │
            │     login_time   │  │    ambiguous    │
            │     logout_time  │  │    created_at   │
            └──────────────────┘  └─────────────────┘
                     │
                     │ 1..N
                     │ session
                     │
                     ▼
            ┌──────────────────┐
            │  forecast_data   │
            ├──────────────────┤
            │    date          │
            │    ensemble      │
            │    arima         │
            │    xgboost       │
            │    prophet       │
            │    confidence    │
            │    created_at    │
            └──────────────────┘
```

### 3.6.2 Normalization

**Normalization Level: 3NF (Third Normal Form)**

#### Table 1: users

**1NF Compliance:** No repeating groups ✓
**2NF Compliance:** All attributes depend on PK (id) ✓
**3NF Compliance:** No transitive dependencies ✓

```sql
users {
  id (PK)
  username (UNIQUE)
  password
  role ← refers to valid role values only
  email
  created_at
  is_active
}
```

#### Table 2: audit_log

**1NF Compliance:** No repeating groups ✓
**2NF Compliance:** All attributes depend on PK (id) ✓
**3NF Compliance:** No transitive dependencies ✓

```sql
audit_log {
  id (PK)
  user_id (FK) → users.id
  action
  timestamp
  details
}
```

#### Table 3: sessions

**1NF Compliance:** No repeating groups ✓
**2NF Compliance:** All attributes depend on PK (id) ✓
**3NF Compliance:** No transitive dependencies ✓

```sql
sessions {
  id (PK)
  user_id (FK) → users.id
  session_token (UNIQUE)
  ip_address
  user_agent
  login_time
  last_activity
  logout_time
}
```

---

### 3.6.3 Database Tables

**Database Name:** `digital_pulse.db` (SQLite)
**File Location:** `instance/digital_pulse.db`

#### Table: users

```sql
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,
    role TEXT NOT NULL DEFAULT 'user',
    email TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT 1
);

INDEXES:
  - PRIMARY KEY (id)
  - UNIQUE (username)
  - INDEX (role) for role-based queries
  - INDEX (is_active) for active user queries
```

**Sample Data:**
| id | username | password | role | email | created_at | is_active |
|----|----------|----------|------|-------|------------|-----------|
| 1 | admin | admin123 (hashed) | admin | admin@digitalpulse.local | 2026-01-01 | 1 |
| 2 | authority | auth123 (hashed) | authority | authority@digitalpulse.local | 2026-01-01 | 1 |
| 3 | user | user123 (hashed) | user | user@digitalpulse.local | 2026-01-01 | 1 |

#### Table: audit_log

```sql
CREATE TABLE IF NOT EXISTS audit_log (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER,
    action TEXT NOT NULL,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    details TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

INDEXES:
  - PRIMARY KEY (id)
  - FOREIGN KEY (user_id) → users(id)
  - INDEX (timestamp) for time-based queries
  - INDEX (action) for action-type filtering
  - INDEX (user_id) for user-specific logs
```

**Sample Data:**
| id | user_id | action | timestamp | details |
|----|---------|--------|-----------|---------|
| 1 | 1 | LOGIN | 2026-01-18 10:30:45 | IP: 192.168.1.100 |
| 2 | 1 | VIEW_SENTIMENT | 2026-01-18 10:31:12 | Viewed 15-day analysis |
| 3 | 1 | LOGOUT | 2026-01-18 14:45:30 | Normal logout |

#### Table: sessions

```sql
CREATE TABLE IF NOT EXISTS sessions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    session_token TEXT UNIQUE NOT NULL,
    ip_address TEXT,
    user_agent TEXT,
    login_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_activity TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    logout_time TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

INDEXES:
  - PRIMARY KEY (id)
  - FOREIGN KEY (user_id) → users(id)
  - UNIQUE (session_token)
  - INDEX (user_id)
  - INDEX (login_time)
  - INDEX (logout_time)
```

**Sample Data:**
| id | user_id | session_token | ip_address | login_time | logout_time |
|----|---------|---------------|------------|------------|-------------|
| 1 | 1 | a8f3d7c2e1b9... | 192.168.1.100 | 2026-01-18 10:30:45 | 2026-01-18 14:45:30 |
| 2 | 2 | f9e2c1a4b7d6... | 192.168.1.101 | 2026-01-18 11:15:22 | NULL (active) |

---

### 3.6.4 Database Manipulation

#### Create Operations (INSERT)

```python
# Create new user
def create_user(username, password, role='user', email=None):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('''
        INSERT INTO users (username, password, role, email)
        VALUES (?, ?, ?, ?)
    ''', (username, password, role, email))
    conn.commit()
    conn.close()
```

#### Read Operations (SELECT)

```python
# Get user by username
def get_user(username):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM users WHERE username = ?', (username,))
    user = cursor.fetchone()
    conn.close()
    return user

# Get all users
def get_all_users():
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('SELECT id, username, role, email, is_active FROM users')
    users = cursor.fetchall()
    conn.close()
    return users
```

#### Update Operations (UPDATE)

```python
# Update password
def update_user_password(username, new_password):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('''
        UPDATE users
        SET password = ?, updated_at = CURRENT_TIMESTAMP
        WHERE username = ?
    ''', (new_password, username))
    conn.commit()
    conn.close()

# Update user role
def update_user_role(username, new_role):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('''
        UPDATE users
        SET role = ?, updated_at = CURRENT_TIMESTAMP
        WHERE username = ?
    ''', (new_role, username))
    conn.commit()
    conn.close()
```

#### Delete Operations (DELETE/SOFT DELETE)

```python
# Soft delete - deactivate account
def deactivate_user(username):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('''
        UPDATE users
        SET is_active = 0, updated_at = CURRENT_TIMESTAMP
        WHERE username = ?
    ''', (username,))
    conn.commit()
    conn.close()

# Hard delete (rarely used)
def delete_user(username):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('DELETE FROM users WHERE username = ?', (username,))
    conn.commit()
    conn.close()
```

#### Audit Logging

```python
# Log user action
def log_audit(user_id, action, details=None):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    cursor.execute('''
        INSERT INTO audit_log (user_id, action, details)
        VALUES (?, ?, ?)
    ''', (user_id, action, details))
    conn.commit()
    conn.close()

# Retrieve audit logs
def get_audit_logs(user_id=None, limit=100):
    conn = sqlite3.connect('instance/digital_pulse.db')
    cursor = conn.cursor()
    if user_id:
        cursor.execute('''
            SELECT * FROM audit_log
            WHERE user_id = ?
            ORDER BY timestamp DESC
            LIMIT ?
        ''', (user_id, limit))
    else:
        cursor.execute('''
            SELECT * FROM audit_log
            ORDER BY timestamp DESC
            LIMIT ?
        ''', (limit,))
    logs = cursor.fetchall()
    conn.close()
    return logs
```

---

### 3.6.5 Database Connection Controls and Strings

#### Connection String

**SQLite (Development):**

```python
DB_PATH = 'instance/digital_pulse.db'
CONNECTION_STRING = f'sqlite:///{DB_PATH}'
```

**PostgreSQL (Production - future):**

```python
import os
CONNECTION_STRING = os.environ.get(
    'DATABASE_URL',
    'postgresql://user:password@localhost:5432/digital_pulse'
)
```

#### Connection Management

```python
import sqlite3
from contextlib import contextmanager

# Connection pooling context manager
@contextmanager
def get_db_connection():
    """Safely manage database connections"""
    conn = sqlite3.connect('instance/digital_pulse.db')
    conn.row_factory = sqlite3.Row  # Return rows as dictionaries
    try:
        yield conn
        conn.commit()
    except Exception as e:
        conn.rollback()
        raise e
    finally:
        conn.close()

# Usage example:
with get_db_connection() as conn:
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM users')
    users = cursor.fetchall()
```

#### Connection Security

```python
# Enable foreign key constraints
conn = sqlite3.connect('instance/digital_pulse.db')
conn.execute('PRAGMA foreign_keys = ON')

# Enable write-ahead logging for better concurrency
conn.execute('PRAGMA journal_mode = WAL')

# Set timeout for concurrent access
conn.timeout = 10.0  # 10 seconds

# Parameterized queries to prevent SQL injection
cursor.execute('SELECT * FROM users WHERE username = ?', (username,))
```

#### Database Backup Strategy

```python
import shutil
from datetime import datetime

def backup_database():
    """Create database backup"""
    timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
    backup_file = f'backups/digital_pulse_{timestamp}.db'

    shutil.copy('instance/digital_pulse.db', backup_file)
    print(f'Backup created: {backup_file}')

def restore_database(backup_file):
    """Restore from backup"""
    shutil.copy(backup_file, 'instance/digital_pulse.db')
    print(f'Restored from: {backup_file}')

# Automated backup schedule (run daily at 2 AM)
def schedule_backups():
    """Schedule daily backups using APScheduler"""
    from apscheduler.schedulers.background import BackgroundScheduler

    scheduler = BackgroundScheduler()
    scheduler.add_job(
        backup_database,
        'cron',
        hour=2,
        minute=0,
        id='daily_backup'
    )
    scheduler.start()
```

---

## 3.7 Methodology

### 3.7.1 Design Methodology: SDLC (Systems Development Life Cycle)

**Model Used:** Agile with Scrum Framework

### 3.7.2 Design Review Process

1. **Requirements Review:** Validate against SRS document
2. **Architecture Review:** Ensure alignment with design principles
3. **Code Review:** Peer review before integration
4. **Testing Review:** Verify test coverage and quality
5. **Stakeholder Sign-off:** Approval before deployment

### 3.7.3 Design Documentation Standards

- **Code Comments:** Inline documentation for complex logic
- **Docstrings:** Python docstrings for all functions/classes
- **README Files:** In each module directory
- **API Documentation:** Swagger/OpenAPI specs for endpoints
- **ER Diagrams:** Maintained in documentation/
- **Architecture Diagrams:** Updated with system changes

---

**Next Chapter:** [Chapter 4: Implementation, Testing, and Maintenance](./03_IMPLEMENTATION_AND_TESTING.md)

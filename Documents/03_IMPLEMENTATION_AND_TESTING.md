# Chapter 4: Implementation, Testing, and Maintenance

## 4.1 Introduction to Languages, IDEs, Tools, and Technologies Used

### 4.1.1 Programming Languages

#### Python 3.13

- **Role:** Backend development and ML/data science processing
- **Version:** 3.13.x
- **Justification:**
  - Rich ecosystem for ML libraries (scikit-learn, XGBoost, PyTorch)
  - Excellent data processing with Pandas and NumPy
  - Web framework support with Flask
  - Easy learning curve for data scientists
- **Compatibility:** All major operating systems (Windows, Linux, macOS)

#### HTML5 & CSS3

- **Role:** Frontend markup and styling
- **Version:** Latest standards-compliant
- **Justification:**
  - Semantic markup for accessibility
  - CSS Grid and Flexbox for responsive layouts
  - Bootstrap framework integration
- **Tools:** Jinja2 templating engine

#### JavaScript (ECMAScript 6+)

- **Role:** Client-side interactivity and visualization
- **Libraries Used:**
  - Chart.js 4.4.0 - Data visualization
  - Leaflet.js - Geolocation mapping
  - Bootstrap JS - Interactive components
- **Justification:** No transpilation needed, modern browser support

### 4.1.2 Development IDEs and Editors

| IDE/Editor             | Purpose        | Features                                         |
| ---------------------- | -------------- | ------------------------------------------------ |
| **Visual Studio Code** | Primary IDE    | Git integration, Python extension, Debug support |
| **PyCharm Community**  | Alternative    | Advanced Python debugging, code inspection       |
| **Jupyter Notebook**   | ML Development | Interactive development for model training       |
| **SQLiteStudio**       | Database       | Visual database management and queries           |

### 4.1.3 Development Tools and Technologies

#### Backend Framework

**Framework:** Flask 3.1.2

```python
# Key Features:
- Lightweight and modular
- Built-in development server
- Jinja2 templating
- Session management
- Request/response handling
```

#### Database

**Database Engine:** SQLite 3

```
- Advantages:
  - Zero configuration
  - Serverless
  - Lightweight (suitable for demo/small deployment)
  - Excellent for prototyping
- Migration Path: SQLite → PostgreSQL ready
```

#### Data Processing Libraries

```python
- Pandas 2.3.3        # Data manipulation and analysis
- NumPy 2.4.1         # Numerical computing
- Scikit-learn 1.8.0  # Machine learning algorithms
```

#### ML/Forecasting Models

```python
- statsmodels 0.14.6  # Statistical models (ARIMA)
- XGBoost 3.1.3       # Gradient boosting
- Prophet 1.2.1       # Time series forecasting
- PyTorch             # Deep learning (for NLP)
```

#### Frontend Libraries

```javascript
- Bootstrap 5.3.0     # CSS framework
- Chart.js 4.4.0      # Interactive charts
- Leaflet.js          # Maps and geolocation
- Font Awesome 6.4.0  # Icons
```

#### Development and Deployment Tools

| Tool       | Version | Purpose                     |
| ---------- | ------- | --------------------------- |
| Git        | 2.x     | Version control             |
| pip        | Latest  | Package manager             |
| virtualenv | Latest  | Environment isolation       |
| Gunicorn   | Latest  | WSGI server (production)    |
| Nginx      | Latest  | Reverse proxy (production)  |
| Docker     | Latest  | Containerization (optional) |

#### Code Quality Tools

| Tool            | Purpose                   |
| --------------- | ------------------------- |
| **pytest**      | Unit testing framework    |
| **coverage.py** | Code coverage measurement |
| **black**       | Code formatting           |
| **flake8**      | Linting                   |
| **mypy**        | Type checking             |

#### Version Control and Collaboration

- **Repository:** Git (GitHub/GitLab)
- **Branch Strategy:** Git Flow
  - `main` - Production releases
  - `develop` - Development branch
  - `feature/*` - Feature branches
  - `bugfix/*` - Bug fix branches
  - `release/*` - Release preparation

### 4.1.4 Technology Stack Summary

```
┌─────────────────────────────────────────┐
│         Digital Pulse Monitor            │
├─────────────────────────────────────────┤
│  Frontend Layer (Client)                 │
│  ├─ HTML5 / CSS3                        │
│  ├─ JavaScript (ES6+)                   │
│  ├─ Bootstrap 5.3                       │
│  └─ Chart.js + Leaflet.js               │
├─────────────────────────────────────────┤
│  Application Layer (Flask)               │
│  ├─ Flask 3.1.2 (Web framework)         │
│  ├─ Jinja2 (Templating)                 │
│  ├─ SQLite3 (Database)                  │
│  └─ Python 3.13 (Runtime)               │
├─────────────────────────────────────────┤
│  Data Processing Layer                   │
│  ├─ Pandas (Data manipulation)          │
│  ├─ NumPy (Numerical computing)         │
│  └─ Scikit-learn (ML utilities)         │
├─────────────────────────────────────────┤
│  ML Models Layer                         │
│  ├─ statsmodels (ARIMA)                 │
│  ├─ XGBoost (Boosting)                  │
│  ├─ Prophet (Time series)               │
│  └─ PyTorch (Deep learning)             │
├─────────────────────────────────────────┤
│  Infrastructure Layer                    │
│  ├─ Linux/Windows OS                    │
│  ├─ Nginx (Reverse proxy)               │
│  ├─ Gunicorn (WSGI server)              │
│  └─ Docker (Optional containerization) │
└─────────────────────────────────────────┘
```

---

## 4.2 Coding Standards of Language Used

### 4.2.1 Python Coding Standards (PEP 8)

#### Naming Conventions

```python
# Classes: PascalCase
class UserAuthentication:
    pass

class SentimentAnalyzer:
    pass

# Functions and variables: snake_case
def load_sentiment_data():
    return {}

def verify_user(username, password):
    pass

current_user_id = 1
max_retries = 3

# Constants: UPPERCASE_WITH_UNDERSCORES
DEFAULT_TIMEOUT = 30
MAX_LOGIN_ATTEMPTS = 5
DATABASE_PATH = 'instance/digital_pulse.db'

# Private methods: _leading_underscore
class DataProcessor:
    def _validate_input(self, data):
        pass

    def _clean_data(self, data):
        pass
```

#### Code Style

```python
# Line length: Maximum 79 characters (PEP 8)
# But 88-100 acceptable for code readability

# Imports: Organized in groups
import sys
import os
from datetime import datetime

import pandas as pd
import numpy as np
from flask import Flask, render_template

from database import init_db, verify_user

# Spacing: Two blank lines between top-level functions/classes
class User:
    pass


class AuditLog:
    pass


def load_data():
    pass
```

#### Documentation Style

```python
def load_sentiment_data():
    """Load sentiment data from CSV or generate mock data.

    This function attempts to load sentiment data from the CSV file.
    If the file is not available or corrupted, it generates mock data
    for demonstration purposes.

    Returns:
        List[dict]: List of sentiment data dictionaries, each containing:
            - date (str): Data date in YYYY-MM-DD format
            - positive (int): Positive sentiment count
            - negative (int): Negative sentiment count
            - neutral (int): Neutral sentiment count
            - ambiguous (int): Ambiguous sentiment count

    Raises:
        IOError: If both CSV and mock data generation fail

    Example:
        >>> data = load_sentiment_data()
        >>> len(data)
        15
        >>> data[0]['date']
        '2026-01-04'
    """
    pass

class SentimentAnalyzer:
    """Analyzer for sentiment data processing.

    Attributes:
        data_path (str): Path to sentiment data CSV file
        window_size (int): Size of rolling window (default: 15)
        data (pd.DataFrame): Loaded sentiment data

    Methods:
        load_data(): Load data from CSV file
        calculate_percentages(): Convert counts to percentages
        get_trend(): Calculate trend changes
    """
    pass
```

#### Error Handling

```python
# Use specific exceptions
def verify_user(username, password):
    try:
        user = get_user(username)
        if user is None:
            raise ValueError(f"User '{username}' not found")
        if not check_password(user['password'], password):
            raise ValueError("Invalid password")
        return True
    except (ValueError, DatabaseError) as e:
        logger.error(f"Authentication error: {e}")
        return False
    except Exception as e:
        logger.critical(f"Unexpected error: {e}")
        raise

# Use context managers for resource management
def load_data_from_csv(filepath):
    try:
        with open(filepath, 'r') as f:
            data = pd.read_csv(f)
        return data
    except FileNotFoundError:
        logger.warning(f"File not found: {filepath}")
        return None
    finally:
        # Cleanup happens automatically
        pass
```

#### Type Hints

```python
from typing import List, Dict, Optional, Tuple

def verify_user(username: str, password: str) -> bool:
    """Verify user credentials."""
    pass

def load_sentiment_data() -> List[Dict[str, any]]:
    """Load sentiment data from CSV."""
    pass

def get_user(username: str) -> Optional[Dict]:
    """Get user by username or None if not found."""
    pass

def predict_ensemble(features: List[float]) -> Tuple[float, float]:
    """Return prediction and confidence score."""
    pass
```

### 4.2.2 HTML/CSS Coding Standards

#### HTML5 Structure

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Digital Pulse Monitor</title>
    <!-- CSS in head -->
    <link
      rel="stylesheet"
      href="{{ url_for('static', filename='style.css') }}"
    />
  </head>
  <body>
    <!-- Semantic HTML -->
    <header>
      <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <!-- Navigation content -->
      </nav>
    </header>

    <main class="container mt-4">
      <!-- Main content -->
    </main>

    <footer class="bg-dark text-white mt-5 py-4">
      <!-- Footer content -->
    </footer>

    <!-- Scripts at end of body -->
    <script src="{{ url_for('static', filename='chart.js') }}"></script>
  </body>
</html>
```

#### CSS Organization

```css
/* 1. Reset and Base Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* 2. Variables and Colors */
:root {
  --primary-color: #2e86ab;
  --secondary-color: #a23b72;
  --success-color: #06a77d;
  --danger-color: #d62828;
}

/* 3. Typography */
body {
  font-family: "Roboto", sans-serif;
  font-size: 14px;
  line-height: 1.6;
  color: #333;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: 600;
  margin-bottom: 1rem;
}

/* 4. Layout Components */
.container {
}
.card {
}
.navbar {
}

/* 5. Utility Classes */
.mt-4 {
  margin-top: 2rem;
}
.text-center {
  text-align: center;
}
```

### 4.2.3 JavaScript Coding Standards

```javascript
// Use ES6+ syntax
const loadSentimentData = async () => {
  try {
    const response = await fetch("/api/sentiment");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error loading sentiment data:", error);
    return null;
  }
};

// Clear naming conventions
const USER_ROLES = ["admin", "authority", "user"];
const API_ENDPOINT = "/api";

// Arrow functions for callbacks
document.querySelectorAll(".btn-delete").forEach((btn) => {
  btn.addEventListener("click", (e) => {
    if (confirm("Are you sure?")) {
      deleteItem(e.target.dataset.id);
    }
  });
});

// Proper error handling
function initChart(containerId, data) {
  const container = document.getElementById(containerId);
  if (!container) {
    console.error(`Container #${containerId} not found`);
    return;
  }

  new Chart(container, {
    type: "bar",
    data: data,
  });
}
```

---

## 4.3 Project Scheduling Using PERT and GANTT Charts

### 4.3.1 PERT Chart (Program Evaluation Review Technique)

**Formula:** Expected Time = (Optimistic + 4×Most Likely + Pessimistic) / 6

#### PERT Estimates by Phase

| Phase          | Optimistic | Most Likely | Pessimistic | Expected | Variance |
| -------------- | ---------- | ----------- | ----------- | -------- | -------- |
| Setup          | 1          | 2           | 3           | 2.0      | 0.11     |
| Backend Dev    | 6          | 8           | 12          | 8.3      | 1.0      |
| Frontend Dev   | 5          | 7           | 10          | 7.2      | 0.69     |
| ML Integration | 4          | 6           | 9           | 6.2      | 0.69     |
| Testing        | 3          | 4           | 6           | 4.2      | 0.25     |
| Documentation  | 2          | 3           | 5           | 3.2      | 0.25     |
| **Total**      | **21**     | **30**      | **45**      | **30.9** | **3.0**  |

**Critical Path:** Setup → Backend → Frontend → ML Integration → Testing → Documentation
**Expected Project Duration:** ~31 weeks (7.7 months)

### 4.3.2 GANTT Chart

```
Timeline: January 2026 - September 2026

Week 1-2:   Setup & Infrastructure
            ████████░░░░░░░░░░░░░░░░░░░░░░░░░░ [Complete]

Week 3-10:  Backend Development
            ████████████████████░░░░░░░░░░░░░░░ [Complete]
            ├─ Database Schema
            ├─ Authentication
            ├─ API Endpoints
            └─ Data Pipeline

Week 6-12:  Frontend Development (Parallel)
            ██████████████░░░░░░░░░░░░░░░░░░░░░ [Complete]
            ├─ Templates
            ├─ Dashboard UI
            └─ Styling

Week 9-14:  ML Integration
            ███████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ [Complete]
            ├─ Model Training
            ├─ Integration
            └─ Testing

Week 13-16: Testing & QA
            ████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ [Complete]
            ├─ Unit Tests
            ├─ Integration Tests
            └─ UAT

Week 16-18: Documentation & Deployment
            ███░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ [In Progress]
            ├─ Final Docs
            ├─ Deployment
            └─ Training

Legend: ████ = Completed, ░░░░ = Planned
```

---

## 4.4 Testing Techniques and Test Plans

### 4.4.1 Testing Levels

#### 1. Unit Testing

**Scope:** Individual functions and methods
**Framework:** pytest
**Target Coverage:** >80%

```python
# tests/test_database.py
import pytest
from database import create_user, get_user, verify_user

class TestUserManagement:
    """Test user management functions"""

    def test_create_user_success(self):
        """Test creating a new user"""
        result = create_user('testuser', 'password123', 'user', 'test@example.com')
        assert result == True

    def test_create_user_duplicate_username(self):
        """Test creating user with duplicate username"""
        create_user('testuser', 'password123')
        result = create_user('testuser', 'password456')
        assert result == False

    def test_get_user_exists(self):
        """Test retrieving existing user"""
        create_user('testuser', 'password123')
        user = get_user('testuser')
        assert user is not None
        assert user['username'] == 'testuser'

    def test_get_user_not_exists(self):
        """Test retrieving non-existent user"""
        user = get_user('nonexistent')
        assert user is None

    def test_verify_user_correct_password(self):
        """Test user verification with correct password"""
        create_user('testuser', 'password123')
        result = verify_user('testuser', 'password123')
        assert result == True

    def test_verify_user_wrong_password(self):
        """Test user verification with wrong password"""
        create_user('testuser', 'password123')
        result = verify_user('testuser', 'wrongpassword')
        assert result == False
```

#### 2. Integration Testing

**Scope:** Multiple components working together
**Framework:** pytest with Flask test client

```python
# tests/test_integration.py
import pytest
from app import app

@pytest.fixture
def client():
    """Create test client"""
    app.config['TESTING'] = True
    with app.test_client() as client:
        yield client

def test_login_flow(client):
    """Test complete login flow"""
    # 1. Get login page
    response = client.get('/login')
    assert response.status_code == 200

    # 2. Submit login form
    response = client.post('/login', data={
        'user': 'admin',
        'pass': 'admin123'
    }, follow_redirects=True)
    assert response.status_code == 200
    assert b'mental_signals' in response.data

def test_sentiment_analysis_flow(client):
    """Test sentiment analysis access"""
    # 1. Login first
    client.post('/login', data={'user': 'admin', 'pass': 'admin123'})

    # 2. Access sentiment analysis
    response = client.get('/mental_signals')
    assert response.status_code == 200
    assert b'MENTAL SIGNALS' in response.data
```

#### 3. System Testing

**Scope:** Complete system functionality
**Test Scenarios:**

- End-to-end user workflows
- Data integrity checks
- Performance under load
- Security validations

#### 4. User Acceptance Testing (UAT)

**Scope:** Stakeholder validation
**Participants:** Admin, Authority, User representatives
**Success Criteria:** All functional requirements met

### 4.4.2 Test Plan Document

#### Test Objective

Ensure the Digital Pulse Monitor system meets all functional and non-functional requirements before production deployment.

#### Test Scope

- Functional requirements (authentication, forecasting, visualization)
- Non-functional requirements (performance, security, usability)
- Data integrity and consistency
- Error handling and recovery

#### Test Environment

- **Hardware:** 8GB RAM, 2.4GHz CPU, 50GB SSD
- **Software:** Python 3.13, Flask 3.1.2, SQLite 3
- **Browser:** Chrome, Firefox (latest versions)
- **OS:** Windows 10/11, Ubuntu 22.04

#### Test Cases

**TC-001: User Authentication**

```
Test ID: TC-001
Title: Valid User Login
Priority: High
Pre-condition: User account exists in database
Steps:
  1. Navigate to /login
  2. Enter valid username: 'admin'
  3. Enter valid password: 'admin123'
  4. Click 'Login'
Expected Result: Redirect to /mental_signals, user session created
Post-condition: User logged in, audit log entry created
```

**TC-002: Invalid Login Attempt**

```
Test ID: TC-002
Title: Invalid Password Login
Priority: High
Pre-condition: User account exists
Steps:
  1. Navigate to /login
  2. Enter username: 'admin'
  3. Enter invalid password: 'wrongpass'
  4. Click 'Login'
Expected Result: Error message displayed, no redirection
Post-condition: User not logged in
```

**TC-003: Sentiment Data Display**

```
Test ID: TC-003
Title: Load and Display Sentiment Analysis
Priority: High
Pre-condition: User logged in, CSV data available
Steps:
  1. Access /mental_signals
  2. Wait for data loading
  3. Verify chart renders
Expected Result: 15-day sentiment chart displayed with data
Post-condition: Chart interactive and responsive
```

**TC-004: Forecast Generation**

```
Test ID: TC-004
Title: Generate 30-Day Forecast
Priority: High
Pre-condition: User logged in, model files available
Steps:
  1. Access /prediction_analysis
  2. Wait for forecast calculation
  3. Verify multiple models displayed
Expected Result: 4-model forecast table shown with confidence intervals
Post-condition: Data exportable
```

### 4.4.3 Test Automation Script Example

```python
# tests/test_api.py
import pytest
import json
from app import app

class TestPredictionAPI:
    """Test ML prediction API endpoints"""

    @pytest.fixture
    def client(self):
        app.config['TESTING'] = True
        return app.test_client()

    def test_predict_single_text(self, client):
        """Test single text prediction endpoint"""
        payload = {
            'text': 'I am feeling sad and hopeless today'
        }

        response = client.post(
            '/api/predict',
            data=json.dumps(payload),
            content_type='application/json'
        )

        assert response.status_code == 200
        data = json.loads(response.data)
        assert 'success' in data
        assert 'predictions' in data
        assert 'MS' in data['predictions']
        assert 'ME' in data['predictions']
        assert 'RL' in data['predictions']

    def test_predict_empty_text(self, client):
        """Test prediction with empty text"""
        payload = {'text': ''}

        response = client.post(
            '/api/predict',
            data=json.dumps(payload),
            content_type='application/json'
        )

        assert response.status_code == 400
        data = json.loads(response.data)
        assert 'error' in data

class TestSentimentAPI:
    """Test sentiment data API"""

    @pytest.fixture
    def client(self):
        app.config['TESTING'] = True
        return app.test_client()

    def test_get_sentiment_data(self, client):
        """Test sentiment data retrieval"""
        response = client.get('/api/sentiment')

        assert response.status_code == 200
        data = json.loads(response.data)
        assert isinstance(data, list)
        assert len(data) > 0
        assert 'date' in data[0]
        assert 'positive' in data[0]
        assert 'negative' in data[0]
```

### 4.4.4 Performance Testing

```python
# tests/test_performance.py
import time
import pytest
from app import app

def test_homepage_load_time():
    """Test home page load performance"""
    client = app.test_client()

    start = time.time()
    response = client.get('/')
    elapsed = time.time() - start

    assert response.status_code == 200
    assert elapsed < 1.0, f"Page load took {elapsed}s, expected <1s"

def test_sentiment_analysis_load_time():
    """Test sentiment analysis page load performance"""
    client = app.test_client()

    start = time.time()
    response = client.get('/mental_signals')
    elapsed = time.time() - start

    assert response.status_code == 200
    assert elapsed < 2.0, f"Page load took {elapsed}s, expected <2s"

def test_api_response_time():
    """Test API response performance"""
    client = app.test_client()

    payload = {'text': 'Test tweet for sentiment analysis'}
    start = time.time()
    response = client.post(
        '/api/predict',
        json=payload
    )
    elapsed = time.time() - start

    assert response.status_code == 200
    assert elapsed < 0.5, f"API call took {elapsed}s, expected <0.5s"
```

### 4.4.5 Security Testing

```python
# tests/test_security.py
import pytest
from app import app

def test_sql_injection_prevention():
    """Test SQL injection protection"""
    client = app.test_client()

    # Try SQL injection in login
    malicious_input = "' OR '1'='1"
    response = client.post('/login', data={
        'user': malicious_input,
        'pass': 'anything'
    })

    # Should reject invalid input or not execute injection
    assert response.status_code == 200
    assert b'Invalid' in response.data or b'error' in response.data

def test_xss_prevention():
    """Test XSS attack prevention"""
    client = app.test_client()

    malicious_text = '<script>alert("XSS")</script>'
    response = client.post('/api/predict', json={
        'text': malicious_text
    })

    # Response should escape script tags
    data = response.get_json()
    # Verify script not executed
    assert '<script>' not in str(data)

def test_authentication_required():
    """Test protected routes require authentication"""
    client = app.test_client()

    protected_routes = [
        '/prediction_analysis',
        '/admin_dashboard',
        '/authority_dashboard'
    ]

    for route in protected_routes:
        response = client.get(route)
        # Should redirect to login or return 401
        assert response.status_code in [302, 401]
```

---

## 4.5 Bug Tracking and Resolution Process

### 4.5.1 Bug Classification

| Severity     | Impact                 | Examples               | Resolution Time |
| ------------ | ---------------------- | ---------------------- | --------------- |
| **Critical** | System down, data loss | DB connection fail     | 2 hours         |
| **High**     | Feature broken         | Model crash, auth fail | 8 hours         |
| **Medium**   | Feature degraded       | Slow performance       | 1-2 days        |
| **Low**      | Minor issues           | UI glitch, typo        | 1 week          |

### 4.5.2 Bug Report Template

```
BUG-XXX: [Brief Description]

Severity: [Critical/High/Medium/Low]
Status: [Open/In Progress/Resolved/Closed]
Assigned To: [Developer]

Description:
Clear description of the bug and its impact

Steps to Reproduce:
1. Step 1
2. Step 2
3. ...

Expected Behavior:
What should happen

Actual Behavior:
What actually happens

Environment:
- OS: Windows 10
- Browser: Chrome 120
- Python Version: 3.13
```

---

## 4.6 Maintenance and Support

### 4.6.1 Maintenance Strategy

**Preventive Maintenance:**

- Regular dependency updates (quarterly)
- Security patches (immediately upon release)
- Performance monitoring and optimization
- Database maintenance (index rebuilding, vacuum)

**Corrective Maintenance:**

- Bug fixes based on priority
- User-reported issues resolution
- Error log analysis and correction

**Adaptive Maintenance:**

- Feature enhancements
- Platform updates
- Scalability improvements

### 4.6.2 Deployment Process

```
Development → Staging → Production

1. Development
   - Feature branch created
   - Code written and tested
   - Pull request reviewed

2. Staging
   - Deploy to staging server
   - Run full test suite
   - Performance testing
   - UAT with stakeholders

3. Production
   - Backup current database
   - Deploy during maintenance window
   - Verify all systems
   - Monitor for issues
   - Rollback plan ready
```

### 4.6.3 Monitoring and Alerting

```python
# Monitoring checklist:
- Application error rate
- API response times
- Database query performance
- Disk space usage
- Memory consumption
- CPU utilization
- Failed login attempts
- System uptime

# Alerts triggered when:
- Error rate > 5%
- Response time > 2 seconds
- Disk usage > 80%
- More than 10 failed logins in 5 minutes
```

---

**Next Chapter:** [Chapter 5: Results and Discussions](./04_RESULTS_AND_DISCUSSIONS.md)

# NBA Rebounding Prop Valuation & Edge Detection System

A production-grade Python analytics system that identifies undervalued/overvalued NBA rebounding prop bets through statistical modeling, feature engineering, and predictive analytics. Designed for actionable edge detection in sports betting markets.

---

## Overview

This project demonstrates end-to-end quantitative analysis: data ingestion → feature engineering → probabilistic modeling → market valuation. The system generates statistically-grounded probability distributions for NBA player rebounding performance, enabling comparative analysis against market odds and identification of profitable betting opportunities.

### Key Capabilities

- **Predictive Modeling**: Scikit-learn based regression models calibrated on historical NBA rebounding data
- **Feature Engineering**: Domain-specific feature construction (opponent strength, pace adjustments, player role metrics, seasonal trends)
- **Data Pipeline**: Automated web scraping, data transformation, and bulk execution across multiple players
- **Player Tracking**: Dynamic player-to-team mapping system that accommodates mid-season trades and roster changes
- **Performance Analytics**: Comparative analysis framework between model predictions and realized outcomes

---

## Technical Architecture

### System Components

#### 1. **Model Development & Validation** (`(1) - MAIN - NBA Rebounding Valuation - Model Logic.ipynb`)

The core analytics engine containing:

- **Data Preparation**: Historical rebounding datasets, feature normalization, handling missing values
- **Feature Engineering**:
  - Player-level metrics (usage rate, rebound share, positional role)
  - Opponent-level metrics (defensive rebounding strength, pace of play)
  - Contextual variables (home/away, back-to-back status, injury adjustments)
- **Model Training**: Scikit-learn regression pipelines (linear regression, gradient boosting) with cross-validation
- **Calibration & Backtesting**: Out-of-sample probability distribution validation
- **Output**: Trained model artifacts and confidence intervals for individual player performance predictions

**Technical Stack**: Python, Scikit-learn, Pandas, NumPy

#### 2. **Bulk Execution Pipeline** (`(2) - Bulk Upload Execution.ipynb`)

Operationalizes the core model logic for scalable, multi-player analysis:

- Applies trained model logic sequentially across roster of NBA players
- Generates probability distributions for each player's rebounding performance
- Outputs structured datasets for downstream BI tooling (Looker dashboards)
- Handles edge cases and data quality validation during batch processing

**Use Case**: Daily/weekly updates across entire NBA league without manual re-execution

#### 3. **Player Roster Management** (`(3) - Player URL Mapping Data.ipynb`)

Maintains authoritative mapping table for player identity, team affiliation, and data source references:

- Tracks player-to-team relationships with effective dates
- Manages mid-season roster changes and trades
- Indexes player URLs for web scraping operations
- Enables consistent player identification across data sources and seasons

**Business Value**: Ensures analytics remain current despite roster volatility; reduces data engineering overhead for identity matching

---

## Data Outputs

### 1. Performance Tracking Data (`SAMPLE - Performance Tracking Data (Looker).xlsx`)

Underlying dataset feeding Looker analytics dashboards:

- **Schema**: Player, Date, Team, Opponent, Minutes Played, Rebounds Actual, Model Prediction, Variance
- **Grain**: Game-level rebounding performance
- **Use Case**: Trend analysis, comparative metrics, historical benchmarking

### 2. Prop Valuation & Probability Distribution (`SAMPLE - Prop Valuation and Probability Distribution.xlsx`)

Primary output file containing statistical analysis and market edge detection:

- **Prediction Metrics**:
  - Point estimate (expected rebounds)
  - Confidence intervals (68th, 90th, 95th percentiles)
  - Probability of over/under specific line values (e.g., P(Rebounds > 8.5))
  
- **Market Comparison**:
  - Sportsbook line vs. model prediction (basis analysis)
  - Implied probability vs. statistical probability
  - Edge magnitude and direction (+EV identification)

- **Data Transformations Applied**:
  - Feature scaling and normalization
  - Outlier handling and robust estimation
  - Seasonal adjustment and trend components
  - Position-specific benchmarking

---

## Methodology & Analysis Framework

### Feature Engineering Pipeline

| Feature Category | Examples | Purpose |
|---|---|---|
| **Player Metrics** | Usage Rate, Rebound %, Positional Role | Capture individual player propensity and role |
| **Opponent Metrics** | Defensive Rebounding %, Pace Rating | Account for defensive difficulty and game tempo |
| **Contextual Variables** | Home/Away, Back-to-Back, Injury Status | Adjust for situational factors affecting performance |
| **Time Series Components** | Seasonal trend, moving averages, momentum | Capture performance dynamics and form |

### Model Validation Strategy

- **Cross-Validation**: K-fold validation to assess generalization
- **Backtesting**: Historical prediction accuracy against realized outcomes
- **Calibration Analysis**: Probability distribution alignment (predicted vs. observed frequencies)
- **Sensitivity Testing**: Feature importance ranking and prediction elasticity

### Edge Detection Logic

The system identifies profitable opportunities by comparing:
1. **Model Probability** (P(Rebounds > Line) from statistical model)
2. **Implied Probability** (derived from sportsbook odds)
3. **Expected Value**: EV = (Model Prob × Payout) - (1 - Model Prob) × Stake

Bets with positive EV above a confidence threshold are flagged for consideration.

---

## Usage & Workflow

### Quick Start

1. **Initial Model Development**: Run `(1) - MAIN - NBA Rebounding Valuation - Model Logic.ipynb`
   - Loads historical data, engineers features, trains model
   - Outputs trained model artifacts

2. **Bulk Execution**: Run `(2) - Bulk Upload Execution.ipynb`
   - Applies model across current NBA roster
   - Generates `Prop Valuation and Probability Distribution.xlsx`

3. **Update Player Mapping** (as needed): Run `(3) - Player URL Mapping Data.ipynb`
   - Sync roster changes and trade information
   - Refresh player identifiers for web scraping

4. **Analytics & Dashboard**: Load outputs into Looker
   - Connect `Performance Tracking Data (Looker).xlsx` for trend dashboards
   - Reference `Prop Valuation and Probability Distribution.xlsx` for opportunity identification

### Data Refresh Frequency

- **Daily**: Bulk execution pipeline (post-games) to update realized outcomes
- **Weekly**: Model retraining to incorporate recent performance data
- **As-Needed**: Player mapping updates following trades/roster moves

---

## Key Technical Decisions

### Why Scikit-learn?

- Fast iteration and experimentation during model development
- Robust cross-validation and hyperparameter tuning utilities
- Transparent model interpretability (feature importance, coefficients)
- Sufficient for linear and tree-based regression tasks

### Data Quality & Error Handling

- Handles missing data through imputation and interpolation strategies
- Validates feature distributions and flags anomalies
- Implements bounds checking for physically implausible predictions
- Maintains audit trail of data transformations for reproducibility

### Player Mapping as a Separate Layer

- Decouples identity/reference data from analytical logic
- Enables rapid roster synchronization without retraining models
- Supports multiple downstream use cases (scraping, scheduling, analytics)
- Reduces brittle joins across disparate data sources

---

## Business Context & Applications

This system demonstrates practical quantitative analysis applied to real-world markets:

- **Statistical Rigor**: Probability distributions grounded in historical data and cross-validation
- **Operational Efficiency**: Automated bulk processing eliminates manual re-execution
- **Dynamic Adaptation**: Roster tracking ensures analytics remain current despite roster volatility
- **Actionable Output**: Clear valuation metrics enable rapid opportunity assessment

### Extensions & Future Work

- Incorporate in-game context (score differential, quarter) for live prop adjustments
- Expand to additional prop markets (assists, points, player combinations)
- Integrate betting exchange API for real-time line monitoring
- Implement Bayesian hierarchical models for enhanced uncertainty quantification

---

## Tools & Dependencies

| Component | Purpose |
|---|---|
| **Python 3.8+** | Core scripting and orchestration |
| **Scikit-learn** | Statistical modeling and validation |
| **Pandas** | Data manipulation and transformation |
| **NumPy** | Numerical computation and array operations |
| **Web Scraping Libraries** | Data ingestion from NBA/sportsbook sources |
| **Looker** | Dashboard and analytics visualization |
| **Excel/Google Sheets** | Output file format for stakeholder accessibility |

---

## Project Structure

```
NBA-Rebounding-Prop-Valuation-Edge-Detection-System/
│
├── Python Scripts/
│   ├── (1) - MAIN - NBA Rebounding Valuation - Model Logic.ipynb
│   ├── (2) - Bulk Upload Execution.ipynb
│   └── (3) - Player URL Mapping Data.ipynb
│
└── Data Outputs/
    ├── SAMPLE - Performance Tracking Data (Looker).xlsx
    └── SAMPLE - Prop Valuation and Probability Distribution.xlsx
```

---

## Author

**Lou Ledesma** | Senior Analytics Engineer

Specialized in quantitative analysis, predictive modeling, and building data-driven decision frameworks. This project demonstrates feature engineering rigor, statistical validation methodology, and operational data pipeline design.

---

## Disclaimers

1. This project is for educational and analytical purposes. Sports betting involves financial risk. Users should conduct their own analysis and adhere to applicable gambling regulations in their jurisdiction.
2. README.md file description was generated using Perplexity AI.

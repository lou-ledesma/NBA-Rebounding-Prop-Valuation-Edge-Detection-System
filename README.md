# NBA Rebounding Prop Valuation & Edge Detection System

A Python data science project that explores NBA rebounding prop valuation through statistical modeling, feature engineering, and Monte Carlo simulation. Built to translate historical performance into predictive insights and probability-based analysis.

---

## Overview

This project demonstrates end-to-end quantitative analysis: data ingestion → feature engineering → probabilistic modeling → market valuation. The analysis generates simulated distributions of NBA player rebounding predictions, supporting comparison with prop lines and a closer look at uncertainty around expected performance.

### Key Capabilities

- **Predictive Modeling**: Scikit-learn linear regression fitted to historical NBA rebounding data
- **Feature Engineering**: Basketball-specific feature construction (rebounding efficiency, estimated possessions, minutes played, home/away indicators)
- **Analytical Data Preparation**: Web scraping, data transformation, and batch analysis across multiple players
- **Player Reference Data**: Player identifiers, positions, and team mappings that support consistent analysis
- **Performance Analytics**: Comparative analysis framework between model predictions and realized outcomes

---

## Analytical Framework

### Analysis Components

#### 1. **Model Development & Validation** (`(1) - MAIN - NBA Rebounding Valuation - Model Logic.ipynb`)

The core modeling workflow containing:

- **Data Preparation**: Historical game logs, data type standardization, filtering non-playing entries, and player/team data joins
- **Feature Engineering**:
  - Player-level metrics (minutes played, rebounds per minute, rebounding ratios)
  - Team and opponent statistics (field-goal attempts, rebounds, estimated possessions)
  - Contextual variables (home/away indicators and recent performance summaries)
- **Model Training**: Scikit-learn linear regression with an 80/20 random train/test split
- **Model Evaluation**: Mean squared error calculated on held-out historical rows
- **Output**: Median simulated rebound predictions and estimated over/under probabilities

**Technical Stack**: Python, Scikit-learn, Pandas, NumPy

#### 2. **Multi-Player Analysis** (`(2) - Bulk Upload Execution.ipynb`)

Applies the modeling workflow across a list of players for comparative analysis:

- Runs the model notebook sequentially for each player in the input sheet
- Generates probability distributions for each player's rebounding performance
- Outputs structured datasets for downstream BI tooling (Looker dashboards)
- Skips entries that raise an IndexError during batch processing

**Use Case**: Repeated analysis across selected players without manually running each case

#### 3. **Player Reference Mapping** (`(3) - Player URL Mapping Data.ipynb`)

Builds a reference table for player identity, position, team affiliation, and source identifiers:

- Extracts player names, positions, and teams for the selected season
- Standardizes player names for matching across analytical inputs
- Indexes player URLs for web scraping operations
- Supports player identification in the model notebook

**Business Value**: Provides consistent player context for feature preparation, modeling, and reporting

---

## Data Outputs

### 1. Performance Tracking Data (`SAMPLE - Performance Tracking Data (Looker).xlsx`)
Link to Looker Dashboard: https://lookerstudio.google.com/s/vouaEROV4oA

Underlying dataset feeding Looker analytics dashboards:

- **Schema**: Player, Date, Team, Opponent, Minutes Played, Rebounds Actual, Model Prediction, Variance
- **Grain**: Game-level rebounding performance
- **Use Case**: Trend analysis, comparative metrics, historical benchmarking

### 2. Prop Valuation & Probability Distribution (`SAMPLE - Prop Valuation and Probability Distribution.xlsx`)

Sample output file presenting statistical analysis and prop valuation:

- **Prediction Metrics**:
  - Point estimate (median simulated rebound prediction)
  - Simulated predictions used to summarize the range of modeled outcomes
  - Probability of over/under specific line values (e.g., P(Rebounds > 8.5))
  
- **Market Comparison**:
  - Sportsbook line vs. model prediction (basis analysis)
  - Estimated probability of exceeding a selected prop line
  - Probability estimates across a range of half-point prop lines

- **Data Transformations Applied**:
  - Data type standardization and feature calculations
  - Filtering non-playing entries and zero-minute games
  - Weighted recent and historical performance summaries
  - Player position included in summary outputs

---

## Methodology & Analysis Framework

### Feature Engineering Pipeline

| Feature Category | Examples | Purpose |
|---|---|---|
| **Player Metrics** | Minutes Played, Rebounds per Minute, Rebounding Ratios | Describe playing time and rebounding efficiency |
| **Opponent Metrics** | Team Game Logs, Estimated Possessions | Incorporate matchup context and game pace |
| **Contextual Variables** | Home/Away Indicators | Represent game location in the regression |
| **Recent Performance** | Recent and Historical Summary Statistics | Set simulation inputs that reflect recent and longer-term performance |

### Model Validation Strategy

- **Holdout Evaluation**: An 80/20 random split and mean squared error provide an initial historical model check
- **Backtesting**: Chronological evaluation is a next step for assessing prospective accuracy
- **Calibration Analysis**: Comparing predicted probabilities with observed frequencies is a future validation step
- **Feature Review**: Replacing same-game outcome-derived predictors with pregame features is necessary to address target leakage

### Edge Detection Logic

The current analysis supports prop comparison through:

1. **Simulated Predictions**: Apply the regression to 250,000 simulated feature combinations
2. **Model Probability**: Calculate the share of predictions above or below the selected line
3. **Prop Valuation Map**: Compare over-probability estimates across half-point lines

Sportsbook implied probability, expected value, and automated positive-EV flags are potential extensions, rather than implemented notebook outputs.

---

## Usage & Workflow

### Quick Start

These notebooks originated in a wider local workflow. Execution requires the external reference workbook and player input sheet, along with updates to local paths, season settings, and the bulk notebook's referenced filename.

1. **Initial Model Development**: Run `(1) - MAIN - NBA Rebounding Valuation - Model Logic.ipynb`
   - Loads historical data, engineers features, trains model
   - Produces player summaries and simulated probability estimates

2. **Bulk Execution**: Run `(2) - Bulk Upload Execution.ipynb`
   - Runs the analysis for players in the input sheet
   - Generates an Excel workbook with Details, Prop Spreads, and Player Gamelogs tabs

3. **Update Player Mapping** (as needed): Run `(3) - Player URL Mapping Data.ipynb`
   - Rebuild player reference data for the selected season
   - Refresh player identifiers for web scraping

4. **Analytics & Dashboard**: Load outputs into Looker
   - Connect `Performance Tracking Data (Looker).xlsx` for trend dashboards
   - Reference `Prop Valuation and Probability Distribution.xlsx` for opportunity identification

### Data Refresh Frequency

Suggested cadence for the wider workflow; scheduling is not implemented in these notebooks:

- **Daily**: Bulk execution pipeline (post-games) to update realized outcomes
- **Weekly**: Model retraining to incorporate recent performance data
- **As-Needed**: Player mapping updates following trades/roster moves

---

## Key Technical Decisions

### Why Scikit-learn?

- Fast iteration and experimentation during model development
- Robust cross-validation and hyperparameter tuning utilities
- Accessible linear regression coefficients for inspecting model relationships
- Sufficient for linear and tree-based regression tasks

### Data Quality & Error Handling

- Filters non-playing entries and repeated table headers
- Standardizes dates and converts statistical fields to numeric types
- Removes zero-minute games before calculating per-minute metrics
- Keeps feature calculations visible within the analytical notebook

### Player Mapping as a Separate Layer

- Decouples identity/reference data from analytical logic
- Allows reference data to be refreshed separately from the modeling notebook
- Supports data collection, feature preparation, and analytical reporting
- Reduces brittle joins across disparate data sources

---

## Business Context & Applications

This system demonstrates practical quantitative analysis applied to real-world markets:

- **Statistical Modeling**: Regression and simulation connect historical performance to modeled outcomes
- **Analytical Efficiency**: Batch execution supports consistent comparisons across players
- **Domain Context**: Basketball-specific features connect model inputs to player performance and game conditions
- **Decision Support**: Probability summaries and visual reporting make the analysis easier to interpret

### Extensions & Future Work

- Incorporate in-game context (score differential, quarter) for live prop adjustments
- Expand to additional prop markets (assists, points, player combinations)
- Integrate betting exchange API for real-time line monitoring
- Implement Bayesian hierarchical models for enhanced uncertainty quantification

---

## Tools & Dependencies

| Component | Purpose |
|---|---|
| **Python 3.8+** | Data preparation, statistical analysis, and modeling |
| **Scikit-learn** | Statistical modeling and validation |
| **Pandas** | Data manipulation and transformation |
| **NumPy** | Numerical computation and array operations |
| **Web Scraping Libraries** | Historical game-log and player-reference collection |
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

**Lou Ledesma** | Full-Stack Data Analyst | Full-Stack Data Scientist | [LinkedIn](https://www.linkedin.com/in/lou-ledesma/)

Focused on advanced analytics, predictive modeling, and translating complex data into useful insights. This project brings together domain-specific feature engineering, regression analysis, Monte Carlo simulation, and analytical storytelling.

---

## Disclaimers

1. This project is for educational and analytical purposes. Sports betting involves financial risk. Users should conduct their own analysis and adhere to applicable gambling regulations in their jurisdiction.
2. Simulation outputs depend on modeling assumptions and should not be interpreted as validated betting probabilities or guaranteed outcomes.

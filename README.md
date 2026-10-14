# 🏀 NBA Rebounding Prop Valuation & Edge Detection System

**Advanced Analytics | Predictive Modeling | Monte Carlo Simulation**

How can player history, rebounding efficiency, and game context help us evaluate an NBA rebounding prop?

This project explores that question through Python, linear regression, and simulation. It connects historical game logs to engineered features, simulated rebounding outcomes, and probability estimates across different prop lines.

I built it to bring the analysis together: understand the data, model the relationships, explore a range of possible outcomes, and make the results easier to review.

---

## 📖 About the Project

A single prediction only tells part of the story. Minutes played, rebounding opportunities, and game pace can all vary, so this workflow looks beyond a point estimate to explore how simulated inputs affect predicted rebounds.

The notebooks combine:

- **Feature engineering:** Turn player and team game logs into measures of rebounding efficiency, opportunities, and pace.
- **Predictive modeling:** Fit a scikit-learn linear regression model to historical player performance.
- **Simulation:** Generate 250,000 scenarios using normally distributed feature draws and pass them through the model.
- **Probability analysis:** Estimate the share of simulated predictions above or below a selected prop line.
- **Analytical reporting:** Organize player summaries, prop probabilities, and game logs for further analysis in Excel and Looker Studio.

---

## 🧠 Inside the Analysis

### 1. Prepare the Historical Data

The workflow retrieves player and team game logs from Basketball Reference, removes non-playing entries and repeated headers, standardizes data types, and joins player records with team statistics by date and team.

A separate mapping notebook assembles player names, source identifiers, positions, and teams to support those lookups.

### 2. Build the Modeling Features

The regression uses minutes played, rebounds per minute, a calculated rebounding rate, offensive and defensive rebounding ratios, estimated possessions, and home/away indicators.

These features connect the analysis to basketball context: how long a player is on the floor, how frequently they collect rebounds, and how many opportunities a game may create. The notebook includes the Basketball Reference source for its possessions formula.

### 3. Fit a Regression Baseline

The main notebook uses `LinearRegression` with an 80/20 random train/test split and calculates mean squared error on the held-out rows.

This provides a baseline for exploring relationships in the historical data. It does not establish prospective forecasting accuracy on its own, particularly because some predictors are derived from the same game's rebound outcome.

### 4. Explore Simulated Outcomes

The simulation blends recent and earlier performance summaries to set feature means and uses observed standard deviations to generate normal draws. It then applies the regression model to 250,000 simulated feature combinations.

The outputs include a median predicted rebound value and the fraction of predictions above or below a user-supplied line. A separate prop map repeats the over-probability calculation across half-point lines from 1 to 20.

These are model-based scenario estimates. Their usefulness depends on the feature definitions, distribution assumptions, and validation of the underlying model.

---

## 📊 Explore the Outputs

- **[Looker Studio Dashboard](https://lookerstudio.google.com/s/vouaEROV4oA):** Explore the project's performance reporting.
- **[Sample Performance Tracking Data](Data%20Outputs/SAMPLE%20-%20Performance%20Tracking%20Data%20%28Looker%29.xlsx):** Review the sample workbook associated with dashboard reporting.
- **[Sample Prop Valuation & Probability Distribution](Data%20Outputs/SAMPLE%20-%20Prop%20Valuation%20and%20Probability%20Distribution.xlsx):** Explore the sample valuation and probability outputs.

The bulk execution notebook combines player-level results into an Excel workbook with **Details**, **Prop Spreads**, and **Player Gamelogs** tabs.

The valuation focus is on comparing modeled outcomes with a selected prop line. The published notebooks do not calculate sportsbook implied probabilities, expected value, or automated profitable-bet flags.

---

## 🛠️ Explore the Notebooks

### [1. Model Logic](Python%20Scripts/%281%29%20-%20MAIN%20-%20NBA%20Rebounding%20Valuation%20-%20Model%20Logic.ipynb)

The main analytical workflow: data preparation, feature calculations, regression fitting, Monte Carlo simulation, and player-level probability outputs. Start here to follow the modeling logic.

### [2. Bulk Upload Execution](Python%20Scripts/%282%29%20-%20Bulk%20Upload%20Execution.ipynb)

Reads a player input sheet, runs the model notebook for each entry, and combines the results into reporting workbooks.

### [3. Player URL Mapping Data](Python%20Scripts/%283%29%20-%20Player%20URL%20Mapping%20Data.ipynb)

Builds a reference workbook of player names, Basketball Reference identifiers, positions, and teams from the selected season's data.

**A note on setup:** These notebooks were part of a wider local workflow and are shared here to showcase the analytical logic. Running them requires the external reference workbook and player input sheet, compatible dependencies, and updates to local paths, season settings, and the bulk notebook's referenced filename. They are not a standalone application with a one-step setup.

---

## 🔍 Modeling Considerations & Next Steps

The most useful next step is stronger evaluation of the forecasting assumptions:

- **Prevent target leakage:** Replace same-game outcome-derived predictors with features available before tipoff.
- **Validate over time:** Use chronological holdouts or rolling backtests instead of relying on a random split.
- **Evaluate probabilities:** Check calibration against observed outcomes before interpreting simulated proportions as reliable betting probabilities.
- **Refine simulation assumptions:** Account for relationships between features and realistic bounds on minutes, ratios, and rebound predictions. The current feature draws are independent and unbounded.
- **Make comparisons reproducible:** Seed the simulations and compare results against simple historical baselines.

These are opportunities to extend the analysis, not capabilities already implemented in the notebooks.

---

## 🧰 Tools Used

- **Analysis & data preparation:** Python, pandas, NumPy
- **Predictive modeling:** scikit-learn
- **Simulation:** SciPy's normal distribution functions and NumPy
- **Data collection:** Requests, Beautiful Soup
- **Exploration & reporting:** Jupyter Notebooks, Excel, Looker Studio

---

## 🔌 Let's Connect

**Lou Ledesma | Full-Stack Data Analyst 📊 | Full-Stack Data Scientist 🧠**

I enjoy working through questions like this, from messy source data to models and results we can inspect. If you're interested in advanced analytics, sports data, or predictive modeling, I'd be happy to connect.

- [LinkedIn](https://www.linkedin.com/in/lou-ledesma/)
- [GitHub](https://github.com/lou-ledesma)

Thanks for taking a look! Feel free to explore the notebooks and leave a ⭐ if you find the project interesting.

---

## Project Note

This project is for educational and analytical purposes. Its outputs do not establish a profitable betting strategy or guarantee future outcomes.

#  FPL Live Points Predictor

An end-to-end Machine Learning pipeline that predicts upcoming Gameweek (GW+1) Fantasy Premier League performance using live REST API data, rolling statistical feature engineering, and XGBoost gradient boosting.

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![XGBoost](https://img.shields.io/badge/Model-XGBoost-green.svg)
![Pandas](https://img.shields.io/badge/Data-Pandas-150458.svg)

---

##  Project Overview
Predicting FPL player performance requires capturing underlying form rather than relying on noisy overall totals. This project ingests live match history directly from the official FPL REST API, engineers rolling performance indicators over a 3-Gameweek lag window, and predicts next-week point yields using **XGBoost Regressor**.

The pipeline automatically selects the highest-projected starting XI under a standard **4-3-3 tactical formation** and outputs a visual pitch graphic alongside CSV exports.

---

## Data & Feature Engineering
Data is fetched in real-time from official Fantasy Premier League API endpoints:
* **Bootstrap Static Endpoint:** `/api/bootstrap-static/` (Player metadata, position, price, team)
* **Element Summary Endpoint:** `/api/element-summary/{player_id}/` (Match-by-match historical performance)

### Key Features Used:
* **3-Gameweek Rolling Averages (`roll3_*`):**
  * Expected Goals ($xG$) & Expected Assists ($xA$)
  * ICT Index (Influence, Creativity, Threat)
  * Bonus Points System (BPS)
  * Minutes Played
* **Match Context:** Home/Away indicator (`was_home`) and Opponent Team ID.

---

##  Model Architecture & Evaluation
* **Algorithm:** XGBoost Regressor (`n_estimators=100`, `learning_rate=0.04`, `max_depth=4`)
* **Target Variable:** `target_next_points` (Points scored in GW+1)
* **Evaluation Metric:** Mean Absolute Error (MAE) evaluated on unseen chronological rounds.

---

## Repository Structure

```text
fpl-live-predictor/
├── fpl_live_predictor.ipynb  # Main Jupyter Notebook
├── fpl_xgboost_model.pkl     # Saved trained XGBoost model weights
├── next_gw_predictions.csv   # Model projections for the upcoming Gameweek
├── requirements.txt          # Dependencies & environment libraries
└── README.md                 # Project documentation
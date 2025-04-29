# 🏀 Fantasy Basketball Roster Optimization

A data-driven approach to mastering Yahoo 9-category fantasy basketball leagues through statistical analysis, machine learning, and optimization modeling. This project forecasts future performance, ranks players using z-scores, and generates optimal rosters to maximize competitive edge in a 12-team league.

---

## 🧠 Background

A Yahoo Fantasy 9-category league evaluates players across the following statistical categories:

1. Points (PTS)  
2. Rebounds (REB)  
3. Assists (AST)  
4. Steals (STL)  
5. Blocks (BLK)  
6. Field Goal % (FG%)  
7. Free Throw % (FT%)  
8. Three-Pointers Made (3P)  
9. Turnovers (TOV — lower is better)

Weekly matchups are won by outperforming your opponent in as many of these categories as possible. The ultimate goal is to optimize your roster to win the most matchups throughout the season and secure a playoff spot.

---

## 🎯 Project Objectives

### 1. **Identify the Most Correlated Categories**
- Analyzed season statistics (2023–2024) using correlation matrices.
- Analyzed historical Yahoo league outcomes to find the five categories most associated with winning.

### 2. **Forecast Future Performance**
- Collected player stats from 2013–2024.
- Applied LSTM time-series models to forecast five key categories and games played (PTS, REB, AST, 3P, STL, GP).

### 3. **Roster Optimization**
- Developed a Gurobi optimization model to construct a 13-man roster.
- Objective: Maximize the sum of forecasted z-scores under positional and round constraints.

---

## ⚙️ Project Structure

### `category_correlation_analysis.py`
- Preprocessing functions: `read_csv()`, `dataframe_cleaning()`, `combining_dataframes()`
- Analysis functions:
  - `correlation_between_categories()`: Identifies the most statistically correlated 5-category set.
  - `yahoo_league_analysis()`: Determines categories with the highest win correlations.
  - `top_200_default()`, `top_200_correlated()`: Produces z-score-based player rankings.
  - `injury_analysis()`: Adjusts for average games played.
  - `categorical_weights()`: Analyzes scarcity and distribution of categories.
  - `MLR()`, `pca_weights()`: Computes category weights using regression and PCA.
  - `writing_to_csv()`: Outputs rankings under various weighting schemes.

### `future_performance_prediction.py`
- `forecasting()`: Predicts player stats (PTS, REB, AST, STL, 3P, GP) using LSTM models.

### `roster_optimization.py`
- `predicted_zscores()`: Converts predicted stats into weighted z-scores.
- `optimal_roster()`: Selects an optimal 13-player team using constraints.

---

## 📊 Results

### ✅ Most Correlated Categories
**PTS, REB, AST, 3P, STL**  
- Highest intra-category correlation (5.887)
- Most strongly correlated with wins (28 weekly wins in Yahoo league data)

### 📉 Categorical Scarcity Weights
| Method         | PTS   | REB   | AST   | 3P    | STL   |
|----------------|-------|-------|-------|-------|-------|
| Yahoo Scarcity | 1.38  | 1.30  | 1.41  | 1.25  | 1.16  |
| NBA Distribution | 1.00 | 1.50  | 1.50  | 1.25  | 1.25  |
| MLR Weights    | 0.0015| 0.0026| -0.0003| 0.0122| 0.0230|
| PCA Weights    | 0.9713| 0.0675| 0.2146| 0.0760| 0.0146|

### 📈 Forecasted Player Stats
- Output in: `Forecasted_Player_Stats.csv`

### 🏆 Top 200 Player Rankings
Available in four methods for 2023–2024 and 2024–2025:
- `Yahoo.csv`, `NBA.csv`, `MLR.csv`, `PCA.csv`
- `Predicted_Yahoo.csv`, `Predicted_NBA.csv`, `Predicted_MLR.csv`, `Predicted_PCA.csv`

### 🔢 Optimized Rosters (13 players each)
Example from `Predicted_Yahoo.csv`:
> Tyrese Maxey, Giannis Antetokounmpo, Nikola Vucevic, Scottie Barnes, Alperen Sengun, Ja Morant, Jordan Clarkson, Jimmy Butler, Jaden Ivey, Jusuf Nurkic, Cameron Johnson, Javonte Green, Cole Anthony.

(See README for full lists across methods.)

---

## 🔬 Next Steps

- **Improve LSTM Models**: Fine-tune hyperparameters and data windows.
- **Explore New Category Weighting Techniques**: Beyond PCA and MLR.
- **Z-Score Alternatives**: Experiment with percentile rankings, Bayesian adjustments.

---

## 📚 References & Inspiration

Sources include:  
- [Hashtag Basketball](https://hashtagbasketball.com)  
- [Fantasy Helper](https://fhelper.azurewebsites.net/)  
- [NBA Player Prediction GitHub Projects](https://github.com/topics/nba-player-prediction)  
- [Advanced Fantasy Basketball Theory on Reddit](https://www.reddit.com/r/fantasybball/)  
- [Papers & Tutorials on PCA, Z-scores, and Category Scarcity](https://arxiv.org/pdf/2307.02188)

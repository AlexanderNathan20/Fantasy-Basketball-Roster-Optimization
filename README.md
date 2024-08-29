# Fantasy Basketball Roster Optimization

## Background

## Objectives
1. Perform Data Analysis on Most Correlated Categories '\n'
Season Statistics Analysis: Performed a correlation matrix analysis on player statistics from the 2023-2024 season for the 9 categories (PTS, REB, AST, 3P, STL, BLK, FG%, FT%, TOV) to determine which five categories were the most highly correlated. '\n'
Yahoo League analysis: Performed a private league analysis from the 2023-2024 season for the 9 categories (PTS, REB, AST, 3P, STL, BLK, FG%, FT%, TOV) to determine which five categories resulted in the most wins.

2. Leverage Player Statistics to Forecast Future Performance '\n'
Data Collection: Gathered player statistics from the 2013-2014 season up until the 2023-2024 season for the five categories and games played (PTS, REB, AST, 3P, STL, GP). '\n'
Modelling: Used time-series inspired LSTM model to predict player statistics from the 2024-2025 season for the five categories and games played (PTS, REB, AST, 3P, STL, GP). '\n'
Player Ranking: Developed a player ranking list based on player z-scores for the five categories and games played (PTS, REB, AST, 3P, STL, GP).

3. Optimize Roster Based on the First Two Objectives '\n'
Optimization Model: Used Gurobi inspired model to formulate a 13-man roster with the objective of maximizing the summation of player z-scores subject to positional constraints.

## Project Steps

## Challenges Faced

## Results

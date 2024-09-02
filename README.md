# Fantasy Basketball Roster Optimization

## Background

### Overview

A Yahoo Fantasy 9-category 12-man league is a type of fantasy basketball league where you and 11 other people manage virtual teams of real NBA players. The league is called "9-category" because your team's success is measured by nine different statistical categories. Here's a breakdown to help you understand it better:

### League Setup

Draft: At the start of the season, there's a draft where you take turns picking players from the NBA to fill your roster. This is like selecting your own dream team.

### 9 Categories

Points: The total number of points your players score in games.

Rebounds: How many rebounds (when a player retrieves the ball after a missed shot) your players get.

Assists: The number of assists (when a player passes the ball to a teammate in a way that leads to a score) your players make.

Steals: How many times your players take the ball away from an opponent.

Blocks: The number of times your players stop an opponent's shot from going into the basket.

Field Goal Percentage: The percentage of shots your players make from the field (how efficient they are at scoring).

Free Throw Percentage: The percentage of free throws your players make (how well they shoot free throws).

Three-Pointers Made: The total number of successful three-point shots your players make.

Turnovers: The number of times your players lose possession of the ball to the other team (you want fewer of these).

### How It Works

Weekly Matchups: Each week, your team is matched up against another team in the league. Your goal is to win in as many of the nine categories as possible.

Winning Categories: For example, if your team scores more points than your opponent's team that week, you win the "Points" category. If your players have fewer turnovers, you win that category too.

### Scoring: The more categories you win in a week, the better your overall record becomes.

Season Goals

Playoffs: The league usually lasts for the regular NBA season, with the best-performing teams making it to the playoffs at the end.

## Objectives

### Perform Data Analysis on Most Correlated Categories
Season Statistics Analysis: Performed a correlation matrix analysis on player statistics from the 2023-2024 season for the 9 categories (PTS, REB, AST, 3P, STL, BLK, FG%, FT%, TOV) to determine which five categories were the most highly correlated.

Yahoo League analysis: Performed a private league analysis from the 2023-2024 season for the 9 categories (PTS, REB, AST, 3P, STL, BLK, FG%, FT%, TOV) to determine which five categories resulted in the most wins.

### Leverage Player Statistics to Forecast Future Performance
Data Collection: Gathered player statistics from the 2013-2014 season up until the 2023-2024 season for the five categories and games played (PTS, REB, AST, 3P, STL, GP).

Modelling: Used time-series inspired LSTM model to predict player statistics from the 2024-2025 season for the five categories and games played (PTS, REB, AST, 3P, STL, GP).

Player Ranking: Developed a player ranking list based on player z-scores for the five categories and games played (PTS, REB, AST, 3P, STL, GP).

### Optimize Roster Based on the First Two Objectives
Optimization Model: Used Gurobi inspired model to formulate a 13-man roster with the objective of maximizing the summation of player z-scores subject to positional constraints.

## Project Steps

### Overview
The project consists of three main steps, each represented by a different script that specifically addresses each of the three objectives.

### category_correlation_analysis
read_csv(): This function reads, from four csv files, player stats from the past three seasons and a list of yahoo player names and their corresponding positions, and saves them as dataframes.

dataframe_cleaning(): This functions drops duplicate rows, drops unused columns, and converts dataframe columns entries to its appropriate data types.

combining_dataframes(): This functions renames columns, fixed player name discrepancies between dataframes, and merges the dataframe into a single dataframe.

correlation_between_categories(): This function creates a correlation matrix based on the 2023-2024 season between the nine categories, and creates all possible five category combinations along with a total correlation value. The idea behind this is that the higher the total correlation value, the more correlated those five categories are. The five category combination with the highest total correlation value is Steals, Assists, Rebounds, Points, and Three-Points.

<img width="652" alt="Screenshot 2024-09-01 at 19 46 57" src="https://github.com/user-attachments/assets/debd190b-8325-4a0b-aab0-9a6287fcab29">

Above is the correlation matrix that I analyzed. The values range from -1 to 1 with 1 representing the strongest correlation and -1 representing the weakest correlation. 

Note: Turnovers-to-category correlation values are subtracted from the total as the lower the turnovers, the better. 

yahoo_league_analysis(): This function analyses the most common combination of exactly five categories that led to a win per week for every player in the Yahoo league. The idea behind this is to identify which combinaton of exactly five categories is most highly correlated with a win. The five category combination with the most wins is Steals, Assists, Rebounds, Points, and Three-Points.

top_200_default(): This function converts the nine categories into z-scores (normal distribution) based on the 2023-2024 season and sums them up for a total player z-score. This mimics real-world basketball 9-category ranking websites where they use z-scores to rank players. Only the top 200 players are selected as usually only around 156 players realistically get rostered, with the remaining 44 players on the waiver wire.

Note: A weight of 0.25 is placed on turnovers in order to minimize its negative impact.

injury_analysis(): This function finds the average number of games played for each player over the past three seasons. Since injuries are a major part of the fantasy landscape as well, we account for injury history of players to determine what percentage of games they'll actually contribute in throughout the season. 

top_200_correlated(): This function converts the five categories (combination of five most highly correlated categories) into z-scores (normal distribution) based on the top 200 players already selected and sums them up for a total player z-score. This is done to rank the players based on the five category combination within the 200 players that realistically get rostered.

categorical_weights(): This function analyses scarcity per category per round, analyses scarcity per category between rostered players an waiver wire players (first 156 and remaining 44), and analyses frequency distributions per category based on the 2023-2024 season. 

MLR(): This function performs multiple linear regression on the five categories (combination of five most highly correlated categories) with the five categories as the independent variables and the win attribute as the dependent variable, to determine weights on each of the five categories in relation to winning.

pca_weights(): This function performs principal component analysis on the five categories (combination of five most highly correlated categories) based on the top 200 players (based on nine categories), to determine weights on each of the five categories in relation to the player statistics.

writing_to_csv(): This function writes four dataframes into csv files based on the previous three functions to represent player rankings based on a formula involving z-scores. The four dataframes are based on the MLR() function analysis, the pca_weights() function analysis, the categorical_weights() round-per-round analysis, and the categorical_weights() general scarcity analysis.

Formula: (method_pts_weight * [PTS Z-Score] + method_ast_weight * [AST Z-Score] + method_trb_weight * [REB Z-Score] + method_3p_weight * [3P Z-Score] + method_stl_weight * [STL Z-Score]) * [Average Games Played]

### future_performance_prediction
forecasting(): This function applies a time-series LSTM model on the top 200 players from the previous script dating back from the 2013-2014 season in order to forecast the five categories (combination of five most highly correlated categories) and games played for the upcoming season.

Note: Steps determined is set to 3.
Note: Players with three or less seasons played are projected with stats from their most recent season.
Note: Forecasted games played was set to the average games played from the past three seasons per player as the model was projecting over 82 games at times.

### roster_optimization
predicted_zscores(): This function converts the forecasted five categories into z-scores (normal distribution) based on the top 200 players and sums them up for a total forecasted player z-score. 

MLR(): This function performs multiple linear regression on the five categories (combination of five most highly correlated categories) with the five categories as the independent variables and the win attribute as the dependent variable, to determine weights on each of the five categories in relation to winning.

pca_weights(): This function performs principal component analysis on the five categories (combination of five most highly correlated categories) based on the forecasted top 200 players, to determine weights on each of the five categories in relation to the player statistics.

writing_to_csv(): This function writes four dataframes into csv files to represent player rankings based on a formula involving z-scores. The four dataframes are based on the MLR() function analysis, the pca_weights() function analysis, the categorical_weights() round-per-round analysis, and the categorical_weights() general scarcity analysis.

Formula: (method_pts_weight * [Forecasted PTS Z-Score] + method_ast_weight * [Forecasted AST Z-Score] + method_trb_weight * [Forecasted REB Z-Score] + method_3p_weight * [Forecasted 3P Z-Score] + method_stl_weight * [Forecasted STL Z-Score]) * [Forecasted Average Games Played]

optimal_roster(): This functions applies an optimization-based model on the top 200 players to come up with the most optimal roster of 13 players based on player round constraints(13 rounds, 12 players each round), and positional constraints. This is performed on the four dataframes within the previous function.

## Results

## Next Steps

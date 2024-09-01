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

correlation_between_categories(): This function creates a correlation matrix based on the 2023-2024 NBA season between the nine categories, and creates all possible five category combinations along with a total correlation value. The idea behind this is that the higher the total correlation value, the more correlated those five categories are.

<img width="652" alt="Screenshot 2024-09-01 at 19 46 57" src="https://github.com/user-attachments/assets/debd190b-8325-4a0b-aab0-9a6287fcab29">

Above is the correlation matrix that I analyzed. The values range from -1 to 1 with 1 representing the strongest correlation and -1 representing the weakest correlation. 

Note: turnovers-to-category correlation values are subtracted from the total as the lower the turnovers, the better. 


### future_performance_prediction


### roster_optimization


## Results

## Next Steps

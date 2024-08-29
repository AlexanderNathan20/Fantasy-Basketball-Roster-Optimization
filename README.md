# Fantasy Basketball Roster Optimization

## Background
A Yahoo Fantasy 9-category 12-man league is a type of fantasy basketball league where you and 11 other people manage virtual teams of real NBA players. The league is called "9-category" because your team's success is measured by nine different statistical categories. Here's a breakdown to help you understand it better:

### League Setup
Draft: At the start of the season, there's a draft where you take turns picking players from the NBA to fill your roster. This is like selecting your own dream team.

### 9 Categories
Points: The total number of points your players score in games.

Rebounds: How many rebounds (when a player retrieves the ball after a missed shot) your players get.

Assists: The number of assists (when a player passes the ball to a teammate in a way that leads to a score) your players make.

Steals: How many times your players take the ball away from an opponent.

Blocks: The number of times your players stop an opponent's shot from going into the basket.

Field Goal Percentage (FG%): The percentage of shots your players make from the field (how efficient they are at scoring).

Free Throw Percentage (FT%): The percentage of free throws your players make (how well they shoot free throws).

Three-Pointers Made (3PM): The total number of successful three-point shots your players make.

Turnovers: The number of times your players lose possession of the ball to the other team (you want fewer of these).

### How It Works
Weekly Matchups: Each week, your team is matched up against another team in the league. Your goal is to win in as many of the nine categories as possible.

Winning Categories: For example, if your team scores more points than your opponent's team that week, you win the "Points" category. If your players have fewer turnovers, you win that category too.

Scoring: The more categories you win in a week, the better your overall record becomes.

### Season Goals
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

## Challenges Faced

## Results

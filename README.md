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

### Most Correlated Categories Analysis
The most correlated group of five categories based on the correlation matrix is Points, Rebounds, Assists, Three-Points, and Steals with a total correlation value of 5.887.
The most correlated group of five categories that correlates to winning is Points, Rebounds, Assists, Three-Points, and Steals with a total win count of 28.

These results tell us that the recommended categories to focus on when drafting are Points, Rebounds, Asists, Three-Points, and Steals as these five categories are the most correlated to each other (meaning a player scoring a high number of points, will most likely also be contributing a high number of assists), and contribute most to winning your week (whether due to categorical scarcity which will later be explored, or due to an external factor).

### Categorical Scarcity Analysis
There are four types of categorical scarcity analysis done on the dataset. First is the rostered against waiver wire analysis, and the round-per-round analysis. Second is the league frequency distribution analysis. Third is the multiple linear regression analysis. Fourth is the principal component analysis.

The rostered against waiver wire analysis essentially compares the first 156 (13 rounds * 12 users) rostered player category averages against the last 44 waiver wire player category averages (totaling to 200 players). The round-per-round analysis compares the category averages per round of the draft (13 rounds). The results of these analysis are then combined to form the Yahoo categorical weights with Points having a weight of 1.38, Rebounds having a weight of 1.3, Assists having a weight of 1.41, Three-Points having a weight of 1.25, and Steals having a weight of 1.16.

The league frequency distribution analysis depends on the use of frequency distribution graphs per category and percentile count analysis depending on the number of bins per category. An example of the frequency distribution graph is shown below.
<img width="589" alt="Screenshot 2024-09-08 at 18 09 05" src="https://github.com/user-attachments/assets/6927dce4-fad4-4768-aa76-134cdbf70577">
The results of this analysis is then used to form the NBA categorical weights with Points having a weight of 1, Rebounds having a weight of 1.5, Assists having a weight of 1.5, Three-Points having a weight of 1.25, and Steals having a weight of 1.25.

The multiple linear regression analysis models the five categories as independent variables and the win count as the dependent variable, estimating the best fit line on the graph for each category. The results of this analysis is then used to form the MLR categoriccal weights with Points having a weight of 0.00152018, Rebounds having a weight of 0.00257365, Assists having a weight of -0.00033497, Three-Points having a weight of 0.01215019, and Steals having a weight of 0.02304029.

The principal component analysis treats the five categories as five different dimensions and reduces them to one dimension representing a multi-dimensional category. The results of this analysis is then used to form the MLR categoriccal weights with Points having a weight of 0.97129463, Rebounds having a weight of 0.06747427, Assists having a weight of 0.21458534, Three-Points having a weight of 0.07599245, and Steals having a weight of 0.01456861.

### Forecasted Player Stats
Refer to Forecasted_Player_Stats.csv

### Top 200 Player Ranking Lists (2023-2024 Season and 2024-2025 Season) 
Refer to Yahoo.csv
Refer to NBA.csv
Refer to MLR.csv
Refer to PCA.csv
(Based on actual 2023-2024 season stats from 2023_2024.csv)

Refer to Predicted_Yahoo.csv
Refer to Predicted_NBA.csv
Refer to Predicted_MLR.csv
Refer to Predicted_PCA.csv
(Based on predicted 2024-2025 season stats from the Forecasted_Player_Stats.csv)

### Optimized Rosters
The 13 Player Roster derived from the Predicted_Yahoo.csv: Tyrese Maxey, Giannis Antetokounmpo, Nikola Vucevic, Scottie Barnes, Alperen Sengun, Ja Morant, Jordan Clarkson, Jimmy Butler, Jaden Ivey, Jusuf Nurkic, Cameron Johnson, Javonte Green, Cole Anthony.

The 13 Player Roster derived from the Predicted_NBA.csv: Tyrese Maxey, Joel Embiid, Paolo Banchero, Kevin Durant, Jarrett Allen, Kawhi Leonard, Saddiq Bey, Caris Levert, Aaron Gordon, Mike Conley, Jerami Grant, Corey Kispert, Draymond Green.

The 13 Player Roster derived from the Predicted_MLR.csv: Tyrese Maxey, Shai-Gilgeous Alexander, Buddy Hield, Terry Rozier, Kyle Kuzma, Kyrie Irving, Royce O'Neale, Kevin Huerter, Josh Hart, Jaden McDaniels, Andrew Wiggins, Keyonte George, Nic Claxton.

The 13 Player Roster derived from the Predicted_PCA.csv: Tyrese Maxey, Tyrese Haliburton, Paolo Banchero, Franz Wagner, Jalen Williams, Spencer Dinwiddie, Kristaps Porzingis, Derrick White, Dennis Schroder, Keyonte George, Malik Beasley, Shaedon Sharpe, Herbert Jones.

## Next Steps

### Time-Series Model
Changes on the time-series model itself could lead to improved predicted stats depending on number of factors, training and testing splits, etc.

### Categorical Weights
There may be newer and or more advanced mathemical analysis techniques besides PCA or MLR that could lead to more accurate categorical weights.

### Z-Score Rescaling
There may be more advanced mathemical resampling and or studies to improve or even substitute z-scores as the standard measurement of evaluating players.

## Sources
1. https://github.com/Jayplect/nba-player-points-prediction
2. https://github.com/alexmuhr/NBA_Player_Predictions
3. https://courses.cs.washington.edu/courses/cse547/23wi/old_projects/23wi/NBA_Performance.pdf
4. https://fhelper.azurewebsites.net/
5. https://yfpy.uberfastman.com/
6. https://hashtagbasketball.com/fantasy-basketball-rankings
7. https://medium.com/nerd-for-tech/statistical-categories-scarcity-level-scarcity-impact-88ced85a3222
8. https://sports.yahoo.com/nba-playoffs-nuggets-stun-timberwolves-with-jamal-murray-prayer-tie-series-reclaim-home-court-advantage-023021545.html
9. https://www.espn.com/fantasy/basketball/fba/story?page=hardcorenba120126
10. https://sports.yahoo.com/fantasy-basketball-complete-breakdown-and-guide-to-playing-in-category-leagues-005213840.html#:~:text=Categorical%20scarcity&text=All%20of%20the%20non%2Dpoint,rarely%20are%20high%2Dimpact%20passers.
11. https://www.youtube.com/watch?v=6v9d_HvvBxo
12. https://basketball.razzball.com/an-early-look-at-statistical-scarcity
13. https://medium.com/@gioraomer/fantasy-nba-player-valuation-f66da694ab28#:~:text=Z%2DScores%20are%20the%20standard,the%20average%20of%20a%20dataset.
14. https://docs.google.com/spreadsheets/d/1E4xFUo9-z0j9BHLXD5cIrnb0hHiDKtLIvFE8sgTIfgE/edit#gid=409417588
15. https://www.reddit.com/r/fantasybball/comments/16shwc9/introducing_caruso_metric_a_gamechanger_in_nba/
16. https://github.com/zer2/Fantasy-Basketball--in-progress-/blob/main/readme.md
17. https://www.rotowire.com/basketball/article/nba-auction-strategy-part-2-21393
18. https://www.reddit.com/r/fantasybball/comments/krqvmn/building_an_alternative_valuation_model_why/
19. https://docs.google.com/spreadsheets/d/1tvktzLmeSQsvNMrBZdzyTVj1qZdVZiCxOShkwH2BBkY/edit#gid=1558515508
20. https://georgeberry.substack.com/p/pseudo-z-scores-for-basketball-counting
21. https://arxiv.org/pdf/2307.02188
22. https://basketbawful.blogspot.com/2010/03/fantasy-basketball-stats-standard.html
23. https://www.reddit.com/r/fantasybball/comments/57uo5z/simple_zscore_derived_rankings_oc/
24. https://stats.stackexchange.com/questions/348192/combining-z-scores-by-weighted-average-sanity-check-please
25. https://www.reddit.com/r/fantasybball/comments/1285d20/the_10_layers_of_9_cat_h2h_playoffs_manager/
26. https://rpubs.com/HassanOUKHOUYA/NBA_Player_Performance_Analysis_PCA_Hierarchical_Clustering_and_K-Means_Clustering
27. https://medium.com/@bball-fantasy-talks/drafting-in-h2h-fantasy-basketball-player-risk-player-values-timing-when-drafting-e2edfcc35272
28. https://www.reddit.com/r/nbadiscussion/comments/lwwumu/how_do_you_measure_the_value_of_an_assist/
29. https://fivethirtyeight.com/features/the-hidden-value-of-the-nba-steal/
30. https://theconversation.com/data-reveals-the-value-of-an-assist-in-basketball-113893
31. https://www.espn.com/fantasy/basketball/story/_/id/10522839/statistical-variance-fantasy-hoops-categories-fantasy-basketball
32. https://www.stata.com/
33. https://learning.oreilly.com/library/view/statistics-slam-dunk/9781633438682/OEBPS/Text/ch19.htm#sigil_toc_id_249
34. https://apbr.org/metrics/viewtopic.php?t=9848

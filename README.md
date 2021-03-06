# ETM
## Basketball Metric Development

This README file describes the data and the code used to develop the end-of-game tactics metric (ETM). This project started in MATLAB for a class project, where much of the data cleansing and manipulation took place. Since then, I have converted the project to R and have only been working with the cleaner data. 

### Win probability model

`NBA_WPM.R` builds a logistic regression model that gives the win probability of a team in the last three minutes of an NBA team given possession, time remaining, score differential, and the Las Vegas point spread. This code imports play-by-play data files `situationxxyy.mat`, which contains the matrix `newest.situation` and is a cleansed version of the raw play-by-play text data. The `xx` and `yy` part of the `.mat` file name represent the last two digits of the year for the NBA season (i.e. 0506 is the play-by-play data from the 2005-2006 season). This win probability serves as the foundation of ETM. 

### ETM Development

`simulate_season.R` runs a possession through the win probability model from `NBA_WPM.R`, which is `nbawp.RData`. This code finds the optimal decision a team can make during a possession. For an offensive team, these decisions include whether to shoot a two-point or three-point field goal. For a defensive team, these decisions included whether to intentionally foul or not. ETM is the difference between the win probability of the optimal decision and the win probability of the actual decision a team makes. 

In its current state, `simulate_season.R` is set to run through all qualified possessions from the 2015-2016 NBA season in `PlayByPlay3.csv`. A possession qualifies if it occurs in the last three minutes of a game that ended with a score differential of five points or fewer. Periods ending with a score differential of 0 are included. The team statistics for the 2015-2016 NBA teams are in `leagues_NBA_2016_team.csv`. To account for the affect of the shot clock time remaining on shooting percentage, coefficients of a quadratic function fit to this variable are applied to shooting percentages as shooting weights. These coefficients are in `shootingcoeff.mat`. 

### ETM Analysis

`simulate_season.R` outputs `newPBP_1.csv` which contains possession information along with ETM values. `newGame.R` marches through each game in `newPBP_1.csv` and generates aggregate game statistics related to ETM stored in `newGBG_1.csv`. 

`ETM.R` then uses both `newPBP_1.csv` and `newGBG_1.csv` to generate ETM season summary statistics for each team. 

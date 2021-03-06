---
layout: post
categories: jekyll
title: 'Predicting Wins in the NBA'
author: "Jason Spector"
htmlwidgets: true
always_allow_html: true
excerpt: "Can we accurately predict wins before an NBA schedule release?"
image: "assets/images/G-ant.jpg"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
---

\*\* updates will be made again after the 2021 draft and free agency

There are currently many season and game prediction models for NBA games
but due to teams resting star players most of the models rely on
schedules and match-ups. Fivethirtyeight’s model for example, predicts
how many minutes a player will play in a game against a certain
opponent. By predicting how many minutes a player will play, you can
look at the impact of that player on a given game. However, free agency
and the draft happen before the schedule is released. Here I look at
building a model that predicts wins and playoff probability (with and
without a play-in) before a schedule release to mimic the building of a
team as a GM would.

Before starting let’s make some assumptions clear. I am predicting in a
standard 82 game season, without a bubble, every team is available to
play, etc. a standard year per say. Changes could be made, such as a 72
game season, if needed. We are predicting for the time period after the
NBA draft and early free agency to give an idea of how a team might
perform given their players without a schedule. Simply adjusting the
roster manually can show how a team might perform before free agency. A
short explanation of this model to a coach would convey that this model
predicts the probability that a team wins any given game based on the
team’s player make-up, then it simulates 100,000 seasons and counts how
often each team makes the playoffs out of 100,000. This results in the
probability any given team makes the playoffs.

For the more in depth explanation: I came up with what I am calling the
team genetics model. This idea was based on the way a GM might think
about assembling a team. In general, the free agency period is before
the schedule release causing a GM to think about assembling the best
team regardless of the schedule. The data collection process started by
scraping each team’s beginning of the season roster and joining each
player’s previous season basic and advanced per game data. Then the top
ten players by descending minutes played per game (creating a somewhat
ten-man rotation) were aggregated at a team level. However, the
aggregation wasn’t simply summing up all the data, it was structured how
a GM might build a team. For example, I created a column for the top
scorer’s PPG, the second top scorer’s PPG, and even the third; creating
three columns to gauge the level of superstars on a team. Below is a
quick list of the variables on a per-game basis and the thought behind
them:

  - Max PPG: Top superstar scorer

  - 2nd Max PPG: How far back is the second superstar?

  - 3rd Max PPG: Is there a third star?

  - Sum PPG: Ten rotation players PPG

  - FT% AVg: How good is the team at shooting FTs?

  - Sum TOV: How many times is the team turning the ball over a game?

  - Max TRB: Does the team have a dominant rebounder?

  - Sum TRB: Ten rotation players TRB

  - Max AST: How many ASTs is the main distributor achieving?

  - Sum AST: Is there good ball movement or more of an Iso team?

  - Max BLK: Does the team have a rim protector?

  - Sum BLK: Ten rotation players BLK

  - Sum PF: Does the team foul more than others?

  - Average eFG: Distinguishes shooting ability

  - Max 3PAr: Does the team have a three-point specialist?

  - Average 3PAr: How often does the team shoot threes?

  - Average FTr: How often does the team get to the line?

  - Average TOV Percentage: Helps with pacing compared to total TOVs

  - Average USG: Iso team or balanced?

  - Average OBPM: Is the team generally outscoring the opponent
    offensively?

  - Max DBPM: Is there a defensive stopper?

  - Average DBPM: General measure of defensive ability
    
    This process was completed for ten seasons (2010-2020) where the
    final training data set contained each team’s 10-man rotation with
    the players aggregated data from the previous season (which I am
    calling the team’s genetics), and the win total that came from that
    season. The exception was the 2020 season where the roster from
    before the season started and the aggregated 2019 player data was
    used, but not win total because this was the end prediction value.
    The data set was randomly split into a training and development set
    with a 80-20 split and a random forest which forced at least 20
    predictors in a tree was performed. The final model had an mean
    absolute error (MAE) of 8.7%, which is about 7 games, and not too
    bad. 100,000 simulated seasons were ran with each team having 82
    Bernoulli trials where the chance of success equaled the predicted
    random forest win probability for that given team. The simulations
    then recorded the playoff results for each season based on wins
    (splitting by east and west of course). Tie breakers were decided at
    random as there weren’t technically head to head match-ups. The
    final predicted number of wins was the average number of wins for a
    given team across the 100,000 simulations while the playoff
    probability was the number of times the team was the 8 seed or
    higher divided by 100,000.
    
    Lastly, I needed to calculate the difference when using a play-in
    tournament where the 8 and 9 seed are within two games of each
    other. The play-in tournament is similar to a 2-game playoff series
    where it is assumed that the 8 seed would have home court for both
    games. Thus, I decided to find each team’s win percentage by
    sampling playoff games where the opponents differed by two games in
    the regular season. In total, there were 60 playoff games where the
    two teams differed by two games since 2000. The better seed was
    decided based on the new rules where the superior record achieves
    the better seed, not the old rules with divisional titles. Of the 60
    series only 8 had the worse seed winning the first two games (about
    13%). The other 87% of the time the better seed won one of the first
    two games, although in this play-in scenario only 16 (26%) would
    have even gone to a second game. This allowed for the following
    equations for the probabilities with a play-in:

P(Playoff given No Play-in) = number of times being top 8 seed/number of simulated seasons 

P(Playoff given Play-in) = P(top 7 seed) + P(8 seed)P(Win Playin as 8 seed) + P(9 seed)P(Win Playin as 9 seed) 

P(Playoff given Play-in) = P(top 7 seed) + P(8 seed)(52/60) + P(9 seed)(8/60)

Lastly, below is the table output for my final win projections and
probabilities.

|    | team\_name             | conference | predicted\_wins | playoff\_probability | playoff\_playin\_probability |
| :--------------------- | :--------- | --------------: | -------------------: | ---------------------------: |
| Milwaukee Bucks        | East       |              55 |                 0.99 |                         0.99 |
| Indiana Pacers         | East       |              53 |                 0.99 |                         0.99 |
| Philadelphia 76ers     | East       |              48 |                 0.96 |                         0.95 |
| Toronto Raptors        | East       |              47 |                 0.93 |                         0.93 |
| Brooklyn Nets          | East       |              45 |                 0.88 |                         0.87 |
| Boston Celtics         | East       |              44 |                 0.80 |                         0.79 |
| Miami Heat             | East       |              43 |                 0.76 |                         0.76 |
| Orlando Magic          | East       |              43 |                 0.77 |                         0.77 |
| Chicago Bulls          | East       |              40 |                 0.47 |                         0.48 |
| Atlanta Hawks          | East       |              36 |                 0.20 |                         0.20 |
| New York Knicks        | East       |              34 |                 0.10 |                         0.10 |
| Washington Wizards     | East       |              34 |                 0.09 |                         0.09 |
| Detroit Pistons        | East       |              32 |                 0.04 |                         0.04 |
| Cleveland Cavaliers    | East       |              30 |                 0.02 |                         0.02 |
| Charlotte Hornets      | East       |              25 |                 0.00 |                         0.00 |
| Denver Nuggets         | West       |              54 |                 0.99 |                         0.99 |
| Los Angeles Lakers     | West       |              53 |                 0.99 |                         0.99 |
| Utah Jazz              | West       |              51 |                 0.99 |                         0.98 |
| Houston Rockets        | West       |              49 |                 0.97 |                         0.97 |
| Los Angeles Clippers   | West       |              49 |                 0.96 |                         0.96 |
| Dallas Mavericks       | West       |              45 |                 0.85 |                         0.85 |
| Memphis Grizzlies      | West       |              42 |                 0.58 |                         0.58 |
| Oklahoma City Thunder  | West       |              42 |                 0.63 |                         0.63 |
| Sacramento Kings       | West       |              40 |                 0.44 |                         0.44 |
| Portland Trail Blazers | West       |              37 |                 0.17 |                         0.18 |
| San Antonio Spurs      | West       |              37 |                 0.18 |                         0.18 |
| Minnesota Timberwolves | West       |              35 |                 0.11 |                         0.11 |
| New Orleans Pelicans   | West       |              35 |                 0.08 |                         0.08 |
| Phoenix Suns           | West       |              32 |                 0.03 |                         0.03 |
| Golden State Warriors  | West       |              30 |                 0.01 |                         0.01 |

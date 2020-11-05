---
title: "Predicting Wins in the NBA"
author: "Jason Spector"
layout: post
output:
  md_document:
    variant: gfm
    preserve_yaml: yes
  html_document:
    df_print: paged
htmlwidgets: yes
always_allow_html: yes
excerpt: Can we accurately predict wins before an NBA Schedule Release
image: /images/ad.jpg
categories: jekyll
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

\[ P(Playoff | No \thinspace Play-in) =  \frac{number \thinspace of \thinspace times \thinspace being \thinspace top \thinspace 8 \thinspace seed}{number \thinspace of \thinspace simulated \thinspace seasons} \]

\[ P(Playoff | Play-in) = P(top \thinspace 7 \thinspace seed) + P(8 \thinspace seed)*(Win \thinspace Playin \thinspace as \thinspace 8 \thinspace seed) + P(9 \thinspace seed)*(Win \thinspace Playin \thinspace as \thinspace 9 \thinspace seed)\]

\[ P(Playoff | Play-in) = P(top \thinspace 7 \thinspace seed) + P(8 \thinspace seed)*(\frac{52}{60}) + P(9 \thinspace seed)*(\frac{8}{60}) \]

Lastly, below is the table output for my final win projections and
probabilities.

# 

<!--html_preserve-->

<div class="standings">

<div class="title">

<h2>

Playoff Predictions

</h2>

</div>

<div id="htmlwidget-e15e55acec7e9b241e03" class="reactable html-widget" style="width:auto;height:auto;">

</div>

<script type="application/json" data-for="htmlwidget-e15e55acec7e9b241e03">{"x":{"tag":{"name":"Reactable","attribs":{"data":{"team_name":["Dallas Mavericks","Denver Nuggets","Golden State Warriors","Houston Rockets","Los Angeles Clippers","Los Angeles Lakers","Memphis Grizzlies","Minnesota Timberwolves","New Orleans Pelicans","Oklahoma City Thunder","Phoenix Suns","Portland Trail Blazers","Sacramento Kings","San Antonio Spurs","Utah Jazz","Atlanta Hawks","Boston Celtics","Brooklyn Nets","Chicago Bulls","Charlotte Hornets","Cleveland Cavaliers","Detroit Pistons","Indiana Pacers","Miami Heat","Milwaukee Bucks","New York Knicks","Orlando Magic","Philadelphia 76ers","Toronto Raptors","Washington Wizards"],"conference":["West","West","West","West","West","West","West","West","West","West","West","West","West","West","West","East","East","East","East","East","East","East","East","East","East","East","East","East","East","East"],"predicted_wins":[45,54,30,49,49,53,42,35,35,42,32,37,40,37,51,36,44,45,40,25,30,32,53,43,55,34,43,48,47,34],"playoff_probability":[0.85,1,0.01,0.97,0.96,1,0.58,0.11,0.08,0.63,0.03,0.17,0.44,0.18,0.99,0.2,0.8,0.88,0.47,0,0.02,0.04,1,0.76,1,0.1,0.77,0.96,0.93,0.09],"playoff_playin_probability":[0.85,1,0.01,0.97,0.96,1,0.58,0.11,0.08,0.63,0.03,0.18,0.44,0.18,0.98,0.2,0.79,0.87,0.48,0,0.02,0.04,1,0.76,1,0.1,0.77,0.95,0.93,0.09]},"columns":[{"accessor":"team_name","name":"Team","type":"factor","className":"cell","headerClassName":"header","defaultSortDesc":false,"maxWidth":200,"headerStyle":{"fontWeight":700}},{"accessor":"conference","name":"Conference","type":"factor","className":"cell group","headerClassName":"header","defaultSortDesc":false,"maxWidth":200,"align":"center","headerStyle":{"fontWeight":700}},{"accessor":"predicted_wins","name":"Predicted Wins","type":"numeric","className":"cell number border-left","headerClassName":"header","maxWidth":150,"align":"center"},{"accessor":"playoff_probability","name":"Playoff Probability","type":"numeric","className":"cell number border-left","headerClassName":"header","cell":[" 85%",">99%","  1%"," 97%"," 96%",">99%"," 58%"," 11%","  8%"," 63%","  3%"," 17%"," 44%"," 18%"," 99%"," 20%"," 80%"," 88%"," 47%"," <1%","  2%","  4%",">99%"," 76%",">99%"," 10%"," 77%"," 96%"," 93%","  9%"],"maxWidth":200,"style":[{"color":"#111","background":"#55BBAB"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#FCFEF7"},{"color":"#111","background":"#3BB2AB"},{"color":"#111","background":"#3DB3AB"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#8FD1AB"},{"color":"#111","background":"#E7F7CA"},{"color":"#111","background":"#EEF9CF"},{"color":"#111","background":"#84CDAB"},{"color":"#111","background":"#F8FDE9"},{"color":"#111","background":"#DAF2C0"},{"color":"#111","background":"#A8DCAE"},{"color":"#111","background":"#D8F1BF"},{"color":"#111","background":"#37B0AB"},{"color":"#111","background":"#D3F0BC"},{"color":"#111","background":"#5FC0AB"},{"color":"#111","background":"#4EB9AB"},{"color":"#111","background":"#A2DAAD"},{"color":"#aaa"},{"color":"#111","background":"#FAFDF0"},{"color":"#111","background":"#F6FCE2"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#68C3AB"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#E9F8CB"},{"color":"#111","background":"#66C2AB"},{"color":"#111","background":"#3DB3AB"},{"color":"#111","background":"#44B5AB"},{"color":"#111","background":"#EBF8CD"}]},{"accessor":"playoff_playin_probability","name":"Playoff Play-in Probability","type":"numeric","className":"cell number border-left","headerClassName":"header","cell":[" 85%",">99%","  1%"," 97%"," 96%",">99%"," 58%"," 11%","  8%"," 63%","  3%"," 18%"," 44%"," 18%"," 98%"," 20%"," 79%"," 87%"," 48%"," <1%","  2%","  4%",">99%"," 76%",">99%"," 10%"," 77%"," 95%"," 93%","  9%"],"maxWidth":200,"style":[{"color":"#111","background":"#55BBAB"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#FCFEF7"},{"color":"#111","background":"#3BB2AB"},{"color":"#111","background":"#3DB3AB"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#8FD1AB"},{"color":"#111","background":"#E7F7CA"},{"color":"#111","background":"#EEF9CF"},{"color":"#111","background":"#84CDAB"},{"color":"#111","background":"#F8FDE9"},{"color":"#111","background":"#D8F1BF"},{"color":"#111","background":"#A8DCAE"},{"color":"#111","background":"#D8F1BF"},{"color":"#111","background":"#39B1AB"},{"color":"#111","background":"#D3F0BC"},{"color":"#111","background":"#62C0AB"},{"color":"#111","background":"#50BAAB"},{"color":"#111","background":"#A1D9AD"},{"color":"#aaa"},{"color":"#111","background":"#FAFDF0"},{"color":"#111","background":"#F6FCE2"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#68C3AB"},{"color":"#111","background":"#35B0AB"},{"color":"#111","background":"#E9F8CB"},{"color":"#111","background":"#66C2AB"},{"color":"#111","background":"#3FB4AB"},{"color":"#111","background":"#44B5AB"},{"color":"#111","background":"#EBF8CD"}]}],"defaultSortDesc":true,"defaultSorted":[{"id":"conference","desc":false},{"id":"predicted_wins","desc":true}],"defaultPageSize":30,"paginationType":"numbers","showPageInfo":true,"minRows":1,"highlight":true,"bordered":true,"striped":true,"compact":true,"showSortIcon":false,"className":"standings-table","dataKey":"39b2b17eb45c711b7c18567184947717","key":"39b2b17eb45c711b7c18567184947717"},"children":[]},"class":"reactR_markup"},"evals":[],"jsHooks":[]}</script>

</div>

<!--/html_preserve-->

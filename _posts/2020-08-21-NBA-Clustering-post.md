---
layout: post
categories: jekyll
title: "Finding NBA Players Deserving of More Minutes"
author: "Jason Spector"
htmlwidgets: true
always_allow_html: true
excerpt: "Are players outplaying their role?"
image: "assets/images/mpj.jpg"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
---

![test](https://jspective.github.io/thebackpost/assets/images/mpj.jpg =1000x500)

Every basketball loving fan at some point in their life will have the
discussion of which player is better. As a child, my friends and I would
often argue over Jason Kidd or Steve Nash, D-Wade or Lebron, or Kobe or
Allen Iverson (AI). To this day, I have friends who say the 2001-2002
season AI was a better scorer than Kobe and I continue to respond with
it wasn’t as close as you think. AI scored 31.4 points per game in the
01-02 season and Kobe scored 25.2 points per a game. Six points is a
significant difference in points per game but once you take into account
that AI played 43.7 minutes per game and Kobe played 38.3 minutes,
simple math tells you that Kobe scored .66 points per minute and AI
scored .71 points per minute. That means if both played forty minute
games at their season scoring rate Kobe would have 26.4 points and AI
would have 28.4 points. A measly two-point difference. Add in the fact
that Kobe shot 46.9 percent from the floor and AI shot 39.8 percent from
the floor, the statement becomes said with a little less confidence.
While we could argue over who was a better or more efficient scorer they
are both all time greats, any team would’ve loved to have them on their
team.

Instead I focused on determining players who were underutilized in the
2018-19 season with clustering. Once identified, I analyzed how certain
underutilized players performed in the 2019-2020 season compared to the
previous season. In this experiment I solely looked at offensive ability
as defensive statistics are not very good at completely capturing
defensive ability.

## The Method

Typically, for offensive evaluation I would look at efficiency and
scoring ability using effective field goal percentage, true shooting
percentage, or more advanced metrics but kept it simple for
visualizations by only using three dimensions: points, assists, and
rebounds; the classic triple double. I kept the statistics simple in
hopes that someone without significant statistical background would have
less difficulty interpreting the results.

The data set came from basketballreference, I used player’s per game and
per 36 minute tables found here
\[<https://www.basketball-reference.com/leagues/NBA_2020_per_minute.html>\].
Now that we know the setup, here’s how my clustering unsupervised
learning works.

NOTE: Analysis up until break of 2020 NBA Break, with update to data
analysis will not match numbers for 2020 exactly.

1)  I began by looking at the per-36-minute data to search for outliers.
    Players with a small sample of minutes could have a large volume of
    scoring. For example, scoring a two-point basket in a minute would
    skew the per-36-minute numbers to score seventy-two points
    per-36-minutes. To deal with this I filtered for players whose
    minutes per game were in the top ninety percentile or players who
    played in more than ten games and played for more than two standard
    deviations below the mean number of minutes played per game. This
    controls for players who may have played large minutes in a few
    games but were injured after or players who were just barely on
    rosters.

2)  I then ran a cluster analysis of offensive ability using both
    2018-19 and 2019-2020 seasons to create the clusters. More than
    three variables can be used in creating clusters but, in this
    situation I only used three to allow for 2-D and 3-D interpretable
    graphs that coaches can use to take action. Again, I kept it simple
    and clustered using PTS per game, AST, and TRB. I standardized AST,
    PTS, and TRB per game with rescaling (min-max normalization) by
    subtracting the minimum and then dividing each value by the range.
    After I fit a hierarchical agglomerative cluster analysis (HCA)
    model to determine the number of clusters recommended by critical
    values on per\_game data basis. Note however, there was no wrong
    answer for numbers of cluster as this was an exploratory analysis.
    Once I determined the number of clusters I assigned the players to a
    cluster. I used HCA over k-means because k-means requires me to know
    the number of clusters I desire beforehand and HCA makes less
    assumptions about data shape.

3)  I applied the player’s assigned per-game cluster to the players
    per-36-minute data. By doing this I was able to analyze which
    players when remaining constant with their per game pace would be
    considered much better players compared to their counterparts when
    given the same minute's opportunity.

4)  Lastly, I analyzed if a player changed clusters year over year.

I started by determining the number of cluster to use. HCA had seven
indexes suggest two clusters and six suggest four clusters, all other
number of cluster only received three or less index votes. If you would
like the code to see the output, email me\! I fit the model with four
clusters as it was the most practical when it came to the NBA. With four
clusters I hoped to see an elite or superstar player, starters, a bench
player cluster, and reserves.

<iframe width="800" height="500" frameborder="0" scrolling="no" src="//plotly.com/~jspective/19.embed">

</iframe>

The graph above shows a trend I hoped for. The blue represents elite
offensive players, the red and green represents potentially starter or
role players, the deep green represents bench players who come in for a
few points a game, and the purple are deep reserves. The guard and
forward groups do not necessarily have to be guards or forwards but the
clusters are splitting by rebounds which are commonly higher in forwards
and centers. Immediately, I saw a major flaw in the clusters, assists
are barely taken into account when splitting the clusters. It mostly
looked like the clusters were being split by points. However, notice the
overlap? The overlap was due to rebounds, some players with less points
were in the “higher” group due to their rebound value.

However, what I was more interested in was if I should be classifying
players differently based on their per-36-minute statistics. The below
plot assigns the per game statistic cluster to a player on their
per-36-minute numbers.

<iframe width="800" height="500" frameborder="0" scrolling="no" src="//plotly.com/~jspective/21.embed">

</iframe>

A simple glance shows there are some players who on a per minute basis
were better than their per game statistics show. I do acknowledge the
large variation of reserve players; with small minutes, usually when
games have already been won or lost, they score quickly as defense
lacks. If the graph is too crowded, it is interactive, simply click on
the reserve group (or any group for that matter) to remove them. Double
click on one cluster if you would like only that cluster to be shown.
Hover over a mark to see the details of that player such as the season,
PTS, AST, and TRB.

Overall, I did find some players who were more similar than many think;
their statistics can be seen in the tables below. In 2018-19 Victor
Oladipo only averaged two more points, half an assist and 3 rebounds
more than 2018-19 Spencer Dinwiddie. But, give Dinwiddie the three extra
minutes Oladipo is playing they would be almost identical in points and
assists, yet Oladipo was an all star reserve that year. Looking at 2020
and given the minutes Dinwiddie improved on his efficiency jumping from
21.5 to 23.8 points per 36 minutes.

Also shown below is Dante Exum in 2019, a candidate to deserve more
minutes. He is classified as a bench player per game but given more
minutes could be a starter/role. Lastly, a player I believe is deserving
of more minutes is Michael Porter Jr. Playing just 14 minutes per game
in his rookie year in the 2019-2020 season he is averaging more than .5
points per minute could score close to twenty a game if given the
opportunity.

These players are just a few of the examples I picked out, because I
like those player's game, but there are many other players that have
similar situations that can be seen from the graphs.

    Per Game data

| Player\_season          | Tm  |   MP |  PTS | AST | TRB |
| :---------------------- | :-- | ---: | ---: | --: | --: |
| Spencer Dinwiddie 2019  | BRK | 28.1 | 16.8 | 4.6 | 2.4 |
| Dante Exum 2019         | UTA | 15.8 |  6.9 | 2.6 | 1.6 |
| Victor Oladipo 2019     | IND | 31.9 | 18.8 | 5.2 | 5.6 |
| Spencer Dinwiddie 2020  | BRK | 31.2 | 20.6 | 6.8 | 3.5 |
| Michael Porter Jr. 2020 | DEN | 16.4 |  9.3 | 0.8 | 4.7 |

    Per 36 Minute

| Player\_season          | Tm  |  PTS | AST |  TRB |
| :---------------------- | :-- | ---: | --: | ---: |
| Spencer Dinwiddie 2019  | BRK | 21.5 | 5.8 |  3.1 |
| Dante Exum 2019         | UTA | 15.6 | 6.0 |  3.7 |
| Victor Oladipo 2019     | IND | 21.2 | 5.8 |  6.3 |
| Spencer Dinwiddie 2020  | BRK | 23.8 | 7.8 |  4.0 |
| Michael Porter Jr. 2020 | DEN | 20.4 | 1.8 | 10.3 |

Now, would Dante Exum actually score 15.6 points if he played 36
minutes. Most likely no, it is hard to sustain scoring ability over
time. But he could possibly contribute more than a coach might initially
realize if he was given more time.

My visualizations made it difficult to see the breaks for rebounds, but
made the clusters easily digestible in 2-D. Here’s an interactive 3-D
visualization which encapsulates all variables used for clustering.

<iframe width="800" height="500" frameborder="0" scrolling="no" src="//plotly.com/~jspective/23.embed">

</iframe>

Lastly, I wanted to see which players changed clusters year over year.
By doing this I was able to easily find a list of players who have
improved year over year and could use this list to see what the reasons
were.

# Players who went from a Starter/Role player to Elite player in 2020.

| Player                  | PTS\_19 | PTS\_20 | AST\_19 | AST\_20 | TRB\_19 | TRB\_20 | clust\_19                      | clust\_20 | team\_19 | team\_20 | MP\_19 | MP\_20 |
| :---------------------- | ------: | ------: | ------: | ------: | ------: | ------: | :----------------------------- | :-------- | :------- | :------- | -----: | -----: |
| Malik Beasley           |    11.3 |    20.7 |     1.2 |     1.9 |     2.5 |     5.1 | Guard Starter/Role             | Elite     | DEN      | MIN      |   23.2 |   33.1 |
| Bojan Bogdanović        |    18.0 |    20.2 |     2.0 |     2.1 |     4.1 |     4.1 | Guard Starter/Role             | Elite     | IND      | UTA      |   31.8 |   33.1 |
| Malcolm Brogdon         |    15.6 |    16.5 |     3.2 |     7.1 |     4.5 |     4.9 | Guard Starter/Role             | Elite     | MIL      | IND      |   28.6 |   30.9 |
| Jaylen Brown            |    13.0 |    20.3 |     1.4 |     2.1 |     4.2 |     6.4 | Guard Starter/Role             | Elite     | BOS      | BOS      |   25.9 |   33.9 |
| Spencer Dinwiddie       |    16.8 |    20.6 |     4.6 |     6.8 |     2.4 |     3.5 | Guard Starter/Role             | Elite     | BRK      | BRK      |   28.1 |   31.2 |
| Evan Fournier           |    15.1 |    18.5 |     3.6 |     3.2 |     3.2 |     2.6 | Guard Starter/Role             | Elite     | ORL      | ORL      |   31.5 |   31.5 |
| Shai Gilgeous-Alexander |    10.8 |    19.0 |     3.3 |     3.3 |     2.8 |     5.9 | Guard Starter/Role             | Elite     | LAC      | OKC      |   26.5 |   34.7 |
| Tobias Harris           |    20.0 |    19.6 |     2.8 |     3.2 |     7.9 |     6.9 | Forward or Center Starter/Role | Elite     | TOT      | PHI      |   34.7 |   34.3 |
| Caris LeVert            |    13.7 |    18.7 |     3.9 |     4.4 |     3.8 |     4.2 | Guard Starter/Role             | Elite     | BRK      | BRK      |   26.6 |   29.6 |
| Kyle Lowry              |    14.2 |    19.4 |     8.7 |     7.5 |     4.8 |     5.0 | Guard Starter/Role             | Elite     | TOR      | TOR      |   34.0 |   36.2 |
| Marcus Morris           |    13.9 |    19.6 |     1.5 |     1.4 |     6.1 |     5.4 | Guard Starter/Role             | Elite     | BOS      | NYK      |   27.9 |   32.3 |
| Dennis Schröder         |    15.5 |    18.9 |     4.1 |     4.0 |     3.6 |     3.6 | Guard Starter/Role             | Elite     | OKC      | OKC      |   29.3 |   30.8 |
| Pascal Siakam           |    16.9 |    22.9 |     3.1 |     3.5 |     6.9 |     7.3 | Forward or Center Starter/Role | Elite     | TOR      | TOR      |   31.9 |   35.2 |
| Jayson Tatum            |    15.7 |    23.4 |     2.1 |     3.0 |     6.0 |     7.0 | Forward or Center Starter/Role | Elite     | BOS      | BOS      |   31.1 |   34.3 |
| Fred VanVleet           |    11.0 |    17.6 |     4.8 |     6.6 |     2.6 |     3.8 | Guard Starter/Role             | Elite     | TOR      | TOR      |   27.5 |   35.7 |
| T.J. Warren             |    18.0 |    19.8 |     1.5 |     1.5 |     4.0 |     4.2 | Guard Starter/Role             | Elite     | PHO      | IND      |   31.6 |   32.9 |

# Bench or Reserve to Elite Players:

| Player          | PTS\_19 | PTS\_20 | AST\_19 | AST\_20 | TRB\_19 | TRB\_20 | clust\_19 | clust\_20 | team\_19 | team\_20 | MP\_19 | MP\_20 |
| :-------------- | ------: | ------: | ------: | ------: | ------: | ------: | :-------- | :-------- | :------- | :------- | -----: | -----: |
| Devonte’ Graham |     4.7 |    18.2 |     2.6 |     7.5 |     1.4 |     3.4 | Bench     | Elite     | CHO      | CHO      |   14.7 |   35.1 |
| Terry Rozier    |     9.0 |    18.0 |     2.9 |     4.1 |     3.9 |     4.4 | Reserve   | Elite     | BOS      | CHO      |   22.7 |   34.3 |

Only Devonte’ Grahm of the Hornets and Norman Powell went from a reserve
or bench player to an Elite player. Jumping 14 and 8 points respectively
a game does help\!

# Bench to Role/Starter Players:

| Player             | PTS\_19 | PTS\_20 | AST\_19 | AST\_20 | TRB\_19 | TRB\_20 | clust\_19 | clust\_20          | team\_19 | team\_20 | MP\_19 | MP\_20 |
| :----------------- | ------: | ------: | ------: | ------: | ------: | ------: | :-------- | :----------------- | :------- | :------- | -----: | -----: |
| Bruce Brown        |     4.3 |     8.9 |     1.2 |     4.0 |     2.5 |     4.7 | Bench     | Guard Starter/Role | DET      | DET      |   19.6 |   28.2 |
| Troy Brown Jr.     |     4.8 |    10.4 |     1.5 |     2.6 |     2.8 |     5.6 | Bench     | Guard Starter/Role | WAS      | WAS      |   14.0 |   25.8 |
| Alec Burks         |     1.7 |    12.2 |     0.8 |     2.1 |     1.7 |     3.1 | Bench     | Guard Starter/Role | SAC      | PHI      |    9.8 |   20.2 |
| Marquese Chriss    |     4.2 |     9.3 |     0.5 |     1.9 |     3.3 |     6.2 | Bench     | Guard Starter/Role | TOT      | GSW      |   11.6 |   20.3 |
| Donte DiVincenzo   |     4.9 |     9.2 |     1.1 |     2.3 |     2.4 |     4.8 | Bench     | Guard Starter/Role | MIL      | MIL      |   15.2 |   23.0 |
| Brandon Knight     |     3.0 |    11.6 |     0.8 |     4.2 |     0.8 |     2.3 | Bench     | Guard Starter/Role | HOU      | DET      |    9.8 |   24.6 |
| Damion Lee         |     4.9 |    12.7 |     0.4 |     2.7 |     2.0 |     4.9 | Bench     | Guard Starter/Role | GSW      | GSW      |   11.7 |   29.0 |
| Ben McLemore       |     3.9 |    10.1 |     0.2 |     0.8 |     0.9 |     2.2 | Bench     | Guard Starter/Role | SAC      | HOU      |    8.3 |   22.8 |
| Jordan McRae       |     5.9 |    11.5 |     1.1 |     2.5 |     1.5 |     3.4 | Bench     | Guard Starter/Role | WAS      | TOT      |   12.3 |   21.2 |
| Duncan Robinson    |     3.3 |    13.5 |     0.3 |     1.4 |     1.3 |     3.2 | Bench     | Guard Starter/Role | MIA      | MIA      |   10.7 |   29.7 |
| Glenn Robinson III |     4.2 |    11.7 |     0.4 |     1.5 |     1.5 |     4.4 | Bench     | Guard Starter/Role | DET      | TOT      |   13.0 |   28.8 |
| Garrett Temple     |     4.7 |    10.3 |     1.4 |     2.5 |     2.5 |     3.5 | Bench     | Guard Starter/Role | LAC      | BRK      |   19.6 |   27.9 |
| Christian Wood     |     2.8 |    13.1 |     0.2 |     1.0 |     1.5 |     6.3 | Bench     | Guard Starter/Role | MIL      | DET      |    4.8 |   21.4 |

# Reserve players to Starter/Role players:

| Player              | PTS\_19 | PTS\_20 | AST\_19 | AST\_20 | TRB\_19 | TRB\_20 | clust\_19 | clust\_20                      | team\_19 | team\_20 | MP\_19 | MP\_20 |
| :------------------ | ------: | ------: | ------: | ------: | ------: | ------: | :-------- | :----------------------------- | :------- | :------- | -----: | -----: |
| OG Anunoby          |     7.0 |    10.6 |     0.7 |     1.6 |     2.9 |     5.3 | Reserve   | Guard Starter/Role             | TOR      | TOR      |   20.2 |   29.9 |
| Aron Baynes         |     5.6 |    11.5 |     1.1 |     1.6 |     4.7 |     5.6 | Reserve   | Guard Starter/Role             | BOS      | PHO      |   16.1 |   22.2 |
| Dragan Bender       |     5.0 |     9.0 |     1.2 |     2.1 |     4.0 |     5.9 | Reserve   | Guard Starter/Role             | PHO      | GSW      |   18.0 |   21.7 |
| Dāvis Bertāns       |     8.0 |    15.4 |     1.3 |     1.7 |     3.5 |     4.5 | Reserve   | Guard Starter/Role             | SAS      | WAS      |   21.5 |   29.3 |
| Miles Bridges       |     7.5 |    13.0 |     1.2 |     1.8 |     4.0 |     5.6 | Reserve   | Guard Starter/Role             | CHO      | CHO      |   21.2 |   30.7 |
 Dillon Brooks       |     7.5 |    16.2 |     0.9 |     2.1 |     1.7 |     3.3 | Reserve   | Guard Starter/Role             | MEM      | MEM      |   18.3 |   28.9 |
| Trey Burke          |     9.7 |    12.0 |     2.6 |     3.8 |     1.5 |     1.9 | Reserve   | Guard Starter/Role             | DAL      | DAL      |   17.4 |   23.9 |
| Alec Burks          |     8.8 |    12.2 |     2.0 |     2.1 |     3.7 |     3.1 | Reserve   | Guard Starter/Role             | TOT      | PHI      |   21.5 |   20.2 |
| Marquese Chriss     |     5.7 |     9.3 |     0.6 |     1.9 |     4.2 |     6.2 | Reserve   | Guard Starter/Role             | CLE      | GSW      |   14.6 |   20.3 |
| Seth Curry          |     7.9 |    12.4 |     0.9 |     1.9 |     1.6 |     2.3 | Reserve   | Guard Starter/Role             | POR      | DAL      |   18.9 |   24.6 |
| Dorian Finney-Smith |     7.5 |     9.5 |     1.2 |     1.6 |     4.8 |     5.7 | Reserve   | Guard Starter/Role             | DAL      | DAL      |   24.5 |   29.9 |
| Markelle Fultz      |     8.2 |    12.1 |     3.1 |     5.1 |     3.7 |     3.3 | Reserve   | Guard Starter/Role             | PHI      | ORL      |   22.5 |   27.7 |
| Langston Galloway   |     8.4 |    10.3 |     1.1 |     1.5 |     2.1 |     2.3 | Reserve   | Guard Starter/Role             | DET      | DET      |   21.8 |   25.8 |
| Josh Hart           |     7.8 |    10.1 |     1.4 |     1.7 |     3.7 |     6.5 | Reserve   | Guard Starter/Role             | LAL      | NOP      |   25.6 |   27.0 |
| Juan Hernangómez    |     5.8 |    12.9 |     0.8 |     1.3 |     3.8 |     7.3 | Reserve   | Forward or Center Starter/Role | DEN      | MIN      |   19.4 |   29.4 |
| Richaun Holmes      |     8.2 |    12.3 |     0.9 |     1.0 |     4.7 |     8.1 | Reserve   | Forward or Center Starter/Role | PHO      | SAC      |   16.9 |   28.2 |
| Rodney Hood         |     9.6 |    11.0 |     1.3 |     1.5 |     1.7 |     3.4 | Reserve   | Guard Starter/Role             | POR      | POR      |   24.4 |   29.5 |
| Danuel House        |     9.4 |    10.5 |     1.0 |     1.3 |     3.6 |     4.2 | Reserve   | Guard Starter/Role             | HOU      | HOU      |   25.1 |   30.4 |
| Kevin Huerter       |     9.7 |    12.2 |     2.9 |     3.8 |     3.3 |     4.1 | Reserve   | Guard Starter/Role             | ATL      | ATL      |   27.3 |   31.4 |
| James Johnson       |     7.8 |    12.0 |     2.5 |     3.8 |     3.2 |     4.7 | Reserve   | Guard Starter/Role             | MIA      | MIN      |   21.2 |   24.1 |
| Frank Kaminsky      |     8.6 |     9.7 |     1.3 |     1.9 |     3.5 |     4.5 | Reserve   | Guard Starter/Role             | CHO      | PHO      |   16.1 |   19.9 |
| Maxi Kleber         |     6.8 |     9.1 |     1.0 |     1.2 |     4.6 |     5.2 | Reserve   | Guard Starter/Role             | DAL      | DAL      |   21.2 |   25.5 |
| Brandon Knight      |     6.8 |    11.6 |     1.8 |     4.2 |     1.5 |     2.3 | Reserve   | Guard Starter/Role             | TOT      | DET      |   18.9 |   24.6 |
| Furkan Korkmaz      |     5.8 |     9.8 |     1.1 |     1.1 |     2.2 |     2.3 | Reserve   | Guard Starter/Role             | PHI      | PHI      |   14.1 |   21.7 |
| Doug McDermott      |     7.3 |    10.3 |     0.9 |     1.1 |     1.4 |     2.5 | Reserve   | Guard Starter/Role             | IND      | IND      |   17.4 |   19.9 |
| Patty Mills         |     9.9 |    11.6 |     3.0 |     1.8 |     2.2 |     1.6 | Reserve   | Guard Starter/Role             | SAS      | SAS      |   23.3 |   22.5 |
| Malik Monk          |     8.9 |    10.3 |     1.6 |     2.1 |     1.9 |     2.9 | Reserve   | Guard Starter/Role             | CHO      | CHO      |   17.2 |   21.3 |
| Markieff Morris     |     6.5 |    11.0 |     0.8 |     1.6 |     3.8 |     3.9 | Reserve   | Guard Starter/Role             | OKC      | DET      |   16.1 |   22.5 |
| Shabazz Napier      |     9.4 |     9.6 |     2.6 |     5.2 |     1.8 |     3.1 | Reserve   | Guard Starter/Role             | BRK      | MIN      |   17.6 |   23.8 |
| Cameron Payne       |     6.3 |    10.9 |     2.7 |     3.0 |     1.8 |     3.9 | Reserve   | Guard Starter/Role             | TOT      | PHO      |   17.8 |   22.9 |
| Norman Powell       |     8.6 |    16.0 |     1.5 |     1.8 |     2.3 |     3.7 | Reserve   | Guard Starter/Role             | TOR      | TOR      |   18.8 |   28.4 |
| Mitchell Robinson   |     7.3 |     9.7 |     0.6 |     0.6 |     6.4 |     7.0 | Reserve   | Guard Starter/Role             | NYK      | NYK      |   20.6 |   23.1 |
| Marcus Smart        |     8.9 |    12.9 |     4.0 |     4.9 |     2.9 |     3.8 | Reserve   | Guard Starter/Role             | BOS      | BOS      |   27.5 |   32.0 |
| Ish Smith           |     8.9 |    10.9 |     3.6 |     4.9 |     2.6 |     3.2 | Reserve   | Guard Starter/Role             | DET      | WAS      |   22.3 |   26.3 |
| Garrett Temple      |     7.8 |    10.3 |     1.4 |     2.5 |     2.9 |     3.5 | Reserve   | Guard Starter/Role             | TOT      | BRK      |   27.2 |   27.9 |
| Daniel Theis        |     5.7 |     9.2 |     1.0 |     1.7 |     3.4 |     6.6 | Reserve   | Guard Starter/Role             | BOS      | BOS      |   13.8 |   24.1 |
| Isaiah Thomas       |     8.1 |    12.2 |     1.9 |     3.7 |     1.1 |     1.7 | Reserve   | Guard Starter/Role             | DEN      | WAS      |   15.1 |   23.1 |
| Christian Wood      |     8.2 |    13.1 |     0.4 |     1.0 |     4.0 |     6.3 | Reserve   | Guard Starter/Role             | TOT      | DET      |   12.0 |   21.4 |
| Ivica Zubac         |     8.5 |     8.3 |     0.8 |     1.1 |     4.9 |     7.5 | Reserve   | Guard Starter/Role             | LAL      | LAC      |   15.6 |   18.4 |

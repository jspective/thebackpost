---
layout: post
categories: jekyll
title: 'Intro to Logistic Regression: Predicting NFL Game Win Probabilities'
author: "Jason Spector"
htmlwidgets: true
always_allow_html: true
excerpt: "How much does the QB matter?"
image: "jimmyG.jpg"
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
---

The quarterback is the most important position in football. No one
individual player has a more consistent impact on the game than them.
This short read looks to show an application of logistic regression by
taking a QBs rating scraped from footballreference.com to predict the
probability the QBs team won the matchup for the corresponding QB
rating.

Logistic regression uses a sigmoid function in order to produce a single
value output in the form of a probability. Although multiple variables
(or features) can be used, I am using a simple one variable input (QB
rating) to predict the probability of a win or loss. The data consists
of the career game logs for every starting quarterback from the 2019
season. I filtered to only games where the QB started the game. The end
result produced a data set with 2.7 thousand games.

The distribution of the games shows what we might expect: the higher the
QB rating the more likely the team is to win\!

<iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~jspective/1.embed">

</iframe>

In fact, when a QB has a rating of 100-149 their team wins 80 percent of
the time, 150 or above the team never lost\!

|         | Loss Percentage | Tie Percentage | Win Percentage |
| :------ | --------------: | -------------: | -------------: |
| 0-49    |           0.758 |          0.000 |          0.242 |
| 50-99   |           0.557 |          0.005 |          0.438 |
| 100-149 |           0.194 |          0.002 |          0.804 |
| 150+    |           0.000 |          0.000 |          1.000 |

I split the data into training and testing. Normally, I would randomly
pick training and testing, but since I really want to see how our model
performs on todays game we are making the most recent season that just
occurred the test set. You could also perform cross validation but that
is for another read.

The model results showed that not only was QB rating significant but it
allowed me to see how much probability would increase for every QB
rating point increase. Using the coefficient for rating, our winning
probability increases by .04 for every rating value increase of 1.

Below the summary is the mean probability for QB ratings in groups that
match what was showed a bit earlier. Notice above 150 rating doesn’t
give us a perfect 100 percent like real life. This is good as we can
never know for certain. 100-149 averages a win probability 95 percent
while 50-99 averages 47 percent.

``` 

Call:
glm(formula = WL ~ Rate, family = "binomial", data = qb_train)

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-2.3240  -1.0159   0.5135   0.8976   2.5919  

Coefficients:
             Estimate Std. Error z value Pr(>|z|)    
(Intercept) -3.323630   0.194022  -17.13   <2e-16 ***
Rate         0.040207   0.002092   19.22   <2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 3113.1  on 2292  degrees of freedom
Residual deviance: 2600.4  on 2291  degrees of freedom
AIC: 2604.4

Number of Fisher Scoring iterations: 4
```

|           | Winning\_pct |
| :-------- | -----------: |
| Below 50  |         0.12 |
| 50-99     |         0.47 |
| 100-149   |         0.95 |
| Above 150 |         0.95 |

How do we know what probability is reasonable to say a team wins given
their QB rating. The default for logistic is .5, but here I tested the
threshold using validation sets. I started from probability greater than
.1 to a probability of greater than .9, each stepping by .1. The graph
below told me that .6 was the best percent to use in terms of accuracy
error.

<iframe width="650" height="500" frameborder="0" scrolling="no" src="//plotly.com/~jspective/3.embed">

</iframe>

The final two tables show other metrics commonly used to evaluate
models, this particular model was 69 percent accurate which is pretty
decent but could be improved. Precision, recall, and F1 are all decent
but will be discussed in more detail another day. The final table has
the probability of winning the game in the first column, the QB in the
second, passer rating in the game, a W or L for a win or loss in the
actual game, and finally a W or L for a win or loss predicted by the
logistic model.

|           | Scores |
| :-------- | -----: |
| Accuracy  |   0.69 |
| Percision |   0.76 |
| Recall    |   0.64 |
| F1        |   0.69 |

|     | WinProb | QB              |  Rate | ActualWL | PredWL |
| :-- | ------: | :-------------- | ----: | :------- | :----- |
| 794 |  0.9194 | Patrick Mahomes | 143.2 | W        | W      |
| 795 |  0.8756 | Patrick Mahomes | 131.2 | W        | W      |
| 796 |  0.8791 | Patrick Mahomes | 132.0 | W        | W      |
| 797 |  0.4833 | Patrick Mahomes |  81.0 | W        | L      |
| 798 |  0.5918 | Patrick Mahomes |  91.9 | L        | L      |
| 799 |  0.6356 | Patrick Mahomes |  96.5 | L        | W      |
| 800 |  0.8500 | Patrick Mahomes | 125.8 | W        | W      |
| 801 |  0.8129 | Patrick Mahomes | 119.2 | L        | W      |
| 802 |  0.4012 | Patrick Mahomes |  72.7 | W        | L      |
| 803 |  0.4913 | Patrick Mahomes |  81.8 | W        | L      |
| 804 |  0.5094 | Patrick Mahomes |  83.6 | W        | L      |
| 805 |  0.7906 | Patrick Mahomes | 115.7 | W        | W      |
| 806 |  0.7656 | Patrick Mahomes | 112.1 | W        | W      |
| 807 |  0.4843 | Patrick Mahomes |  81.1 | W        | L      |
| 834 |  0.4753 | Jimmy Garoppolo |  80.2 | W        | L      |
| 835 |  0.8747 | Jimmy Garoppolo | 131.0 | W        | W      |
| 836 |  0.4974 | Jimmy Garoppolo |  82.4 | W        | L      |
| 837 |  0.7386 | Jimmy Garoppolo | 108.5 | W        | W      |
| 838 |  0.4803 | Jimmy Garoppolo |  80.7 | W        | L      |
| 839 |  0.2851 | Jimmy Garoppolo |  59.8 | W        | L      |
| 840 |  0.7590 | Jimmy Garoppolo | 111.2 | W        | W      |
| 841 |  0.8985 | Jimmy Garoppolo | 136.9 | W        | W      |
| 842 |  0.3403 | Jimmy Garoppolo |  66.2 | L        | L      |
| 843 |  0.7886 | Jimmy Garoppolo | 115.4 | W        | W      |
| 844 |  0.9268 | Jimmy Garoppolo | 145.8 | W        | W      |
| 845 |  0.7516 | Jimmy Garoppolo | 110.2 | L        | W      |
| 846 |  0.8778 | Jimmy Garoppolo | 131.7 | W        | W      |
| 847 |  0.5762 | Jimmy Garoppolo |  90.3 | L        | L      |
| 848 |  0.3868 | Jimmy Garoppolo |  71.2 | W        | L      |
| 849 |  0.8098 | Jimmy Garoppolo | 118.7 | W        | W      |

Hope you enjoyed this slight intro into the application of Logistic
Regression\! If you want to learn more or see the code email me\!

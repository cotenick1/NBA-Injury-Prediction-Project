# NBA At Risk Injury Classifier

----

by [Nick Cote](https://www.linkedin.com/in/nicholas-cote1/)

This project was completed with General Assembly in December 2019.

----
## Table of Contents

 - [Problem Statement](#Problem-Statement)
 - [Data Sources](#Data-Sources)
 - [Executive Summary](#Executive-Summary)
 - [Conclusions and Recommendations](#Conclusions-and-Recommendations)

----
## Problem Statement

Injuries to NBA players are often viewed as random events - a twisted ankle, an awkward landing, a poorly timed blow.  However, our hypothesis is that these are not random events and that we can build a model to predict these injuries.  By combining player information such as position, height, and age with statistical game logs for each player from the 2015-2016 to 2017-2018 NBA season, we will seek to build a classification model to predict whether a player will suffer an injury within one month after playing in a given game.  Ultimately, we will evaluate our model on sensitivity, not accuracy.  We feel that risk aversion is crucial as teams seek to build the best possible future and protect their players to any extent they can.  We note that specificity may matter; for instance, if a team needlesly holds a player out of a game, it could cost the team a win and potentially a playoff spot or playoff seeding.  However, we feel that prevention is the most crucial aspect of this project.  From this, we will seek to answer three questions:

1. Can we build a model to predict player injuries?
2. How can teams use these predictions?
3. How can teams use the coefficients/feature importances of the model to drive overall strategy and thinking about how to handle injuries?

----
## Data Sources

We have two primary sources of data for this project:

1. [NBA Injuries from 2010-2018](https://www.kaggle.com/ghopkins/nba-injuries-2010-2018) by Greg Hopkins via Kaggle
2. [Cleaning the Glass](https://cleaningtheglass.com/) is an NBA analytics website run by Ben Falk (former VP of Basketball Strategy with the Philadelphia 76ers).  For this project, I created a custom webscraper using Selenium to compile all available player game logs for the three seasons in question.  Note that to access the data used in this project, one would have to sign up for the free trial or subscribe to CTG's premium features.

----
## Executive Summary

The first step of our process was to buld a webscraper for Cleaning the Glass to access our game log and player feature data.  We chose Selenium as Selenium allows us to interact with the web page and allowed us to access game logs for each player in the player list for each season.  At the end of this stage, we obtained a data set where each row was each game for each player in the three seasons in question.

Second, we read in our injury data and cleaned it.  This data set included announcements for all missing games for a player and as such, the largest part of our cleaning was to remove non-injury references such as illnesses and personal reasons.  

Third, we combined our two data frames.  In this stage, we created different positive classes to look at that answered the question "After playing in this game, did a player suffer an injury in the current game, next week, next two weeks, next three weeks, or next month".  Among these positive class, we ultimately decided to model on the next month question as this gave us an appropriately balanced class to model on (~77% negative class to ~23% positive class).  Additionally we created a number of "to-date" features that summed various statistics for each players season to the game in question, a number of rolling average features that returned the player's average in a variety of statistics to that point in time, and a number of interactive features.

Next, we commenced our modeling stage.  We looked at a number of different classification models and used Grid Searching with cross-validation to tune over the hyper parameters and ultimately obtain our strongest model within each category. When assessing our model performances, we viewed accuracy, sensitivity, specificity, and area under the ROC curve; however, as mentioned above, we ultimately chose sensitivity as our target metric.  Ultimately, our Bagged Decision Tree ultimately performed the best with a sensitivity on the testing set of 81.5%.  In addition, the model's accuracy was 94.6%, specificity was 98.6%, and area under the ROC curve was 0.90.

Finally, we provided a suite of conclusions and recommendations to the problem statement as detailed in the section below.

----
## Conclusions and Recommendations

Ultimately, we outlined three questions to answer in our problem statement:

1. Can we build a model to predict player injuries?
2. How can teams use these predictions?
3. How can teams use the coefficients/feature importances of the model to drive overall strategy and thinking about how to handle injuries?

Our answers to each are as follows:

### Viability of Model for Predicting Player Injuries

Ultimately, we are able to build a model to predict player injuries  At this point, we are unable to provide a narrower timeline than one month on our predictions.  However, we feel comfortable that our model will pick up on roughly 80% of player injuries before they occur.  

We talk about player injuries as random events - a twist of the ankle, a hit to the arm, a pulled muscle and other things that are typically viewed as bad luck.  As a result, we are fairly confident that we have debunked this line of thinking and that we can use PUBLICLY available data to better predict player injuries.

### Model Applications for Teams

There are a number of ways this model can be used.  The first and most obvious is in the moment decision making: if our model predicts that a player is set to pick up an injury, the coaching staff can take a number of measures to attempt to prevent this or limit the damage including holding the player out of games, limiting the player's minutes in the games he does play, or even limiting the players involvement in the minutes he is playing.  It is ultimately up to the discretion of the coach and highly dependent on team context on how the coach can handle this, but these are all valid strategies and can help a team prevent injuries to its prized assets.

The second way this model can be used is to evaluate player transaction decisions.  If this model has recently deemed a player at a higher risk of injury, the front office in question may hesitate to acquire the player in a trade or off free agency or otherwise devalue the player.  This model can also be combined with other predictive models.  For instance, if we have simulations on how a given players season will look, we can potentially also predict the likelihood the player will suffer an injury during the season.  Interestingly, the same can be true for the NBA Draft.  For instance, if our analytics department has a model to simulate a prospect's rookie season, we can potentially assess injury risk for that player and adjust our draft ranking accordingly.  

### Model-Driven Strategy Recommendations

Finally, we can view the feature importances of our model to adjust our high level thinking of injuries.  This is somewhat similar to our answer to Question 2 but allows us to build a general analytical framework for how we view these sorts of problems as opposed to simply making predictions.  Our key takeaways and conclusions from viewing the feature importances are below.  Some are suprising and some are less so.  For the full breakdown of feature importance and further context on the ideas presented below, please see the Modeling codebook.

 - Minutes:  Ultimately, the more minutes you play, the more likely you are to be injury prone.  As such, in trying to reduce injuries we know that additional rest for players in games is crucial.
 - Workload: We see scattered throughout the top features various references to offensive workload.  These include free throw attempts, field goal attempts, and a number of interaction features related to player usage.  Ultimately, thinking about how to limit a players offensive workload in an effective manner can be an important step to limiting injuries.
 - Games Played / Season Timeline:  Unsuprisingly, injuries are more likely the more games played and the further into the season for a player.
 - Height: It appears that taller players suffer more injuries but in particular, taller players who are heavily features in an offense suffer additional injuries.
 - Age:  We see a similar story with age as with height.  Teams need to be more careful with older players, particularly those who still take on a heavy workload.
 
The one interesting feature that does not appear amongst the most important that merits discussion is days off between games.  There has been a great deal of discussion in the NBA surrounding player rest and limiting involvement in games played on back to back nights, particularly with superstar players.  However, our model does not seem to think the breaks between games play a particulary important role in injury prevention.


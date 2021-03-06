**Literature Review**

Zeileis, A., Leitner, C., & Hornik, K. (2018). Probabilistic forecasts for the 2018 FIFA World Cup based on the bookmaker consensus model. IDEAS Working Paper Series from RePEc,IDEAS Working Paper Series from RePEc, 2018.
	
	In this model, Zeileis, Leitner, and Hornik (2018) use winning odds from several online bookmakers and transform those into winning probabilities to compute team specific abilities. With the team-specific abilities all single matches are simulated via paired comparisons and, hence, the complete tournament course is obtained. Brazil is forecasted to win the FIFA World Cup 2018 with a probability of 16.6%, followed by Germany (15.8%) and Spain (12.5%).

“These SA Data Scientists Have Had Great Success with Predicting World Cup Scores - and They Believe 
They Know Who Will Win the Tournament.” Business Insider, 13 July 2018, 
www.businessinsider.co.za/these-sa-data-scientists-say-france-will-win-the-2018-fifa-world-cup-2-0-against-croatia-based-on-this-method-2018-7.

Bayesian Inference can be used to create a reliable model for World Cup predictions.  It 
will be hard to out predict every human because some variation is random scatter and should not be included in the model (i.e. key injuries, family/personal issues, mental state)


**Available Resources/Data**

International Football Results from 1872
•	Includes the scores for each match
•	Type of Match - Friendly or Tournament match

World Cup 2018 Dataset includes the following feature variables for each country participating in the World Cup
•	Previous World Cup Appearances
•	Previous Titles
•	Previous Finals Appearances
•	Previous Semi Final Appearances

Our plan to explore and clean the data is the following:
1.	Look at the win percentage of each team playing in the World Cup vs the number of games they have played
2.	Look at how many team players play in big clubs vs small clubs and see how a team’s cumulative club experience has an impact on the performance 
3.	Create feature variable of population enthusiasm to predict how a country’s support and enthusiasm impacts the team’s performance 

**Exploratory Data Analysis**


Figure 1: Germany, Brazil, Portugal, and Argentina were forecasted to be the early favorites in the 2018 World Cup based on their FIFA Ranking
 


Figure 2: The veterans of the tournament are Brazil, Germany, Argentina, and Mexico lead this year’s world cup competitors in total number of world cup appearances
 















Figure 3: There appears to be a negative relationship between a country’s number of appearances in the world cup and their pre-tournament FIFA ranking
 

Figure 4: Distribution of Age and Overall Skill level of players in the world Cup. We see a positive correlation between age and overall skill level








###Milestone 3: 

**FIFA Project EDA**

In order to guide the process of our feature engineering for the FIFA prediction model, we conducted a preliminary exploratory data analysis on international results gathered from 1873 to 2018. 

When analyzing the international results data, we looked at exploring what were the common types of international matches that were played between countries. As we can see in Figure 2, ‘Friendly’ matches seem to be the most common type of matches played, followed by the FIFA World Cup qualification. The FIFA World Cup matches only seem to the fifth most common matches that are played between countries. 


 














One of the first features we looked at exploring was the advantage a team had when they played at home compared to their opponent. According to the figure below, we can see that the home team usually won 62% of their matches on home ground compared to the 38% of victories that were achieved abroad.  As every match has a home and an away team, if there was no home advantage you would expect these proportions to be closer to 50%. This indicates that the feature, whether a team plays at home or away seems to have influence on their chances of winning the match.  This metric may be significant for predicting the world cup performance of host nations.  Potentially, home advantage could be further explored to see if playing in a world cup on a team’s home continent is also a valuable predictor.

















The following two figures, looked at the difference between the performance of each the World cup teams when they played at home or away. 

 
 

According to the figure above, we can also see that there exists a correlation between the difference in their win percentage at home or away and their FIFA rank. This leads us to believe that higher ranked team generally performs better despite the location and may be an indicator that influences their chances to win the match.  Also, it appears logarithmic feature engineering may be appropriate for this predictor after analyzing the potential curvature in this scatterplot.
 
The above figure displays a correlation between the total number of matches played and the rank of the countries according to FIFA. We observe a pattern here where the teams that have played the most matches thereby having the most experience, are generally the higher ranked teams. 

Based on the above visual analysis we can look at the following features to predict who would win the FIFA World Cup:
-	FIFA Ranking of both team
-	Difference in FIFA Ranking
-	Whether the team is Home/Away or Neutral Ground

**Part 2: Multiple Regression: Preliminary Model**

The prediction of goal difference in matches was explored using a multiple regression model.  The dataset included all international matches taking place from 2015 until the FIFA World Cup. 
However, the model performed quite poorly, requiring further future analysis and fitting to improve predictions.  The scatter matrix of the predictor set indicates that there is collinearity between predictors that needs to be handled appropriately.

 









 














**Part 3: Preliminary Logarithmic Model**
Based on the international results of matches since 1993, we fit a logistic regression model to predict the winner of the FIFA 2018 World Cup. Our model had a 55% accuracy on the train set data and a 43% accuracy on the test set. The following figure shows our model’s predicted results compared to the observed results in the test set. Based on the figure we can see that our model does a relatively good job in predicting wins. However, our model tends to overpredict a team’s loss instead of a draw. This leads to the inaccuracy in the predictions. In order to further develop the model, we will be looking at exploring each side’s playing strategy to see if whether a team is offensive or defensive has significance in the outcome of matches. 






















Our current model predicts a final between Argentina and Portugal with Argentina as the winner. 
 

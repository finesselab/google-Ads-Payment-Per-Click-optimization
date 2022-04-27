# Payment-per-click (PPC) Optimization

XXXX is a Charity Organisation.
PPC is a key channel for driving revenue from the sales of their Product range.

The products ranking in Google is determined by two factors:
 1. Quality score - Google’s rating of the quality of our ads
 2. Bid - price advertisers are willing to pay to be in the auction

A higher quality score for a given bid will enable advertisers to rank higher on Google’s search results page.
Therefore maximising quality score is a key component of PPC.
 
## KPI metric: Number of Conversions 
To improve number of conversions, it starts with Ads keyword **Quality score**.
  
Quality Score is determined by three factors:
 1. Expected CTR: This is the likelihood that our ad will get clicked when shown for a specific keyword.
 2. Ad relevance: This describes how relevant our ad copy is to a specific keyword and hence a user’s search.
 3. Landing page experience: This describes how relevant our ad’s landing page is to users who click on our ad.

## Optimization Approach
   1. Effect of Factors Affecting Quality Score
     `model = ols('Quality_score ~ Ad_relevance_score + Expected_CTR_score +
                                   Landing_page_exp_score + 0', data = training_df).fit()`
   
      The effect of each factor was added to or subtracted from QS upon improvement or recedence of factor respectively.
   
   2. Effect of Quality score on Average Ad position  
     `model2 = ols('position_score ~ Quality_score + 0', data = training_df).fit()`
      
      It was discovered that a unit increase in Quality score result in an average of 1 increase in Ad position score.
  
  
   3. Effect of Average position on Impressions 
     `model3 = ols('Impressions ~ Ad_Group:position_score + 0', data = training_df).fit()`

      I discovered that a unit increase in position score of 'Alternative Gifts' ad group  result in an average
      of 1908 additional impression and vice-versa.

   4. Effect of Impressions on Clicks
     `model4 = ols('Clicks ~ Ad_Group:Impressions + 0', data = training_df).fit()`    
      
      The slope coefficient of a unit increase in Impressions on Clicks for all Ad group was unlocked.
      For example 100 Impressions result in about 4 Clicks for Books Ad group.

   5. Effect of Clicks on Conversions
     `model5 = ols('Conversions ~ Ad_Group:Clicks + 0', data = training_df).fit()`

      The slope coefficient of a unit increase in Clicks on Conversions for all Ad group was unlocked as well.
      A 100 Click result in an average of 6 Conversions for 'Books' Ad Group.


**Functions** were created to use the above models to record the effect of improving or receding the metrics above. 
  For example function 'change_Impressions', uses the slope coefficent of 'model3' to add or decrease the number of
  impression of a Ad keyword depending on the type of change(improve or recede) of the Ad position score.


**Class** 'changeQs' uses the ripple effect of improving or receding one 'Quality Score' factor to
  update an Ad keyword's Quality_score, Ad_relevance_score, Expected_CTR_score, Landing_page_exp_score, 
  position_score, Impressions and Clicks. *Note*: *these effect stack for each change made*

  `data = changeQs(X_test.loc[26,:])`

  `data.change_one_factor('Expected_clickthrough_rate', 'improve', 'Alternative Gifts')`

  `new_data = data.change_one_factor('Landing_page_experience', 'recede', 'Alternative Gifts')`


## Predicting Number of Conversions for a Ad keyword
A linear regression model was trained using Ad_Group, Quality_score,  Ad_relevance_score, Expected_CTR_score, 
Landing_page_exp_score, position_score, Impressions, Clicks as **features** and Conversions as **target**.
The model has a Mean Absolute Error of 1.8 .

This LR model was now used to predict the number of conversions for the new data that resulted from tweaking
one or more 'Quality score' factor.

## Conclusion
With this, the marketing team can quantify the benefits or drawbacks of the respective improvement or decline
of a keyword's quality score.







































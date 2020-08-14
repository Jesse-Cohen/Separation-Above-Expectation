## Goal

The NFL currently uses spatiotemporal tracking to measure player performance over time. One of those measurements is Receiver Separation. Collected at the time a pass arrives, receiver separation measures the distance (in yards) from the receiver to their nearest defender.

While Receiver Separation is helpful in describing specific plays, it fails to capture the receiver's contribution to those plays. In order to truly understand a receiver's contribution, we must compare their actual separation (Receiver Separation) to their predicted separation. The goal of this project was to create that prediction.

## Results

Starting with ~30 features, I iterated through dozens of different feature sets, finally settling with the 5 features (listed below) that account for the most of the variance in separation. Using a tree-based model, ([light gradient boosted](https://lightgbm.readthedocs.io/en/latest/)) the final feature list to predict receiver separation (in order of importance) are:

   1. **Yds_Beyond_Success**
       - The vertical distance beyond the yardage deemed to be a successful play for the offense
           - play success is defined in the [Feature Analysis and Engineering](https://github.com/Jesse-Cohen/Expected-Receiver-Separation/blob/master/Notebooks/Feature_Analysis_and_Engineering.ipynb) notebook and is based on down and distance.
       - Though Air_Yds was most impactful feature when using *all* our features, 'Yds_beyond_success' was the most helpful in predictions in smaller feature spaces
   2. **NDP**
       - Position of the nearest defender: A categorical feature with this mapping {Cornerbacks:0, Safeties+DBs:1, Defenseive Linemen:2, Linebackers:3}
   3. **Cushion**
       - The distance (in yds) from the defender lined up across from the targeted receiver at the time of ball snap
   4. **Air Time**
       - The time (in seconds) it takes for a pass to travel in the air from launch to when it reaches the targeted recevier
   5. **Endzone Distance**
       - The distance (in yds) from where the recevier is targeted to the back of the endzone in the direction the team is facing
       
<img width="700" alt="Screen Shot 2020-08-05 at 11 15 54 AM" src="https://user-images.githubusercontent.com/66449877/89449122-7d99ce80-d70d-11ea-9663-567ca04cd9b8.png"> 

<img width="900" alt="Screen Shot 2020-08-10 at 9 56 28 PM" src="https://user-images.githubusercontent.com/66449877/89858923-a4646480-db54-11ea-9cc2-c0af7ba6ddc7.png">

Using data from the last 3 seasons (2017-2019), the plot above shows the differences between actual separation (Receiver Separation) and predicted separation (min. 175 targets). 

The vertical distance **above** the diagonal line represents *Separation Above Expectation*. We can see how DeSean Jackson's 2.77 yards of actual separation is actually +0.26 yards above expectation, while Golden Tate's 2.66 yards of separation is -0.51 yards above (or 0.51 yards below) expectation. 


   1. Separation Above Expectation is a better evaluator of player performance than separation alone
       - Controlling for factors like depth of target, pre-snap cushion, and nearest defender, separation becomes more insightful in player evaluation

To see an interactive version of this plot, please visit the my [Results Notebook](https://github.com/Jesse-Cohen/Expected-Receiver-Separation/blob/master/Notebooks/Results.ipynb) for more info. 

<img width="900" alt="Screen Shot 2020-08-07 at 10 20 56 AM" src="https://user-images.githubusercontent.com/66449877/89671379-b9f93600-d897-11ea-80bc-27a92aeaaa61.png">

As we can see in the plot above, deeper targets warrant less separation (on average). Without predicted separation, we would be left with the notion that Golden Tate and DeSean Jackson create separation at similar rates, when in fact, Jackson's 2.77 yards of average separation is actually significantly more impressive than Tate's 2.66 yards. 

### Model

By accounting for things like depth of target (Air Yards), pre-snap cushion, the position of the nearest defender, *predicted separation* aids in accounting for the per-play differences to help create a more comprehensive understanding of receiver separation. When aggregated over a course of a game, season, or career, this *predicted separation* value will bring context into the players TRUE ability to separate.

Starting with ~30 features, I iterated through dozens of different feature sets, finally settling with the 5 features (listed below) that account for the most of the variance in separation. Using a tree-based model, ([light gradient boosted](https://lightgbm.readthedocs.io/en/latest/)) the final feature list to predict receiver separation (in order of importance) are:

   1. **Yds_Beyond_Success**
       - The vertical distance beyond the yardage deemed to be a successful play for the offense
           - play success is defined in the [Feature Analysis and Engineering](https://github.com/Jesse-Cohen/Expected-Receiver-Separation/blob/master/Notebooks/Feature_Analysis_and_Engineering.ipynb) notebook and is based on down and distance.
       - Though Air_Yds was most impactful feature when using *all* our features, 'Yds_beyond_success' was the most helpful in predictions in smaller feature spaces
   2. **NDP**
       - A categorical feature with this mapping {Cornerbacks:0, Safeties+DBs:1, Defenseive Linemen:2, Linebackers:3}
   3. **Cushion**
       - The distance (in yds) from the defender lined up across from the targeted receiver at the time of ball snap
   4. **Air Time**
       - The time (in seconds) it takes for a pass to travel in the air from launch to when it reaches the targeted recevier
   5. **Endzone Distance**
       - The distance (in yds) from where the recevier is targeted to the back of the endzone in the direction the team is facing
       
<img width="700" alt="Screen Shot 2020-08-05 at 11 15 54 AM" src="https://user-images.githubusercontent.com/66449877/89449122-7d99ce80-d70d-11ea-9663-567ca04cd9b8.png"> 

## How does the model work?

Below are two contrasting examples to show how the features combine to create predictions.

<img width="1050" alt="Screen Shot 2020-08-06 at 4 43 53 PM" src="https://user-images.githubusercontent.com/66449877/89593485-24609680-d804-11ea-83fe-a02d6fca4e4d.png">

On the above play, the model predicted that the player would have a **separation of 1.5 yards**, -1.4 yards from the baseline prediction of 2.9 yards. 

On this play the pass traveled 35 yards *beyond* the marker for a successful play, it was in the air for 2.5 seconds, and the receiver was defended by a CB at the time the pass arrived (NDP = 0), all factors that would lead us to believe the receiver should have a smaller separation. 

On the play, the player had an **actual separation of 1.3 yards**. 

<img width="1050" alt="Screen Shot 2020-08-06 at 4 49 54 PM" src="https://user-images.githubusercontent.com/66449877/89593808-f6c81d00-d804-11ea-848e-ef421ea33002.png"> 

On this play, the model said that the player would have a **predicted separation of 7.5 yards**, +4.6 yards greater than the baseline of 2.9 yards. 

This was due mostly to the fact that the pass targeted a receiver that was 22 yards *behind* what was considered a success on the play (this was most likely a screen pass). The receiver also had a massive 12.5 yards of pre-snap cushion and was defended by a linebacker (NDP = 2) when the pass arrived. 

On the play, the player had an **actual separation of 6.7 yards**.

## Applications

Below is a scatter plot showing how linearly correlated **receiver separation above expectation** is to **team win percentage** over the last three seasons. 

<img width="900" alt="Screen Shot 2020-08-05 at 10 04 59 AM" src="https://user-images.githubusercontent.com/66449877/89442771-3d821e00-d704-11ea-9ec2-4d57d9f6fb7d.png">

I know what you are thinking...

>"*of course receiver separation is a good thing, teams that have more separation should win more!*"

Yes, that is true, but receiver separation above expectation is a **better predictor** of team success than average separation. Below is a linear correlation matrix to show how receiever separation above expectation and actual separation compare to team success.

 2. Separation Above Expectation has a **higher linear correlation to team success** than separation
       - Separation Above Expectation has a 0.66 linear correlation to Team Win Pct vs 0.48 for average separation

<img width="700" alt="CorrVals" src="https://user-images.githubusercontent.com/66449877/89859139-25bbf700-db55-11ea-9b37-fda3af201de1.png">

## Next Steps
 
With more time/resources/data, how would I improved these predictions?

   1. New Feature - Absolute Yardline
      - Where the play starts from on the field should add predictive power
   2. New Feature - Time Left + Score Differential 
      - What is the game situation? Is one team down 21 points and playing catch up? Is it tied in the 4th quarter?
   3. Create different model for each position
       - Each position is very specialized, that's the beauty of football
           - This model might not understand all of the nuances of separation for every position, but it can be generalized to be applied to all (non-backfield) receivers. There is always a trade-off between applicability and interpretability. If I build separate models for WRs/TEs/RBs it may be more insightful for each position group, but also inherently more complicated to understand and compare accross positions. 
   4. Hyperparameter tuning
      - In the Light Gradient Boosted Model, hyperparameter tuning is time-consuming and computationally heavy. With more time, I would spend more time fine-tuning these paramters to create the most accurate model
   5. Targets only
      - This dataset only includes instances when the player was targeted, so it has limitations. If we wanted to include all routes, we would have to create a proxy for separation (perhaps at pass forward). 
       
 ## Thanks for reading!

Thank you all for reading. If you have any questions, suggestions, or any feedback at all, feel free to reach out at JesseDCohen@gmail.com. 


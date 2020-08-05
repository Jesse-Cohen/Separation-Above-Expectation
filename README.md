## Goal

The goal of this project is to predict an NFL receiver's separation at the time of pass arrives. I will be using the NFL's player-tracking data set from the last 3 seasons (2017-2019) to create these predictions.

## Motivation

Receiver Separation is NOT built equal.

<img width="900" alt="Screen Shot 2020-08-05 at 3 41 07 PM" src="https://user-images.githubusercontent.com/66449877/89471095-1b9f9000-d732-11ea-9a27-aa6fe53dc3cd.png">

In the chart above, we can see how players like **DeSean Jackson** (2.8) and **Golden Tate** (2.7) have nearly identical average separation, but their average depths of target are on opposite ends of the spectrum. If you were comparing these two receivers on separation alone, it would appear appropriate to claim that they gain separation at similar rates. This is clearly flawed, though, as we can see that deeper targets warrant less separation, so Jackson's 2.8 yards of average separation is actually significantly more impressive than Tate's 2.7 yards. 

How much more impressive is it?  Well, that's where predicted separation comes in.

By accounting for things like depth of target (Air Yards), pre-snap cushion, the position of the nearest defender, *predicted separation* will aid in accounting for the per-play differences and help create a more comprehensive understanding of receiver separation. When aggregated over a course of a game, season, or career, this *predicted separation* value will bring context into the players TRUE ability to separate given their in-play circumstances as well as giving descriptive context into the situations a certain receiver sees. 

# Results

The first iteration of our training set left us with ~30 features to help with predictions. After sifting through dozens of different feature sets I settled on using 6 features (listed below) that account for the most of the variation in predicted separation. Using a tree-based model, (light gradient boosted) the final feature list to predict receiver separation (in order of importance) are:

   1. **Yds_Beyond_Success**
       - The vertical distance beyond the yardage deemed to be a successful play for the offense
           - play success is defined in Feature Analysis and Engineering notebook, and is based on down and distance
       - Though Air_Yds was most impactful feature when using *all* our features, 'Yds_beyond_success' was the most helpful in predictions in smaller feature spaces
   2. **NDP**
       - A categorical feature with this mapping {Cornerbacks:0, Safeties+DBs:1, Linebackers:2, Defenseive Linemen:3}
   3. **Cushion**
       - The distance (in yds) from the defender lined up across from the targeted receiver at the time of ball snap
   4. **Air Time**
       - The time (in seconds) it takes for a pass to travel in the air from launch to when it reaches the targeted recevier
   5. **Endzone Distance**
       - The distance (in yds) from where the recevier is targeted to the back of the endzone in the direction the team is facing
       
<img width="700" alt="Screen Shot 2020-08-05 at 11 15 54 AM" src="https://user-images.githubusercontent.com/66449877/89449122-7d99ce80-d70d-11ea-9663-567ca04cd9b8.png"> 

## How does the model work?

Below are two contrasting examples to show how the features combine to create predictions.

<img width="1050" alt="Screen Shot 2020-08-05 at 11 16 23 AM" src="https://user-images.githubusercontent.com/66449877/89449113-7b377480-d70d-11ea-9ba7-868e7a7c16bf.png">

On the above play, the model predicted that the player would have a **separation of 1.5 yards**, 1.4 yards less than the baseline prediction of ~2.9 yards. 

On this play the pass traveled 35 yards *beyond* the marker for a successful play, it was in the air for 2.5 seconds, and the receiver was defended by a CB at the time the pass arrived (NDP = 0), all factors that would lead us to believe it would have a smaller separation. 

On the play, the player had an **actual separation of 1.3 yards**. 

<img width="1050" alt="Screen Shot 2020-08-05 at 11 17 26 AM" src="https://user-images.githubusercontent.com/66449877/89449108-78d51a80-d70d-11ea-9caa-89e22b4fe210.png">
       
On this play, the model said that the player would have a **predicted separation of 7.5 yards**, +4.6 yards greater than the baseline of 2.9 yards. 

This was due mostly to the fact that the pass targeted a receiver that was 22 yards *behind* what was considered a success on the play (this was most likely a screen pass). The receiver also had a massive 12.5 yards of pre-snap cushion and was defended by a LB (NDP = 2) as the nearest defender when the pass arrived. 

On the play, the player had an **actual separation of 6.7 yards**.

## Why should you care?

Below is a scatter plot showing how correlated **receiver separation above expectation** is to **team win percentage** over the last three seasons. 

<img width="900" alt="Screen Shot 2020-08-05 at 10 04 59 AM" src="https://user-images.githubusercontent.com/66449877/89442771-3d821e00-d704-11ea-9ec2-4d57d9f6fb7d.png">

I know what you are thinking...

>"*of course receiver separation is a good thing, teams that have more separation should win more!*"

Yes, that is true, but receiver separation above expectation is actually a **better predictor** of team success than average separation. Below is a correlation matrix to show how receiever separation above expectation and actual separation compare to team success.

<img width="700" alt="Screen Shot 2020-08-05 at 11 48 26 AM" src="https://user-images.githubusercontent.com/66449877/89453158-a6bd5d80-d713-11ea-9dce-d2313a892539.png">

## Player Evaluation

<img width="900" alt="Screen Shot 2020-08-05 at 4 09 16 PM" src="https://user-images.githubusercontent.com/66449877/89472777-09275580-d736-11ea-8f43-bb5255b78c47.png">

The plot above shows the differences between predicted and actual average separations of receivers with at least 175 targets in the last three seasons. 

The vertical distance **above the diagonal line** represents Separation Above Expectation, and we can now see more clearly how DeSean Jackson's 2.7 yards of separation is actually +0.3 yards above expectation, while Golden Tate's 2.7 yards of separation is -0.5 yards above (or .5 yards below) expectation. 

To see an interactive version of this plot, please visit the my [Results Notebook](https://github.com/Jesse-Cohen/Expected-Receiver-Separation/blob/master/Notebooks/Results.ipynb) for more info. 


## Main Takeaways

   1. Separation Above Expectation is a **better predictor of team success** than Separation
       - Separation Above Expectation has a 0.66 correlation to Win % vs .48 for Avg Separation
   2. Receiver Separation fails to account for in-play circumstances, and should be supplemented with Separation Above Expectation
       - Controlling for factors like depth of target, pre-snap cushion, and nearest defender, Separation becomes more insightful
       - **Tyreek Hill, Davante Adams, Cooper Kupp, and Tyler Lockett** some of the best route-runners/separation-getters in the NFL.
   3. Compare receviers **within clusters**, and select for positive Separation Above Expectation
       - From a team-building perspective, you cannot replace Antonio Brown with JuJu Smith-Schuster (as we all saw). Players with different body types and abilities have different team roles, and need to be accounted for when building a receiving corps. 
       
 ## Thanks for reading!

Thank you all for reading. If you have any questions, suggestions, or any feedback at all, feel free to reach out at JesseDCohen@gmail.com. 


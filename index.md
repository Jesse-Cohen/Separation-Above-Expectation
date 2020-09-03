<img src="https://user-images.githubusercontent.com/66449877/92156714-40912c80-edde-11ea-80b0-f499d0bcc258.jpeg" alt="drawing" width="235"/>

For questions, suggestions, or feedback, reach out to me at [JesseDCohen@gmail.com](mailto:JesseDCohen@gmail.com).

## **Goal**

The NFL uses spatiotemporal tracking data to measure player movement on the field at any given time. One of those measurements is *Receiver Separation*. Collected at the time a pass arrives, *Receiver Separation* measures the distance (in yards) from the receiver to their nearest defender.

While *Receiver Separation* is helpful in describing plays, it fails to measure performance, or a receiver’s overall ability to get open. In order to understand that, we must compare *Receiver Separation* (actual separation) to *Predicted Separation* (described below). The goal of this project was to create that comparison, a measurement of receiver performance called *Separation Above Expectation*.

## **Predicted Separation**

Before I could create *Separation Above Expectation*, I had to create *Predicted Separation* - a prediction of *Receiver Separation*.   

To predict a measurement like *Receiver Separation* in a given play, I looked at a combination of other measurements ("features") within that play. By comparing features like how close the nearest defender was at the start of a play or how long the ball was in the air during a play, we can predict how much separation a receiver typically gets in a similar play. 

Starting with 22 features in 43,559 plays from 2017-2019, I iterated through dozens of combinations to find the 5 features (listed below) that create the simplest and most accurate model of *Predicted Separation*. In order of importance, those features are:

1. *Yards Beyond Success*

    * The difference (in yds) between the *Yards to Success* (based on down and distance) and the *Air Yards* of a play. *Yards to Success* and its use in *Yards Beyond Success* is further defined in **[Feature Analysis and Engineering](./Notebooks/Feature_Analysis_and_Engineering.html)**.

2. *Nearest Defender Position*

    * The position (categorical, e.g. Cornerback, Linebacker, etc) of the receiver’s nearest defender at pass arrival.

3. *Cushion*

    * The distance (in yds) from the defender lined up across from the receiver at ball snap.

4. *Air Time*

    * The time (in seconds) it takes for a pass to travel from launch to when it reaches the receiver.

5. *Endzone Distance*

    * The distance (in yds) from where the receiver is targeted to the back of the team’s scoring endzone.

> _(To see process of creating this model in detail, visit **[Model Selection and Tuning](./Notebooks/Model_Selection_and_Tuning.html))**_

## **Results**

Using measurements of *Predicted Separation* (from the model described above) and *Receiver Separation* (from the NFL tracking data), we can compare how much separation a receiver is expected to get to how much they actually get. This comparison, *Separation Above Expectation*, measures the receiver's performance by exposing their ability to get open. 

The plot below shows *Separation Above Expectation* for 72 receivers with a minimum of 175 targets from 2017 - 2019. The vertical distance away from the diagonal line represents their *Separation Above Expectation*. 

> _(To see an interactive version of this plot, visit **[Results](./Notebooks/Results.html))**_

<img width="900" alt="Screen Shot 2020-08-10 at 9 56 28 PM" src="https://user-images.githubusercontent.com/66449877/89858923-a4646480-db54-11ea-9cc2-c0af7ba6ddc7.png">

From 2017 to 2019, DeSean Jackson averaged 2.77 yards of *Reciever Separation* while Golden Tate averaged a similar 2.66 yards. Using *Separation Above Expectation* we can see that Jackson’s separation was +0.26 yards above expectation, while Tate's separation was -0.51 yards above (or 0.51 yards below) expectation.

Jackson and Tate have similar average *Receiver Separation*, but drastically different *Separation Above Expectation* because the difference in their *Predicted Separation*. 

The table below illustrates this difference, showing the average values of each of the numerical factors of *Predicted Separation*. 

<img width="450" alt="Screen Shot 2020-09-01 at 10 54 41 PM" src="https://user-images.githubusercontent.com/66449877/92038154-ce5e1080-ed27-11ea-9512-f270b2e6eeb9.png">

While most of their stats are similar, Jackson’s average *Yards Beyond Success* (12.27) was more than 10 yards further downfield that Tate's (1.37). This indicates that Jackson was more of a threat, so opposing defenses should have made it more difficult for him to gain separation. 

While Jackson and Tate appear to have been equally good at getting open when looking at *Receiver Separation* alone, *Separation Above Expectation* shows that because Jackson faced more difficult situations, his performance was, in fact, better than Tate’s. 

## **Bonus Applications**

Because a receiver’s performance is directly tied to a team’s performance, *Separation Above Expectation* can also be used to predict team success.

The scatter plot below shows that over the last three seasons, *Separation Above Expectation* is strongly correlated to team win percentage.

<img width="900" alt="Screen Shot 2020-08-05 at 10 04 59 AM" src="https://user-images.githubusercontent.com/66449877/89442771-3d821e00-d704-11ea-9ec2-4d57d9f6fb7d.png">

While you may be thinking, "*Of course, receiver separation is a good thing! Teams that generate more separation should win more!*" the correlation matrix below shows that *Separation Above Expectation* is actually a better predictor of team success than *Receiver Separation*. 

<img width="700" alt="Screen Shot 2020-09-02 at 2 24 56 PM" src="https://user-images.githubusercontent.com/66449877/92038465-49bfc200-ed28-11ea-9cd4-93e4724c123e.png">

## **Next Steps**

With more time, resources and data, here’s how I would improve my predictions (in order of priority):

1. Create a new feature, *Game Status*

    * Using the time left in the game and the score differential at that point in time, include a stat that describes the status of the game. Has the game just begun? Is one team down 21 points and playing catch up? Is it tied in the 4th quarter? 

2. Create a different model for each position

    * *Predicted Separation* is generalized to be applied to all (non-backfield) receivers, so building separate models for WRs/TEs/RBs would likely be more insightful for comparing within each position group alone.

3. Hyperparameter tuning

    * In the Light Gradient Boosted Model I used, hyperparameter tuning is time-consuming and computationally heavy. With more time, I would fine-tune these parameters to create a more accurate model.

4. Expand to include all routes

    * This dataset only includes instances when the player was targeted, so it doesn’t measure separation on all routes. If we wanted a more inclusive measurement of separation, we would have to create proxy features for routes that weren't targeted (like *Separation at Pass Forward*).

## **Process Documentation**

For a more in-depth look at the cleaning, analysis, and modeling process, here is code below:

1. **[Data Cleaning](./Notebooks/Data_Cleaning.html)**
2. **[Feature Analysis and Engineering](./Notebooks/Feature_Analysis_and_Engineering.html)**
3. **[Model Selection and Tuning](./Notebooks/Model_Selection_and_Tuning.html)**
4. **[Results](./Notebooks/Results.html)**

## Goal

The goal of this project is to predict an NFL receiver's separation at the time of pass arrives. I will be using the NFL's player-tracking data set from the last 3 seasons (2017-2019) to create these predictions.

## Motivation

Receiver Separation is NOT built equal.

Receivers like Tyler Lockett (Seahawks) are short, quick, and line up often in the 'slot.' Others, like Cooper Kupp (Rams), are slightly bigger, line up 'tight' to the line of scrimmage, and often run shallow routes, rarely stretching deep downfield. Different still are the physical, big-framed receivers (Mike Williams, Chargers or Kenny Golladay, Lions) whom often line up out 'Wide',  are usually smothered by the opposing team's best cornerback, and are targeted deep often. 

It's unfair to compare receiver separations to say "hey, receiver X is better at creating separation than receiver Y because he averages more separation." 

To try and account for this, my goal is to create a predicted separation metric that will aid in accounting for the per-player differences in team role/resposibility/body type. When aggregated over a course of a game, season, or career, this 'expected separation' value will bring context into the players TRUE ability to separate given their in-play circumstances as well as giving descriptive context into the situations a certain receiver sees. 

# Results

The first iteration of our training set left us with ~30 features to help with predictions. After sifting through dozens of different feature sets I settled on using 6 features (listed below) that account for the most of the variation in predicted separation. Using a tree-based model, (light gradient boosted) the final feature list to predict receiver separation (in order of importance) are:

   1. **Yds_Beyond_Success**
       - The vertical distance beyond the yardage deemed to be a successful play for the offense
           - play success is defined in Feature Analysis and Engineering notebook, and is based on down and distance
       - Though Air_Yds was most impactful feature when using *all* our features, 'Yds_beyond_success' was the most helpful in predictions in smaller feature spaces
   2. **Cushion**
       - The distance (in yds) from the defender lined up across from the targeted receiver at the time of ball snap
   3. **NDP**
       - A categorical feature with this mapping {Cornerbacks:0, Safeties+DBs:1, Linebackers:2, Defenseive Linemen:3}
   4. **Air Time**
       - The time (in seconds) it takes for a pass to travel in the air from launch to when it reaches the targeted recevier
   5. **Endzone Distance**
       - The distance (in yds) from where the recevier is targeted to the back of the endzone in the direction the team is facing
       
<img width="700" alt="Screen Shot 2020-08-05 at 11 15 54 AM" src="https://user-images.githubusercontent.com/66449877/89449122-7d99ce80-d70d-11ea-9663-567ca04cd9b8.png"> 

<img width="900" alt="Screen Shot 2020-08-05 at 11 17 26 AM" src="https://user-images.githubusercontent.com/66449877/89449108-78d51a80-d70d-11ea-9caa-89e22b4fe210.png">

On the above play, the model predicted that the player would have a **separation of 1.5 yards**, 1.4 yards less than the baseline prediction of ~2.9 yards. 

On this play the pass traveled 35 yards *beyond* the marker for a successful play, it was in the air for 2.5 seconds, and the receiver was defended by a CB at the time the pass arrived (NDP = 0), all factors that would lead us to believe it would have a smaller separation. 

On the play, the player had an **actual separation of 1.3 yards**. 

<img width="900" alt="Screen Shot 2020-08-05 at 11 16 23 AM" src="https://user-images.githubusercontent.com/66449877/89449113-7b377480-d70d-11ea-9ba7-868e7a7c16bf.png">
       
On this play, the model said that the player would have a **predicted separation of 7.5 yards**, +4.6 yards greater than the baseline of 2.9 yards. 

This was due mostly to the fact that the pass targeted a receiver that was 22 yards *behind* what was considered a success on the play (this was most likely a screen pass). The receiver also had a massive 12.5 yards of pre-snap cushion and was defended by a LB (NDP = 2) as the nearest defender when the pass arrived. 

On the play, the player had an **actual separation of 6.7 yards**.

<img width="900" alt="Screen Shot 2020-08-05 at 10 04 59 AM" src="https://user-images.githubusercontent.com/66449877/89442771-3d821e00-d704-11ea-9ec2-4d57d9f6fb7d.png">

<img width="900" alt="Screen Shot 2020-08-05 at 10 09 40 AM" src="https://user-images.githubusercontent.com/66449877/89442790-42df6880-d704-11ea-971b-e71fb81b564f.png">


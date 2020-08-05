## Goal

The goal of this project is to predict an NFL receiver's separation at the time of pass arrives. I will be using the NFL's player-tracking data set from the last 3 seasons (2017-2019) to create these predictions.

## Motivation

Receiver Separation is NOT built equal.

Receivers like Tyler Lockett (Seahawks) are short, quick, and line up often in the 'slot.' Others, like Cooper Kupp (Rams), are slightly bigger, line up 'tight' to the line of scrimmage, and often run shallow routes, rarely stretching deep downfield. Different still are the physical, big-framed receivers (Mike Williams, Chargers or Kenny Golladay, Lions) whom often line up out 'Wide',  are usually smothered by the opposing team's best cornerback, and are targeted deep often. 

It's unfair to compare receiver separations to say "hey, receiver X is better at creating separation than receiver Y because he averages more separation." 

To try and account for this, my goal is to create a predicted separation metric that will aid in accounting for the per-player differences in team role/resposibility/body type. When aggregated over a course of a game, season, or career, this 'expected separation' value will bring context into the players TRUE ability to separate given their in-play circumstances as well as giving descriptive context into the situations a certain receiver sees. 

# Results

The first iteration of our training set left us with ~30 features to help with predictions. After sifting through dozens of different feature sets I settled on using 5 features (listed below) that account for the most of the variation in predicted separation. Using a tree-based model, (light gradient boosted) the final feature list to predict receiver separation (in order of importance) are:

  1. **Yards from Success** - A combination of multiple features, 'Yards From Success' is how far down the field a receiver is targeted *relative* to 
  2. **Pre-snap Cushion** - The distance
  3. **NDP_CB** - Whether or not the nearest defender was a CB (this was the only categorical value to be of real value to the model)
  4. **Time to Throw** - How long from the time of snap did the QB take to pass the ball (seconds)
  5. **Air Time** - How long the pass took to reach its targeted recevier (seconds)

<img width="800" alt="Screen Shot 2020-08-05 at 10 04 59 AM" src="https://user-images.githubusercontent.com/66449877/89442771-3d821e00-d704-11ea-9ec2-4d57d9f6fb7d.png">

<img width="800" alt="Screen Shot 2020-08-05 at 10 09 40 AM" src="https://user-images.githubusercontent.com/66449877/89442790-42df6880-d704-11ea-971b-e71fb81b564f.png">


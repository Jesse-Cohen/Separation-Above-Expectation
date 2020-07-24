## Goal

The goal of this project is to predict an NFL receiver's separation at the time of pass arrives. I will be using the NFL's player-tracking data set from the last 3 seasons (2017-2019) to create these predictions.

## Motivation

Receiver Separation is NOT built equal.

Receivers like Tyler Lockett (Seahawks) are short, quick, and line up often in the 'slot.' Others, like Cooper Kupp (Rams), are slightly bigger, line up 'tight' to the line of scrimmage, and often run shallow routes, rarely stretching deep downfield. Different still are the physical, big-framed receivers (Allen Robinson, Bears or Mike Evans, Bucs) whom  often line up out 'Wide',  are usually smothered by the opposing team's best cornerback, and are targeted deep often. 

It's not fair to compare receiver separations to say "hey, receiver X is better at creating separation than receiver Y because he averages more separation." To try and account for this, I want to create a predicted separation metric that will aide in accounting for these differences in team role/resposibility/body type. When aggregated over a course of a game, season, or career, this 'expected separation' value will bring context into the players TRUE ability to separate given their in-play circumstances as well as giving descriptive context into the situations a certain receiver sees. 

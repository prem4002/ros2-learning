### OMPL
open motion planning library is an open source library for robot motion planning
It computed collision free paths from start state to end state, it tell the robot how to move and how it's built

it must move from position A to position B without touching an obstacle

It doesn't do
- kinematics computation
- no collision checking
- no robot description
These things are provided externally, for example through moveit

Typical OMPL algorithms include
RRT, RRT*, PRM, BIT*, EST, FMT
it mainly uses sampling based algorithms
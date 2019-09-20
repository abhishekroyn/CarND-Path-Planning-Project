# **Path Planning** 
---

**CarND-Path-Planning-Project**

The goals / steps of this project are the following:
* Build a path planner that creates, smooth safe trajectories for the car to follow.
* Added 2 points from previous trajectory, and rest of the points using spline method for 30, 60, and 90 m mark.
* Checked for collision avoidance and safe lane change on both left and right sides using provided sensor fusion data.
* Increased and decreased speeds to keep it within required range as per encounter with or without traffic in lanes.
* Included few additional fine tuning as and when needed to make the trajectory smoother and avoid collision.
* Provided the path planner output as a list of x and y global map coordinates, which together formed a trajectory.


[//]: # (Image/Video References)

[video1]: ./output_videos/path_planning_highway_driving.mp4  "Output video with autonomous driving using path planning for highway driving"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/1971/view) individually and describe how I addressed each point in my implementation.  

---
### Compilation

#### 1. The code compiles correctly.

Code compiled without errors with cmake and make.

No change was made in CMakeLists.txt and thus the code is expected to compile on any platform.

### Valid Trajectories

#### 1. The car is able to drive at least 4.32 miles without incident.

Based on the top right screen of the simulator indicating current/best miles driven without incident, it was confirmed that the car was was able to drive more than 4.32 miles. The same was verified by running the simulator multiple times and letting it run more than 4.32 miles each time.

#### 2. The car drives according to the speed limit.

The car drove according to the speed limit. The car started from zero speed (main.cpp lines 58) and incremented in step-wise manner till it reaches close to but less than the defined speed limit (main.cpp lines 168-174). It slowed down, and that too in step-wise manner, only when obstructed by traffic, and again tried to achieve near close to speed limit in absence of any obstruction by traffic. 

#### 3. Max Acceleration and Jerk are not Exceeded.

Based on the top right screen of the simulator indicating acceleration and jerk, it was confirmed that the car did not exceed a total acceleration of 10 m/s^2 and a jerk of 10 m/s^3. Also, based on increment and decrement of speed limit, it was clearly expected to have acceleration and jerk within defined limits (main.cpp lines 168-174).

#### 4. Car does not have collisions.

The car did not come into contact with any of the other cars on the road. While driving in own lane, it was always looking for obstacle 30m ahead to slow down if needed to avoid any collision (main.cpp lines 133-135). The same was true for lane changes, it was looking for obstacle 20m ahead and behind to avoid lan change if needed for collision avoidance (main.cpp lines 150-151, lines 161-162).

#### 5. The car stays in its lane, except for the time between changing lanes.

The car did not spend more than a 3 second length out side the lane lanes during changing lanes, and every other time the car stayed inside one of the 3 lanes on the right hand side of the road. Before changing lanes it was always checked first to ensure the lane change happened to one of the 3 lanes on the right side of the road only, for example, no left lane change when in lane 0 (main.cpp lines 146), and no right lane change when in lane 2 (main.cpp lines 157). Also after changing lane it was made sure the car stayed in the lane for 4 seconds before trying to make a lane change again (main.cpp line 177 & 184). This helped in avoiding quick toggling of lanes for the car when there was obstacle in two adjacent lanes, thus ensuring the car did not stay longer out side the lane lanes during changine lanes. For changing lanes, it always checked first to ensure there was no obstacle within +/- 20m in the new lane (main.cpp lines 150-151, lines 161-162). Thus, lane change happened smoothly and did not take more than a 3 second length.

#### 6. The car is able to change lanes.

The car was able to smoothly change lanes when it made sense to do so, such as when behind a slower moving car and an adjacent lane was clear of other traffic. Before changing lanes it was always checked first to ensure the lane change happened to one of the 3 lanes on the right side of the road only, for example, no left lane change when in lane 0 (main.cpp lines 146), and no right lane change when in lane 2 (main.cpp lines 157). Also after changing lane it was made sure the car stayed in the lane for 4 secons before trying to make a lane change again (main.cpp line 177 & 184). This helped in avoiding quick toggling of lanes for the car when there was obstacle in two adjacent lanes, thus ensuring the car did not stay longer out side the lane lanes during changine lanes. For changing lanes, it always checked first to ensure there was no obstacle within +/- 20m in the new lane (main.cpp lines 150-151, lines 161-162). Thus, lane change happened smoothly.

### Reflection

#### 1. There is a reflection on how to generate paths.

The code model for generating paths was described in detail in the README.

`path_planning_highway_driving.mp4` captured the output video with autonomous driving using path planning for highway driving. Here's a ![link to my video result - path_planning_highway_driving.mp4][video1]

### Future Work

* Use cost functions for more precise and safer lane changes
* Check 5 seconds in the future for all cars in traffic to ensure no collision during lane-change
* Add different cost function for being in different lanes
* Use additional previous path points for smoothing the trajectories
* Minimize jerk, calculate multiple splines, and chose the one with lesser jerk
* JMT could be used with spline to smooth trajectory generation even more
* Verify if additonal constraints are needed to handle the case when some other car changes lane at the same time to the intended lane where ego vehicle is changing its lane
* Verify if additonal constraints are needed to handle the case when ego vehicle is in leftmost (or rightmost) lane, and finds obstacle in front in the same lane and also obstacle in front at the adjancent lane (middle lane), where both the obstacles are at similar distance from ego vehicle, and both the obstacles have similar velocity. And there is another car (obstacle) in the middle lane. In that case our ego vehicle may keep on slowing down on seeing obstacle, and keep speeding up when obstacle is out of range but can't change lane due to obstacle in adjacent lane. This can at times create a deadlock situation where ego vehicle will have to keep moving  entire journey behind the obstacle in its lane, instead of being able to somehow change lane twice to other outermost lane to overtake obstacles in the other two lanes.


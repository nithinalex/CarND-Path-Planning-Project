# Write Up

This write up is included to detail how the project meets the requirements of the [rubric](https://review.udacity.com/#!/rubrics/1020/view)

## Requirements

### Compilation

The project compile without problems and any warning. To build you need to:

1. Clone this repo. `git clone https://github.com/rarce/CarND-Path-Planning-Project`
2. Make a build directory: `cd CarND-Path-Planning-Project && mkdir build && cd build`
3. Compile: `cmake .. && make`

### Valid Trajectories

#### The car is able to drive at least 4.32 miles without incident

The implementation can drive at least 4.32 miles without any incident as show in the following picture.

![More than 4.32 miles without incidents](https://raw.githubusercontent.com/rarce/CarND-Path-Planning-Project/master/writeup_files/noincidents.png)

#### The car drives according to the speed limit.

To ensure the speed limit the trajectories are generated taking in account the `ref_vel` variable who must be slower than `49.5` this is set in [line the 314](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L314) of `main.cpp`, the trajectory is generated in [line 412](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L412) this generate the increments in the points accordint to `ref_vel`

#### Max Acceleration and Jerk are not Exceeded.

To meet this requirement the velocity don't increment suddently, this is se in the [line 309](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L309) and in the [line 315](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L315) incrementing the speed only by `.224` in every cycle.

#### Car does not have collisions.

The implementation, check the position of the cars using the data from the sensor fusion and check in every cycle if there is any car in the same lane with risk of collision, in that case try to change of lane if there a lane empty, if not reduce the speed to follow the car in the same lane, until can change of lane, the check in the same lane is do in [line 270](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L270) and the check of lane change is check in [line 283](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L283) and [295](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L295)

#### The car stays in its lane, except for the time between changing lanes.

The implementation use frenet coordinates, and always set changes between existing lanes, so don't exit of the road, and alway travel in the center of the lane except for changing lane.

#### The car is able to change lanes

The implementation, change of lane keeping a variable called `lane` to track the target lane, this start as 1 (the center lane) and when require to change is set to the target lane. This is later used to generate the required path in [line 364](https://github.com/rarce/CarND-Path-Planning-Project/blob/master/src/main.cpp#L364) where are generated the waypoints of the path.

### Reflection

The model used in this implementation relies in spline to smoth the path generated, in a reference velocity to meet the speed limit and avoid collisions, and in collision detection to change lane and try to keep the pace at maximun allowed speed. Also relies in the use of frenet coordinates to keep always un the lane, and make calculations easy to follow. To apply the spline is also used a momentary rotation to avoid problematic function fit.
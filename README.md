# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Simulator.
You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases).

### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

#### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

Here is the data provided from the Simulator to the C++ Program

#### Main car's localization Data (No Noise)

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH

#### Previous path data given to the Planner

//Note: Return the previous list but with processed points removed, can be a nice tool to show how far along
the path has processed since last time. 

["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator

#### Previous path's end s and d values 

["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

#### Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

## Details

1. The car uses a perfect controller and will visit every (x,y) point it recieves in the list every .02 seconds. The units for the (x,y) points are in meters and the spacing of the points determines the speed of the car. The vector going from a point to the next point in the list dictates the angle of the car. Acceleration both in the tangential and normal directions is measured along with the jerk, the rate of change of total Acceleration. The (x,y) point paths that the planner recieves should not have a total acceleration that goes over 10 m/s^2, also the jerk should not go over 50 m/s^3. (NOTE: As this is BETA, these requirements might change. Also currently jerk is over a .02 second interval, it would probably be better to average total acceleration over 1 second and measure jerk from that.

2. There will be some latency between the simulator running and the path planner returning a path, with optimized code usually its not very long maybe just 1-3 time steps. During this delay the simulator will continue using points that it was last given, because of this its a good idea to store the last points you have used so you can have a smooth transition. previous_path_x, and previous_path_y can be helpful for this transition since they show the last points given to the simulator controller with the processed points already removed. You would either return a path that extends this previous path or make sure to create a new path that has a smooth transition with this last path.

## Tips

A really helpful resource for doing this project and creating smooth trajectories was using http://kluge.in-chemnitz.de/opensource/spline/, the spline function is in a single hearder file is really easy to use.

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```

## Project Instructions and Rubric

Path Planning in a self-driving vehicle is a complex routing problem with varying environment behavior  that needs to be modelled or sensed constantly. It cna be broken up into a few distinct modules:

1. Scene Recognition
2. Obstacle Avoidance
3. Trajectory Planning

### Scene Recognition

Scene recognition can be achievied using a variety of sensors(RADAR, LIDAR, Imaging). In this project, the scene/environment information is provided as sensor inputs by the simulator. This comes in the form of other vehicles of the road and their positions, speeds & other information.

### Obstacle Avoidance

Once we have the positional information of all the objects, the first task is to avoid hitting any of them. We achieve this calculating the speed of the objects in the vehicle's lane and slowing down. 

### Trajectory Planning

This is where most of the work for the project is done. 

#### Rubric Criteria:
1. The car is able to drive at least 4.32 miles without incident.

The code/car was run multiple times on the simulator and each time I ran over 4.32 miles without any incident. The car was also ran for longer periods of time (over 10 miles) multiple times without any incident.

2. The car drives according to the speed limit.

The car's maximum velocity is set to 49.5 mph (50 is the speed limit). I also made sure that the car does not travel slowly for long periods of time by increasing its speend whenever it can (if not stuck in traffic)

3. Max Acceleration and Jerk are not Exceeded.

The deceleration and accelerations at each update are set so as to not exceed 10 m/s^2  while also making sure the jerk does not go over 10 m/s^3.

4. Car does not have collisions.

The first check at every update is whether there is a car infront that's closer than 30m. If so, we slow down. Then, we evaluate options to change lanes which also takes vehicles thata re closer to ours before deciding whether shift lanes or stay in the slower lane.

5. The car stays in its lane, except for the time between changing lanes.

The car does stay in its lane all the time except for when it needs to change lanes.

6. The car is able to change lanes.

We trigger a lane change feasibility check whenever there is a slower moving car in front of the vehicle that's at less 30m away. If we can safely exceute a lane change, we do so. Else, we reduce the vehicle's speed and stay in the same lane. We keep checking this lane change possibility as long as the car is behind a slow-moving vehicle.

7. Path Generation.

We used a spline to map points for the vehicle's trajectory. A set of points are anchored at equal distances (at 30m, 45m, 60m) and new points are generated along these anchors to generate a smooth trajectory. This trajectory is constantly updated taking any lane chnages and slow traffic into consideration.

#### Improvements
There are many aspects of this model that can be enhanced. A few that are on my mind and will possibily implement soon (for the final project) are:

1. Designing a cost function (using criteria like lane changes, desired speed, collisions, etc)
2. Improvement in path planning where the car doesn't get boxed in slower moving traffic by using current speed, acceleration of the vehicles in the vicinity and picking a seemingly sub-optimal path in the short run but overall the best.



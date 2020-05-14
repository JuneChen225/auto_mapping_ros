# auto_mapping_ros
Auto Mapping ROS software for autonomously constructing a High Definition Map using Multiple Robots.

This project is still under development and the goal is to develop a ROS service which has the ability to autonomously navigate a blueprint to build a high definition map using SLAM using multiple autonomous robots (F110 Cars).

<p align="center"><img src="media/auto_mapping_ros.jpg" width="400" height="600">
</p>


**How to run this code:**

Install dependencies
```
sudo apt-get install libconfig++-dev
```

You will need to build the simulator. You can follow the tutorial [here](https://github.com/YashTrikannad/f110-simulator-multi-agent) to build the simulator.

This repository depends on a planner service. You can use the FMT* Planning Service from my repository here.
```
cd catkin_ws/src
git clone https://github.com/YashTrikannad/fmt_star_ros.git
cd ..
catkin_make

```
Clone and Make this repository
```
cd catkin_ws/src
git clone https://github.com/YashTrikannad/auto_mapping_ros.git
cd ..
catkin_make
```

Source the ROS environment before launching ROS nodes in new terminal
```
cd catkin_ws
source devel/setup.bash
```
**Launching Autonomous Mapper**

The first step is to find the best optimzed routes for the cars to follow. We run ant colony optimization (ACO) in background to get multiple routes for the available cars. Note- All cars may not be used depending on your capacity parameters for ACO. 

To generate the csv files of the route sequences, you'll need to run the multi sequence generator. The multi sequence generator depends on configuration parameters inside the [aco_router](https://github.com/YashTrikannad/aco_router/blob/8964081f2319e5ae4dd99a25f29365ba24645b78/config.cfg). 


1. Launch roscore
```
cd catkin_ws
source devel/setup.bash
roscore
```
2. In new terminal, run the seqeunce creator
```
cd catkin_ws
source devel/setup.bash
rosrun auto_mapping_ros coverage_sequence_creator
```
3. When you see a window popping up with your map titled as *Functional Area Registration*, click on the top left corner and then the bottom right corner which describes the rectangular area where you want the auto mapper to function.

4. After the graph is generated, a window will pop up asking you for the initial depot (starting position for your cars). Click on one of the nodes where you want your cars to start. 

This should generate sequences in [csv folder](https://github.com/YashTrikannad/auto_mapping_ros/tree/master/csv) for ideal number of cars <= vehicles_available parameter set in the config.


<p align="center"><img src="media/multi_agent.gif" width="400" height="600">
</p>


To run the auto mapping module once the sequence is saved, you'll also need to run the FMT* Sevice:

In Terminal 1, launch the simulator:
```
roslaunch f110_simulator simulator.launch
```

In Terminal 2, run your auto mapping module which will also start the fmt star action server:
```
roslaunch auto_mapping_ros auto_mapping_ros.launch
```

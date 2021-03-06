---
title: "Controlling a Simple Differential Drive Car Through a 2D Cluttered World"
excerpt: "Adaptive Control and Reinforcement Learning Class Project, Spring 2019"
collection: portfolio
---

Navigating a range of maze environments with unknown obstacles with a 2-wheeled robot.  Navigation done with A* for path planning and DDP-MPC for control. 

In our simulation, the optimal routes for the car are generated through A* algorithm, also, these routes will be updated according to the detected state of bridges (or gates), which represented by the small gray squares in each map. In addition,  other auxiliary codes are implemented to fine tune the route mainly keep them away from the walls and have better curvature around the corner of walls.

<br/><img src='/images/astar_viz.PNG'>

In order to successfully navigate the A* waypoints a controller was required.  Initially, a series of controllers were investigated.  A PID controller was tested, but proved unsuccessful.  Since the differential-drive system is unable to stop (as there is a nominal control input), the PID controller proved unsuccessful.
In the end, DDP, or Differential Dynamic Programming, was selected as it has shown success in cases with nonlinear dynamics that are difficult to linearize, such as the inverted pendulum problem. 
DDP is described in detail in [2], therefore there is no equations described in this report.  However, the key benefits of DPP are described here.  DDP utilizes local dynamic programming methods and a quadratic approximation of the Q function.  A pseudo-hamilton is formed from the second-order expansion of the Q-function.  Also, the optimal control is defined as a perturbation from a nominal control.
For this case, processing time was a critical factor.  Therefore, a first order finite-difference was used providing much faster processing times. 
After successful implementation of this controller, it was discovered that the controls must be limited.  Two methods were tested:  hard bounds on the control after the line search approximation and a squashing function.  Ultimately, the squashing function was used to limit the control during both the backward and forward pass. 
Though these final parameters showed promising results, a decision was made to use a dynamic horizon approach to the DDP algorithm.  This was because of the range of A* solution distances that came from the tight corridors and wall padding calculations.
In combination with the controller, a set of logic functions were required to decide when to bypass a waypoint or when to determine when the waypoint was successfully met.  A 0.1 space circle around the waypoints had to be met, unless one of the next 3 waypoints in the list was closer.  Therefore, certain scenarios can be seen where a set of waypoints is skipped since the DDP controller’s horizon is not long enough to reach the waypoints.

<br/><img src='/images/maze_nav_one.PNG'>

<br/><img src='/images/maze_solve_pic.jpg'>

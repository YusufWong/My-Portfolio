# Self-Driving Vehicle Control Modeling using Python & PID Control on CARLA Simulator 
<p align="center">
  <a href="https://github.com/YusufWong/My-Portfolio/tree/main/Projects/Self-Driving-Vehicle-Control-Modeling-Project">
    <img src="https://github.com/YusufWong/My-Portfolio/blob/main/images/Car-Simulation.gif" width="800">
  </a>

</p>
Here is a 1-minute driving simulation of an autonomous car driving on a racetrack on CARLA, an open source virtual driving simulator. By utilizing Python and lateral/longitudinal vehicle control modeling strategies, I was able to adjusting its speed profiles to communicate the car's movements and interactions on the track.


## Background

<p align="center">
  <img src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Self-Driving-Vehicle-Control-Modeling-Project/images/CARLA_logo.jpg"
  width = "350" />
  <img src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Self-Driving-Vehicle-Control-Modeling-Project/images/SimulatorWithPython.png"
  width = "650" />
</p>


For this project, I used a Software called **CARLA**, an open source autonomous driving simulator. It’s an API for Vehicle control, and by using Python, I can communicate the car’s movements and interactions within the simulation. It uses a client-server architecture, where the Python client sends commands to the CARLA server to control a vehicle on virtual roads and racetracks. CARLA is open-source and free for everyone to download and use, allowing researchers to test their code on this simulator.

## Overview

The goal of this project is to develop a controller that enables a vehicle to autonomously follow a race track by navigating through preset waypoints using Python and Vehicle Control Modeling Strategies. It’s similar to Google Maps, where you have a starting point and a destination, but here, the vehicle is automatically controlled to reach the destination while adhering to speed limits and roadway rules. For simplicity, the course provided predefined waypoints that specify both the path and the recommended velocity profile. Using Python, I implemented both lateral control for steering and longitudinal control for acceleration, braking, and throttle to ensure smooth and precise vehicle navigation.

## Objectives
<img align="left"  src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Self-Driving-Vehicle-Control-Modeling-Project/images/VehicleControlStrategyArchitecture.png" 
  height = "400"
  />
<p align="justify"> 
Program Driving Simulation using: 
 -Python
 -Vehicle Control Modelling Strategies
Implement PID Control Methods
  -Longitudinal Control (Throttle/Braking)
 -Lateral Control (Steering)
Utilized Python & CARLA Simulation Cohesively
 -Control vehicle to follow race track
 -Navigate through preset waypoints along predefined path (think Google Maps!)
 -Receive information of relative position & velocity 
 -Update/Command steering angle & throttle/velocity profiles from Python to CARLA
 -Adjust Steering Angle & Speed Profiles as Needed
 -Must reach waypoints at certain desired speeds
</p>

## Test

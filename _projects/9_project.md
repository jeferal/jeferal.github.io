---
layout: page
title: Web GUI to control a ROS2 robot
description: Implementation of a web GUI to control a rehabilitation parallel robot
img: assets/img/web_based_gui.jpeg
importance: 9
category: work
---

One of the proposed tasks during the internship I carried out at [ai2.upv](https://www.ai2.upv.es/en/home-english/) was to implement a Graphical User Interface
for the parallel robots with the control architecture that I implemented through ROS2. The aim of this task was to propose an architecture and create a
prototype of a GUI to show that is possible rather than implementing a very complex one, that would be for future work.

One of the first decisions that was made was to implement this GUI as a web application. This way the compatibility with different devices is much more flexible than implementing a QT application for example.
An operator would be able to control the robot from a computer or even a tablet as long as it has a web browser and access to the lab's network.

The implemented architecture is based on the [ROS2 Web Bridge](https://github.com/RobotWebTools/ros2-web-bridge), which at the moment is not maintained anymore (this GUI was implemented in 2020), but the package
[ROSBridge suite](https://github.com/RobotWebTools/rosbridge_suite) should be capable of doing the same. Anyway, ROS2 Web Bridge is a package that can convert the information from ROS2 topics to web sockets. It can also handle
ROS2 service, but it can not handle actions.

In the following diagram we can see the architecture of the control scheme and also the web application:

<p align="center">
  <img width="600" height="300" src="/assets/img/ros2_web_flow.png">
</p>
<div class="caption">
    Web GUI for a rehabilitation robot basic architecture
</div>

The architecture includes:
* The Robot
* Control Computer
* `ROS2 Web Bridge`: Translates the ROS2 topics and services to web sockets.
* `Web Server`: Provides a client with the web application.
* `Rehabilitation exercises server`: Provides a client with rehabilitation exercices. There is an API that interfaces between the client and the database that contains the exercises, which was implemented through NodeJS.
* `Database`: Database that contains rehabilitation exercises hosted by mySQL.

As you can see, there are different technologies that were used. First of all `SQL` to create the database, `NodeJS` to interface with the database and provide the API and also the framework `ReactJS` to create the web application.
Graphically, this is the result of the application.

<p align="center">
  <img width="600" height="350" src="/assets/img/web_based_gui.jpeg">
</p>
<div class="caption">
    Web GUI result
</div>

It is possible to select the rehabilitation exercise to execute. Also, if the user wanted to control the 3DOF robot or the 4DOF robot that were available in the laboratory and start and stop the robot. Finally, the user would be able to visualize the trajectory while it was performed by the robot. As mentioned before, this is a prototype. For future work, someone would also first consider who the user of such application would be.
For instance, a doctor might not need to visualize with such detail the trajectory of the robot, but an engineer to check the status of the robot might need so.

Another thing to consider would be security, the application should implement a way of
bringing the robot to a home position and also consider network security aspects.

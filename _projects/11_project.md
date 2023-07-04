---
layout: page
title: Bachelor's Thesis
description: Implementation of dynamic controllers for a 4DOF parallel robot by a real time embedded industrial controller and an FPGA
img: assets/img/compact_rio.png
importance: 11
category: work
---

I remember that before starting my work on the Bachelor's Thesis I was usnure of focusing my career on robotics although it was something that really interted me before. Due to my interest in the course on `Advanced Computer Control` I asked the profesor if they were proposing any Thesis related to this in any laboratory. I was lucky enough that it was the case and this is where I started working on parallel robots for rehabilitation purposes with Marina Vallés.

The purpose of this Bachelor's Thesis was to implement a control architecture for 4DOF parallel robots through a `real time embedded industrial controller and an FPGA`. This controller was the `Compact-RIO` from National Instrument whose CPU and FPGA were programmable by the language `LAB-VIEW`.

<p align="center">
  <img width="350" height="280" src="/assets/img/compact_rio.png">
</p>
<div class="caption">
    The system NI CompactRIO.
</div>

I loved solving all sort of problems related to a distributed real time control architecture, such as:
* Implementing a `buffer` in the Compact Rio to store trajectory points that were sent form the host and do not lose them.
* Measuring the `latency` in the communication between the Compact Rio and the host.
* Measuing important `variables in the control loop` like: position error, control action, proportional action, derivative action, gravity compensation action, etc.
* Implementing a controller that was not only based on a PID, but also was able to `compensate gravity`.
* Creating an `abstract enough interface` to be able to test the software and hardware on `simulation` (`MATLAB / Simulink`) as if it was the real robot.

One of the advantages of using LabView is that it is very easy to create a Graphical User Interface to control the robot and also to visualize important variables in the system. This was the result:

<p align="center">
  <img width="600" height="300" src="/assets/img/crio_gui_1.png">
</p>
<div class="caption">
    Graphical user interface 1 with LabView.
</div>

<p align="center">
  <img width="600" height="350" src="/assets/img/crio_gui_2.png">
</p>
<div class="caption">
    Graphical user interface 2 with LabView.
</div>

Programming in LabView works different than a standard programming language. It basically consists of connecting blocks (functions) through wires. It was a valuable experience, but I personally prefer standard programming languages.

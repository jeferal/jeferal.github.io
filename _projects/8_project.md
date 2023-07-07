---
layout: page
title: Master's Thesis
description: Implementation of modular position/force controllers based on ROS2 for lower member rehabilitation parallel robots
img: assets/img/masters_thesis.png
importance: 5
category: work
---

I carried my Master's Thesis in a research laboratory that belonged to a research institute [ai2.upv](https://www.ai2.upv.es/en/home-english/) were I was also taking an internship. The aim of the Thesis was to implement modular position/force controllers
based on ROS2 for lower member rehabilitation parallel robots.
After defending the Master's Thesis, it was qualified with the highest grade 10/10 and with a `Special Mention`.
I have been discussing in other posts some other implementation, but here you will find a summary of what my Thesis covered.

### 1. Parallel robots for rehabilitation and control hardware
As mention in the title, I worked with parallel robots for rehabilitation purposes of the lower member: ankle 3DOF, knee and ankle 4DOF. Parallel robots have the advantage that they are more accurate and have a better stiffness and strength/weight ratio
than serial robots. In the following image you can see the robots I worked with:
<p align="center">
  <img width="700" height="300" src="/assets/img/parallel_robots_rehabilitation.png">
</p>
<div class="caption">
    All the rehabilitation parallel robots I worked with. 3DOF, 4DOF and newer 4DOF.
</div>

The control hardware was based on an industrial computer, which was running Ubuntu and that contained Data Acquisition Cards to be able to read the encoders of the robots and output the control action:
<p align="center">
  <img width="400" height="300" src="/assets/img/daq_1.png">
</p>
<div class="caption">
    Advantech PCI-1784U used for reading the joint position sensors (high resolution encoders).
</div>

### 2. Control architecture - ROS2
There was a need to implement a control architecture that was based in a real time framework and also that was modular, so that it could be easily adapted to different robots and also so that researches and developers could easily implement newer and more complex controllers than position controllers. For this reason, ROS2 was chosen as the implementation framework. I did this work in 2020 and 2021 so the ROS2 version I worked with was Foxy. ROS2 is able to provide a more deterministic communication in the publisher - subscriber model that offers than ROS. This image shows the layers that ROS2 contains:

<p align="center">
  <img width="400" height="300" src="/assets/img/ros2_layers.png">
</p>
<div class="caption">
    ROS2 layers, https://aws.amazon.com/es/blogs/robotics/ros2-foxy-fitzroy-robot-development/.
</div>

### 3. Position Controller
This diagram shows the architecture of the implemented position controller. It is based on a standard PD algorithm and on a compensation of gravity.
<p align="center">
  <img width="600" height="300" src="/assets/img/pdg_diagram.png">
</p>
<div class="caption">
    Diagram of the position controller PD + Gravity Compensation.
</div>

Demonstration of the result:

<p align="center">
    <iframe width="560" height="315"
        src="https://youtube.com/embed/d0ZvOFMC1OQ"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        allowfullscreen>
    </iframe>
</p>
<div class="caption">
    Position Controllers PD + Gravity Compensation.
</div>

The following plots show important variables that are used to evaluate the control performance. To tune the controller, the accumulation of each position error and also the accumulation of the increment of the control action was used.

{% include figure.html path="assets/img/position_trajectory.png" title="example image" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    PD + Gravity compensation - Desired trajectory and measured in joint space.
</div>
{% include figure.html path="assets/img/position_error.png" title="example image" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    PD + Gravity compensation - Desired trajectory and measured in joint space position error.
</div>
{% include figure.html path="assets/img/control_action.png" title="example image" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    PD + Gravity compensation - Control action.
</div>

### 4. More complex controllers and GUI
In this post I have only introduced my Master's Thesis, in the following posts you can also read about particular controllers and implementations that my MS Thesis also covered:
* [Force controllers](/projects/5_project/)
* [Type II Singularity Releaser](/projects/7_project/)
* [Position Controller based on Deep Reinforcement Learning](/projects/6_project/)
* [Web-based graphical user interface](/projects/9_project/)

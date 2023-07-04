---
layout: page
title: Sensor Integration
description: Overview of a sensor integration for a SLAM architecture
img: assets/img/velodyne.jpeg
importance: 5
category: work
---

During my internship in `Space Application Services` I took part in the integration of sensors of different nature for a mobile robot. I covered many of the sensors that are used to solve many different problems in `autonomous mobile navigation` such as
`localization` and `obstacle avoidance`.

First of all, I used a 3D Lidar from Velodyne that was able to provide a 3D Point Cloud data structure of the scene. From this Point Cloud it is possible to detect areas where the robot is not able to travers and areas where it would be able.
These areas can be detected by obtaining the normal from a surface estimator algorithm for example.

<p align="center">
  <img width="500" height="350" src="/assets/img/velodyne.jpeg">
</p>
<div class="caption">
    3D Lidar Velodyne.
</div>

To be able to have a more accurate point cloud of the enviroment, a `Depth Camera (Intel D435)` was used:

<p align="center">
  <img width="300" height="200" src="/assets/img/intel_d435.webp">
</p>
<div class="caption">
    The Depth Camera Intel D435.
</div>

To localize the robot, there were different sensors that were used, these included: `Wheel Encoders`, an `IMU` and a `Tracking Camera`. These sensors provide the system with different `sources of odometry` (wheel odometry, inertial odometry and visual odometry).
As typical in robot `localization`, the signal of these sensors was `fused` to have a better estimation of the position and orientation of the robot in the world. The algorithm used that fused these signals was the `Unscented Kalman Filter`, which is also widely used in autonomous driving.

Something that is very important when using different sensors in an autonomous mobile robot is to know accurately where each sensor is located so that its data can be transform to a common frame and be able to interpret the information and fuse the signals. If this is not accurate enough there will be an error in the estimation. 

I conducted most of the work testing the implementation of the localization and obstacle avoidance Software in the `Husky robot of Clearpath` and then, integrated the architecture to the `LUVMI-X rover` when it was available.

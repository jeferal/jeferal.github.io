---
layout: page
title: Publication on MDPI Sensors
description: Vision-Based Hybrid Controller to Release a 4-DOF Parallel Robot from a Type II Singularity
img: assets/img/mocap_parallel_robot.png
importance: 7
category: work
---

During my Internship in [ai2.upv](https://www.ai2.upv.es/en/home-english/) I contributed to a research paper that was also part of my `Master's Thesis`. The paper was published in `MDPI Sensors` and it is available [here](https://www.mdpi.com/1424-8220/21/12/4080). I contributed by implementing the proposed novel controller, which also makes use of control architecture that I implemented in `ROS2` with `C++ (mostly)` and `Python`.

This paper introduces a novel `hybrid controller for a 4-DOF parallel` robot that is able to detect when the robot is in a `type II singular` position and brings the robot back to a safe configuration. This is very important in the context of `rehabilitation robots`, since there is the possibility of losing control of the robot and making the robot not safe enough for a patient. Solving this type of issues would make a parallel robot more suitable for rehabilitation purposes because they have the advantages of
having a high accuracy as well as a better dynamic performance than serial robots.

This particular controller uses a external sensor to track the position and orientation of the platform. The external sensor used was the `motion capture system Optitrack`. So I implemented first a `driver` to be able to read the data coming from Optitrack and be able to feed the controller with the `cartesian pose of the platform`. The arrangement of the cameras in the laboratory can be seen in the following image:

<p align="center">
  <img width="500" height="400" src="/assets/img/optitrack_lab.png">
</p>
<div class="caption">
Optitrack set-up in the laboratory. Vision-Based Hybrid Controller to Release a 4-DOF Parallel Robot from a Type II Singularity.
</div>

There is this need of having an external sensor because computing Forward Kinematics based on the joint sensors is not possible in a singular position because computing the Jacobian matrix would have numerical innestabilities. As a reminder, Forward Kinematics is the difficult problem in parallel robots (opposite to serial where the difficult one is Inverse Kinematics) and we need a Numerical algorithm that makes use of the Jacobian matrix.
After this, the Type II Singularity Control Module was able to work on top of the position controller to detect first if the robot was in a Type II Singular position and release the robot from this configuration. The control scheme can be visualized in the following diagram:

<p align="center">
  <img width="500" height="300" src="/assets/img/singularity_releaser.png">
</p>
<div class="caption">
Type II Singularity Control Module. Vision-Based Hybrid Controller to Release a 4-DOF Parallel Robot from a Type II Singularity.
</div>

Finally, a demonstration of the controller working on the real robot can be seen in this video:

<p align="center">
    <iframe width="728" height="410"
        src="https://www.youtube.com/embed/fpZMfLe9lRg"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        allowfullscreen>
    </iframe>
</p>
<div class="caption">
Demonstration in the real robot. Vision-Based Hybrid Controller to Release a 4-DOF Parallel Robot from a Type II Singularity.
</div>

Reference:

[Vision-Based Hybrid Controller to Release a 4-DOF Parallel Robot from a Type II Singularity](https://www.mdpi.com/1424-8220/21/12/4080)
* José L. Pulloquinga
* Rafael J. Escarabajal
* Jesús Ferrándiz
* Marina Vallés
* Vicente Mata
* Mónica Urizar

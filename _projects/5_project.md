---
layout: page
title: Force controller
description: Implementation of an admittance controller as a rehabilitation exercise
img: assets/img/force_controller.png
importance: 6
category: work
---

This piece of work belongs to my Master's Thesis as well as to the internship I carried in a research laboratory working with parallel robots for rehabilitation purposes.

In order to perform with robots rehabilitation exercises that are safe for the patient, it is neccessary to know and control the force that the robot is applying to the patient. Another reason to control this force is to be able to implement resistive rehabilitation exercises, where the patient needs to a apply a certain force. For these reasons, a cartessian force sensor has been incorporated to the platform of the parallel robot as seen in the following image:

<p align="center">
  <img width="600" height="400" src="/assets/img/force_sensor.png">
</p>
<div class="caption">
    Cartesian force sensor. It can meassure force and torque in the 6 degrees of freedom.
</div>

This parallel robot in particular has 3 degrees of freedom and the purpose of this force controller is to control the force along these three DOF, which are the height z, and 2 angles (alpha and beta) given the measurements of the force sensor. The controller is based on an admittance model, which is a model that relates the force and the position of the robot. This model will define the dynamic of the response as a mass-spring-damper system.

<p align="center">
  <img width="400" height="200" src="/assets/img/admittance_model.png">
</p>
<div class="caption">
    Admittance model.
</div>

The force controller is implemented as an outer loop of the position controller that has already been implemented. There are two variations:

<p align="center">
  <img width="700" height="300" src="/assets/img/admittance_control_v1.png">
</p>
<p align="center">
  <img width="700" height="300" src="/assets/img/admittance_control_v2.png">
</p>

The first version is simpler to understand, the admittance model would receive the force error as the input and would output a cartesian position. This cartesian position is added to the actual cartesian reference of the robot as an increment and then inverse kinematics is perform to obtain the joint angles that will be fed to the position control loop.
The second version, rather than adding the increment of position in cartesian space, the admitance model would output a velocity in cartesian space, which by mean of the Jacobian matrix this is translated into the joint space. The joint increment to add to the joint state reference is obtained by integrating this velocity.
The result can be seen in the following video:

<p align="center">
    <iframe width="315" height="560"
        src="https://youtube.com/embed/dQw4w9WgXcQ"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        allowfullscreen>
    </iframe>
</p>
<div class="caption">
Freestyle Slalom improvisation in Bois de la Cambre (Brussels)
</div>
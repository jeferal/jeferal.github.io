---
layout: page
title: Pong in an FPGA with VHDL
description: Project to implement the pong game in a Terasic FPGA through VHDL
img: assets/img/pong_fpga.png
importance: 12
category: work
---

This piece of work belongs to my Master's Thesis as well as to the internship I carried in a research laboratory working with parallel robots for rehabilitation purposes.

In order to perform with robots rehabilitation exercises that are safe for the patient, it is neccessary to know and control the force that the robot is applying to the patient. Another reason to control this force is to be able to implement resistive rehabilitation exercises, where the patient needs to a apply a certain force. For these reasons, a cartessian force sensor has been incorporated to the platform of the parallel robot as seen in the following image:

TODO: Make smaller and in the middle.
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/force_sensor.png" title="Force sensor" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Cartesian force sensor. It can meassure force and torque in the 6 degrees of freedom.
</div>

This parallel robot in particular has 3 degrees of freedom and the purpose of this force controller is to control the force along these three DOF, which are the height z, and 2 angles (alpha and beta) given the measurements of the force sensor. The controller is based on an admittance model, which is a model that relates the force and the position of the robot. This model will define the dynamic of the response as a mass-spring-damper system.
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/admittance_model.png" title="Admittance model" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Admittance model.
</div>

The force controller is implemented as an outer loop of the position controller that has already been implemented. There are two variations:

<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/admittance_control_v1.png" title="Admittance control v1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/admittance_control_v2.png" title="Admittance control v2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The first version is simpler to understand, the admittance model would receive the force error as the input and would output a cartesian position. This cartesian position is added to the actual cartesian reference of the robot as an increment and then inverse kinematics is perform to obtain the joint angles that will be fed to the position control loop.
The second version, rather than adding the increment of position in cartesian space, the admitance model would output a velocity in cartesian space, which by mean of the Jacobian matrix this is translated into the joint space. The joint increment to add to the joint state reference is obtained by integrating this velocity.
The result can be seen in the following video:

<iframe width="420" height="315" src="http://www.youtube.com/embed/dQw4w9WgXcQ" frameborder="0" allowfullscreen></iframe>

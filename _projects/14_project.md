---
layout: page
title: Ball on Plate with Hand Teleoperation
description: Simple control project as a test bench to integrate different algorithms.
img: assets/img/ball_teleoperation.png
importance: 1
category: work
---

This is a personal project I have been working with for a few weeks. It consists on controlling the position of a
ball on a platform using the feedback of a camera. This is the hardware that is used:
* 2DOF Platform with Servo Motors
* Micro Controller ESP8266
* 5V DC Power Supply
* Camera Realsense D415
* Computer with GPU

<p align="center">
  <img width="500" height="400" src="/assets/img/ball_on_plate_hw.jpeg">
</p>
<div class="caption">
    Ball on plate Hardware.
</div>

## Interfacing with the Servo Motors
Writing the position commands to the servo motors with a PWM signal is done with the micro controller `ESP8266`.
This micro controller has been programmed with the PlatformIO visual code extention, which is a very powerful IDE to
easily create projects and program many different boards using many libraries. The code for the micro controller can
be found in the [servo_platform_firmware](https://github.com/jeferal/servo_platform_firmware).

This program checks the serial port of the board and looks for position commands that are sent to the servos. Finally,
it will send back to the serial port the current position of the servos and how much time it has taken to compute
this.

## Servo Platform ROS Driver
This component uses a simple serial ROS-less class that can be found in the [sp_driver](https://github.com/jeferal/servo_platform/tree/master/sp_driver) package. This class implements everything that is needed to send messages
through the serial port. The communication protocol characteristics is set with a termios structure and it has the
required parameters to ensure a non-blocking continuous communication with the micro controller.

The SpROSDriver inherits from the SpDriver class (a class that contains the simple serial communication and methods to write roll, pitch commands and process the response) and it is implemented in the package [sp_ros_driver](https://github.com/jeferal/servo_platform/tree/master/sp_ros_driver) package.

There is also a quick script that uses pygame to send commands with the xbox controller [here](https://github.com/jeferal/servo_platform/blob/master/sp_ros_driver/scripts/xbox_controller.py). And another script that sends a sinusoidal position trajectory [here](https://github.com/jeferal/servo_platform/blob/master/sp_ros_driver/scripts/sin_cos_example.py).

## Ball tracking and control
Because of the material of the ball, it suffers from reflections and this makes the problem of tracking the
ball more difficult. I started trying to implement a detector that uses classical computer vision techniques
such as color segmentation or the hough transform to detect circles, but it is difficult to fine tune the parameters
of this algorithms to make them robust against reflections and uncontrolled light conditions.

After that, I experimented with the trackers that are implemented in the OpenCV library. They all use a common
API so it is easy to change between them. The one I tried mostly was `CSRT`. However this tracker would eventually drift and the bounding box of the ball could be smaller or bigger than the truth. Another disadvantage is that
the implementation of this tracker was using the CPU.

For these reasons I decided to do some research and found out about the (DaSiamRPN)[https://arxiv.org/abs/1808.06048] tracker. This tracker is implemented in OpenCV 4.8 with support for Cuda and GPU acceleration. This is when I decided to create a docker image whose Dockerfile builds OpenCV from source with Cuda support. This image starts from an image with Ubuntu 20.04, Cuda 11.4.3 and Cudnn 8 The Dockerfile can be found [here](https://github.com/jeferal/servo_platform_docker/blob/main/servo_platform_build/Dockerfile). [This post](https://jeferal.github.io/projects/13_project/) explains a little bit more about the build process and benchmark of algorithms using [OpenCV Zoo](https://github.com/opencv/opencv_zoo).

This tracker was giving much better results and releasing the CPU from doing this.

The implementation of the ball position control is based on a simple PID controller for the x and y axes. The implementation was quite stright forward.

## Hand teleoperation
Once the ball position control was implemented, I was looking for a nice way for a human to interact with the system.
I decided that it would be nice to use the hand to control the ball position as if some one was grasping it. For this reason, I made use of the mediapipe model to get the landmarks of the hand, and then given the position of the first finger and thumb tips I created a quick script to "pinch grasp" and move it around. The output of the position of the pinch grasp is filtered to avoid sudden changes in the set point and make it look as a more natural movement.

This is the control diagram:

<p align="center">
  <img width="550" height="400" src="/assets/img/servo_platform_mediapipe_diagram_definitive.jpg">
</p>
<div class="caption">
    Control diagram with teleoperation control.
</div>

## Results
The results can be seen in the following video:
<p align="center">
    <iframe width="728" height="410"
    src="https://www.youtube.com/embed/X9RQbtMX3PQ?si=42gEcN67TXZgx2ke"
    title="YouTube video player"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</p>
<div class="caption">
Ball control and teleoperation results.
</div>

## Next steps
This is only the beginning and there are many algorithms that could be implemented. These are the next steps I have planned:
* Fill in the doxygen in the code.
* Implement a simulation to be able to train RL algorithms to control the ball position.
* Create a gym environment.
* Train and evaluate different RL algorithms.
* Deploy the software on a Jetson Nano and use its GPU.
* Use this platform to solve ball mazes.

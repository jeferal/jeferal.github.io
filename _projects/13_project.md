---
layout: page
title: Accelerate OpenCV with GPU, CUDA and CUDNN
description: Compiling OpenCV 4.8.0 with CUDA and CUDNN support and benchmarking with OpenCV Zoo.
img: assets/img/dasiam_inference.png
importance: 1
category: work
---

I was looking into what object tracker to use for a project and encountered that there is a very
interesting tracker `DaSiamRPN` that is implemented in `OpenCV 4.8.0` that can make use of the GPU to accelerate the inference time.

I decided therefore to create a docker image with the following specifications:
* Cuda 11.4.3
* Cudnn 8
* Ubuntu 20.04

And then build OpenCV 4.8.0 from source. This is a link to the [Dockerfile](https://github.com/jeferal/servo_platform_docker/blob/main/servo_platform_build/Dockerfile).

The Dockerfile starts from the base image `nvidia/cuda:11.4.3-cudnn8-devel-ubuntu20.04`, installs the basic dependencies that are needed to build OpenCV, creates a virtual environment with Python 3.8 and then builds OpenCV with the following flags:

```
-D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D WITH_TBB=ON \
-D ENABLE_FAST_MATH=1 \
-D CUDA_FAST_MATH=1 \
-D WITH_CUBLAS=1 \
-D WITH_CUDA=ON \
-D BUILD_opencv_cudacodec=OFF \
-D WITH_CUDNN=ON \
-D OPENCV_DNN_CUDA=ON \
-D CUDA_ARCH_BIN=8.6 \
-D WITH_V4L=ON \
-D WITH_QT=OFF \
-D WITH_OPENGL=ON \
-D WITH_GSTREAMER=ON \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D OPENCV_PC_FILE_NAME=opencv.pc \
-D OPENCV_ENABLE_NONFREE=ON \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D OPENCV_PYTHON3_INSTALL_PATH=/opt/opencv-venv/lib/python3.8/site-packages \
-D PYTHON_EXECUTABLE=/opt/opencv-venv/bin/python3 \
-D INSTALL_C_EXAMPLES=ON \
-D OPENCV_EXTRA_MODULES_PATH=/opt/opencv_contrib/modules \
-D BUILD_EXAMPLES=ON
```

There is one thing to take into accound here, which is the CUDA_ARCH_BIN flag. This is very important because if someone fails to set this flag correctly, the code might build successfully, but when executing it might not make use of GPU acceleration and would use the CPU. This flag is used to specify the GPU architecture that will be used to compile the code and it is important to specify the correct one. In my case, I
have a 3060. If you are not sure which one to use, you can execute:
```
nvidia-smi
```
You should get an output like this with your particular GPU model:
```
GPU 0: NVIDIA GeForce RTX 3060 Laptop GPU
```
Once you know your model, Nvidia provides tables with the corresponding architecture. In my case, I found it [here](https://developer.nvidia.com/cuda-gpus). The architecture for the 3060 is 8.6.

After building OpenCV together with the contrib modules, I installed [here](https://github.com/opencv/opencv_zoo) to run
some benchmarks and see how much faster the inference time is when using the GPU. These benchmarks also helped me to test
if the code that I built was acctually using the GPU or not.

This is a comparison of the preprocessing + inference + post processing time between the GPU and CPU for several algorithms:

<p align="center">
  <img src="/assets/img/table_benchmark.png">
</p>
<div class="caption">
    Results of the benchmark. GPU (Cuda16p) vs CPU
</div>

The GPU benchmark has been run with the following command:
```
python3 benchmark.py --all --fp32 --cfg_exclude wechat --cfg_overwrite_backend_target 2
```
backend=cv.dnn.DNN_BACKEND_CUDA

target=cv.dnn.DNN_TARGET_CUDA_FP16

And the CPU with:
```
python3 benchmark.py --all --fp32 --cfg_exclude wechat --cfg_overwrite_backend_target 0
```
backend=cv.dnn.DNN_BACKEND_OPENCV

target=cv.dnn.DNN_TARGET_CPU

The runs that have used the GPU are likely to be faster than using CPU. However, there are some cases where the CPU is faster than the GPU. This depends on how big the model is and how pararellised the algorithm is. In my case I was interested in DaSiamRPN, which is
around four times faster when using the GPU.

This is an image of the tracker in action:

<p align="center">
  <img src="/assets/img/dasiam_inference.png">
</p>
<div class="caption">
    Trying out the DaSiamRPN tracker.
</div>

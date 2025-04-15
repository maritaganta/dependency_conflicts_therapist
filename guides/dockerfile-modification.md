# Modifying Dockerfiles for you specific application 

In this guide you will learn how to modify Dockerfiles in order to ensure the installation of the libraries you would like to include in your image.

Specifically, the docker image built for ROS development in the [ros-introduction GitHub Repo](https://github.com/MRAC-IAAC/ros-introduction) from [Huanyu Li](https://www.linkedin.com/in/huanyu-li-457590268/) will be used and the main things to change are:

1. The name of the image
2. The packages to be installed

Requirements to follow this exact demo is Ubuntu LTS 20.04 or 22.04, and docker installed.

# Modify existing Docker image

First you will need to modify the name of the image as it appears in the core files for building and running docker images, the `build_image.sh` and the `run_user.sh` files.

In the `ros-introduction`repo from Huanyu Li, there is a `.docker` folder which contains the fundamental files of a docker image. First you need to change the name so that you can ensure you don't overwrite the old image with the new image.

Specifically, change the `build_image.sh` and the `run_user.sh` files to instead of the name `ros_introduction:latest` to `<name_image_name>:latest`.

# Modify the packages to be installed

The Dockerfile is a centralised document where all of the installation commands are written in a sequential manner. Specifically, observe the commands that follow `RUN`.

Notice how `python` is installed, along with other helpful things like `git` or `terminator`.

```
RUN apt-get update && apt-get install -y --no-install-recommends\
    ssh \
    git \
    curl \
    wget \
    build-essential \
    cmake \
    python3-pip \
    python3-flake8 \
    terminator \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
```

Additionally, ROS is installed as:

```
ARG ROS_DISTRO=noetic

FROM ros:$ROS_DISTRO-ros-base

ARG ROS_DISTRO

RUN apt-get update && apt-get install -y --no-install-recommends\
    pkg-config \
    python3-catkin-tools \
    python3-rosdep \
    python3-rosinstall-generator \
    iputils-ping \
    ros-$ROS_DISTRO-rqt \
    ros-$ROS_DISTRO-rqt-common-plugins \
    ros-$ROS_DISTRO-rqt-robot-plugins \
    ros-$ROS_DISTRO-roslint \
    ros-$ROS_DISTRO-rqt-gui \
    ros-$ROS_DISTRO-rqt-gui-py \
    ros-$ROS_DISTRO-rqt-py-common \
    ros-$ROS_DISTRO-rviz \
    ros-$ROS_DISTRO-ecl-threads \
    ros-$ROS_DISTRO-ecl-geometry \
    ros-$ROS_DISTRO-ecl-streams \
    ros-$ROS_DISTRO-diagnostics \
    ros-$ROS_DISTRO-turtlesim \
    ros-$ROS_DISTRO-ros-tutorials \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
```

The above are the exact commands that you would need to run on your system to install ROS.


Now let's assume that you want to install specific python packages. You just add a `RUN` block, like so:

```
RUN pip3 install --no-cache-dir --upgrade pip\
    argcomplete \
    flake8-blind-except \
    flake8-builtins \
    flake8-comprehensions \
    flake8-deprecated \
    flake8-return \
    flake8-length \
    flake8-todo \
    flake8-quotes \
    black \
    importlib-metadata \
    setuptools \
    requests \
    ws4py \
    numpy==1.22.0 \       # Here we install numpy specific version
    opencv-python \       # Here we install opencv
    pyquaternion \
    pyyaml \
    pytransform3d \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
```

In the lines you can add any python library you want. Copy this `RUN` block under the other `RUN` block and before the rest of the setup in the Dockerfile and you are good to `build_image.sh` and `run_user.sh`

---
title: "Install ROS Melodic on Raspberry Pi4"
date: 2020-05-05
categories:
  - ROS
tags:
  - Pi4
  - ROS
---

### This is tutorial how to install ROS Melodic on Raspberry Pi4B.

## 1.Install Ubuntu 18.04 on Pi4

## 2.Install ROS Melodic on Pi4
### 2.1.Setup ROS Repositories
```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

### 2.2. make sure your Debian package index is up-to-date:
```
$ sudo apt-get update
$ sudo apt-get upgrade 
```

### 2.3.Install Bootstrap Dependencies
```
$ sudo apt install -y python-rosdep python-rosinstall-generator python-wstool python-rosinstall build-essential cmake
```

### 2.4.Initializing rosdep
```
$ sudo rosdep init
$ rosdep update
```

### 2.5.Installation
Now, we will download and build ROS Melodic.

- Create a catkin Workspace
- In order to build the core packages, you will need a catkin workspace. Create one now:

```
$ mkdir -p ~/ros_catkin_ws
$ cd ~/ros_catkin_ws
```

- Next we will want to fetch the core packages so we can build them. We will use wstool for this. Select the wstool command for the particular variant you want to install:

ROS-Comm: (recommended) ROS package, build, and communication libraries. No GUI tools.
```
$ rosinstall_generator ros_comm --rosdistro melodic --deps --wet-only --tar > melodic-ros_comm-wet.rosinstall
$ wstool init src melodic-ros_comm-wet.rosinstall
```

Desktop: ROS, rqt, rviz, and robot-generic libraries
```
$ rosinstall_generator desktop --rosdistro melodic --deps --wet-only --tar > melodic-desktop-wet.rosinstall
$ wstool init src melodic-desktop-wet.rosinstall
```

- This will add all of the catkin or wet packages in the given variant and then fetch the sources into the ~/ros_catkin_ws/src directory. The command will take a few minutes to download all of the core ROS packages into the src folder. The -j8 option downloads 8 packages in parallel.


- So far, only the ROS-Comm variant has been tested on the Raspberry Pi in Melodic; however, more are defined in REP 131 such as robot, perception, etc. Just change the package path to the one you want, e.g., for robot do:
```
$ rosinstall_generator robot --rosdistro melodic --deps --wet-only --tar > melodic-robot-wet.rosinstall
$ wstool init src melodic-robot-wet.rosinstall
```

Please feel free to update these instructions as you test more variants.

If wstool init fails or is interrupted, you can resume the download by running:
```
wstool update -j4 -t src
```
### Resolve Dependencies
Before you can build your catkin workspace, you need to make sure that you have all the required dependencies. We use the rosdep tool for this.

#### Resolving Dependencies with rosdep
The dependencies should be resolved by running rosdep:
```
$ cd ~/ros_catkin_ws
$ rosdep install -y --from-paths src --ignore-src --rosdistro melodic -r --os=debian:buster
```

This will look at all of the packages in the src directory and find all of the dependencies they have. Then it will recursively install the dependencies.

The --from-paths option indicates we want to install the dependencies for an entire directory of packages, in this case src. The --ignore-src option indicates to rosdep that it shouldn't try to install any ROS packages in the src folder from the package manager, we don't need it to since we are building them ourselves. The --rosdistro option is required because we don't have a ROS environment setup yet, so we have to indicate to rosdep what version of ROS we are building for. Finally, the -y option indicates to rosdep that we don't want to be bothered by too many prompts from the package manager.

After a while rosdep will finish installing system dependencies and you can continue.

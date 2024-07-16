# ORB-SLAM3 ROS2 Wrapper Docker

This repository contains a dockerized comprehensive wrapper for ORB-SLAM3 on ROS 2 Humble for Ubuntu 22.04.

![ORBSLAM3-GIF](orbslam3.gif)

# Steps to use this wrapper

IMPORTANT: The embedded Jetson boards run out of ram when building with the flags "make -j6". Removing the -j6 will work but result in longer build times. There is the possibility to run out of swap as well on the older Jetson boards, for this you can attempt to increase the swap size (zram): https://jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/

## 1. Clone this repository

1. ```git clone https://github.com/dirksavage88/ORB-SLAM3-ROS2-Docker.git```
2. ```cd ORB-SLAM3-ROS2-Docker```
3. ```git submodule update --init --recursive --remote```

## 2. Install Docker on your system

```bash
cd ORB-SLAM3-ROS2-Docker
sudo chmod +x container_root/shell_scripts/docker_install.sh
./container_root/shell_scripts/docker_install.sh
```

## 3. Build the image with ORB_SLAM3

1. Build the image: ```sudo docker build -t orb-slam-3-ros2:v3 -f Dockerfile.orbslam3 .```
2. Add ```xhost +``` to your ```.bashrc``` to support correct x11-forwarding using ```echo "xhost +" >> ~/.bashrc```
3. ```source ~/.bashrc```
4. You can see the built images on your machine by running ```sudo docker images```.

## 4. Running the container

1. ```cd ORB-SLAM3-ROS2-Docker``` (ignore if you are already in the folder)
2. ```sudo docker-compose run orb_slam3_ros2```

## 5. Building the ORB-SLAM3 Wrapper

The first step is to modify the CmakeList in ORB_SLAM3_ROS2, OpenCV must be added to the target dependencies for each executable. Also set the paths for ORB SLAM3 install directory and python modules path in CMakeModules/* and CMakeList respectively.

Launch the container using steps in (4).
```bash
cd /home/colcon_ws/
. /opt/ros/humble/setup.bash
colcon build --symlink-install
source install/setup.bash
```

## Launching ORB-SLAM3
Play a ros2 bag and remap to /image or change the orb slam input image topic via parameter.

Example for monocular

```ros2 run orbslam3 mono /home/colcon_ws/src/ORB_SLAM3_ROS2/vocabulary/ORBvoc.txt /home/colcon_ws/src/ORB_SLAM3_ROS2/config/monocular/TUM1.yaml /home/colcon_ws/src/ORB_SLAM_ROS2/config/monocular/rpi.yaml --ros-args -p image:=/resize/image_raw```

Example for Stereo
```
ros2 run orbslam3 stereo /home/colcon_ws/src/ORB_SLAM3_ROS2/vocabulary/ORBvoc.txt /home/colcon_ws/src/ORB_SLAM3_ROS2/config/stereo/TUM-VI.yaml false
```


## Important notes


The very initial versions of this code were derived from [thien94/orb_slam3_ros_wrapper](https://github.com/thien94/orb_slam3_ros_wrapper) and [zang9/ORB_SLAM3_ROS2](https://github.com/zang09/ORB_SLAM3_ROS2)

# ArUco Pose Estimation with ROS2 and Intel Realsense D435

This repository contains the methodology for setting up and running ArUco Pose Estimation using an Intel Realsense D435 camera with ROS2. This setup utilizes the `ros2-aruco-pose-estimation` package for detecting and estimating the pose of ArUco markers.

## Table of Contents
1. [System Requirements](#system-requirements)
2. [Installation](#installation)
    - [1. Install ROS2 Iron](#install-ros2-iron)
    - [2. Install Realsense2 Camera Package](#install-realsense2-camera-package)
    - [3. Install OpenCV and Transforms Libraries](#install-opencv-and-transforms-libraries)
    - [4. Clone ArUco Pose Estimation Package](#clone-aruco-pose-estimation-package)
3. [Running ArUco Pose Estimation](#running-aruco-pose-estimation)
4. [Troubleshooting](#troubleshooting)

---

## System Requirements
- **PC**: Ubuntu 22.04 LTS
- **ROS2**: Iron
- **Camera**: Intel Realsense D435
- **Packages**:
  - `ros2-aruco-pose-estimation`
  - OpenCV
  - `tf2_ros`
- **Visualization**: RViz

---

## Installation

### 1. Install ROS2 Iron
Follow the [official ROS2 Iron installation guide](https://docs.ros.org/en/iron/Installation.html) for Ubuntu 22.04.

Once installed, source the ROS2 Iron environment:
```bash
source /opt/ros/iron/setup.bash
```
### 2. Install Realsense2 Camera Package
Install the Realsense2 camera package for ROS2 Iron:
```bash
sudo apt install ros-iron-realsense2-camera
```
### 3. Install OpenCV and Transforms Libraries
Install OpenCV and necessary transforms libraries for ROS2:
```bash
sudo apt install python-opencv
sudo apt install ros-iron-tf2-ros
```
### 4. Clone ArUco Pose Estimation Package
#### Install OpenCV and necessary transforms libraries for ROS2:
```bash
sudo apt install python-opencv
sudo apt install ros-iron-tf2-ros
```
#### Clone the ArUco pose estimation repository:
```bash
git clone ...
cd ...
```
#### Build the package:
```bash
colcon build --symlink-install
```
#### Source the setup file:
```bash
source ~/.../install/setup.bash
```
---

## Running ArUco Pose Estimation
### Launch the ArUco Pose Estimation Node
```bash
ros2 launch aruco_pose_estimation aruco_pose_estimation.launch.py marker_size:=0.1 aruco_dictionary_id:=DICT_5X5_250 camera_frame:=camera_link
```
---

## Troubleshooting

### 1. Topic Mismatch (No Pose Data)

**Error**: No pose data being received.

**Solution**:
- First, verify that the ArUco marker detection is running by checking the `/aruco/image` topic for image data:
```bash
    ros2 topic echo /aruco/image
```
- If there is no data or no ArUco markers detected, make sure the camera is correctly publishing the image data. You can verify the camera feed using:
```bash
    ros2 run rqt_image_view rqt_image_view
```
- Next, check if the topics for ArUco markers and poses are properly configured. You can do this by running:
```bash
    ros2 topic info /aruco/markers
    ros2 topic info /aruco/poses
```
These topics should show the relevant data being published.
- Ensure that the parameters used in the launch file are correct. Double-check the dictionary type (e.g., `DICT_5X5_250`) and marker size.
---

### 2. Invalid Message Type (aruco_interfaces/msg/ArucoMarkers)

**Error**: "The message type 'aruco_interfaces/msg/ArucoMarkers' is invalid."

**Solution**:
- This error indicates that the `aruco_interfaces` package is not properly built or the message types are outdated.
- To resolve this issue, rebuild the `aruco_interfaces` package:
```bash
    colcon build --packages-select aruco_interfaces
```
- After building, source the new setup path to ensure the correct message types are available in your workspace:
```bash
    source ~/.../aruco_interfaces/install/setup.bash
```
- Finally, re-launch the ArUco Pose Estimation node:
```bash
    ros2 launch aruco_pose_estimation aruco_pose_estimation.launch.py marker_size:=0.1 aruco_dictionary_id:=DICT_5X5_250 camera_frame:=camera_link
```

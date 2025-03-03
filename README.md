# Target Detection package for ROS

## Overview

This is a ROS package developed for **Target object detection** using RGB-D camera.

It is assumed that we have pre-trained Object detection system such as YOLO or OpenPose.


## Dependencies

### Azure Kinect SDK
If you want to use Azure Kinect as RGBD camera, you should install Kinect SDK.

#### Install Azure Kinect SDK following https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md
```
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
sudo apt update
sudo apt install -y libk4a1.3 libk4a1.3-dev k4a-tools
```

#### clone Azure Kinect ROS drivers, compile packages in your workspace
```
git clone https://github.com/microsoft/Azure_Kinect_ROS_Driver
catkin build
```

If you have some problem, please follow instrunctios from https://github.com/microsoft/Azure-Kinect-Sensor-SDK


### YOLO

To compile this package, you need to build [darknet_ros_msgs](https://github.com/leggedrobotics/darknet_ros/tree/master/darknet_ros_msgs)

Pleae follow instructions from https://github.com/leggedrobotics/darknet_ros

For more information about YOLO, Darknet, available training data and training YOLO see the following link: [YOLO: Real-Time Object Detection](http://pjreddie.com/darknet/yolo/).

### Test

1) launch rgbd camera node
2) target_frame_id (e.g.,"map") should exist. 
```
roslaunch azure_kinect_ros_driver driver.launch
roslaunch darknet_ros yolov_3.launch
rosrun pointcloud_processing pcl_processing
```

### Test with Lidar
0) Complie packages: pointcloud_processing, transform_pointcloud, rviz_tools_py
1) launch rgb camera node (or robot)
2) launch darknet_ros (or retinanet_ros)
```
roslaunch darknet_ros yolov_3.launch
```
3)  launch transform_pointcloud (this node will convert frame_id of pcl data), 
```
roslaunch transform_pointcloud transform.launch
```
- check pcl data is in target frame_id in launch file and output topic name is changed to : pointcloud_transformer/output_pcl2

4) rosrun rviz_tools_py fov_demo.py
- Parameter should be set: the width, hegiht resolution of rgb image,and the frame_id of rgb image 
- You should change these value in fov_demo.py
- WIDTH_RES, HEIGHT_RES, _SENSOR_TF

6) roslaunch pointcloud_processing sep_processing.launch

Also need to test the launch file target_detection/launch/pose_Estimation.launch
which appeared to have the correct remap args

- current target class is chair. you might change this clas in sep_processing.cpp directly at this moment. will work on configuration later.

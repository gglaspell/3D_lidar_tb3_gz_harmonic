# 3D_lidar_tb3_gz_harmonic
Files to add a 3D lidar to a turtlebot3 in Gazebo Harmonic

## Assumptions

- `ros-jazzy-desktop-full` is installed

- `gz sim --versions` is 8.9.0

- `ros-jazzy-turtlebot3-gazebo` is installed

- modifications were made to the tb3 burger, but could be adpated to waffle or waffle_pi

## Setup

Note, this was done in a docker container.

### Vulkan driver

Within the docker container, when running the command `gz sim  shapes.sdf --render-engine ogre2` I recieved the following error:

`MESA: error: ZINK: vkCreateInstance failed (VK_ERROR_INCOMPATIBLE_DRIVER)` 

The fix was to install vulkan drivers inside docker container.

```
sudo apt update

sudo apt install mesa-vulkan-drivers
```

Quick checks to ensure vulkan driver works.

```
vulkaninfo

gz sim  shapes.sdf --render-engine ogre2
```

### Gz bridge

Next, add the pointcloud topic to gz bridge. The working directory for the file `turtlebot3_burger_bridge.yaml` is `/opt/ros/jazzy/share/turtlebot3_gazebo/params`. Overwite the exisiting file with the file from this repo.

```
cd <this repoo>

sudo cp turtlebot3_burger_bridge.yaml /opt/ros/jazzy/share/turtlebot3_gazebo/params
```


### Turtlebot urdf

Next, replace the file `model.sdf` in the directory `/opt/ros/jazzy/share/turtlebot3_gazebo/models/turtlebot3_burger` with the file in this repo.

```
cd <this repoo>

sudo cp model.sdf /opt/ros/jazzy/share/turtlebot3_gazebo/models/turtlebot3_burger
```


## Run

The following commands are used to launch the simulation

### Terminal 1:

```
export TURTLEBOT3_MODEL=burger

ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

### Terminal 2:

```
ros2 run tf2_ros static_transform_publisher "0" "0" "0" "0" "0" "0" "burger/base_scan/hls_lfcd_lds" "base_scan"
```

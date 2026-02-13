# Differential Drive Robot – ROS 2 Mapping and Navigation

## Project Overview

This project demonstrates a Differential Drive Robot simulated in Gazebo using ROS 2 on Ubuntu 24.04 LTS. The robot is capable of performing 2D mapping using SLAM Toolbox and autonomous navigation using the Navigation2 (Nav2) stack.

The complete implementation, including launch files, configuration files, and robot description, is available in this repository:



## System Environment

Operating System: Ubuntu 24.04 LTS  
ROS Version: ROS 2  
Simulation Tool: Gazebo  
Visualization Tool: RViz2  

### Main ROS 2 Packages Used

- SLAM Toolbox  
- Navigation2 (Nav2)  
- robot_state_publisher  
- diff_drive_controller  


## Repository Structure

The repository contains the following major components:

- `src/` – Source packages for the robot and navigation  
- `launch/` – Launch files for simulation, SLAM, and navigation  
- `config/` – Parameter files for SLAM and Nav2  
- `maps/` – Saved occupancy grid maps  
- `urdf/` – Robot description files  

All implementation files are maintained inside the repository for reproducibility.


## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/Challa200Santhosh/Diff_Drive_Robot
cd Diff_Drive_Robot
```

### 2. Install Dependencies

```bash
rosdep install --from-paths src --ignore-src -r -y
```

### 3. Build the Workspace

```bash
colcon build
source install/setup.bash
```


## Execution Procedure

### Launch Simulation

Launch the robot in Gazebo using the appropriate launch file from the `launch` directory.

```bash
ros2 launch <package_name> <simulation_launch_file>.py
```

### Perform SLAM Mapping

```bash
ros2 launch <package_name> <slam_launch_file>.py
```

The robot can be manually controlled to explore the environment and generate a 2D occupancy grid map.

### Save the Map

```bash
ros2 run nav2_map_server map_saver_cli -f <map_name>
```

The map files will be stored in the `maps/` directory.

### Run Autonomous Navigation

```bash
ros2 launch <package_name> <navigation_launch_file>.py
```

Using RViz2, goal positions can be set, and the robot will navigate autonomously using the Navigation2 stack.


## Challenges Encountered

During development, several issues were encountered:

- Missing dependency packages during `colcon build`
- Command-line syntax errors and incorrect launch arguments
- Gazebo launch failures
- TF frame mismatches between `map`, `odom`, and `base_link`
- Navigation instability due to parameter tuning


## Problem Resolution Approach

Most issues were resolved by:

- Installing missing dependencies using `rosdep`
- Carefully analyzing terminal error logs
- Rebuilding the workspace multiple times
- Ensuring correct sourcing of the workspace
- Verifying file paths and configuration parameters
- Referring to official ROS 2 documentation

Repeated testing and systematic debugging ensured a stable and functional implementation.


## Results

The following objectives were successfully achieved:

- ROS 2 environment configured on Ubuntu 24.04  
- Robot successfully spawned in Gazebo  
- Map generated using SLAM Toolbox  
- Map saved and loaded into Navigation2  
- Autonomous navigation to multiple goal positions  
- Organized and reproducible project structure  


## Demonstration

A screen recording video demonstrates:

- Robot spawning in Gazebo  
- Live mapping using SLAM  
- Map saving  
- Autonomous navigation to selected goals  


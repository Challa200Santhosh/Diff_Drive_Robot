# 🤖 Differential-Drive Robot — ROS 2 SLAM Mapping & Nav2 Autonomous Navigation

![ROS 2](https://img.shields.io/badge/ROS-2-22314E)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-E95420)
![Gazebo](https://img.shields.io/badge/Simulator-Gazebo%20(gz--sim)-orange)
![Nav2](https://img.shields.io/badge/Navigation-Nav2-blue)
![SLAM Toolbox](https://img.shields.io/badge/SLAM-slam__toolbox-green)

A differential-drive robot (TurtleBot3 Burger model) simulated in **Gazebo** with **ROS 2 on Ubuntu 24.04**. The robot builds a 2D occupancy-grid map with **SLAM Toolbox**, fuses wheel odometry and IMU with an **EKF (robot_localization)**, and navigates autonomously to goals with **Nav2**.

🎥 **Demos:** [mapping run](robot_mapping_video.mp4) · [autonomous navigation run](Diff-Drive-Robot_navigation.mp4) · 📄 [project report](Diff_Drive_robot_report.pdf)

---

## 🧭 System architecture

```
 Gazebo (gz-sim) ──ros_gz_bridge──►  /scan  /imu  /robot/odom  /clock  /joint_states
        ▲                                     │
        │ /cmd_vel                            ▼
   twist_mux ◄── Nav2 (/cmd_vel)       imu_filter_madgwick ─► EKF (robot_localization) ─► odom → base_link
        ▲                                     │
        └── keyboard (/cmd_vel_key)           ▼
                                   SLAM Toolbox (online async) ─► /map  ─►  Nav2 (AMCL + planners + controller)
```

## 📦 Packages (`src/`)

| Package | Purpose |
| :--- | :--- |
| `tb3_description` | Robot model (URDF/xacro): chassis, wheels, 2D GPU LiDAR on `/scan`, IMU on `/imu` |
| `robot_gazebo` | Spawns the robot in Gazebo, bridges topics with `ros_gz_bridge`, optional EKF odometry |
| `robot_custom_localization` | EKF configuration (`ekf.yaml`) fusing `/robot/odom` + filtered IMU |
| `robot_mapping` | SLAM Toolbox online-async mapping (`mapper_params_online_async.yaml`) + RViz config |
| `robot_navigation` | Nav2 bringup (localization + navigation), `twist_mux`, saved map `maps/cafe1.yaml` |

## ⚙️ Build

```bash
mkdir -p ~/ros2_ws && cd ~/ros2_ws
git clone https://github.com/Challa200Santhosh/Diff_Drive_Robot.git .
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
source install/setup.bash
```

## ▶️ Run

```bash
# 1) Simulation (add use_ekf_odom:=true to use the EKF-fused odometry)
ros2 launch robot_gazebo gazebo.launch.py use_rviz:=true

# 2) Mapping with SLAM Toolbox, then drive the robot with the keyboard
ros2 launch robot_mapping mapping.launch.py use_sim_time:=true
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r cmd_vel:=cmd_vel_key

# 3) Save the map
ros2 run nav2_map_server map_saver_cli -f src/robot_navigation/maps/my_map

# 4) Autonomous navigation on the saved map (set goals with "Nav2 Goal" in RViz)
ros2 launch robot_navigation navigation.launch.py use_sim_time:=true
```

## 🛠️ Problems solved along the way

- **TF frame mismatches** between `map`, `odom` and `base_link` — traced from terminal logs and fixed in the configuration files.
- **Gazebo launch failures, missing dependencies and launch-argument errors** — resolved with `rosdep`, rebuilds and correct workspace sourcing.
- **Unstable navigation** — improved by tuning parameters in `nav2_params.yaml`.
- Keyboard and Nav2 velocity commands are arbitrated with **`twist_mux`** priorities.

## ✅ Results

- Robot spawns in Gazebo with LiDAR and IMU publishing through the bridge.
- A 2D occupancy-grid map of the world was built with SLAM Toolbox and saved.
- Nav2 localized on the saved map and navigated autonomously to several goal poses.

## 🙏 Credits

The `tb3_description` robot model is based on the ROBOTIS TurtleBot3 description (Apache-2.0, see `src/tb3_description/LICENSE`).

## 👤 Author

**Challa Santhosh** — Model-Based Design & Embedded AI Engineer  
[LinkedIn](https://www.linkedin.com/in/challa-santhosh-36693828a/) · [GitHub](https://github.com/Challa200Santhosh) · sschalla10@gmail.com

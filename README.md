# Basic Mobile Robot (URDF / ROS 2)

This is my implementation of a Udemy course on describing robots with **URDF / Xacro in ROS 2**.
It models a differential-drive mobile robot with a camera and a 2-joint arm, shows it in
RViz, and simulates it in Gazebo.

Built and tested with **ROS 2 Foxy** and **Gazebo Classic** (`gazebo_ros`).

## The robot (`udemy_bot`)

| Part | Details |
|---|---|
| Base | 0.6 × 0.4 × 0.2 m box |
| Drive | Two wheels (radius 0.1 m) and a low-friction caster; Gazebo `diff_drive` plugin publishing `odom` and TF |
| Camera | Front-mounted box with a Gazebo camera sensor at 10 Hz |
| Arm | Two revolute joints (`base_first_joint`, `first_second_joint`), each 0 – 90°; Gazebo joint-state publisher and joint-pose-trajectory plugins |

The Xacro file uses properties for all dimensions and macros for box, cylinder and sphere inertia.

## Packages

| Package | Content |
|---|---|
| `my_robot_description` | `urdf/my_robot.urdf.xacro`, an RViz config, and `display.launch.py` / `display.launch.xml` (robot_state_publisher + joint_state_publisher_gui + RViz) |
| `my_robot_bringup` | `my_robot_gazebo.launch.xml`: starts Gazebo, spawns the robot and opens RViz |

## Build and run

```bash
mkdir -p ~/robot_ws && cd ~/robot_ws
git clone https://github.com/M-Nassef/Udemy_Basic_Robot.git .
rm -rf build install log        # remove the build output that is committed in this repo
colcon build
source install/setup.bash

# View the model in RViz and move the arm joints with sliders
ros2 launch my_robot_description display.launch.xml

# Simulate in Gazebo
ros2 launch my_robot_bringup my_robot_gazebo.launch.xml

# Drive it (second terminal)
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

Dependencies: `xacro`, `robot_state_publisher`, `joint_state_publisher_gui`, `rviz2`,
`gazebo_ros_pkgs` and `teleop_twist_keyboard`.

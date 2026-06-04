# Lab 5 — Physics Simulation in Gazebo (Harmonic)

**Aim:** Add collision geometry and inertial properties to the robot model, spawn it in Gazebo Harmonic, attach the Differential Drive plugin, and control it with the keyboard.

---

## Theory

| Property | What It Does | Effect If Missing |
|----------|-------------|-------------------|
| `<collision>` | Physical boundary for contact detection | Robot clips through walls/floor |
| `<inertial>` | Mass + moment of inertia | Robot flies off or behaves unrealistically |
| Diff-Drive Plugin | Converts `/cmd_vel` → wheel velocities | Robot won't move |
| ROS-Gazebo Bridge | Routes topics between ROS 2 and Gazebo | Teleop won't reach the robot |

> **Jazzy Note:** Uses **Gazebo Harmonic** + `ros-jazzy-ros-gz` bridge. The plugin name changed from the Humble era.

---

## Package Structure

```
robot_description_pkg/
├── package.xml
├── setup.py
├── urdf/
│   └── robot.urdf.xacro    ← full physics model + plugin
└── launch/
    └── sim.launch.py
```

---

## Setup

### Install Simulation Packages (Jazzy + Gazebo Harmonic)
```bash
sudo apt update
sudo apt install \
  ros-jazzy-ros-gz \
  ros-jazzy-xacro \
  ros-jazzy-joint-state-publisher-gui \
  ros-jazzy-teleop-twist-keyboard -y
```

### Build
```bash
cd ~/ros2_ws
colcon build --packages-select robot_description_pkg
source install/setup.bash
```

---

## Running

**Terminal 1 — Launch the simulation:**
```bash
ros2 launch robot_description_pkg sim.launch.py
```
Wait for Gazebo to fully open and the robot to spawn.

**Terminal 2 — Drive the robot:**
```bash
source ~/ros2_ws/install/setup.bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

| Key | Action |
|-----|--------|
| `i` | Forward |
| `,` | Backward |
| `j` | Rotate left |
| `l` | Rotate right |
| `k` | Stop |

---

## What Changed from Lab 4?

| Lab 4 (RViz only) | Lab 5 (Gazebo Physics) |
|-------------------|----------------------|
| `<visual>` only | + `<collision>` + `<inertial>` |
| No physics engine | Gazebo Harmonic simulates physics |
| No movement | Diff-Drive plugin enables `/cmd_vel` control |
| No bridge needed | `ros_gz_bridge` connects ROS ↔ Gazebo |

---

## Conclusion

Adding inertial and collision properties transforms a visual model into a physics-aware digital twin. The Diff-Drive plugin + ROS-Gazebo bridge creates a complete control pipeline: keyboard → `/cmd_vel` → Gazebo → robot movement.

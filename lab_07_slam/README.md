# Lab 7 — Autonomous SLAM & Occupancy Grid Mapping

**Aim:** Configure and run a full SLAM (Simultaneous Localization and Mapping) pipeline using `slam_toolbox` with a simulated TurtleBot3 Waffle. Generate a 2D occupancy grid map and save it.

---

## Theory

### What is SLAM?
SLAM solves the chicken-and-egg problem of robotics:
- To build a map, you need to know where you are
- To know where you are, you need a map

`slam_toolbox` uses **graph-based SLAM** with **loop closure** — when the robot revisits a place, it corrects accumulated drift.

### Key Concepts

| Term | Meaning |
|------|---------|
| Occupancy Grid | 2D map where each cell = free (white) / occupied (black) / unknown (gray) |
| Loop Closure | Recognizing a previously visited place → correcting pose error |
| `base_footprint` | TurtleBot3's base frame (used instead of `base_link`) |
| `/scan` | LiDAR topic — input to SLAM |
| `use_sim_time` | Makes all nodes sync to Gazebo's `/clock` |

### Why We Patch the TurtleBot3 URDF
The TurtleBot3 URDF contains a literal `${namespace}` string that works in some setups but breaks the TF tree inside virtual machines. We strip it programmatically in the launch file.

---

## Package Structure

```
my_robot_slam/
├── package.xml
├── setup.py
├── config/
│   └── mapper_params.yaml    ← SLAM parameters
├── launch/
│   └── slam_launch.py        ← main launch file
└── my_robot_slam/
    └── __init__.py
```

---

## Setup

### 1. Install Required Packages (Jazzy)
```bash
sudo apt update
sudo apt install \
  ros-jazzy-slam-toolbox \
  ros-jazzy-nav2-map-server \
  ros-jazzy-turtlebot3 \
  ros-jazzy-turtlebot3-gazebo \
  ros-jazzy-teleop-twist-keyboard -y
```

### 2. Set TurtleBot3 Model (add to ~/.bashrc)
```bash
echo "export TURTLEBOT3_MODEL=waffle" >> ~/.bashrc
source ~/.bashrc
```

### 3. Build
```bash
cd ~/ros2_ws
colcon build --packages-select my_robot_slam
source install/setup.bash
```

---

## Execution — Follow This Order Strictly

**Terminal 1 — Gazebo World:**
```bash
export TURTLEBOT3_MODEL=waffle
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
> If Gazebo freezes (VM GPU issue), add `headless:=true` at the end.

**Terminal 2 — SLAM Pipeline:**
```bash
export TURTLEBOT3_MODEL=waffle
source ~/ros2_ws/install/setup.bash
ros2 launch my_robot_slam slam_launch.py
```

**Terminal 3 — Drive the Robot:**
```bash
export TURTLEBOT3_MODEL=waffle
ros2 run turtlebot3_teleop teleop_keyboard
```

| Key | Action |
|-----|--------|
| `w` | Forward (slow!) |
| `a` | Left |
| `d` | Right |
| `s` | Stop |

> **Drive slowly** (~0.05 m/s). Give SLAM time to process each scan and close loops.

---

## RViz2 Configuration

1. Bottom status bar: **ROS Time** must be non-zero and incrementing
2. **Global Options** → Fixed Frame → `map`
3. **Add** → Map → set topic to `/map`, Reliability: `Best Effort`
4. **Add** → LaserScan → topic `/scan`
5. **Add** → RobotModel → Description Topic: `/robot_description`

Watch gray cells (unknown) turn to white (free) as the robot explores!

---

## Save the Map

Once you have a complete map with closed loops:
```bash
cd ~/ros2_ws
ros2 run nav2_map_server map_saver_cli -f my_final_map
```

This creates:
- `my_final_map.pgm` — the grayscale map image
- `my_final_map.yaml` — metadata (resolution, origin, thresholds)

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| RViz time = 0 | Make sure Gazebo (Terminal 1) is running first |
| Red TF errors in RViz | Check `TURTLEBOT3_MODEL=waffle` is set in all terminals |
| Map not building | Confirm `/scan` topic exists: `ros2 topic echo /scan` |
| Gazebo GUI crashes | Add `headless:=true` to Terminal 1 command |
| `${namespace}` TF error | Launch file patches this automatically — rebuild if issue persists |

---

## Conclusion

SLAM is the foundation of autonomous navigation. This lab demonstrated a complete mapping pipeline: sensor data → SLAM algorithm → occupancy grid → saved map. The saved `.pgm` + `.yaml` files can be loaded directly into Nav2 for autonomous navigation in the next stage.

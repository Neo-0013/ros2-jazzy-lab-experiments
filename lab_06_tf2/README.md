# Lab 6 — Coordinate Frames & TF2 Transforms

**Aim:** Understand ROS 2's transform system (TF2), broadcast dynamic coordinate frames, and visualize the moving transform tree in RViz2.

---

## Theory

### What is TF2?
TF2 is ROS 2's **transform library**. Every robot has multiple coordinate frames:
- `odom` — fixed world frame (where the robot started)
- `base_link` — the robot's body center
- `left_wheel`, `right_wheel` — attached to joints

TF2 keeps track of how each frame relates to others **over time**. Any node can ask: *"Where is the lidar frame relative to the map frame right now?"*

### Dynamic vs Static Broadcasters
| Type | Use Case |
|------|----------|
| `StaticTransformBroadcaster` | Fixed transforms (e.g. sensor mounted on chassis) |
| `TransformBroadcaster` | Moving transforms (e.g. robot moving in world) |

### Why `use_sim_time` Matters
When running inside Gazebo, the clock comes from the simulator — not the system clock. Without syncing, TF2 timestamps will mismatch and frames will appear broken in RViz2. The `/clock` bridge + `use_sim_time: True` fixes this.

---

## Package Structure

```
lab_06_tf2/
├── robot_description_pkg/        ← robot model (reused from Lab 5)
│   ├── package.xml
│   ├── setup.py
│   ├── urdf/robot.urdf.xacro
│   └── launch/description.launch.py
└── tf_demo_pkg/                  ← TF broadcaster + master launch
    ├── package.xml
    ├── setup.py
    ├── tf_demo_pkg/
    │   └── dynamic_broadcaster.py
    └── launch/
        └── tf_viz.launch.py
```

---

## Setup

### Install Dependencies (Jazzy)
```bash
sudo apt update
sudo apt install \
  ros-jazzy-tf2-ros \
  ros-jazzy-tf2-tools \
  ros-jazzy-ros-gz \
  ros-jazzy-xacro -y
```

### Build Both Packages
```bash
cd ~/ros2_ws
colcon build --packages-select robot_description_pkg tf_demo_pkg
source install/setup.bash
```

---

## Running

**Single command launches everything** (Gazebo + robot + TF broadcaster + RViz2):
```bash
ros2 launch tf_demo_pkg tf_viz.launch.py
```

---

## RViz2 Setup

1. **Fixed Frame** → set to `odom`
2. **Add** → `TF` — shows all coordinate frame axes
3. **Add** → `RobotModel` — shows the 3D robot mesh
4. Watch the robot's frames animate in a circular path!

---

## Verify the TF Tree

In a new terminal:
```bash
# Generate a PDF of the entire TF tree
ros2 run tf2_tools view_frames

# Live TF echo between two frames
ros2 run tf2_ros tf2_echo odom base_link
```

---

## What the Dynamic Broadcaster Does

The `dynamic_broadcaster.py` node publishes a transform that makes `base_link` trace a **circular orbit** around `odom` every ~6 seconds. This demonstrates:
- How `TransformBroadcaster` works in Python
- How ROS time stamps are correctly used with sim time
- How quaternion math encodes rotation

---

## Conclusion

TF2 is the backbone of spatial awareness in ROS 2. Every sensor fusion algorithm, navigation stack, and manipulation planner depends on a clean, consistent TF tree. This lab demonstrated live dynamic broadcasting and verified the tree structure using `view_frames`.

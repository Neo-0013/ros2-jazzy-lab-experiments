# Lab 4 — Robot Modeling with URDF & Xacro

**Aim:** Design a differential drive mobile robot using URDF and simplify the model with Xacro macros. Visualize it in RViz2.

---

## Theory

- **URDF (Unified Robot Description Format):** XML format describing robot geometry, kinematics, and visuals.
- **Xacro (XML Macros):** A preprocessor for URDF. Lets you define reusable macros, properties (variables), and math expressions — avoiding copy-paste.
- **robot_state_publisher:** Reads the URDF and publishes transforms (TF) for every joint.
- **joint_state_publisher_gui:** Provides sliders to manually set joint angles for testing.

---

## Package Structure

```
robot_description_pkg/
├── package.xml
├── setup.py
├── urdf/
│   └── robot.urdf.xacro
└── launch/
    └── display.launch.py
```

---

## Setup

### 1. Install Dependencies (Jazzy)
```bash
sudo apt update
sudo apt install \
  ros-jazzy-robot-state-publisher \
  ros-jazzy-joint-state-publisher-gui \
  ros-jazzy-xacro \
  ros-jazzy-rviz2 -y
```

### 2. Create Package
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python robot_description_pkg \
  --dependencies rclpy robot_state_publisher joint_state_publisher_gui xacro
```

### 3. Build
```bash
cd ~/ros2_ws
colcon build --packages-select robot_description_pkg
source install/setup.bash
```

---

## Running

```bash
# If using VMware/VirtualBox (software rendering fix)
export LIBGL_ALWAYS_SOFTWARE=1

ros2 launch robot_description_pkg display.launch.py
```

---

## RViz2 Setup Steps

1. In **Global Options** → change `Fixed Frame` from `map` to `base_link`
2. Click **Add** (bottom left) → select **RobotModel**
3. Click **Add** again → select **TF**
4. Use the sliders in the `joint_state_publisher_gui` window to rotate the wheels
5. Watch the TF arrows update in real time in RViz2

---

## Conclusion

URDF + Xacro gives us a clean, parametric robot description. Xacro macros eliminated duplicate wheel link/joint definitions — adding a third wheel would only require one extra line. RViz2 confirmed the kinematic tree is correct.

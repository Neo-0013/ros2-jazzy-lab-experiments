<div align="center">

# 🤖 ROS 2 Jazzy — Lab Experiments

**A complete, hands-on lab series for mastering ROS 2 Jazzy Jalopy from scratch.**

*From installation to autonomous SLAM mapping — every concept, every line explained.*

[![ROS2](https://img.shields.io/badge/ROS2-Jazzy%20Jalopy-blue?style=for-the-badge&logo=ros)](https://docs.ros.org/en/jazzy/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04%20LTS-orange?style=for-the-badge&logo=ubuntu)](https://ubuntu.com/)
[![Python](https://img.shields.io/badge/Python-3.12-yellow?style=for-the-badge&logo=python)](https://python.org)
[![License](https://img.shields.io/badge/License-Apache%202.0-green?style=for-the-badge)](./LICENSE)
[![Stars](https://img.shields.io/github/stars/Neo-0013/ros2-jazzy-lab-experiments?style=for-the-badge)](https://github.com/Neo-0013/ros2-jazzy-lab-experiments/stargazers)

<br/>

> **Platform:** Ubuntu 24.04 LTS (Noble Numbat) &nbsp;|&nbsp; **ROS 2 Distro:** Jazzy Jalopy (LTS) &nbsp;|&nbsp; **Simulator:** Gazebo Harmonic

</div>

---

## 📖 About This Repository

This repo documents a **structured, ground-up journey through ROS 2** — built entirely from scratch without skipping the hard parts. Every lab is:

- ✅ **Updated for ROS 2 Jazzy** — not outdated Humble/Foxy tutorials
- ✅ **Ready to clone and run** — no broken steps, no missing files
- ✅ **Fully documented** — theory, procedure, commands, and troubleshooting
- ✅ **VM-friendly** — tested on VMware/VirtualBox with known fixes included

Whether you're a student starting out or an engineer migrating from Humble — this series has you covered.

---

## 📚 Lab Index

| # | Lab | Core Topics | Status |
|---|-----|-------------|--------|
| 01 | [🔧 Linux & ROS 2 Setup](./lab_01_setup/) | Ubuntu CLI, ROS 2 installation, environment sourcing, talker-listener demo | ✅ Complete |
| 02 | [📡 Publisher / Subscriber](./lab_02_pubsub/) | Topics, DDS middleware, rclpy, rqt_graph, QoS | ✅ Complete |
| 03 | [🔁 Services & Parameters](./lab_03_services/) | Request-Response pattern, runtime config, `ros2 param` CLI | ✅ Complete |
| 04 | [🦾 URDF & Xacro Modeling](./lab_04_urdf/) | Links, joints, Xacro macros, robot_state_publisher, RViz2 | ✅ Complete |
| 05 | [🌍 Gazebo Simulation](./lab_05_gazebo_sim/) | Inertia, collision, Diff-Drive plugin, teleop_twist_keyboard | ✅ Complete |
| 06 | [🗺️ TF2 Transforms](./lab_06_tf2/) | Transform trees, dynamic broadcasting, view_frames, quaternions | ✅ Complete |
| 07 | [🧭 Autonomous SLAM](./lab_07_slam/) | slam_toolbox, occupancy grids, TurtleBot3, map saving | ✅ Complete |

---

## ⚡ Quick Start

### Prerequisites

| Requirement | Details |
|-------------|---------|
| OS | Ubuntu 24.04 LTS (bare metal, VMware, or VirtualBox) |
| ROS 2 | Jazzy Jalopy — [install guide in Lab 1](./lab_01_setup/README.md) |
| Disk Space | ~10 GB free |
| RAM | 4 GB minimum, 8 GB recommended for Gazebo |

### 1 — Clone the Repository

```bash
git clone https://github.com/Neo-0013/ros2-jazzy-lab-experiments.git
cd ros2-jazzy-lab-experiments
```

### 2 — Create Your ROS 2 Workspace

```bash
mkdir -p ~/ros2_ws/src
```

### 3 — Copy a Lab Package and Build

```bash
# Example: Lab 2 — Publisher/Subscriber
cp -r lab_02_pubsub/my_pubsub_pkg ~/ros2_ws/src/

cd ~/ros2_ws
colcon build --packages-select my_pubsub_pkg
source install/setup.bash
```

### 4 — Run It

```bash
# Terminal 1
ros2 run my_pubsub_pkg talker

# Terminal 2
ros2 run my_pubsub_pkg listener
```

> 💡 **VM Users:** If RViz2 or Gazebo crashes, add this to your `~/.bashrc`:
> ```bash
> export LIBGL_ALWAYS_SOFTWARE=1
> ```

---

## 🗂️ Repository Structure

```
ros2-jazzy-lab-experiments/
│
├── README.md                        ← You are here
├── .gitignore
│
├── lab_01_setup/
│   └── README.md                    ← Full installation guide
│
├── lab_02_pubsub/
│   └── my_pubsub_pkg/
│       ├── package.xml
│       ├── setup.py
│       └── my_pubsub_pkg/
│           ├── talker_node.py
│           └── listener_node.py
│
├── lab_03_services/
│   └── service_param_pkg/
│       ├── package.xml
│       ├── setup.py
│       └── service_param_pkg/
│           └── server_node.py
│
├── lab_04_urdf/
│   └── robot_description_pkg/
│       ├── urdf/robot.urdf.xacro
│       └── launch/display.launch.py
│
├── lab_05_gazebo_sim/
│   └── robot_description_pkg/
│       ├── urdf/robot.urdf.xacro    ← + physics + Diff-Drive plugin
│       └── launch/sim.launch.py
│
├── lab_06_tf2/
│   ├── robot_description_pkg/
│   └── tf_demo_pkg/
│       ├── tf_demo_pkg/
│       │   └── dynamic_broadcaster.py
│       └── launch/tf_viz.launch.py
│
└── lab_07_slam/
    └── my_robot_slam/
        ├── config/mapper_params.yaml
        └── launch/slam_launch.py
```

---

## 🔑 ROS 2 Jazzy vs Humble — Key Differences

> Most tutorials online still use **Humble**. This repo is fully updated for **Jazzy**.

| Area | Humble (Old) | Jazzy (This Repo) |
|------|-------------|-------------------|
| Ubuntu Version | 22.04 Jammy | **24.04 Noble** |
| Gazebo | Ignition Fortress | **Gazebo Harmonic** |
| Package prefix | `ros-humble-*` | **`ros-jazzy-*`** |
| Gazebo bridge pkg | `ros-humble-ros-gz` | **`ros-jazzy-ros-gz`** |
| Gazebo plugin API | `ignition::gazebo::systems` | **`gz::sim::systems`** |
| Python version | 3.10 | **3.12** |
| Source command | `/opt/ros/humble/setup.bash` | **`/opt/ros/jazzy/setup.bash`** |

---

## 🧠 Concepts Covered

```
Communication     →  Topics, Services, Parameters, DDS, QoS policies
Robot Modeling    →  URDF, Xacro macros, Links, Joints, Inertia tensors
Simulation        →  Gazebo Harmonic, physics, Diff-Drive, sensor plugins
Transforms        →  TF2 tree, static & dynamic broadcasting, quaternions
Navigation        →  SLAM, occupancy grids, loop closure, map saving
Tooling           →  RViz2, rqt_graph, ros2 CLI, colcon
```

---

## 🛠️ Tech Stack

| Layer | Tools |
|-------|-------|
| **Middleware** | ROS 2 Jazzy, DDS (Fast-DDS) |
| **Simulation** | Gazebo Harmonic, RViz2 |
| **Language** | Python 3.12, XML/Xacro |
| **SLAM** | slam_toolbox, nav2_map_server |
| **Robot** | Custom Diff-Drive + TurtleBot3 Waffle (Lab 7) |
| **Build System** | colcon, ament_python |
| **OS** | Ubuntu 24.04 LTS |

---

## 🚀 What's Coming Next

This repo is a foundation. The next phase builds on top of it:

- [ ] **SecureBot** — ROS2 security audit robot (physical build + SROS2 hardening)
- [ ] **ROS2 Security Scanner** — open source tool to audit running ROS2 systems
- [ ] **SROS2 Lab** — certificate generation, access control, encrypted topics
- [ ] **Attack & Defense demos** — topic injection, node spoofing, DDS sniffing

---

## 👤 Author

**Nawaz (Neo)**
*B.Tech Student | Robotics & Cybersecurity Engineer in the making*
*GM University, Ranebennur, India*

[![GitHub](https://img.shields.io/badge/GitHub-Neo--0013-black?style=for-the-badge&logo=github)](https://github.com/Neo-0013)

*Building at the intersection of ROS 2, Cybersecurity, and Autonomous Systems.*

---

## 🤝 Contributing

Found a bug? Have a suggestion? PRs and Issues are welcome.

1. Fork the repo
2. Create your branch: `git checkout -b fix/your-fix-name`
3. Commit: `git commit -m "fix: description of fix"`
4. Push and open a Pull Request

---

## 📄 License

Licensed under the **Apache 2.0 License** — see [LICENSE](./LICENSE) for details.

---

<div align="center">

*If this helped you, drop a ⭐ — it helps others find it.*

</div>

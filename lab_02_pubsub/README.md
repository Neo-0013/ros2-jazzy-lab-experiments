# Lab 2 — Publisher / Subscriber Pattern in ROS 2

**Aim:** Implement the asynchronous Publish/Subscribe communication pattern using `rclpy`. Create a Talker (Publisher) and Listener (Subscriber) node and verify connectivity using `rqt_graph`.

---

## Theory

In ROS 2, nodes communicate via **Topics** — unidirectional message buses.

- **Publisher:** Sends data to a topic. Defines message type and frequency.
- **Subscriber:** Listens to a topic, using a **callback function** to process arriving messages.
- **DDS Middleware:** ROS 2 uses Data Distribution Service for automatic node discovery — no central master needed.

---

## Package Structure

```
my_pubsub_pkg/
├── package.xml
├── setup.py
└── my_pubsub_pkg/
    ├── __init__.py
    ├── talker_node.py
    └── listener_node.py
```

---

## Setup

### 1. Install Dependencies (Jazzy)
```bash
sudo apt update
sudo apt install ros-jazzy-rqt-graph -y
```

### 2. Create the Package
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python my_pubsub_pkg --dependencies rclpy std_msgs
```

### 3. Build
```bash
cd ~/ros2_ws
colcon build --packages-select my_pubsub_pkg
source install/setup.bash
```

---

## Running

Open **3 terminals**, all sourced with `source ~/ros2_ws/install/setup.bash`:

| Terminal | Command | What You'll See |
|----------|---------|-----------------|
| 1 | `ros2 run my_pubsub_pkg talker` | `Publishing: "Hello ROS 2: 0"`, `1`, `2`... |
| 2 | `ros2 run my_pubsub_pkg listener` | `I heard: "Hello ROS 2: 0"`, `1`, `2`... |
| 3 | `rqt_graph` | Visual graph showing nodes + topic bridge |

---

## Verification

```bash
# List all active topics
ros2 topic list

# See topic info
ros2 topic info /topic

# Echo raw messages
ros2 topic echo /topic

# Check publish frequency
ros2 topic hz /topic
```

---

## Conclusion

The Publisher/Subscriber pattern decouples the Talker from the Listener — neither knows about the other, they only share a topic name. This modularity is the foundation of scalable ROS 2 architectures.

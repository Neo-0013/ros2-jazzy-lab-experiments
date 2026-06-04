# Lab 3 — Services & Parameters in ROS 2

**Aim:** Implement the synchronous Request-Response pattern using ROS 2 Services and manage dynamic node configuration using ROS 2 Parameters.

---

## Theory

### ROS 2 Services
Unlike pub/sub (continuous stream), a Service follows a **Request → Response** model:
- **Server:** Offers a capability, waits for requests.
- **Client:** Sends a request and blocks until a response arrives.
- Ideal for discrete actions: "trigger sensor", "reset position", "add two numbers".

### ROS 2 Parameters
Parameters are **runtime configuration values** for a node (e.g., max speed, update rate). They can be changed **without restarting** the node — critical for tuning robot behavior live.

---

## Package Structure

```
service_param_pkg/
├── package.xml
├── setup.py
└── service_param_pkg/
    ├── __init__.py
    └── server_node.py
```

---

## Setup

### 1. Create Package
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python service_param_pkg \
  --dependencies rclpy example_interfaces
```

> `example_interfaces` provides the `AddTwoInts` service type used in this lab.

### 2. Build
```bash
cd ~/ros2_ws
colcon build --packages-select service_param_pkg
source install/setup.bash
```

---

## Running

### Task 1 — Test the Service

**Terminal 1:** Start the server
```bash
ros2 run service_param_pkg server
```

**Terminal 2:** Call the service manually
```bash
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 5, b: 10}"
```

Expected output:
```
response: example_interfaces.srv.AddTwoInts_Response(sum=15)
```

---

### Task 2 — Dynamic Parameter Configuration

**Terminal 3:** List parameters of running node
```bash
ros2 param list
```

Disable logging **at runtime** (no restart needed):
```bash
ros2 param set /minimal_service allow_logging false
```

Now call the service again from Terminal 2 — the server will stop printing request logs. Re-enable anytime:
```bash
ros2 param set /minimal_service allow_logging true
```

---

## Useful CLI Commands

```bash
# List all services
ros2 service list

# Get info about a service
ros2 service type /add_two_ints

# List all params for a node
ros2 param list /minimal_service

# Get a specific param value
ros2 param get /minimal_service allow_logging
```

---

## Conclusion

Services enable synchronous request-response logic in ROS 2. Parameters allow node behavior to be tuned at runtime without a rebuild or restart — essential for real-world robot configuration.

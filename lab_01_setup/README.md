# Lab 1 — Linux & ROS 2 Environment Setup

**Aim:** Familiarize with the Ubuntu 24.04 LTS environment, perform basic file management via the CLI, and successfully install and verify ROS 2 Jazzy Jalopy.

---

## 1. Software Required

| Component | Details |
|-----------|---------|
| Host OS | Windows / macOS / Linux |
| Hypervisor | VMware Workstation Player / VirtualBox |
| Guest OS | **Ubuntu 24.04 LTS (Noble Numbat)** |
| Middleware | **ROS 2 Jazzy Jalopy (Desktop)** |

---

## 2. Theory

### 2.1 Ubuntu 24.04 LTS (Noble Numbat)
Ubuntu 24.04 is the latest Long-Term Support release, providing 5 years of security updates. In robotics, stability is prioritized over bleeding-edge features to ensure motor controllers and sensor drivers don't break due to OS updates.

### 2.2 The Linux Kernel vs. User Space
- **Kernel:** Core of the OS — manages hardware (CPU, RAM, USB ports).
- **User Space:** Where applications like ROS 2 run. It uses **System Calls** to request resources from the Kernel.

### 2.3 ROS 2 Environment Sourcing
ROS 2 installs its scripts in `/opt/ros/jazzy`. To use ROS commands, the shell environment variables must be updated by "sourcing" the `setup.bash` file, which adds ROS paths to the terminal's memory.

---

## 3. Procedure

### Task 1: Basic Linux Navigation (CLI)

Open a terminal and run the following to create a structured workspace:

```bash
# Check current directory
pwd

# Create a folder tree
mkdir -p ~/ros2_ws/src

# Navigate to source folder
cd ~/ros2_ws/src

# Create a dummy script
touch hello_robot.py

# List with permissions
ls -l

# Grant execution permission
chmod +x hello_robot.py
```

---

### Task 2: ROS 2 Jazzy Installation

Execute the following commands **in order**.

#### Step 1 — Set Locale
```bash
sudo apt update && sudo apt install locales -y
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

sudo apt install software-properties-common -y
sudo add-apt-repository universe
```

#### Step 2 — Add ROS 2 GPG Key and Repository
```bash
sudo apt update
sudo apt install curl -y

sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
  http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" \
  | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

> ⚠️ **Note:** The `$()` command substitution above auto-detects your Ubuntu version (`noble` for 24.04) — do NOT hardcode `jammy` or `humble`.

#### Step 3 — Install ROS 2 Jazzy Desktop
```bash
sudo apt update
sudo apt upgrade -y
sudo apt install ros-jazzy-desktop -y
```

#### Step 4 — Install Development Tools
```bash
sudo apt install ros-dev-tools python3-colcon-common-extensions python3-rosdep -y
```

---

### Task 3: Automate the Environment

To avoid typing the source command every time, append it to your `.bashrc`:

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Verify it worked:
```bash
printenv | grep ROS
# You should see ROS_DISTRO=jazzy
```

---

## 4. Results & Verification

### The Communication Test

Open **two terminals**. If they communicate, your installation is successful.

| Terminal | Command | Expected Output |
|----------|---------|-----------------|
| Terminal A | `ros2 run demo_nodes_cpp talker` | `Publishing: 'Hello World: 1'`, `2`, `3`... |
| Terminal B | `ros2 run demo_nodes_py listener` | `I heard: [Hello World: 1]`, `2`, `3`... |

Press `Ctrl+C` in both terminals to stop.

---

## 5. Conclusion

Ubuntu 24.04 was configured and ROS 2 Jazzy Jalopy was installed successfully. Basic CLI proficiency was achieved and the installation was validated using the talker-listener demo nodes.

---

## 🔍 Troubleshooting

| Problem | Fix |
|---------|-----|
| `ros2: command not found` | Run `source /opt/ros/jazzy/setup.bash` |
| Nodes don't see each other | Make sure both terminals are sourced; check `ROS_DOMAIN_ID` |
| GPG key error | Re-run Step 2 curl command carefully |

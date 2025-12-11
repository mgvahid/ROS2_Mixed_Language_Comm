# ROS 2 Distributed Communication Platform (C++ Publisher / Python Subscriber)

This project demonstrates a fundamental multi-machine, mixed-language robotics system using ROS 2 Jazzy. It verifies network communication between an x86_64 development environment (Laptop) and an aarch64 embedded device (Raspberry Pi) using the DDS middleware.

---

## 💻 System Environment and OS Versions

| Device | Operating System | OS Version | Architecture | Role in Project |
| :--- | :--- | :--- | :--- | :--- |
| **Laptop** | Ubuntu (Host) | 24.04 (Noble Numbat) | `x86_64` | Primary Development / C++ Publisher |
| **Raspberry Pi** | Ubuntu Server | 24.04 (Noble Numbat) | `aarch64` | Build Target / Python Subscriber |
| **ROS 2 Core** | | Jazzy Jalisco | | Distributed Communication Middleware |

## 📦 Project Structure

The workspace contains two packages that communicate over the `/chatter` topic:
1.  **`cpp_publisher_pkg`**: `ament_cmake` package running on the Laptop.
2.  **`py_subscriber_pkg`**: `ament_python` package running on the RPi.

---

## 🛠️ Build and Configuration Details

### 1. Local Code Setup (Laptop)

The packages were created with core dependencies:
```bash
ros2 pkg create --build-type ament_cmake cpp_publisher_pkg --dependencies rclcpp std_msgs
ros2 pkg create --build-type ament_python py_subscriber_pkg --dependencies rclpy std_msgs

2. Critical Configuration FixesThe following files required manual updates to ensure successful compilation and execution:FileFix Descriptioncpp_publisher_pkg/CMakeLists.txtAdded add_executable, ament_target_dependencies, and install(TARGETS...) to register the C++ node.py_subscriber_pkg/setup.pyAdded packages=find_packages(...) and corrected the entry_points to resolve the ModuleNotFoundError.3. Build Sequence (Laptop)The build requires sourcing the core ROS 2 installation (/opt/ros/jazzy) before running colcon build.Bashcd ~/ros2_ws
source /opt/ros/jazzy/setup.bash
colcon build
source install/setup.bash # Source the overlay for running
🚀 Distributed Deployment1. Code TransferThe code was successfully transferred using Git clone via SSH to ensure version control integrity on the RPi. This required enabling the SSH server on the Laptop and correctly formatting the clone command:Bash# On Laptop:
sudo systemctl start ssh
sudo systemctl enable ssh

# On RPi (after removing the old src folder):
git clone ssh://mohammad@<LAPTOP_IP>/home/mohammad/ros2_ws/src src
2. Build on Raspberry Pi (aarch64)The packages were compiled specifically for the RPi's architecture:Bashcd ~/ros2_ws
source /opt/ros/jazzy/setup.bash
colcon build
3. Final Verification (Run)Successful communication was verified by setting a common ROS_DOMAIN_ID=55.DeviceCommandRoleLaptopexport ROS_DOMAIN_ID=55Environment SetupLaptopros2 run cpp_publisher_pkg minimal_publisherStarts publishing data.Raspberry Piexport ROS_DOMAIN_ID=55Environment SetupRaspberry Piros2 run py_subscriber_pkg minimal_subscriberConfirms successful data reception.ConclusionThis setup demonstrates foundational competence in robotics software engineering principles, including cross-platform code deployment, mixed-language interoperability, and distributed systems management.

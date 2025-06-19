# Livox ROS Driver - Message Alias Package

A ROS 2 package providing message definitions for legacy Livox ROS bag playback, solving compatibility issues when converting ROS 1 bags to ROS 2.

## Problem Background

When converting ROS 1 bag files containing Livox messages to ROS 2 format using [rosbags](https://ternaris.gitlab.io/rosbags/topics/convert.html), the converted bags reference message types that don't exist in ROS 2:

- `livox_ros_driver/msg/CustomMsg`
- `livox_ros_driver/msg/CustomPoint`

This package provides these message definitions as aliases, enabling playback of converted Livox bags in ROS 2 environments.

## Features

- **CustomMsg**: Complete Livox pointcloud message format with header, timing, and point data
- **CustomPoint**: Individual point format with position, reflectivity, and Livox-specific metadata
- **ROS 2 Compatible**: Built for ROS 2 using ament_cmake and rosidl

## Message Definitions

### CustomMsg
```
std_msgs/Header header    # ROS standard message header
uint64 timebase           # The time of first point
uint32 point_num          # Total number of pointclouds
uint8  lidar_id           # Lidar device id number
uint8[3]  rsvd            # Reserved use
CustomPoint[] points      # Pointcloud data
```

### CustomPoint
```
uint32 offset_time      # offset time relative to the base time
float32 x               # X axis, unit:m
float32 y               # Y axis, unit:m
float32 z               # Z axis, unit:m
uint8 reflectivity      # reflectivity, 0~255
uint8 tag               # livox tag
uint8 line              # laser number in lidar
```

## Installation

### Prerequisites
- ROS 2 (Jazzy/Humble/Rolling)
- colcon build tools

### Build from Source

1. **Create/Navigate to your workspace:**
   ```bash
   mkdir -p ~/ros2_ws/src
   cd ~/ros2_ws/src
   ```

2. **Clone this repository:**
   ```bash
   git clone https://github.com/your-username/livox_ros_driver.git
   ```

3. **Build the package:**
   ```bash
   cd ~/ros2_ws
   colcon build --packages-select livox_ros_driver
   ```

4. **Source the workspace:**
   ```bash
   source install/setup.bash
   ```

## Usage

### Verify Installation

Check if the messages are available:

```bash
# List available interfaces
ros2 interface list | grep livox

# Show message definitions
ros2 interface show livox_ros_driver/msg/CustomMsg
ros2 interface show livox_ros_driver/msg/CustomPoint
```

### Python Usage

```python
from livox_ros_driver.msg import CustomMsg, CustomPoint

# Create message instances
custom_msg = CustomMsg()
custom_point = CustomPoint()
```

### C++ Usage

```cpp
#include "livox_ros_driver/msg/custom_msg.hpp"
#include "livox_ros_driver/msg/custom_point.hpp"

// Use in your code
livox_ros_driver::msg::CustomMsg msg;
livox_ros_driver::msg::CustomPoint point;
```

## Converting ROS 1 Bags

1. **Convert your ROS 1 bag to ROS 2:**
   ```bash
   # Install rosbags converter
   pip install rosbags

   # Convert bag file
   rosbags-convert your_livox_bag.bag --dst your_livox_bag_ros2/
   ```

2. **Ensure this package is built and sourced in your workspace**

3. **Play the converted bag:**
   ```bash
   ros2 bag play your_livox_bag_ros2/
   ```

## Package Structure

```
livox_ros_driver/
├── CMakeLists.txt
├── package.xml
├── msg/
│   ├── CustomMsg.msg
│   └── CustomPoint.msg
└── README.md
```

## Dependencies

- `std_msgs`
- `builtin_interfaces`
- `rosidl_default_generators` (build)
- `rosidl_default_runtime` (exec)

## License

Apache License 2.0

## Contributing

Issues and pull requests are welcome! Please ensure any contributions maintain compatibility with the original Livox message formats.

## Related Links

- [rosbags - ROS bag conversion tool](https://ternaris.gitlab.io/rosbags/topics/convert.html)
- [Livox SDK](https://github.com/Livox-SDK/livox_ros_driver)
- [ROS 2 Message and Service Development](https://docs.ros.org/en/rolling/Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.html)

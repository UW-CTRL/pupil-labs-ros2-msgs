# pupil-labs-ros2-msgs

ROS 2 message definitions for Pupil Labs Neon glasses data.

This repository currently contains one interface package:

- pupillabs_msgs

## Package

- Name: pupillabs_msgs
- Type: ROS 2 interface package (ament_cmake + rosidl)
- Message files:
  - msg/GazeData.msg
  - msg/PupilLabsIMUData.msg

## Messages

### GazeData.msg

Gaze point and eye model related values.

Fields:

- float32 x
- float32 y
- bool worn
- float32 pupil_diameter_left
- float32 eyeball_center_left_x
- float32 eyeball_center_left_y
- float32 eyeball_center_left_z
- float32 optical_axis_left_x
- float32 optical_axis_left_y
- float32 optical_axis_left_z
- float32 pupil_diameter_right
- float32 eyeball_center_right_x
- float32 eyeball_center_right_y
- float32 eyeball_center_right_z
- float32 optical_axis_right_x
- float32 optical_axis_right_y
- float32 optical_axis_right_z
- float32 timestamp_unix_seconds

### PupilLabsIMUData.msg

Linear acceleration, angular velocity, orientation quaternion, and timestamp.

Fields:

- float32 accel_x
- float32 accel_y
- float32 accel_z
- float32 gyro_x
- float32 gyro_y
- float32 gyro_z
- float32 quaternion_x
- float32 quaternion_y
- float32 quaternion_z
- float32 quaternion_w
- float32 timestamp_unix_seconds

## Build

From your ROS 2 workspace root:

```bash
colcon build --packages-select pupillabs_msgs
source install/setup.bash
```

## Inspect Message Interfaces

```bash
ros2 interface show pupillabs_msgs/msg/GazeData
ros2 interface show pupillabs_msgs/msg/PupilLabsIMUData
```

## Python Usage Example

```python
from rclpy.node import Node
from pupillabs_msgs.msg import GazeData


class GazeSubscriber(Node):
	def __init__(self):
		super().__init__('gaze_subscriber')
		self.create_subscription(GazeData, '/neon/gaze', self.callback, 10)

	def callback(self, msg: GazeData) -> None:
		self.get_logger().info(
			f"gaze=({msg.x:.3f}, {msg.y:.3f}) worn={msg.worn} t={msg.timestamp_unix_seconds:.3f}"
		)
```

## Notes

- The package name is pupillabs_msgs (no underscore between pupil and labs).
- timestamp_unix_seconds is represented as float32 in both messages.
- If needed, these messages can be extended later with std_msgs/Header for frame and ROS time metadata.

# agrobot_hardware

## Purpose

`agrobot_hardware` hosts hardware-facing ROS 2 nodes for manipulator and onboard sensing:

- Servo command aggregation into a micro-ROS servo message.
- Lift motor command gating based on limit switches.
- IMU publishing (BNO08x).
- Lidar driver + scan filtering launch.

## Nodes and Executables

### Python executables

- `servo_control.py`
  - Subscribes:
    - `servo_angle/claw` (`std_msgs/Float32`)
    - `servo_angle/grip` (`std_msgs/Float32`)
    - `servo_angle/arm` (`std_msgs/Float32`)
  - Publishes:
    - `servo_angles` (`agrobot_interfaces/ServoAngles`)
  - Applies command clipping to configured servo limits.

- `lift_motor_control.py`
  - Subscribes:
    - `limit_switch_states` (`agrobot_interfaces/LimitSwitchStates`)
    - `lift_direction` (`std_msgs/Int32`)
  - Publishes:
    - `lift_motor_pwm` (`std_msgs/Int32`)
  - Stops lift motion when limit switch trigger is present.

- `imu_driver.py`
  - Publishes:
    - `imu/data` (`sensor_msgs/Imu`)
    - `bno08x/mag` (`sensor_msgs/MagneticField`)
    - `bno08x/status` (`diagnostic_msgs/DiagnosticStatus`)
  - Broadcasts TF for IMU frame alignment.

### C++ executable

- `servo_control` target is present in `src/` as a C++ node artifact.

## Launch Files

- `launch/agrobot_hardware_launch.py`
  - Hardware node launch wrapper.
- `launch/lidar_launch.py`
  - Starts `rplidar_ros/rplidar_node` and `laser_filters/scan_to_scan_filter_chain`.

## Configuration

- `config/agrobot_hardware_params.yaml`
  - Parameter template for hardware nodes.
- `config/lidar.yaml`
  - RPLidar serial, frame, and scan filter configuration.

## Interfaces and Topic Contract

| Topic | Direction | Message |
|---|---|---|
| `servo_angle/claw` | Subscribed | `std_msgs/Float32` |
| `servo_angle/grip` | Subscribed | `std_msgs/Float32` |
| `servo_angle/arm` | Subscribed | `std_msgs/Float32` |
| `servo_angles` | Published | `agrobot_interfaces/ServoAngles` |
| `lift_direction` | Subscribed | `std_msgs/Int32` |
| `limit_switch_states` | Subscribed | `agrobot_interfaces/LimitSwitchStates` |
| `lift_motor_pwm` | Published | `std_msgs/Int32` |
| `imu/data` | Published | `sensor_msgs/Imu` |

## Run

```bash
source install/setup.bash

# Manipulator hardware nodes
ros2 run agrobot_hardware servo_control.py
ros2 run agrobot_hardware lift_motor_control.py

# IMU node
ros2 run agrobot_hardware imu_driver.py

# Lidar pipeline
ros2 launch agrobot_hardware lidar_launch.py
```

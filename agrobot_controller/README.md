# agrobot_controller

## Purpose

`agrobot_controller` provides motion control for the 4-wheel omni X-drive base:

- Inverse kinematics from robot velocity (`cmd_vel/drive`) to wheel angular velocity setpoints.
- Forward kinematics from wheel feedback to odometry (`odom`).
- Closed-loop wheel speed control (PID) from encoder pulses to motor PWM.

## Nodes and Executables

### Python executables

- `x_drive_controller_py.py`
  - Subscribes:
    - `cmd_vel/drive` (`geometry_msgs/Twist`)
    - `wheel_angular_vel/feedback` (`agrobot_interfaces/WheelAngularVel`)
  - Publishes:
    - `wheel_angular_vel/control` (`agrobot_interfaces/WheelAngularVel`)
    - `odom` (`nav_msgs/Odometry`)
    - TF `odom -> base_link` (optional via parameter)
  - Parameters:
    - `debug` (bool)
    - `publish_tf` (bool)

- `wheel_speed_control.py`
  - Subscribes:
    - `encoder_pulses` (`agrobot_interfaces/EncoderPulses`)
    - `wheel_angular_vel/control` (`agrobot_interfaces/WheelAngularVel`)
  - Publishes:
    - `motor_pwm` (`agrobot_interfaces/MotorPWMs`)
    - `wheel_angular_vel/feedback` (`agrobot_interfaces/WheelAngularVel`)
    - `wheel_speed_errors` (`agrobot_interfaces/PIDWheelError`, debug mode)
  - Parameters:
    - `debug`
    - `kp`, `ki`, `kd`
    - `alpha`
    - `pwm_threshold`

### C++ executable

- `x_drive_controller_cpp`
  - C++ target is present as an alternative executable.

## Launch Files

- `launch/agrobot_controller_launch.py`
  - Starts both Python controllers (`wheel_speed_control.py` and `x_drive_controller_py.py`).

## Configuration

- `config/agrobot_controller_params.yaml`
  - Parameter template file for controller nodes.

## Interfaces and Topic Contract

| Topic | Direction | Message |
|---|---|---|
| `cmd_vel/drive` | Subscribed by `x_drive_controller_py` | `geometry_msgs/Twist` |
| `wheel_angular_vel/control` | Published by `x_drive_controller_py`; subscribed by `wheel_speed_control` | `agrobot_interfaces/WheelAngularVel` |
| `encoder_pulses` | Subscribed by `wheel_speed_control` | `agrobot_interfaces/EncoderPulses` |
| `motor_pwm` | Published by `wheel_speed_control` | `agrobot_interfaces/MotorPWMs` |
| `wheel_angular_vel/feedback` | Published by `wheel_speed_control`; subscribed by `x_drive_controller_py` | `agrobot_interfaces/WheelAngularVel` |
| `odom` | Published by `x_drive_controller_py` | `nav_msgs/Odometry` |

## Run

```bash
source install/setup.bash

# Launch full controller stack
ros2 launch agrobot_controller agrobot_controller_launch.py

# Or run individual nodes
ros2 run agrobot_controller x_drive_controller_py.py
ros2 run agrobot_controller wheel_speed_control.py
```

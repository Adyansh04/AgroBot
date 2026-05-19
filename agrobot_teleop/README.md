# agrobot_teleop

## Purpose

`agrobot_teleop` provides joystick-based control for both:

- Base velocity teleoperation
- Manipulator command actions (lift and servo angles)

It also defines velocity arbitration via `twist_mux`.

## Nodes and Executables

### Python executable

- `joy_control.py`
  - Subscribes:
    - `joy` (`sensor_msgs/Joy`)
  - Publishes:
    - `lift_direction` (`std_msgs/Int32`)
    - `servo_angle/claw` (`std_msgs/Float32`)
    - `servo_angle/grip` (`std_msgs/Float32`)
    - `servo_angle/arm` (`std_msgs/Float32`)

### C++ executable

- `joy_control` target exists in `src/`.

### Teleop input stack launched from config

- `joy_linux/joy_linux_node`
- `teleop_twist_joy/teleop_node` with `cmd_vel -> cmd_vel/joy` remapping

## Launch Files

- `launch/joystick.launch.py`
  - Starts Linux joystick driver and teleop_twist_joy node.

## Configuration

- `config/joystick_params.yaml`
  - Joystick axis/button mapping and scaling.
- `config/twist_mux.yaml`
  - Arbitration priorities for navigation/tracking commands vs joystick commands.

## Interfaces and Topic Contract

| Topic | Direction | Message |
|---|---|---|
| `joy` | Subscribed | `sensor_msgs/Joy` |
| `cmd_vel/joy` | Published by teleop stack | `geometry_msgs/Twist` |
| `lift_direction` | Published | `std_msgs/Int32` |
| `servo_angle/claw` | Published | `std_msgs/Float32` |
| `servo_angle/grip` | Published | `std_msgs/Float32` |
| `servo_angle/arm` | Published | `std_msgs/Float32` |

## Run

```bash
source install/setup.bash

# Start joystick + teleop velocity source
ros2 launch agrobot_teleop joystick.launch.py

# Start manipulator joystick mapping node
ros2 run agrobot_teleop joy_control.py
```

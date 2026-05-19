# agrobot_bringup

## Purpose

`agrobot_bringup` is the system orchestration package for Agrobot. It composes the runtime graph for base control, actuator control, teleoperation, vision tracking, static TF, and micro-ROS agent processes.

## Nodes and Executables

This package does not define custom nodes; it launches and composes nodes from other packages.

## Launch Files

- `launch/robot_bringup_launch.py`
  - Top-level bringup that includes actuator, base, and description launch groups.
- `launch/base_launch.py`
  - Starts `twist_mux`, joystick stack, teleop manipulator controls, and `agrobot_controller` launch.
- `launch/actuator_control_launch.py`
  - Starts manipulator actuator nodes (`servo_control.py`, `lift_motor_control.py`) and optional sensor nodes.
- `launch/vision_tracking_launch.py`
  - Starts camera driver include (`orbbec_camera`) and vision detection/tracking nodes.
- `launch/description_launch.py`
  - Publishes static transforms among `map`, `odom`, `base_link`, `base_footprint`, `laser`, and `imu_link`.
- `launch/uros_agents_launch.py`
  - Starts serial micro-ROS agents for ESP32 base and ESP32-S3 sensor/manipulator firmware.

## Configuration

- `config/object_tracking_params.yaml`
  - Parameters for:
    - `yolo_results` (`model_name`, `conf`, `iou`, image mode)
    - `object_tracking` PID and threshold tuning (`desired_contour_area`, `kp_*`, `threshold_*`, `target_class_id`, velocity bounds)

## Interfaces and Integration Contracts

### Brought-up subsystems

- Base motion chain: `agrobot_controller` + `twist_mux` + teleop velocity source.
- Manipulator chain: servo and lift command processing from `agrobot_hardware`.
- Vision chain: camera stream + YOLO result publishing + tracker integration.
- MCU bridge: host-side `micro_ros_agent` for both firmware boards.

## Run

```bash
source install/setup.bash

# Full robot bringup (base + actuator + TF)
ros2 launch agrobot_bringup robot_bringup_launch.py

# Start micro-ROS serial agents
ros2 launch agrobot_bringup uros_agents_launch.py

# Start vision tracking pipeline
ros2 launch agrobot_bringup vision_tracking_launch.py
```

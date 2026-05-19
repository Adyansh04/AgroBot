# agrobot_demos

## Purpose

`agrobot_demos` contains high-level mission logic for demonstration workflows. The primary demo is an autonomous cotton plucking sequence driven by a finite-state machine.

## Nodes and Executables

### Python executable

- `cotton_plucking.py`
  - State machine phases:
    - `INIT`
    - `START_TRACKING`
    - `TRACKING`
    - `POST_TRACKING`
  - Subscribes:
    - `object_tracking_completed` (`std_msgs/Bool`)
  - Publishes:
    - `start_object_tracking` (`std_msgs/Bool`)
    - `cmd_vel` (`geometry_msgs/Twist`)
    - `lift_direction` (`std_msgs/Int32`)
    - `servo_angle/claw` (`std_msgs/Float32`)
    - `servo_angle/grip` (`std_msgs/Float32`)
    - `servo_angle/arm` (`std_msgs/Float32`)

### C++ executable

- `cotton_plucking` target is present in `src/` as a C++ node artifact.

## Launch Files

- `launch/cotton_plucking_demo_launch.py`
  - Launches the cotton plucking demo node with config.

## Configuration

- `config/cotton_plucking_demo.yaml`
  - Demo parameter template.

## Interfaces and Integration Contract

This package coordinates three major subsystems:

- Vision/tracking subsystem (`start_object_tracking`, completion feedback).
- Manipulator subsystem (servo and lift topics).
- Base motion subsystem (`cmd_vel` motion control).

## Run

```bash
source install/setup.bash

# Run via launch
ros2 launch agrobot_demos cotton_plucking_demo_launch.py

# Run node directly
ros2 run agrobot_demos cotton_plucking.py
```

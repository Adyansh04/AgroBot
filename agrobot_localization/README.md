# agrobot_localization

## Purpose

`agrobot_localization` provides localization and mapping configuration for Agrobot:

- Extended Kalman Filter fusion (`robot_localization`)
- 2D SLAM mapping (`cartographer_ros`)

## Nodes and Executables

This package does not define custom node executables. It launches external localization stacks with Agrobot-specific parameters.

## Launch Files

- `launch/ekf_launch.py`
  - Starts `robot_localization/ekf_node` with `config/ekf.yaml`.
- `launch/cartographer_launch.py`
  - Starts:
    - `cartographer_ros/cartographer_node`
    - `cartographer_ros/cartographer_occupancy_grid_node`

## Configuration

- `config/ekf.yaml`
  - EKF sensor fusion setup for odometry, IMU, and AMCL pose inputs.
  - Publishes filtered odometry and TF.
- `config/cartographer.lua`
  - Cartographer trajectory builder and pose graph tuning for lidar-based 2D mapping.

## Interfaces and Data Contract

### EKF fusion inputs

- `/odom`
- `/imu/data`
- `/amcl_pose`

### EKF output

- `/odometry/filtered`
- TF according to `map/odom/base_link` frame configuration.

### Cartographer integration

- Uses lidar scan input and filtered odometry remapping (`/odometry/filtered`).
- Publishes map and occupancy grid outputs.

## Run

```bash
source install/setup.bash

# EKF
ros2 launch agrobot_localization ekf_launch.py

# Cartographer mapping
ros2 launch agrobot_localization cartographer_launch.py
```

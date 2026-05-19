# agrobot_navigation

## Purpose

`agrobot_navigation` contains Nav2 bringup and parameterization for Agrobot's omni-drive navigation stack.

It provides:

- Nav2 launch composition
- Navigation module orchestration in composable or non-composable mode
- Agrobot-specific `params.yaml` tuning (controller, planner, costmaps, behavior tree, smoothing)

## Nodes and Executables

This package does not define custom runtime nodes. It launches Nav2 stack components with Agrobot configuration.

## Launch Files

- `launch/navigation_launch.py`
  - High-level wrapper for nav2 bringup with map + params.
- `launch/nav2_bringup.launch.py`
  - Main nav2 composition launcher with namespace, map, params, composition, respawn, and logging options.
- `launch/navigation_modules.launch.py`
  - Starts planner/controller/behavior/bt/waypoint/smoother/lifecycle modules and velocity smoother composition.

## Configuration

- `config/params.yaml`
  - Nav2 parameter set including:
    - `amcl`
    - `bt_navigator`
    - `controller_server` (MPPI omni motion model)
    - `local_costmap` and `global_costmap`
    - `planner_server` (Smac Hybrid)
    - `behavior_server`
    - `velocity_smoother`

## Interfaces and Integration Contract

### Command velocity routing

- Navigation controller generates `cmd_vel_nav`.
- Velocity smoother remaps output to `cmd_vel`.
- `twist_mux` (from teleop bringup) arbitrates `cmd_vel` against joystick control.

### Odometry and TF expectations

- Uses `/odometry/filtered` for stabilized odometry input.
- Expects standard `map`, `odom`, `base_link` frame chain.

## Run

```bash
source install/setup.bash

# Direct nav2 bringup with explicit map and params
ros2 launch agrobot_navigation nav2_bringup.launch.py \
  map:=/absolute/path/to/map.yaml \
  params_file:=$(ros2 pkg prefix agrobot_navigation)/share/agrobot_navigation/config/params.yaml

# Wrapper launch
ros2 launch agrobot_navigation navigation_launch.py
```

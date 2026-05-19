# agrobot_interfaces

## Purpose

`agrobot_interfaces` defines custom ROS 2 interfaces shared across:

- Host ROS 2 packages (control, vision, demos, hardware)
- micro-ROS firmware (`agrobot_firmware`, `agrobot_firmware_s3`)

## Messages

### `EncoderPulses.msg`

| Field | Type | Description |
|---|---|---|
| `encoder_1_pulse` | `int64` | Encoder count for wheel 1 |
| `encoder_2_pulse` | `int64` | Encoder count for wheel 2 |
| `encoder_3_pulse` | `int64` | Encoder count for wheel 3 |
| `encoder_4_pulse` | `int64` | Encoder count for wheel 4 |

### `LimitSwitchStates.msg`

| Field | Type | Description |
|---|---|---|
| `limit_switch_1` | `bool` | Limit switch state |
| `limit_switch_2` | `bool` | Limit switch state |
| `limit_switch_3` | `bool` | Limit switch state |
| `limit_switch_4` | `bool` | Limit switch state |
| `limit_switch_5` | `bool` | Limit switch state |

### `MotorPWMs.msg`

| Field | Type | Description |
|---|---|---|
| `motor1pwm` | `int16` | PWM command for drive motor 1 |
| `motor2pwm` | `int16` | PWM command for drive motor 2 |
| `motor3pwm` | `int16` | PWM command for drive motor 3 |
| `motor4pwm` | `int16` | PWM command for drive motor 4 |

### `PIDWheelError.msg`

| Field | Type | Description |
|---|---|---|
| `wheel_1_error` | `float32` | Closed-loop speed error wheel 1 |
| `wheel_2_error` | `float32` | Closed-loop speed error wheel 2 |
| `wheel_3_error` | `float32` | Closed-loop speed error wheel 3 |
| `wheel_4_error` | `float32` | Closed-loop speed error wheel 4 |

### `SensorDatas.msg`

| Field | Type | Description |
|---|---|---|
| `cobra_ir_1` | `int32` | Cobra line sensor channel 1 |
| `cobra_ir_2` | `int32` | Cobra line sensor channel 2 |
| `cobra_ir_3` | `int32` | Cobra line sensor channel 3 |
| `cobra_ir_4` | `int32` | Cobra line sensor channel 4 |
| `ultrasonic_1` | `float32` | Ultrasonic reading 1 |
| `ultrasonic_2` | `float32` | Ultrasonic reading 2 |
| `ultrasonic_3` | `float32` | Ultrasonic reading 3 |
| `ultrasonic_4` | `float32` | Ultrasonic reading 4 |
| `sharp_ir_1` | `float32` | Sharp IR distance channel 1 |
| `sharp_ir_2` | `float32` | Sharp IR distance channel 2 |

### `ServoAngles.msg`

| Field | Type | Description |
|---|---|---|
| `servo1_angle` | `float32` | Servo angle command 1 |
| `servo2_angle` | `float32` | Servo angle command 2 |
| `servo3_angle` | `float32` | Servo angle command 3 |
| `servo4_angle` | `float32` | Servo angle command 4 |
| `servo5_angle` | `float32` | Servo angle command 5 |

### `WheelAngularVel.msg`

| Field | Type | Description |
|---|---|---|
| `wheel_1_angular_vel` | `float64` | Target/feedback wheel angular velocity 1 (rad/s) |
| `wheel_2_angular_vel` | `float64` | Target/feedback wheel angular velocity 2 (rad/s) |
| `wheel_3_angular_vel` | `float64` | Target/feedback wheel angular velocity 3 (rad/s) |
| `wheel_4_angular_vel` | `float64` | Target/feedback wheel angular velocity 4 (rad/s) |

### `XyXy.msg`

| Field | Type | Description |
|---|---|---|
| `tl_x` | `float64` | Bounding box top-left x |
| `tl_y` | `float64` | Bounding box top-left y |
| `br_x` | `float64` | Bounding box bottom-right x |
| `br_y` | `float64` | Bounding box bottom-right y |

### `Xywh.msg`

| Field | Type | Description |
|---|---|---|
| `center_x` | `float64` | Bounding box center x |
| `center_y` | `float64` | Bounding box center y |
| `width` | `float64` | Bounding box width |
| `height` | `float64` | Bounding box height |

### `YoloResults.msg`

| Field | Type | Description |
|---|---|---|
| `class_ids` | `float64[]` | Detected class IDs |
| `confidence` | `float64[]` | Detection confidences |
| `tracking_id` | `float64[]` | Tracker IDs |
| `contour_area` | `float64[]` | Derived size metric per detection |
| `x_differences` | `float64[]` | Horizontal error from camera center line |
| `z_differences` | `float64[]` | Vertical error metric |
| `center_y_to_track` | `float64[]` | Y reference used by tracker |
| `xywh` | `Xywh[]` | Bounding boxes in center format |
| `xyxy` | `XyXy[]` | Bounding boxes in corner format |

## Services

### `ServiceExample.srv`

| Request | Type |
|---|---|
| `request` | `int32` |

| Response | Type |
|---|---|
| `response` | `int32` |

## Actions

### `ActionExample.action`

| Section | Field | Type |
|---|---|---|
| Goal | `goal` | `int32` |
| Feedback | `feedback` | `int32` |
| Result | `result` | `int32` |

## Build and Use

```bash
source /opt/ros/humble/setup.bash
colcon build --packages-select agrobot_interfaces --symlink-install
source install/setup.bash
```

Any dependent package can then import generated interfaces in Python/C++.

# agrobot_firmware_s3

## Purpose

`agrobot_firmware_s3` is the ESP32-S3 micro-ROS firmware for manipulator and auxiliary sensing.

Responsibilities:

- Receive multi-servo angle commands from ROS.
- Drive manipulator servos through MCPWM.
- Read limit switches and ultrasonic sensors.
- Publish sensor and switch state back to ROS via micro-ROS serial transport.

## Platform and Build

- PlatformIO environment: `esp32-s3-devkitc-1`
- File: `platformio.ini`
- micro-ROS distro: `humble`
- Transport: micro-ROS serial

Build and flash:

```bash
cd agrobot_firmware_s3
pio run
pio run --target upload
pio device monitor -b 115200
```

## Firmware Runtime Topics

### Published

- `sensor_data` (`agrobot_interfaces/SensorDatas`)
- `limit_switch_states` (`agrobot_interfaces/LimitSwitchStates`)

### Subscribed

- `servo_angles` (`agrobot_interfaces/ServoAngles`)

## Key Source Files

- `src/main.cpp`
  - micro-ROS entities, executor loop, sensor publish timer, and servo callback.
- `include/mcpwm_servo.hpp`
  - Servo MCPWM setup and angle-to-pulsewidth conversion.
- `include/ultrasonic_sensor.hpp`
  - Multi-echo ultrasonic sensor interface.
- `include/pin_map.hpp`
  - Servo, switch, and sensor pin assignment map.
- `include/cobra_line_sensor.hpp`, `include/sharp_ir_driver.hpp`
  - Additional sensor drivers used for expansion and testing.

## Test and Bringup Utilities

- `test/` contains standalone hardware test sketches and notes for servo, ultrasonic, and limit switch validation.

## ROS Integration Notes

- micro-ROS domain ID is set in firmware init options: `20`.
- Host-side `micro_ros_agent` should be running during operation.

Start host bridge:

```bash
source ../install/setup.bash
ros2 launch agrobot_bringup uros_agents_launch.py
```

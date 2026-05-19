# agrobot_firmware

## Purpose

`agrobot_firmware` is the ESP32 micro-ROS firmware for Agrobot base motion control.

Responsibilities:

- Subscribe to host-side drive PWM commands.
- Drive four base motors through MCPWM motor drivers.
- Read wheel encoders and publish pulse counts.
- Bridge all communication over micro-ROS serial transport.

## Platform and Build

- PlatformIO environment: `esp32doit-devkit-v1`
- File: `platformio.ini`
- Transport: micro-ROS serial (`board_microros_transport = serial`)

Build and flash:

```bash
cd agrobot_firmware
pio run
pio run --target upload
pio device monitor -b 115200
```

## Firmware Runtime Topics

### Published

- `encoder_pulses` (`agrobot_interfaces/EncoderPulses`)
- `encoder_lift_motor` (`std_msgs/Int32`)

### Subscribed

- `motor_pwm` (`agrobot_interfaces/MotorPWMs`)
- `lift_motor_pwm` (`std_msgs/Int32`)

## Key Source Files

- `src/main.cpp`
  - micro-ROS entity lifecycle and executor spin loop.
  - Encoder timer callback and motor command callbacks.
- `include/motor_driver_mcpwm.hpp`
  - MCPWM-based motor speed mapping and direction control.
- `include/pin_map.hpp`
  - GPIO pin mappings for motor driver and encoder channels.

## ROS Integration Notes

- micro-ROS domain ID is set in firmware init options: `20`.
- Host-side `micro_ros_agent` should be running before normal control operation.

Start host bridge:

```bash
source ../install/setup.bash
ros2 launch agrobot_bringup uros_agents_launch.py
```

# agrobot_vision

## Purpose

`agrobot_vision` implements cotton target perception and tracking control:

- YOLO-based object detection/tracking from camera images.
- Target error extraction (area and center offsets).
- Closed-loop alignment and approach using `cmd_vel`.

## Nodes and Executables

### Core executables

- `yolo_results.py`
  - Subscribes:
    - `gemini_e/color/image_raw` or compressed variant
  - Publishes:
    - `yolo_results` (`agrobot_interfaces/YoloResults`)
    - `annotated_compressed_image` (`sensor_msgs/CompressedImage`)
  - Parameters:
    - `use_compressed_image`
    - `show_image`
    - `model_name`
    - `iou`
    - `conf`

- `object_tracking.py`
  - Subscribes:
    - `yolo_results` (`agrobot_interfaces/YoloResults`)
    - `start_object_tracking` (`std_msgs/Bool`)
  - Publishes:
    - `cmd_vel` (`geometry_msgs/Twist`)
    - `lift_direction` (`std_msgs/Int32`)
    - `object_tracking_completed` (`std_msgs/Bool`)
  - Parameters:
    - desired target metrics and PID gains (`kp_*`, `ki_*`, `kd_*`)
    - threshold metrics (`threshold_area`, `threshold_x`, `threshold_z`)
    - class selection (`target_class_id`)
    - search/velocity bounds (`sweep_angular_z`, `max_velocity`)

### Utility executables

- `click_photos.py` - capture dataset images from camera topic.
- `compressed_image_viewer.py` - inspect annotated compressed stream.
- `yolo_test.py` - direct OpenCV camera YOLO quick test.

## Launch Files

- `launch/agrobot_vision_launch.py`
  - Starts camera include (`orbbec_camera`) and YOLO result node.

## Configuration

- `config/agrobot_vision_params.yaml`
  - Parameters for `yolo_results` runtime selection and thresholds.
- `agrobot_vision/model/`
  - Model artifacts (`.pt`, `.onnx`, `.engine`) for deployment and training outputs.

## Interfaces and Topic Contract

| Topic | Direction | Message |
|---|---|---|
| `gemini_e/color/image_raw` | Subscribed | `sensor_msgs/Image` |
| `yolo_results` | Published / Subscribed | `agrobot_interfaces/YoloResults` |
| `start_object_tracking` | Subscribed | `std_msgs/Bool` |
| `object_tracking_completed` | Published | `std_msgs/Bool` |
| `cmd_vel` | Published | `geometry_msgs/Twist` |
| `annotated_compressed_image` | Published | `sensor_msgs/CompressedImage` |

## Run

```bash
source install/setup.bash

# Launch camera + YOLO results node
ros2 launch agrobot_vision agrobot_vision_launch.py

# Run tracker node (if not started through bringup)
ros2 run agrobot_vision object_tracking.py

# Run YOLO node directly
ros2 run agrobot_vision yolo_results.py
```

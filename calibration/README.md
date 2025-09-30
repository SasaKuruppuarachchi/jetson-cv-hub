# Calibration Files

This directory contains calibration data for the Jetson CV Hub sensors.

## Overview

Proper calibration is essential for accurate computer vision and sensor fusion applications. This directory includes calibration files for:

- FLIR machine vision cameras (intrinsic and extrinsic parameters)
- Xsense IMU (accelerometer, gyroscope, magnetometer)
- Camera-to-IMU transformation matrices
- Multi-camera stereo/multi-view calibration

## Directory Structure

```
calibration/
├── camera/
│   ├── intrinsics/          # Camera intrinsic parameters
│   ├── extrinsics/          # Camera extrinsic parameters
│   └── stereo/              # Stereo calibration files
├── imu/
│   ├── factory/             # Factory calibration data
│   └── custom/              # Custom calibration results
├── camera_imu/              # Camera-IMU calibration
└── templates/               # Template files for calibration
```

## Camera Calibration

### Intrinsic Parameters

Camera intrinsic calibration files contain:
- Focal length (fx, fy)
- Principal point (cx, cy)
- Distortion coefficients (k1, k2, p1, p2, k3)
- Image resolution

**Format**: YAML, JSON, or OpenCV XML
**Naming**: `camera_[id]_intrinsics.yaml`

Example structure:
```yaml
camera_matrix:
  rows: 3
  cols: 3
  data: [fx, 0, cx, 0, fy, cy, 0, 0, 1]
distortion_coefficients:
  rows: 1
  cols: 5
  data: [k1, k2, p1, p2, k3]
image_width: 1920
image_height: 1200
```

### Extrinsic Parameters

Camera extrinsic calibration defines the camera pose relative to a reference frame:
- Rotation matrix (3x3)
- Translation vector (3x1)

**Format**: YAML, JSON
**Naming**: `camera_[id]_extrinsics.yaml`

## IMU Calibration

### Factory Calibration

Xsense IMUs typically include factory calibration. Store factory calibration files here for reference.

### Custom Calibration

Additional calibration may be performed for:
- Bias correction
- Scale factor adjustment
- Axis alignment
- Temperature compensation

**Format**: YAML, JSON
**Naming**: `imu_calibration.yaml`

## Camera-IMU Calibration

Spatial and temporal calibration between cameras and IMU:
- Transformation matrix (rotation + translation)
- Time offset/synchronization

**Tools**: Kalibr, camera_imu_calib, etc.

## Calibration Procedures

### Camera Calibration Procedure

1. Print a calibration pattern (checkerboard or AprilTag grid)
2. Capture 20-30 images of the pattern from different angles
3. Run calibration software (OpenCV, ROS camera_calibration, etc.)
4. Save calibration results to appropriate directory
5. Verify calibration with test images

### IMU Calibration Procedure

1. Place IMU on a stable, level surface
2. Collect stationary data for bias estimation
3. Perform six-position calibration for scale factors
4. Save calibration parameters
5. Verify with known orientation tests

### Camera-IMU Calibration Procedure

1. Ensure both camera and IMU calibrations are completed
2. Set up calibration environment with target pattern
3. Collect synchronized camera-IMU data while moving
4. Run calibration tool (e.g., Kalibr)
5. Save transformation matrices
6. Verify temporal and spatial alignment

## File Formats

Calibration files are provided in standard formats:
- **YAML**: Human-readable, widely supported
- **JSON**: Lightweight, easy to parse
- **XML**: OpenCV compatible
- **TXT**: Simple parameter lists

## Validation

After calibration:
1. Check reprojection errors (should be < 1 pixel for cameras)
2. Verify IMU readings against known orientations
3. Test sensor fusion with visual-inertial odometry
4. Document calibration date and conditions

## Usage in Software

To load calibration files in your application:

**Python (OpenCV)**:
```python
import cv2
import yaml

with open('calibration/camera/intrinsics/camera_0_intrinsics.yaml', 'r') as f:
    calib_data = yaml.safe_load(f)
    camera_matrix = np.array(calib_data['camera_matrix']['data']).reshape(3, 3)
    dist_coeffs = np.array(calib_data['distortion_coefficients']['data'])
```

**ROS**:
```bash
rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.108 image:=/camera/image_raw
```

## Notes

- Recalibrate after physical modifications to the system
- Store calibration files with date stamps
- Keep backup copies of calibration data
- Document environmental conditions during calibration (temperature, lighting)
- For multi-camera setups, calibrate all cameras relative to a common reference frame

## References

- OpenCV Camera Calibration: https://docs.opencv.org/master/dc/dbb/tutorial_py_calibration.html
- Kalibr (Camera-IMU): https://github.com/ethz-asl/kalibr
- Xsense MTi Documentation: [Manufacturer's calibration guide]

## License

Calibration procedures and template files are released under the Apache License 2.0. See the [LICENSE](../LICENSE) file for details.

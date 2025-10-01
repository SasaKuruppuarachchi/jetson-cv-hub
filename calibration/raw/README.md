# Raw Calibration Data

This directory contains raw calibration data, results, and historical calibration files from various calibration sessions.

## Contents

### FLIR Camera Calibration (`Flir calib/`)
- Camera intrinsics and extrinsics calibration results
- Camera-IMU calibration with synchronized data at 100 Hz
- AprilGrid calibration targets (6x6 grid, 0.2m x 0.2m)
- Results from Kalibr calibration tool

**Key files have been copied to:**
- Camera intrinsics → `../camera/intrinsics/`
- IMU calibration → `../imu/custom/`
- Camera-IMU calibration → `../camera_imu/`
- Calibration targets → `../templates/`

### Basalt Calibration (`Very good calib/`, `very good calib 2/`)
- Calibration results from Basalt visual-inertial calibration
- Includes camera poses, detected corners, and convergence status
- JSON format calibration files

### T265 Calibration (`calib_t265/`)
- Intel RealSense T265 tracking camera calibration
- Camera-IMU calibration results
- OpenVINS configuration files
- MATLAB evaluation results
- Trajectory estimation and ground truth data

### PX4 IMU Calibration
- `px4_imu_intrinsics` - Factory calibration for PX4 onboard IMU
- Copied to: `../imu/factory/px4_imu_intrinsics.txt`

## Usage

These raw files are provided for:
- Reference and reproducibility
- Historical record of calibration sessions
- Comparison with new calibration results
- Understanding calibration procedures

**For actual use in your system**, refer to the organized calibration files in:
- `../camera/` - Camera calibration
- `../imu/` - IMU calibration
- `../camera_imu/` - Camera-IMU extrinsic calibration
- `../templates/` - Calibration targets and templates

## Calibration Tools Used

- **Kalibr**: Multi-camera and camera-IMU calibration
- **Basalt**: Visual-inertial calibration and SLAM
- **ROS camera_calibration**: Camera intrinsic calibration
- **MT Manager**: Xsense IMU calibration
- **MATLAB**: Trajectory evaluation and analysis

## Notes

- Calibration was performed with 100 Hz camera-IMU synchronization
- AprilGrid targets: 6x6 grid with 0.2m x 0.2m spacing
- Results include both pinhole and equidistant camera models
- IMU noise parameters are included for sensor fusion algorithms

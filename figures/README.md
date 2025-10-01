# Figures Directory

This directory contains images and figures used in the Jetson CV Hub documentation.

## Available Figures

### Device Images

- **`device_front.jpg`** (3.5 MB) - Front view of the assembled Jetson CV Hub showing cameras and sensors
- **`device_back.jpg`** (3.5 MB) - Back view showing Jetson, connectors, and power components
- **`device_isometric.gif`** (1.5 MB) - Animated isometric view of the device
- **`device_parts_named.png`** (1.5 MB) - Annotated diagram showing all major components

### Assembly Photos (`assembly/`)

- **`3d printed_finish.jpg`** (3.9 MB) - Completed 3D printed parts ready for assembly
- **`3d_printing_in_progress.jpg`** (10 KB) - 3D printer working on component parts
- **`bottom_altitude_sensor_assmbly.jpg`** (4.0 MB) - Bottom-mounted altitude sensor installation
- **`flir_camera_assembly.jpg`** (3.0 MB) - FLIR camera mounting and installation
- **`i2c_display_aseembly_with_buttons.jpg`** (3.7 MB) - I2C status display with control buttons assembly
- **`power_assembly_with regulator.jpg`** (4.0 MB) - Power distribution system with voltage regulator

### Hardware Components (`hardware/`)

- **`FLIR_BFLY-U3-23S6.png`** (17 KB) - FLIR Blackfly camera component image
- **`alt_sensor.jpg`** (4.0 MB) - Altitude sensor component
- **`flir_cameras.jpg`** (4.4 MB) - FLIR machine vision cameras
- **`jetson.jpg`** (7.7 KB) - NVIDIA Jetson Orin module
- **`onboard_power.jpg`** (4.0 MB) - Onboard power distribution system
- **`px4.jpg`** (2.9 KB) - PX4 flight controller for hardware synchronization
- **`status_screan.jpg`** (3.7 MB) - Status display screen with system information
- **`xsense_imu.png`** (23 KB) - Xsense MTi IMU component

### System Diagrams (`diagrams/`)

- **`Calib.png`** (352 KB) - Camera and IMU calibration workflow diagram
- **`Sync.jpg`** (313 KB) - Hardware synchronization architecture showing PX4-based trigger system
- **`results.png`** (136 KB) - Example system results and performance metrics

### Attachments (`attachments/`)

- **`IMG_20211206_060001.jpg`** (4.4 MB) - Device with attachments and accessories
- **`vicon_attachment.jpg`** (3.6 MB) - VICON motion capture marker holder attachment

### Hardware Synchronization

The system uses PX4-based hardware synchronization (see `diagrams/Sync.jpg`):
- PX4 as master device generating 100 Hz trigger signal
- Cameras and Xsense IMU connected in parallel to receive sync signals
- Precise temporal alignment for visual-inertial SLAM

## Usage

Images in this directory can be referenced in markdown documentation files:

```markdown
![Device Front View](../figures/device_front.jpg)
![Device Back View](../figures/device_back.jpg)
![Device Parts Diagram](../figures/device_parts_named.png)
![Device Animation](../figures/device_isometric.gif)
```

## Organization

Organize images by category:

```
figures/
├── assembly/           # Assembly photos and diagrams
├── hardware/          # Hardware component photos
├── setup/             # Setup screenshots
├── diagrams/          # System diagrams and schematics
└── results/           # Example output and results
```

## Image Guidelines

- Use descriptive filenames (e.g., `jetson-orin-mounting.jpg` instead of `img001.jpg`)
- Optimize images for web (recommended max width: 1200px)
- Use PNG for diagrams and screenshots
- Use JPEG for photos
- Include alternative text in markdown for accessibility

## GitHub Pages

All images in this directory are automatically copied to the GitHub Pages site and can be displayed in the documentation pages.

## Examples

To add an assembly photo:
1. Place the image in `figures/assembly/`
2. Reference it in `docs/ASSEMBLY_INSTRUCTIONS.md`:
   ```markdown
   ![Jetson Orin Installation](../figures/assembly/jetson-orin-mounting.jpg)
   ```

The image will then appear both in the GitHub repository view and on the GitHub Pages site.

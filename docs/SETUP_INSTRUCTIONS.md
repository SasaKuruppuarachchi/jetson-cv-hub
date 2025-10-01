# Setup Instructions

## Jetson CV Hub - Software Setup and Configuration Guide

### Document Information

- **Version**: 1.0 (Draft)
- **Last Updated**: [Date]
- **Author**: [Author Name]
- **Status**: Draft - Subject to refinement

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Initial System Setup](#initial-system-setup)
3. [Jetson Flashing Instructions](#jetson-flashing-instructions)
4. [ROS2 Humble Installation](#ros2-humble-installation)
5. [NVIDIA Docker Setup](#nvidia-docker-setup)
6. [Isaac ROS Docker Setup](#isaac-ros-docker-setup)
7. [Jetson Configuration](#jetson-configuration)
8. [Camera Setup](#camera-setup)
9. [IMU Setup](#imu-setup)
10. [PX4 Setup and Hardware Synchronization](#px4-setup-and-hardware-synchronization)
11. [Network Configuration](#network-configuration)
12. [Software Installation](#software-installation)
13. [System Calibration](#system-calibration)
14. [Verification and Testing](#verification-and-testing)
15. [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Hardware Requirements

Before beginning setup, ensure:
- [ ] Hardware assembly is complete (see [Assembly Instructions](ASSEMBLY_INSTRUCTIONS.md))
- [ ] All quality checks passed
- [ ] Power supply is connected and tested
- [ ] You have access to:
  - Display (HDMI/DisplayPort) or SSH capability
  - USB keyboard and mouse (for initial setup)
  - Ethernet connection (recommended for initial setup)
  - Host computer for remote access

### Software Requirements

Download the following on your host computer:
- [ ] NVIDIA Jetson OS image (JetPack)
- [ ] Balena Etcher or similar SD card flashing tool
- [ ] SSH client (PuTTY, Terminal, etc.)
- [ ] FLIR Spinnaker SDK
- [ ] Xsense MT Software Suite
- [ ] Text editor for configuration files

### Network Requirements

- [ ] DHCP-enabled network for initial setup
- [ ] Internet connectivity for package downloads
- [ ] Router with available Ethernet port
- [ ] (Optional) Static IP address for production use

---

## Initial System Setup

### Step 1: Prepare Jetson Storage Media

#### Option A: microSD Card Setup

1. **Download JetPack**
   - Visit: https://developer.nvidia.com/embedded/jetpack
   - Select appropriate version for your Jetson Orin model
   - Download SD Card Image (typically 8GB+ download)

2. **Flash SD Card**
   ```bash
   # On Linux
   sudo dd if=jetson-orin-image.img of=/dev/sdX bs=4M status=progress
   sync
   
   # Or use Balena Etcher (cross-platform GUI tool)
   # 1. Select image file
   # 2. Select target drive
   # 3. Flash!
   ```

3. **Insert SD Card**
   - Power off Jetson
   - Insert prepared microSD card
   - Ensure card is fully seated

#### Option B: NVMe SSD Setup

1. Follow NVIDIA's documentation for flashing to NVMe
2. May require SDK Manager on host Ubuntu computer
3. Connect Jetson via USB in recovery mode

### Step 2: First Boot

1. **Connect peripherals**:
   - HDMI display
   - USB keyboard and mouse
   - Ethernet cable
   - Power supply

2. **Power on the system**
   - Press power button or connect power
   - Watch boot sequence on display
   - First boot may take 2-5 minutes

3. **Initial configuration wizard**:
   - Accept license agreement
   - Set username and password
   - Configure timezone and keyboard layout
   - Select language preferences
   - Connect to network (if WiFi available)

**Recommended Settings**:
- Username: `jetson` (or your preference)
- Hostname: `jetson-cv-hub`
- Enable automatic login: No (for security)

### Step 3: System Update

After initial boot, update the system:

```bash
# Update package lists
sudo apt update

# Upgrade installed packages
sudo apt upgrade -y

# Clean up
sudo apt autoremove -y

# Reboot to ensure all updates applied
sudo reboot
```

---

## Jetson Flashing Instructions

### Overview

This section provides detailed instructions for flashing NVIDIA Jetson devices with JetPack OS. The Jetson CV Hub system requires JetPack 5.1 or later for optimal ROS2 Humble compatibility.

### Method 1: SD Card Flashing (Easiest)

This method is suitable for Jetson Orin Nano and Jetson Orin NX Developer Kits.

#### Step 1: Download JetPack Image

1. Visit NVIDIA's official download page:
   - URL: https://developer.nvidia.com/embedded/jetpack
   - Select your Jetson model (Orin Nano/NX/AGX)
   - Download the latest JetPack SD Card Image (JetPack 5.1.2+ recommended)
   - File size: ~10-15 GB

2. Verify the download:
   ```bash
   # Check SHA256 sum
   sha256sum jetson-orin-*.img.zip
   ```

#### Step 2: Flash SD Card

**Using Balena Etcher (Recommended for all platforms)**:

1. Download Balena Etcher:
   - Windows/Mac/Linux: https://www.balena.io/etcher/
   
2. Flash the image:
   - Launch Balena Etcher
   - Click "Flash from file" and select the downloaded .img or .zip file
   - Click "Select target" and choose your SD card (minimum 32GB recommended)
   - Click "Flash!" and wait for completion (10-20 minutes)
   - Etcher will automatically verify the flash

**Using Command Line (Linux)**:

```bash
# Extract if zipped
unzip jetson-orin-*.img.zip

# Identify SD card device (be careful!)
lsblk

# Flash the image (replace /dev/sdX with your SD card)
sudo dd if=jetson-orin-*.img of=/dev/sdX bs=4M status=progress conv=fsync
sync

# Verify (optional)
sudo dd if=/dev/sdX of=/tmp/verify.img bs=4M count=2048
```

#### Step 3: First Boot

1. Insert the flashed SD card into Jetson
2. Connect peripherals (display, keyboard, mouse, Ethernet)
3. Power on the Jetson
4. Follow the on-screen setup wizard:
   - Accept EULA
   - Set username/password: `jetson` / `jetson` (or your preference)
   - Set hostname: `jetson-cv-hub`
   - Configure timezone and language
   - Connect to network

### Method 2: SDK Manager Flashing (Advanced)

This method provides more control and is necessary for NVMe SSD installation.

#### Prerequisites

- Ubuntu 18.04/20.04/22.04 host computer
- USB cable (Type-A to Type-C or Micro-USB depending on Jetson model)
- Jetson in recovery mode

#### Step 1: Install SDK Manager

```bash
# On Ubuntu host computer
# Download SDK Manager from: https://developer.nvidia.com/nvidia-sdk-manager

# Install the .deb package
sudo apt install ./sdkmanager_[version]_amd64.deb

# Launch SDK Manager
sdkmanager
```

#### Step 2: Put Jetson in Recovery Mode

1. Power off Jetson completely
2. Connect USB cable from host to Jetson
3. Hold the RECOVERY button
4. Press and release POWER button
5. Hold RECOVERY for 2 more seconds
6. Release RECOVERY button

7. Verify recovery mode on host:
   ```bash
   lsusb | grep -i nvidia
   # Should show: "NVIDIA Corp. APX"
   ```

#### Step 3: Flash Using SDK Manager

1. In SDK Manager:
   - Select your Jetson model (auto-detected if in recovery mode)
   - Select JetPack version (5.1.2+ recommended)
   - Choose target components:
     - ✅ Jetson OS
     - ✅ CUDA, cuDNN, TensorRT
     - ✅ OpenCV (with CUDA)
     - ⬜ Multimedia components (optional)
   
2. Configure installation:
   - Storage: SD Card or NVMe SSD
   - Username/password: `jetson` / `jetson`
   - Automatic login: Disabled (recommended)
   
3. Start flashing (takes 30-60 minutes):
   - SDK Manager will download components
   - Flash the OS
   - Install additional SDK components

4. After flashing, Jetson will reboot automatically

### Method 3: Flashing to NVMe SSD

For best performance, install JetPack directly to NVMe SSD.

#### Option A: Flash Directly to NVMe (via SDK Manager)

Follow Method 2 above, but select "NVMe SSD" as the target storage in SDK Manager.

#### Option B: Clone from SD Card to NVMe

1. First boot from SD card with flashed JetPack
2. Install NVMe SSD in Jetson
3. Clone system to NVMe:
   ```bash
   # Install cloning tools
   git clone https://github.com/jetsonhacks/rootOnNVMe
   cd rootOnNVMe
   
   # Run clone script
   ./copy-rootfs-ssd.sh
   
   # Update boot configuration
   ./setup-service.sh
   
   # Reboot to boot from NVMe
   sudo reboot
   ```

### Post-Flash Verification

After flashing, verify the installation:

```bash
# Check JetPack version
cat /etc/nv_tegra_release

# Check CUDA version
nvcc --version

# Check available disk space
df -h

# Check GPU
nvidia-smi  # May not work on all Jetson models

# Check Jetson stats
sudo pip3 install jetson-stats
sudo jtop
```

---

## ROS2 Humble Installation

The Jetson CV Hub natively runs **ROS2 Humble** on Ubuntu 22.04 (included in JetPack 5.1+).

### Prerequisites

- JetPack 5.1 or later (Ubuntu 22.04 base)
- Internet connection
- At least 10 GB free disk space

### Step 1: Set up ROS2 Repository

```bash
# Set locale
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

# Setup sources
sudo apt install software-properties-common
sudo add-apt-repository universe

# Add ROS2 GPG key
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

# Add repository to sources list
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

### Step 2: Install ROS2 Humble

```bash
# Update package lists
sudo apt update
sudo apt upgrade -y

# Install ROS2 Humble Desktop (includes RViz, demos, tutorials)
sudo apt install ros-humble-desktop -y

# Or install ROS2 Base (minimal, no GUI tools)
# sudo apt install ros-humble-ros-base -y

# Install development tools
sudo apt install ros-dev-tools -y
sudo apt install python3-colcon-common-extensions -y
```

### Step 3: Environment Setup

```bash
# Source ROS2 setup script
source /opt/ros/humble/setup.bash

# Add to bashrc for automatic sourcing
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

# Install argcomplete for command-line completion
sudo apt install python3-argcomplete -y
```

### Step 4: Verify Installation

```bash
# Check ROS2 environment variables
printenv | grep -i ROS

# Test ROS2 installation
# Terminal 1:
ros2 run demo_nodes_cpp talker

# Terminal 2 (open new terminal):
ros2 run demo_nodes_cpp listener

# List ROS2 topics
ros2 topic list

# If you see messages passing, ROS2 is working correctly!
```

### Step 5: Create ROS2 Workspace

```bash
# Create workspace directory
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws

# Build workspace (even if empty)
colcon build

# Source workspace
source ~/ros2_ws/install/setup.bash

# Add to bashrc
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```

### Step 6: Install Common ROS2 Packages

```bash
# Camera drivers and image processing
sudo apt install ros-humble-v4l2-camera -y
sudo apt install ros-humble-image-transport-plugins -y
sudo apt install ros-humble-vision-opencv -y

# IMU and sensor drivers
sudo apt install ros-humble-imu-tools -y

# Visualization tools
sudo apt install ros-humble-rqt* -y
sudo apt install ros-humble-rviz2 -y

# TF (coordinate transforms)
sudo apt install ros-humble-tf2-tools -y
sudo apt install ros-humble-tf-transformations -y

# Diagnostic tools
sudo apt install ros-humble-diagnostic-updater -y
```

---

## NVIDIA Docker Setup

NVIDIA Docker enables GPU-accelerated containers on Jetson, essential for Isaac ROS and other GPU workloads.

### Step 1: Install Docker

```bash
# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Set up stable repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y

# Add user to docker group
sudo usermod -aG docker $USER

# Log out and back in for group changes to take effect
# Or run: newgrp docker
```

### Step 2: Verify Docker Installation

```bash
# Test Docker
docker run hello-world

# Check Docker version
docker --version
docker compose version
```

### Step 3: Configure NVIDIA Container Runtime

The NVIDIA Container Runtime is pre-installed on JetPack. Verify and configure:

```bash
# Verify NVIDIA Container Runtime
sudo docker run --rm --runtime nvidia nvidia/cuda:11.4.2-base-ubuntu20.04 nvidia-smi

# Configure Docker to use NVIDIA runtime by default
sudo nano /etc/docker/daemon.json
```

Add the following content:

```json
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

Restart Docker:

```bash
# Restart Docker daemon
sudo systemctl restart docker

# Verify configuration
docker info | grep -i runtime
```

### Step 4: Test GPU Access in Container

```bash
# Run a GPU-enabled container
docker run --rm --runtime nvidia nvidia/cuda:11.4.2-base-ubuntu20.04 nvidia-smi

# Test CUDA in container
docker run --rm --runtime nvidia nvidia/cuda:11.4.2-base-ubuntu20.04 bash -c "nvcc --version"
```

---

## Isaac ROS Docker Setup

Isaac ROS provides GPU-accelerated ROS2 packages optimized for Jetson.

### Prerequisites

- Docker with NVIDIA runtime (see previous section)
- At least 15 GB free disk space
- Internet connection

### Step 1: Clone Isaac ROS Common

```bash
# Create workspace for Isaac ROS
mkdir -p ~/workspaces/isaac_ros-dev/src
cd ~/workspaces/isaac_ros-dev/src

# Clone Isaac ROS Common repository
git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git

# Pull additional Isaac ROS packages as needed
# Example: Visual SLAM
git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam.git

# Example: Image processing
git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline.git
```

### Step 2: Set Up Isaac ROS Docker

```bash
cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common

# Pull Isaac ROS Docker image (this may take 20-30 minutes)
scripts/run_dev.sh -d ~/workspaces/isaac_ros-dev

# This will:
# - Pull the Isaac ROS development Docker image
# - Set up X11 forwarding for GUI applications
# - Mount your workspace
# - Start a container with GPU access
```

### Step 3: Build Isaac ROS Packages Inside Container

Inside the Isaac ROS Docker container:

```bash
# The container prompt will look like: user@docker:~/workspaces/isaac_ros-dev$

# Build all packages
cd /workspaces/isaac_ros-dev
colcon build --symlink-install

# Source the workspace
source install/setup.bash
```

### Step 4: Configure Isaac ROS for Jetson

```bash
# Inside Isaac ROS container, optimize for Jetson
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp

# Increase DDS buffer sizes for high-bandwidth data (cameras)
export FASTRTPS_DEFAULT_PROFILES_FILE=/workspaces/isaac_ros-dev/fastdds.xml
```

Create `/workspaces/isaac_ros-dev/fastdds.xml`:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<profiles xmlns="http://www.eprosima.com/XMLSchemas/fastRTPS_Profiles">
    <transport_descriptors>
        <transport_descriptor>
            <transport_id>udp_transport</transport_id>
            <type>UDPv4</type>
            <sendBufferSize>1048576</sendBufferSize>
            <receiveBufferSize>1048576</receiveBufferSize>
        </transport_descriptor>
    </transport_descriptors>
</profiles>
```

### Step 5: Test Isaac ROS

Inside Isaac ROS container:

```bash
# Test with a simple Isaac ROS node
# Example: Image format converter
ros2 run isaac_ros_image_proc isaac_ros_image_format_converter_node
```

### Step 6: Using Isaac ROS Container

To re-enter the Isaac ROS development container:

```bash
# From host
cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common
scripts/run_dev.sh -d ~/workspaces/isaac_ros-dev
```

To run a specific command in container:

```bash
# From host
cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common
scripts/run_dev.sh -d ~/workspaces/isaac_ros-dev -c "ros2 topic list"
```

### Alternative: Build Custom Isaac ROS Docker Image

For a production deployment, build a custom image:

```bash
# Create Dockerfile
cd ~/workspaces/isaac_ros-dev
nano Dockerfile.custom
```

Example Dockerfile:

```dockerfile
FROM nvcr.io/nvidia/isaac/ros:aarch64-ros2_humble_692506f75bc249228841d70e2e8c5f35

# Install additional packages
RUN apt-get update && apt-get install -y \
    ros-humble-YOUR-PACKAGES \
    && rm -rf /var/lib/apt/lists/*

# Copy your workspace
COPY . /workspaces/isaac_ros-dev

# Build workspace
WORKDIR /workspaces/isaac_ros-dev
RUN . /opt/ros/humble/setup.sh && colcon build --symlink-install

# Source workspace on container start
RUN echo "source /workspaces/isaac_ros-dev/install/setup.bash" >> ~/.bashrc
```

Build and run:

```bash
# Build custom image
docker build -t jetson-cv-hub:latest -f Dockerfile.custom .

# Run custom image
docker run --runtime nvidia -it --network host jetson-cv-hub:latest
```

---

## Jetson Configuration

### Step 1: Configure Power Mode

Jetson Orin supports multiple power modes. Select based on your requirements:

```bash
# List available power modes
sudo nvpmodel -q

# Set to maximum performance (highest power consumption)
sudo nvpmodel -m 0

# Set to balanced mode
sudo nvpmodel -m 2

# Make persistent across reboots
sudo systemctl enable nvpmodel
```

**Recommended**: Start with maximum performance for testing, optimize later.

### Step 2: Enable Maximum Clocks (Optional)

For benchmarking or maximum performance:

```bash
# Enable max clocks
sudo jetson_clocks

# Check current clock speeds
sudo jetson_clocks --show
```

### Step 3: Configure Swap Space

Add swap for memory-intensive operations:

```bash
# Check current swap
free -h

# Add swap file (4GB example)
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make permanent
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### Step 4: Install Development Tools

```bash
# Essential build tools
sudo apt install -y build-essential cmake git

# Python development
sudo apt install -y python3-pip python3-dev python3-venv

# Useful utilities
sudo apt install -y htop nano vim curl wget
```

---

## Camera Setup

### Step 1: Install FLIR Spinnaker SDK

1. **Download Spinnaker SDK**
   - Visit: https://www.flir.com/products/spinnaker-sdk/
   - Download ARM64 version for Ubuntu (JetPack version)
   - Transfer to Jetson via SCP or USB drive

2. **Install dependencies**
   ```bash
   sudo apt install -y libusb-1.0-0 libavcodec58 libavformat58 \
        libswscale5 libswresample3 libavutil56 libgtkmm-2.4-1v5 \
        libglademm-2.4-1v5
   ```

3. **Install Spinnaker**
   ```bash
   # Extract downloaded archive
   tar -xzf spinnaker-*-arm64-pkg.tar.gz
   cd spinnaker-*-arm64
   
   # Run installer
   sudo sh install_spinnaker_arm.sh
   
   # Follow prompts, accept license
   # Add user to flirimaging group
   sudo usermod -aG flirimaging $USER
   ```

4. **Reboot** to apply group changes
   ```bash
   sudo reboot
   ```

5. **Verify installation**
   ```bash
   # List connected cameras
   spinview
   # Or command line
   SpinnakerConsole
   ```

### Step 2: Configure Camera Settings

1. **Set USB buffer size** (for reliable high-speed capture)
   ```bash
   # Increase USB buffer memory
   echo 1000 | sudo tee /sys/module/usbcore/parameters/usbfs_memory_mb
   
   # Make permanent
   echo 'options usbcore usbfs_memory_mb=1000' | sudo tee /etc/modprobe.d/usb-buffer.conf
   ```

2. **Test camera capture**
   ```bash
   # Using Spinnaker examples
   cd /opt/spinnaker/bin
   ./Acquisition
   ```

3. **Configure camera parameters** (in application or via SpinView):
   - Resolution: Set based on requirements (e.g., 1920x1200)
   - Frame rate: Balance between speed and bandwidth
   - Exposure: Auto or manual based on application
   - Gain: Start with auto gain
   - Pixel format: Mono8, Mono16, or BayerRG8 depending on camera

### Step 3: Multi-Camera Configuration

For systems with multiple cameras:

1. **Identify cameras**
   ```bash
   # List all connected cameras with serial numbers
   v4l2-ctl --list-devices
   # Or using Spinnaker
   SpinnakerConsole
   ```

2. **Set up udev rules** for consistent device naming
   ```bash
   sudo nano /etc/udev/rules.d/99-flir-cameras.rules
   ```
   
   Add rules (example):
   ```
   SUBSYSTEM=="usb", ATTR{idVendor}=="1e10", ATTR{serial}=="12345678", SYMLINK+="camera0"
   SUBSYSTEM=="usb", ATTR{idVendor}=="1e10", ATTR{serial}=="87654321", SYMLINK+="camera1"
   ```
   
   ```bash
   # Reload udev rules
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   ```

3. **Label physical cameras** to match device assignments

---

## IMU Setup

### Step 1: Install Xsense Software

1. **Install MT Software Suite**
   ```bash
   # Download from Xsense website
   # Install dependencies
   sudo apt install -y libusb-1.0-0-dev
   
   # Install MT SDK (adjust version as needed)
   # Follow Xsense documentation for ARM64 installation
   ```

2. **Set up USB permissions**
   ```bash
   # Create udev rule for Xsense device
   sudo nano /etc/udev/rules.d/99-xsense.rules
   ```
   
   Add:
   ```
   SUBSYSTEM=="usb", ATTR{idVendor}=="2639", MODE="0666"
   SUBSYSTEM=="usb", ATTR{idVendor}=="2639", ATTR{idProduct}=="0017", MODE="0666"
   ```
   
   ```bash
   # Reload rules
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   ```

### Step 2: Configure IMU

1. **Test IMU connection**
   ```bash
   # Using MT Manager (GUI)
   mtmanager
   
   # Or command line tools
   # Check device enumeration
   lsusb | grep Xsens
   ```

2. **Set IMU parameters**:
   - Output rate: 100Hz or 400Hz (based on application)
   - Output mode: Orientation, acceleration, angular velocity
   - Coordinate system: ENU or NED (document choice)
   - Filter settings: Configure onboard fusion filter

3. **Save configuration to device**

### Step 3: Install ROS Integration (Optional)

If using ROS for sensor fusion:

```bash
# Install ROS 2 (Humble for Ubuntu 22.04)
sudo apt install -y ros-humble-desktop

# Install Xsense ROS driver
sudo apt install -y ros-humble-xsens-mti-driver

# Source ROS
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

## PX4 Setup and Hardware Synchronization

### Overview

The Jetson CV Hub uses PX4 as the master timing source for hardware synchronization. PX4 produces a trigger signal at 100 Hz that synchronizes the FLIR cameras and Xsense IMU. This setup provides:

- **Precise Temporal Alignment**: All sensors capture data at exactly the same time
- **Redundant IMU**: PX4 provides an additional IMU source for sensor fusion
- **Easy Low-Level Control**: Direct access to PWM outputs, GPIO, and other interfaces
- **Drone Deployability**: System can be directly integrated into UAV/drone platforms

### Hardware Synchronization Architecture

```
PX4 Flight Controller (Master, 100 Hz)
    ├── Trigger Output → FLIR Camera 1
    ├── Trigger Output → FLIR Camera 2
    ├── Trigger Output → FLIR Camera 3 (if present)
    ├── Trigger Output → FLIR Camera 4 (if present)
    └── Trigger Output → Xsense IMU
```

All devices receive the same trigger signal simultaneously, ensuring synchronized data capture.

### Step 1: PX4 Hardware Setup

1. **Connect PX4 to Jetson**:
   - USB connection for MAVLink communication and configuration
   - Trigger output pins connected to camera GPIO inputs
   - Optional: Serial UART connection for higher bandwidth

2. **Wire Trigger Outputs**:
   - PX4 AUX outputs can be configured as camera triggers
   - Connect PX4 AUX1-4 to camera GPIO trigger inputs
   - Use appropriate voltage level conversion (3.3V or 5V depending on camera)
   - Parallel connection to Xsense IMU sync input

### Step 2: Install PX4 Software

```bash
# Install MAVLink dependencies
sudo apt install -y python3-pip
sudo pip3 install pymavlink

# Install QGroundControl (for PX4 configuration)
# Download from: http://qgroundcontrol.com/
# Or use command line:
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y

wget https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage
chmod +x QGroundControl.AppImage
```

### Step 3: Configure PX4 for Camera Triggering

Connect to PX4 using QGroundControl or MAVLink console:

```bash
# Launch QGroundControl
./QGroundControl.AppImage

# Or use MAVLink console
mavproxy.py --master=/dev/ttyUSB0 --baudrate 57600
```

Configure camera trigger parameters:

```bash
# In PX4 console (MAVLink shell or QGroundControl MAVLink Console)

# Enable camera trigger
param set CAM_TRIG_MODE 1

# Set trigger interval for 100 Hz (10 ms)
param set CAM_TRIG_INTERVAL 10.0

# Set trigger polarity (active high/low depending on camera)
param set CAM_TRIG_POLARITY 0

# Set trigger on time (pulse width in milliseconds)
param set CAM_TRIG_DURATION 1.0

# Map trigger to AUX output (e.g., AUX1)
param set CAM_TRIG_PINS 56  # AUX1 = pin 56

# Save parameters
param save

# Reboot PX4
reboot
```

### Step 4: Configure FLIR Cameras for External Trigger

Using Spinnaker SDK or SpinView:

```bash
# Launch SpinView
spinview

# Or configure via command line (Python example)
python3 << EOF
import PySpin

system = PySpin.System.GetInstance()
cam_list = system.GetCameras()

for cam in cam_list:
    cam.Init()
    
    # Set trigger mode
    cam.TriggerMode.SetValue(PySpin.TriggerMode_On)
    
    # Set trigger source to Line0 (GPIO)
    cam.TriggerSource.SetValue(PySpin.TriggerSource_Line0)
    
    # Set trigger activation (rising edge)
    cam.TriggerActivation.SetValue(PySpin.TriggerActivation_RisingEdge)
    
    # Optional: Set trigger delay
    cam.TriggerDelay.SetValue(0.0)
    
    print(f"Camera {cam.DeviceSerialNumber()} configured for external trigger")
    
    cam.DeInit()

cam_list.Clear()
system.ReleaseInstance()
EOF
```

### Step 5: Configure Xsense IMU for External Sync

Using MT Manager or MT Software Suite:

1. **Launch MT Manager**:
   ```bash
   mtmanager
   ```

2. **Configure Sync Settings**:
   - Open Device Settings
   - Navigate to Synchronization Settings
   - Set Sync Mode: External Trigger
   - Set Sync Signal: Rising Edge
   - Set Output Rate: 100 Hz (matching PX4 trigger)
   - Clock Source: External
   - Save configuration to device

3. **Or configure via command line** (if MT SDK available):
   ```bash
   # Example configuration (device-specific)
   # Consult Xsense documentation for exact commands
   ```

### Step 6: Verify Synchronization

Test that all sensors are receiving triggers:

```bash
# Monitor PX4 trigger output
# In MAVLink console:
listener camera_trigger

# Check camera frame rate
# Should show exactly 100 Hz
ros2 topic hz /camera/image_raw

# Check IMU data rate
# Should show exactly 100 Hz
ros2 topic hz /imu/data
```

### Step 7: Install MAVROS for ROS2 Integration

MAVROS enables ROS2 communication with PX4:

```bash
# Install MAVROS for ROS2 Humble
sudo apt install ros-humble-mavros ros-humble-mavros-extras -y

# Install GeographicLib datasets (required for GPS)
sudo /opt/ros/humble/lib/mavros/install_geographiclib_datasets.sh
```

### Step 8: Launch MAVROS

Create a launch file for MAVROS:

```bash
mkdir -p ~/ros2_ws/src/jetson_cv_hub_launch/launch
nano ~/ros2_ws/src/jetson_cv_hub_launch/launch/mavros.launch.py
```

Add:

```python
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='mavros',
            executable='mavros_node',
            name='mavros',
            output='screen',
            parameters=[{
                'fcu_url': '/dev/ttyUSB0:57600',
                'gcs_url': '',
                'target_system_id': 1,
                'target_component_id': 1,
                'fcu_protocol': 'v2.0',
            }]
        )
    ])
```

Launch MAVROS:

```bash
cd ~/ros2_ws
colcon build
source install/setup.bash

ros2 launch jetson_cv_hub_launch mavros.launch.py
```

### Step 9: Monitor PX4 Status in ROS2

```bash
# List MAVROS topics
ros2 topic list | grep mavros

# Monitor PX4 state
ros2 topic echo /mavros/state

# Monitor IMU data from PX4
ros2 topic echo /mavros/imu/data

# Monitor battery status
ros2 topic echo /mavros/battery
```

### Synchronization Verification

To verify proper synchronization:

1. **Check Timestamp Alignment**:
   ```bash
   # Record data from all sensors
   ros2 bag record /camera/image_raw /imu/data /mavros/imu/data
   
   # Analyze timestamps in recorded bag
   ros2 bag info rosbag2_*
   ```

2. **Verify Trigger Frequency**:
   - All sensors should report exactly 100 Hz
   - Timestamps should be aligned within 1-2 milliseconds

3. **Visual Verification**:
   - Record synchronized video and IMU data
   - Fast motion should show no temporal misalignment
   - Use visual-inertial SLAM to verify sync quality

### Troubleshooting Synchronization

If synchronization issues occur:

```bash
# Check PX4 trigger output
# In QGroundControl MAVLink console:
listener camera_trigger

# Verify camera trigger configuration
spinview  # Check trigger settings in SpinView

# Check GPIO connections
# Ensure trigger signals reach camera GPIO pins
# Use oscilloscope if available

# Verify Xsense sync input
mtmanager  # Check sync status in MT Manager

# Check cable connections and signal levels
# PX4 output should match camera/IMU input voltage requirements
```

---

## Network Configuration

### Step 1: Configure Static IP (Optional)

For production deployment:

```bash
# Edit netplan configuration
sudo nano /etc/netplan/01-network-manager-all.yaml
```

Example configuration:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

Apply configuration:
```bash
sudo netplan apply
```

### Step 2: Enable SSH

```bash
# Install SSH server if not present
sudo apt install -y openssh-server

# Enable and start SSH
sudo systemctl enable ssh
sudo systemctl start ssh

# Configure firewall (if ufw is used)
sudo ufw allow ssh
sudo ufw enable
```

### Step 3: Set up Remote Access

```bash
# From host computer, test SSH connection
ssh jetson@192.168.1.100

# Set up SSH keys for passwordless login (optional)
ssh-copy-id jetson@192.168.1.100
```

---

## Software Installation

### Step 1: Install Computer Vision Libraries

```bash
# OpenCV (usually pre-installed with JetPack)
python3 -c "import cv2; print(cv2.__version__)"

# If not installed or need different version
sudo apt install -y python3-opencv

# Additional CV libraries
pip3 install numpy scipy scikit-image
```

### Step 2: Install Deep Learning Frameworks

```bash
# PyTorch (for Jetson)
# Download from NVIDIA: https://forums.developer.nvidia.com/t/pytorch-for-jetson/
wget https://nvidia.box.com/shared/static/[pytorch-wheel-url]
pip3 install torch-*.whl

# TensorFlow (for Jetson)
# Follow NVIDIA's guide for ARM64 version
sudo apt install -y python3-tensorflow
```

### Step 3: Install Development Environment

```bash
# Jupyter for interactive development
pip3 install jupyter jupyterlab

# VS Code Server (for remote development)
# Download and install code-server or use VS Code Remote-SSH

# Create virtual environment for projects
python3 -m venv ~/cv_env
source ~/cv_env/bin/activate
```

### Step 4: Clone Project Repositories

```bash
# Clone this repository
cd ~
git clone https://github.com/SasaKuruppuarachchi/jetson-cv-hub.git
cd jetson-cv-hub

# Install any project-specific dependencies
pip3 install -r requirements.txt  # If provided
```

---

## System Calibration

### Step 1: Camera Calibration

Refer to [calibration/README.md](../calibration/README.md) for detailed procedures.

**Quick Start**:

```bash
# Using OpenCV calibration
python3 calibrate_camera.py --pattern checkerboard --size 9x6 --square 0.025

# Or using ROS camera_calibration
ros2 run camera_calibration cameracalibrator --size 8x6 --square 0.108 \
     image:=/camera/image_raw camera:=/camera
```

Save results to `calibration/camera/intrinsics/`

### Step 2: IMU Calibration

```bash
# Follow Xsense calibration procedure
# Usually involves:
# 1. Stationary calibration (bias)
# 2. Six-position orientation calibration
# 3. Save calibration to device

# Use MT Manager GUI or command-line tools
```

Save calibration data to `calibration/imu/`

### Step 3: Camera-IMU Calibration

```bash
# Using Kalibr (recommended)
# Install Kalibr dependencies
# Run calibration with synchronized camera-IMU data

kalibr_calibrate_cameras --target target.yaml --bag calibration.bag \
                          --models pinhole-radtan --topics /camera/image
```

Save results to `calibration/camera_imu/`

---

## Verification and Testing

### Step 1: Hardware Tests

```bash
# Check system resources
htop

# Monitor temperatures
sudo tegrastats

# Test GPU
/usr/local/cuda/samples/1_Utilities/deviceQuery/deviceQuery

# Disk usage
df -h
```

### Step 2: Camera Tests

```bash
# Test camera stream
# Using OpenCV
python3 -c "
import cv2
cap = cv2.VideoCapture(0)
ret, frame = cap.read()
print('Camera working!' if ret else 'Camera failed')
cap.release()
"

# Test multiple cameras
# [Add multi-camera test script]
```

### Step 3: IMU Tests

```bash
# Test IMU data stream
# Using MT SDK or ROS
# Verify orientation, acceleration, gyro data
```

### Step 4: Integration Tests

```bash
# Test synchronized camera-IMU capture
# Verify timestamps alignment
# Check data quality and rates
```

### Step 5: Performance Benchmarks

```bash
# Measure camera FPS
# Measure IMU rate
# Check CPU/GPU utilization
# Monitor power consumption
```

---

## Troubleshooting

### Common Issues

#### Issue: Camera not detected

**Check**:
```bash
lsusb  # Should show FLIR camera
dmesg | grep -i usb  # Check for USB errors
```

**Solutions**:
- Verify USB 3.0 connection (blue port)
- Check cable quality
- Increase USB buffer size
- Try different USB port

#### Issue: Low camera frame rate

**Check**:
- USB bandwidth limitations (multiple cameras)
- Exposure time too long
- Processing bottleneck

**Solutions**:
- Reduce resolution or frame rate
- Optimize image processing pipeline
- Use hardware acceleration

#### Issue: IMU data unavailable

**Check**:
```bash
lsusb | grep -i xsens
dmesg | tail -20
```

**Solutions**:
- Verify udev rules
- Check USB connection
- Restart IMU device
- Update firmware

#### Issue: High CPU temperature

**Check**:
```bash
sudo tegrastats  # Monitor temperature
```

**Solutions**:
- Verify fan operation
- Check thermal paste application
- Improve airflow
- Reduce power mode if acceptable

#### Issue: Network connectivity problems

**Check**:
```bash
ip addr show
ping 8.8.8.8
```

**Solutions**:
- Verify cable connection
- Check router DHCP settings
- Review netplan configuration
- Check firewall rules

---

## Performance Optimization

### Step 1: Enable Performance Governors

```bash
# Set CPU governor to performance
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

### Step 2: Optimize Camera Pipeline

- Use hardware-accelerated codecs (NVENC/NVDEC)
- Leverage CUDA for image processing
- Optimize memory allocation and copying
- Use zero-copy techniques where possible

### Step 3: System Tuning

```bash
# Disable GUI if not needed (saves memory)
sudo systemctl set-default multi-user.target

# Re-enable GUI
sudo systemctl set-default graphical.target
```

---

## Maintenance

### Regular Tasks

**Weekly**:
- Check disk space
- Review system logs
- Verify sensor functionality

**Monthly**:
- Update software packages
- Clean dust from cooling system
- Backup calibration data

**Quarterly**:
- Full system backup
- Review and update documentation
- Performance benchmarking

---

## Next Steps

After setup completion:

1. **Develop Applications**:
   - Start with simple camera capture scripts
   - Add IMU data integration
   - Implement sensor fusion algorithms

2. **Optimize Performance**:
   - Profile application bottlenecks
   - Leverage Jetson hardware acceleration
   - Optimize power/performance balance

3. **Deploy**:
   - Package applications for deployment
   - Set up auto-start services
   - Implement logging and monitoring

---

## Additional Resources

### Documentation
- [NVIDIA Jetson Documentation](https://docs.nvidia.com/jetson/)
- [FLIR Spinnaker SDK](https://www.flir.com/products/spinnaker-sdk/)
- [Xsense MTi Documentation](https://www.xsens.com/software-downloads)

### Community
- NVIDIA Jetson Forums
- ROS Discourse
- GitHub Issues (this repository)

### Support
For technical support:
- Open an issue on GitHub
- Check FAQ section
- Contact component manufacturers

---

## Document Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | [Author] | Initial draft release |

---

**End of Setup Instructions**

For questions or improvements to this document, please open an issue or submit a pull request on the GitHub repository.

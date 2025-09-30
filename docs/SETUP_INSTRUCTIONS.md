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
3. [Jetson Configuration](#jetson-configuration)
4. [Camera Setup](#camera-setup)
5. [IMU Setup](#imu-setup)
6. [Network Configuration](#network-configuration)
7. [Software Installation](#software-installation)
8. [System Calibration](#system-calibration)
9. [Verification and Testing](#verification-and-testing)
10. [Troubleshooting](#troubleshooting)

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

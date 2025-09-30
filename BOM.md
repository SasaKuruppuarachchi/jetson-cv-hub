# Bill of Materials (BOM)

## Jetson CV Hub - Component List

### Core Components

| Item | Description | Quantity | Manufacturer/Part Number | Notes |
|------|-------------|----------|--------------------------|-------|
| 1 | NVIDIA Jetson Orin (NX/Nano/AGX) | 1 | NVIDIA | Specify model based on requirements |
| 2 | FLIR Machine Vision Camera | 1-4 | FLIR/Teledyne | Model: [Specify model] |
| 3 | Xsense IMU | 1 | Xsense | Model: [Specify model] |
| 4 | Power Supply Unit | 1 | [TBD] | Input: 100-240V AC, Output: [TBD]V DC |
| 5 | Power Distribution Board | 1 | Custom/COTS | Provides power to all components |

### Mounting Hardware

| Item | Description | Quantity | Part Number | Notes |
|------|-------------|----------|-------------|-------|
| 6 | M3 x 8mm Socket Head Cap Screws | 20 | ISO 4762 | For general assembly |
| 7 | M3 x 12mm Socket Head Cap Screws | 10 | ISO 4762 | For thicker mounting points |
| 8 | M3 Hex Nuts | 30 | ISO 4032 | Standard hex nuts |
| 9 | M3 Washers | 30 | ISO 7089 | Flat washers |
| 10 | M2.5 x 6mm Screws | 16 | ISO 7045 | For camera mounting |

### Connectors & Cables

| Item | Description | Quantity | Part Number | Notes |
|------|-------------|----------|-------------|-------|
| 11 | USB 3.0 Cable | 2-4 | [TBD] | For camera connections |
| 12 | Ethernet Cable | 1 | CAT6 | For network connectivity |
| 13 | Power Cable (IEC C13/C14) | 1 | [TBD] | Mains power input |
| 14 | GPIO Connector | 1 | [TBD] | For IMU interface |

### 3D Printed Parts

| Item | Description | Quantity | CAD File | Material |
|------|-------------|----------|----------|----------|
| 15 | Main Housing - Top | 1 | `housing_top.stl` | PETG |
| 16 | Main Housing - Bottom | 1 | `housing_bottom.stl` | PETG |
| 17 | Camera Mount | 1-4 | `camera_mount.stl` | PLA/PETG |
| 18 | IMU Mount | 1 | `imu_mount.stl` | PLA |
| 19 | Jetson Mount Plate | 1 | `jetson_mount.stl` | PETG |
| 20 | Cable Management Clips | 5-10 | `cable_clip.stl` | PLA |

### Electronics & Accessories

| Item | Description | Quantity | Part Number | Notes |
|------|-------------|----------|-------------|-------|
| 21 | Cooling Fan | 1-2 | [TBD] | 40mm x 10mm, 5V/12V |
| 22 | Thermal Pads | As needed | [TBD] | For heat dissipation |
| 23 | Velcro Straps | 5 | [TBD] | For cable management |
| 24 | Anti-slip Pads | 4 | [TBD] | For base stability |

### Optional Components

| Item | Description | Quantity | Part Number | Notes |
|------|-------------|----------|-------------|-------|
| 25 | microSD Card | 1 | SanDisk/Samsung | 64GB+ for Jetson storage |
| 26 | SSD (NVMe) | 1 | [TBD] | Optional expanded storage |
| 27 | WiFi/Bluetooth Module | 1 | [TBD] | If not integrated in Jetson |
| 28 | LED Indicators | 3-5 | [TBD] | Status/power indicators |

## Notes

- Quantities are for a single unit configuration
- Part numbers marked as [TBD] should be specified based on specific project requirements
- Camera quantity (1-4) depends on the specific application needs
- All fasteners are metric; imperial equivalents may be substituted where appropriate
- Refer to assembly instructions for specific component placement

## Total Estimated Cost

- Core Components: $[TBD]
- Hardware & Cables: $[TBD]
- 3D Printing Materials: $[TBD]
- **Total**: $[TBD]

## Suppliers

- Electronics: Digi-Key, Mouser, Arrow
- Fasteners: McMaster-Carr, Fastenal
- Cameras: FLIR/Teledyne Direct, Authorized Distributors
- Jetson: NVIDIA Partners, SparkFun, Adafruit

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | [Author] | Initial BOM release |


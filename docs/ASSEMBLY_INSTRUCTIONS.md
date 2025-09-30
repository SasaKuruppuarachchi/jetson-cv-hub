# Assembly Instructions

## Jetson CV Hub - Hardware Assembly Guide

### Document Information

- **Version**: 1.0 (Draft)
- **Last Updated**: [Date]
- **Author**: [Author Name]
- **Status**: Draft - Subject to refinement

---

## Table of Contents

1. [Safety Precautions](#safety-precautions)
2. [Required Tools](#required-tools)
3. [Component Overview](#component-overview)
4. [Pre-Assembly Preparation](#pre-assembly-preparation)
5. [Assembly Steps](#assembly-steps)
6. [Quality Checks](#quality-checks)
7. [Troubleshooting](#troubleshooting)

---

## Safety Precautions

**READ BEFORE STARTING ASSEMBLY**

- Work in a clean, static-free environment
- Use an ESD wrist strap when handling electronic components
- Disconnect all power sources before assembly
- Handle cameras and IMU with care - these are precision instruments
- Ensure proper ventilation when 3D printing parts
- Follow manufacturer safety guidelines for all components

---

## Required Tools

### Essential Tools

- [ ] Hex key set (metric: 1.5mm, 2mm, 2.5mm)
- [ ] Phillips screwdriver (#1, #2)
- [ ] Wire cutters/strippers
- [ ] Precision tweezers
- [ ] Multimeter (for electrical continuity testing)
- [ ] ESD wrist strap
- [ ] Magnifying glass or microscope (optional, for small connectors)

### Consumables

- [ ] Thermal paste or thermal pads
- [ ] Cable ties (various sizes)
- [ ] Velcro straps
- [ ] Isopropyl alcohol (for cleaning)
- [ ] Lint-free cloths

---

## Component Overview

### Verify Components

Before beginning assembly, verify all components from the [Bill of Materials (BOM)](../BOM.md):

#### Core Electronics
- [ ] NVIDIA Jetson Orin module
- [ ] FLIR machine vision camera(s) - [Quantity: 1-4]
- [ ] Xsense IMU
- [ ] Power supply unit
- [ ] Power distribution board

#### 3D Printed Parts
- [ ] Main housing (top and bottom)
- [ ] Camera mount(s)
- [ ] IMU mounting bracket
- [ ] Jetson mounting plate
- [ ] Cable management clips

#### Hardware & Fasteners
- [ ] M3 screws (various lengths)
- [ ] M2.5 screws
- [ ] Hex nuts
- [ ] Washers
- [ ] Standoffs (if required)

#### Cables & Connectors
- [ ] USB 3.0 cables
- [ ] Ethernet cable
- [ ] Power cables
- [ ] GPIO/interface cables

---

## Pre-Assembly Preparation

### Step 1: Prepare 3D Printed Parts

1. **Inspect printed parts** for quality
   - Check for layer adhesion
   - Verify dimensional accuracy
   - Look for warping or defects

2. **Clean printed parts**
   - Remove support material completely
   - Sand any rough edges with fine-grit sandpaper (220-400 grit)
   - Clean dust with compressed air or soft brush

3. **Test fit components**
   - Dry-fit major components without fasteners
   - Verify mounting holes align
   - Check clearances for cables and connectors

### Step 2: Prepare Electronic Components

1. **Inspect electronics**
   - Check for shipping damage
   - Verify model numbers match BOM
   - Test components individually if possible

2. **Prepare Jetson Orin**
   - Install heatsink/cooling solution if not pre-installed
   - Apply thermal paste according to manufacturer guidelines
   - Install microSD card or NVMe SSD if using

3. **Prepare cameras**
   - Remove protective lens caps (save for later)
   - Verify lens mount is secure
   - Check cable connectors for damage

---

## Assembly Steps

### Phase 1: Base Assembly

#### Step 1: Install Jetson Module

1. Place the Jetson mounting plate in the bottom housing
2. Align mounting holes with standoffs or mounting posts
3. Secure Jetson to mounting plate using M2.5 screws
   - **Torque**: Hand-tight, do not over-tighten
   - **Pattern**: Tighten in diagonal pattern to ensure even pressure
4. Connect cooling fan to Jetson power header (if applicable)
5. Verify Jetson is firmly mounted and level

**Visual Check**: ✓ Jetson should be secure with no wobble

#### Step 2: Mount Power Distribution

1. Position power distribution board in designated area
2. Secure with M3 screws
3. Verify clearance from other components (minimum 5mm)
4. Check mounting is electrically isolated (if required)

**Visual Check**: ✓ No short circuits to housing

#### Step 3: Install IMU

1. Attach IMU to mounting bracket using M2.5 screws
2. **IMPORTANT**: Ensure IMU orientation is correct
   - Note axis alignment (X, Y, Z marked on device)
   - Document orientation for calibration
3. Mount bracket assembly to main housing using M3 screws
4. Ensure rigid mounting - no flex or vibration
5. Connect IMU cable to Jetson (GPIO or USB, depending on interface)

**Visual Check**: ✓ IMU is rigidly mounted, axes aligned correctly

### Phase 2: Camera Installation

#### Step 4: Mount Cameras

**For each camera:**

1. Attach camera to camera mount using M2.5 screws
   - Use appropriate mounting pattern for your camera model
   - **Torque**: Very light - do not over-tighten precision equipment
   
2. Position camera mount on housing
   - Consider field of view requirements
   - Ensure no obstruction to lens
   - Plan cable routing path
   
3. Secure camera mount to housing using M3 screws
   - Verify camera orientation (horizontal/vertical)
   - Check viewing angle
   
4. **Multi-camera setups**:
   - Maintain consistent baseline (distance between cameras)
   - Ensure cameras are level relative to each other
   - Document camera positions for calibration

**Visual Check**: ✓ Cameras securely mounted, lenses unobstructed, orientation correct

### Phase 3: Electrical Connections

#### Step 5: Connect Power

⚠️ **IMPORTANT**: Complete all connections BEFORE connecting mains power

1. Connect power distribution outputs:
   - Jetson power input (verify voltage: typically 9-19V)
   - Camera power (verify voltage requirements)
   - IMU power (if separate supply needed)
   - Fan power (5V or 12V as required)

2. Route power cables neatly
   - Use cable clips to secure routing
   - Avoid crossing signal cables
   - Ensure no strain on connectors

3. **Do NOT connect mains power yet**

**Visual Check**: ✓ All power connections secure, polarity correct, no exposed wires

#### Step 6: Connect Data Cables

1. **Camera connections**:
   - Connect USB 3.0 cables from cameras to Jetson USB ports
   - Verify connector orientation (USB-C/USB-A)
   - Secure cables with cable management clips
   - Label each camera (Camera 0, 1, 2, 3)

2. **IMU connection**:
   - Connect IMU to Jetson via GPIO or USB
   - Ensure proper pin mapping
   - Secure connector

3. **Network connection**:
   - Connect Ethernet cable if using wired networking
   - Route to external port on housing

**Visual Check**: ✓ All data cables connected, labels clear, routing neat

### Phase 4: Cable Management

#### Step 7: Organize Cables

1. Bundle similar cables together
   - Use velcro straps (not zip ties initially - easier to adjust)
   - Maintain separation between power and signal cables
   - Leave some slack for serviceability

2. Secure cables to housing
   - Use 3D printed cable clips
   - Avoid pinch points
   - Ensure cables don't obstruct airflow

3. Final routing check
   - No cables touching hot components
   - No strain on connectors
   - Clean, professional appearance

**Visual Check**: ✓ Neat cable routing, no stress on connectors, proper cable separation

### Phase 5: Final Assembly

#### Step 8: Close Housing

1. Verify all components are secured
2. Check all connections one final time
3. Ensure no tools or loose items inside housing
4. Position top housing cover
5. Align mounting holes
6. Install screws progressively (don't fully tighten until all are started)
7. Tighten in star/cross pattern for even pressure
8. Verify housing closes completely with no gaps

**Visual Check**: ✓ Housing fully closed, no gaps, all screws tight

#### Step 9: External Connections

1. Install any external connectors or ports
2. Attach cable strain reliefs
3. Install rubber feet or mounting hardware
4. Attach any external indicators (LEDs, buttons)

---

## Quality Checks

### Pre-Power Checks

Before connecting power, perform these checks:

- [ ] Visual inspection of all connections
- [ ] Multimeter continuity test (no shorts between power rails)
- [ ] Verify correct voltage at power inputs (use adjustable bench supply if available)
- [ ] Check USB cable integrity
- [ ] Verify no loose screws or parts inside housing
- [ ] Confirm cooling fan spins freely (if manually testable)

### Initial Power-On

1. **First power-up**:
   - Use a current-limited power supply if possible
   - Monitor current draw during startup
   - Expected range: [TBD]A at [TBD]V
   
2. **Visual indicators**:
   - Jetson power LED should illuminate
   - Check for any unexpected heat
   - Listen for unusual sounds
   
3. **If any issues**:
   - Immediately disconnect power
   - Refer to troubleshooting section
   - Do NOT continue until issue is resolved

### Functional Tests

After successful power-on:

- [ ] Jetson boots successfully (connect display/SSH)
- [ ] Cameras detected by system (`lsusb` or camera software)
- [ ] IMU communication established
- [ ] Network connectivity (if applicable)
- [ ] Cooling fan operational
- [ ] All LEDs functioning

---

## Troubleshooting

### Common Issues

#### Problem: Jetson doesn't boot

**Possible Causes**:
- Insufficient power supply current
- Incorrect voltage
- Loose power connections
- Faulty microSD/NVMe

**Solutions**:
1. Verify power supply specifications
2. Check all power connections
3. Try different storage media
4. Test Jetson separately outside housing

#### Problem: Camera not detected

**Possible Causes**:
- Loose USB connection
- Insufficient USB power
- Wrong USB port type (USB 2.0 vs 3.0)
- Driver issues

**Solutions**:
1. Reconnect USB cable firmly
2. Try different USB port
3. Check camera LED indicators
4. Verify drivers installed
5. Test camera on different computer

#### Problem: IMU not responding

**Possible Causes**:
- Incorrect pin connections
- Communication protocol mismatch
- Power supply issue
- Faulty cable

**Solutions**:
1. Verify pin mapping against documentation
2. Check power to IMU
3. Test with manufacturer's software
4. Verify communication settings (baud rate, protocol)

#### Problem: Overheating

**Possible Causes**:
- Inadequate cooling
- Blocked airflow
- Thermal paste not applied
- Fan not operational

**Solutions**:
1. Check fan operation
2. Verify airflow paths not blocked
3. Reapply thermal compound
4. Consider additional cooling
5. Verify power load within specifications

### Getting Help

If problems persist:
1. Document the issue with photos
2. Note all error messages
3. Check system logs (`dmesg`, `/var/log/`)
4. Open an issue on the GitHub repository
5. Include assembly photos and system configuration

---

## Next Steps

After successful assembly:

1. Proceed to [Setup Instructions](SETUP_INSTRUCTIONS.md)
2. Install necessary software and drivers
3. Perform sensor calibration (see [calibration/README.md](../calibration/README.md))
4. Run system validation tests

---

## Appendix

### Recommended Screw Torque Values

| Screw Size | Maximum Torque | Notes |
|------------|----------------|-------|
| M2.5 | 0.4 Nm | For cameras and sensitive electronics |
| M3 | 0.6 Nm | For structural assembly |
| M4 | 1.2 Nm | If used for heavy components |

**Note**: Hand-tighten screws for plastic parts. Use torque wrench for critical applications.

### Maintenance Schedule

- **Monthly**: Visual inspection, dust removal
- **Quarterly**: Check screw tightness, cable condition
- **Annually**: Reapply thermal paste, deep cleaning

### Document Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | [Author] | Initial draft release |

---

**End of Assembly Instructions**

For questions or improvements to this document, please open an issue or submit a pull request on the GitHub repository.

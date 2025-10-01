# CAD Files

This directory contains 3D printable CAD files and design source files for the Jetson CV Hub.

## Directory Structure

```
cad/
├── 3d_prints/              # Ready-to-print STL files
│   ├── base_jetplate.stl           # Standard base plate
│   ├── base_jetplate_bat.stl       # Base plate with battery mounting
│   ├── Handle.stl                  # Carrying handle
│   ├── jet_display_mount.stl       # Display mount for Jetson
│   ├── jet_display_mount_cover.stl # Display mount cover
│   ├── t265_mount_imm.stl          # Primary T265/camera mount
│   ├── t265_mount_imm_secondary.stl # Secondary camera mount
│   ├── t265cover1.stl              # Camera protective cover
│   └── VICON_HOLDER.stl            # VICON marker holder
│
├── cad/                    # Source CAD files (Autodesk Inventor)
│   ├── jetson_cv_hub_ver1.ipj     # Main project file
│   ├── ver1_jetstation.iam        # Main assembly
│   ├── Imm_mount_ver1.iam         # IMM mount assembly
│   ├── t265_mount_imm.ipt         # Camera mount part
│   ├── t265_mount_imm_secondary.ipt # Secondary mount part
│   ├── t265cover.ipt              # Cover part
│   ├── blkflyu3.ipt               # FLIR camera model
│   ├── mti_3.ipt                  # MTi IMU model
│   └── External_models/           # External component models
│       └── Jetson NX/             # Jetson NX 3D models
│
├── Cuts/                   # 2D cutting files (DXF/DWG)
│   ├── Floor1_jetstation.dxf      # Floor plate cutting file
│   └── Floor1_jetstation.dwg      # Floor plate CAD file
│
└── README.md              # This file
```

## Contents

The CAD files include designs for:
- Base plates for Jetson mounting (standard and battery versions)
- Camera mounts for FLIR machine vision cameras
- IMU (Xsense) mounting bracket
- Jetson display mount and cover
- Carrying handle for portability
- Protective covers
- Modular mounting interfaces for reconfigurability
- VICON marker holder for motion capture integration

## File Formats

CAD files are provided in the following formats:
- `.stl` - Ready for 3D printing (in `3d_prints/` directory)
- `.ipt`, `.iam` (Autodesk Inventor) - Editable source files (in `cad/` directory)
- `.dxf`, `.dwg` - 2D cutting files for metal/acrylic parts (in `Cuts/` directory)
- `.ipj` - Autodesk Inventor project file

### Opening Source Files

To edit the source CAD files:
- Install **Autodesk Inventor** (commercial or educational license)
- Open `jetson_cv_hub_ver1.ipj` project file
- Main assembly: `ver1_jetstation.iam`
- Individual parts can be edited in `.ipt` files

## Printing Recommendations

- Material: PLA, PETG, or ABS recommended
- Layer Height: 0.2mm
- Infill: 20-30% for structural parts
- Supports: May be required for some components
- Print orientation: Refer to individual part documentation

## Assembly Notes

See the [Assembly Instructions](../docs/ASSEMBLY_INSTRUCTIONS.md) for detailed guidance on assembling printed parts.

## License

All CAD files are released under the Apache License 2.0. See the [LICENSE](../LICENSE) file for details.

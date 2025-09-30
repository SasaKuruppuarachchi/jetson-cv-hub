# Figures Directory

This directory contains images and figures used in the Jetson CV Hub documentation.

## Usage

Images in this directory can be referenced in markdown documentation files:

```markdown
![Figure description](../figures/image-name.png)
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

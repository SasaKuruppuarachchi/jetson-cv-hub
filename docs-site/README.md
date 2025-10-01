# GitHub Pages Site for Jetson CV Hub

This directory contains the static site files for the Jetson CV Hub GitHub Pages website.

## Overview

The GitHub Pages site provides a user-friendly interface to access all project documentation, including:
- Project overview (from README.md)
- Assembly instructions (from docs/ASSEMBLY_INSTRUCTIONS.md)
- Setup instructions (from docs/SETUP_INSTRUCTIONS.md)
- Quick start guide (from QUICK_START.md)
- Bill of Materials (from BOM.md)

## Site Structure

```
docs-site/
├── index.html           # Landing page with project overview
├── assembly.html        # Assembly instructions page
├── setup.html          # Setup instructions page
├── quick-start.html    # Quick start guide page
├── bom.html            # Bill of Materials page
├── css/
│   └── style.css       # Site styling
└── README.md           # This file
```

## How It Works

The site uses a static HTML approach with JavaScript to dynamically load markdown content from the repository. This ensures that:

1. **Content is always up-to-date**: The site pulls markdown files directly from the main branch
2. **Single source of truth**: Documentation is maintained in one place (the repository)
3. **Figures are accessible**: Any images in the repository can be referenced in markdown files

## Deployment

The site is automatically deployed to GitHub Pages using GitHub Actions:

1. When changes are pushed to the main branch, the workflow runs
2. The workflow copies the `docs-site` folder and documentation files to a build directory
3. The build is deployed to GitHub Pages
4. The site becomes available at: `https://sasakuruppuarachchi.github.io/jetson-cv-hub/`

## Local Development

To preview the site locally:

1. Use any static web server (e.g., Python's http.server):
   ```bash
   cd docs-site
   python3 -m http.server 8000
   ```

2. Open your browser to `http://localhost:8000`

Note: Due to CORS restrictions, you'll need to test with the full build to see markdown content loaded properly.

## Adding Figures

To add figures that will be displayed on the site:

1. Organize images in the `figures/` directory by category:
   ```
   figures/
   ├── assembly/           # Assembly photos (3D printing, camera assembly, power assembly, etc.)
   ├── hardware/          # Hardware component photos (cameras, Jetson, IMU, PX4, etc.)
   ├── diagrams/          # System diagrams and schematics (sync, calibration, results)
   ├── attachments/       # Optional attachments (VICON holders, etc.)
   └── (root files)       # Main device images (front, back, isometric, parts diagram)
   ```

2. Add your images to the appropriate subdirectory with descriptive filenames

3. Reference them in your markdown files using relative paths:
   ```markdown
   ![Description](../figures/assembly/your-image.jpg)
   ![Component](../figures/hardware/component-name.png)
   ![Diagram](../figures/diagrams/system-diagram.jpg)
   ```

4. The workflow automatically copies the entire `figures/` directory to the site

## Customization

### Styling

Edit `css/style.css` to customize the appearance of the site.

### Colors

The site uses CSS variables defined in `:root`:
- `--primary-color`: Main accent color (NVIDIA green)
- `--secondary-color`: Dark color for headers and navigation
- `--accent-color`: Additional accent color

### Navigation

To add new pages:
1. Create a new HTML file in `docs-site/`
2. Add the page to the navigation menu in all HTML files
3. Update the GitHub Actions workflow if new markdown files need to be copied

## License

This site and all documentation are licensed under the Apache License 2.0.

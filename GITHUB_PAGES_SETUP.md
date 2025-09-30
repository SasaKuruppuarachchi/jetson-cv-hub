# GitHub Pages Setup Guide

This guide explains how to enable and configure GitHub Pages for the Jetson CV Hub repository.

## Overview

The repository is configured to automatically deploy a documentation website to GitHub Pages. The site pulls content from:
- README.md (main project overview)
- docs/ASSEMBLY_INSTRUCTIONS.md (assembly guide)
- docs/SETUP_INSTRUCTIONS.md (setup guide)
- QUICK_START.md (quick start guide)
- BOM.md (bill of materials)
- figures/ directory (images and diagrams)

## Enabling GitHub Pages

### Step 1: Configure Repository Settings

1. Go to your repository on GitHub: https://github.com/SasaKuruppuarachchi/jetson-cv-hub
2. Click on **Settings** (top menu)
3. Scroll down to **Pages** in the left sidebar
4. Under **Build and deployment**, set:
   - **Source**: GitHub Actions
   
That's it! The GitHub Actions workflow will handle the deployment automatically.

### Step 2: Wait for Deployment

After merging this PR to the main branch:
1. The GitHub Actions workflow will run automatically
2. Go to the **Actions** tab to monitor the deployment
3. Once complete, your site will be available at:
   ```
   https://sasakuruppuarachchi.github.io/jetson-cv-hub/
   ```

## How It Works

### Automatic Deployment

The `.github/workflows/gh-pages.yml` workflow:
1. Triggers on every push to the main branch
2. Builds the site by copying:
   - Static HTML/CSS files from `docs-site/`
   - Markdown documentation files
   - Images from the `figures/` directory
3. Deploys the build to GitHub Pages
4. Makes the site accessible at the GitHub Pages URL

### Content Updates

When you update any of these files on the main branch:
- README.md
- docs/ASSEMBLY_INSTRUCTIONS.md
- docs/SETUP_INSTRUCTIONS.md
- QUICK_START.md
- BOM.md
- Any images in figures/

The GitHub Pages site will automatically update within a few minutes!

## Site Structure

The deployed site includes:

```
https://sasakuruppuarachchi.github.io/jetson-cv-hub/
├── index.html              # Landing page (from README.md)
├── assembly.html           # Assembly instructions
├── setup.html             # Setup instructions
├── quick-start.html       # Quick start guide
├── bom.html               # Bill of Materials
├── docs/                  # Original markdown files
├── figures/               # Images and diagrams
└── css/style.css          # Styling
```

## Adding New Pages

To add a new documentation page:

1. Create a new HTML file in `docs-site/`:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>New Page - Jetson CV Hub</title>
       <link rel="stylesheet" href="css/style.css">
       <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
   </head>
   <body>
       <!-- Navigation -->
       <!-- Content -->
       <script>
           fetch('your-markdown-file.md')
               .then(response => response.text())
               .then(markdown => {
                   const html = marked.parse(markdown);
                   document.getElementById('markdown-content').innerHTML = html;
               });
       </script>
   </body>
   </html>
   ```

2. Add the page to the navigation menu in all existing HTML files

3. Update `.github/workflows/gh-pages.yml` if the new page requires additional markdown files

## Adding Images

To add images that will appear on the site:

1. Create subdirectories in `figures/` if needed:
   ```
   figures/
   ├── assembly/
   ├── hardware/
   └── diagrams/
   ```

2. Add your images to the appropriate subdirectory

3. Reference them in your markdown files:
   ```markdown
   ![Assembly Step 1](../figures/assembly/step1.jpg)
   ```

4. The workflow automatically copies the entire `figures/` directory to the site

## Customization

### Changing Colors

Edit `docs-site/css/style.css` and modify the CSS variables:

```css
:root {
    --primary-color: #76b900;     /* NVIDIA green */
    --secondary-color: #333;      /* Dark gray */
    --accent-color: #ff6c00;      /* Orange */
}
```

### Changing Layout

Edit the HTML files in `docs-site/` to modify:
- Navigation structure
- Page layout
- Footer content
- Hero section on the landing page

## Troubleshooting

### Site Not Deploying

1. Check the **Actions** tab for workflow errors
2. Ensure GitHub Pages is set to "GitHub Actions" as the source
3. Verify the workflow file has correct permissions

### Images Not Loading

1. Ensure images are in the `figures/` directory on the main branch
2. Check that the workflow copies the `figures/` directory
3. Verify image paths in markdown use relative paths (e.g., `../figures/image.png`)

### Markdown Not Rendering

1. Check browser console for JavaScript errors
2. Verify markdown files are copied to the build directory
3. Ensure the fetch paths in HTML files are correct

## Manual Deployment

To manually trigger a deployment:

1. Go to **Actions** tab
2. Select **Deploy GitHub Pages** workflow
3. Click **Run workflow**
4. Choose the main branch
5. Click **Run workflow**

## Status Badge

Add a deployment status badge to your README:

```markdown
![Deploy Status](https://github.com/SasaKuruppuarachchi/jetson-cv-hub/actions/workflows/gh-pages.yml/badge.svg)
```

## Support

For issues with GitHub Pages deployment:
- Check GitHub's [official documentation](https://docs.github.com/en/pages)
- Review the workflow logs in the Actions tab
- Open an issue in the repository

---

**Note**: The first deployment may take a few minutes. Subsequent deployments are typically faster.

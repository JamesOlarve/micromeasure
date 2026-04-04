# MicroMeasure

MicroMeasure is a browser-based image analysis and measurement tool for scientific images. It supports calibrated length, angle, and area measurements together with particle and hole analysis directly in the browser.

## Links

- Live App: https://jamesolarve.github.io/micromeasure/
- Project Page: https://inanolab.com/tools/micromeasure

## Features

- Load local microscopy or other scientific images
- Calibrate image scale using a known reference length
- Measure lengths, angles, and polygon or circle areas
- Analyze particles and holes with thresholding and preprocessing controls
- Export screenshots and TSV measurement data

## Run Locally

Because this is a static site, you can preview it with any local web server.

### Python

```bash
python -m http.server 8000
```

Then open:

http://localhost:8000/

## Deployment

This repository is configured to deploy automatically to GitHub Pages from the `main` branch using GitHub Actions.

## Notes

- Optional host-specific SDK files are loaded only when available.
- The GitHub Pages deployment serves the static files in this repository as-is.

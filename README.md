# MicroMeasure

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19418862.svg)](https://doi.org/10.5281/zenodo.19418862)

A Web-Based Scientific Image Analysis and Measurement Tool  
Version: 1.0

Developer: Dr. James Salveo Olarve  
Affiliation: i-Nano Research Facility, De La Salle University Manila

Live App: https://jamesolarve.github.io/micromeasure/  
Project Page: https://inanolab.com/tools/micromeasure

---

## Overview

MicroMeasure is a browser-based image analysis application designed for quantitative measurements of scientific images, particularly microscopy images such as scanning electron microscopy (SEM) micrographs.

It enables calibrated measurements of length, angle, area, particle size, and hole size directly within a web browser without requiring software installation.

Inspired by established tools such as ImageJ, MicroMeasure is optimized for accessibility, fast workflows, and educational use.

---

## Intended Users

- Researchers and laboratory personnel
- Graduate and undergraduate students
- Educators and instructors
- Users in materials science, physics, chemistry, biology, and engineering

---

## Key Features

- Load local microscopy and scientific images
- Calibrate scale using a known reference length
- Measure:
	- Lengths
	- Angles
	- Areas (polygon and circular)
- Perform particle and hole (void) analysis
- Apply preprocessing:
	- Background subtraction
	- Smoothing filters
	- Morphological operations
- Thresholding methods:
	- Otsu (auto)
	- Manual
	- Adaptive
- Generate descriptive statistics and histograms
- Export:
	- Annotated images (PNG)
	- Measurement data (TSV format)

---

## 📘 Methodology

See detailed methodology in [docs/methodology.md](docs/methodology.md)

---

## Assumptions

- The image is free from geometric distortion
- Calibration reference is accurate
- Measurements are performed on a single plane

---

## Quick Start

1. Load an image (drag and drop or file upload)
2. Set the scale using a known reference length
3. Select a measurement mode (length, angle, area, particle, hole)
4. Perform measurements
5. Export results (TSV or image)

---

## Particle And Hole Analysis

Includes automated detection using:

- ROI (Region of Interest) selection
- Background subtraction
- Smoothing filters (Gaussian / Median)
- Morphological opening
- Thresholding (Auto, Manual, Adaptive)

Outputs include:

- Count
- Mean area
- Equivalent Circular Diameter (ECD)
- Size distribution

---

## Statistics And Visualization

- Descriptive statistics (mean, median, standard deviation, quartiles)
- Histogram visualization
- Available when sufficient data points are present

---

## Validation

MicroMeasure is designed to provide consistent measurements comparable to standard tools such as ImageJ under proper calibration conditions.

Users are encouraged to validate results for their specific applications.

---

## Use Cases

- SEM micrograph analysis
- Materials characterization
- Biological image measurement
- Laboratory instruction and demonstrations
- Rapid browser-based analysis

---

## Run Locally

Because this is a static web application:

```bash
python -m http.server 8000
```

Open:

http://localhost:8000/

---

## Deployment

This project is deployed using GitHub Pages and runs entirely in the browser.

---

## Citation

Olarve, J. S. L. (2026). MicroMeasure: A web-based image analysis and measurement tool.  
i-Nano Research Facility, De La Salle University Manila.  
DOI: (to be assigned)

This project now includes a citation metadata file: [CITATION.cff](CITATION.cff)

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).

---

## Disclaimer

MicroMeasure is intended for research and educational use.

While efforts have been made to ensure accuracy, users are responsible for verifying calibration and validating results for their specific applications.

This tool does not replace certified metrology software or instrument calibration standards.

---

## Acknowledgment

Developed by the i-Nano Research Facility  
De La Salle University Manila

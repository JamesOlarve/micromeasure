---
title: 'MicroMeasure: A Web-Based Scientific Image Analysis and Measurement Tool'
tags:
  - image analysis
  - microscopy
  - scientific measurement
  - scanning electron microscopy
  - materials characterization
  - browser-based tool
authors:
  - name: James Salveo Olarve
    orcid: 0000-0000-0000-0000
    affiliation: 1
affiliations:
  - name: i-Nano Research Facility, De La Salle University Manila, Philippines
    index: 1
date: 01 June 2026
bibliography: paper.bib
---

# Summary

MicroMeasure is a browser-based scientific image analysis tool designed for calibrated quantitative measurements of microscopy and other scientific images. Without requiring any software installation, MicroMeasure enables researchers, students, and educators to perform length, angle, area, particle size, and hole (void) measurements directly in a web browser. The tool implements standard geometric and image processing algorithms — including Euclidean distance, dot-product angle computation, the Shoelace formula for polygon area, and Otsu thresholding for automated particle segmentation — and converts all pixel-based measurements to physically meaningful units through user-defined scale calibration. Results can be exported as annotated images (PNG) or tabular data (TSV) for downstream analysis. MicroMeasure runs entirely client-side, meaning all image data and computations remain on the user's machine, with no server upload required.

# Statement of Need

Quantitative image analysis is a routine task across the physical, biological, and materials sciences. Researchers working with scanning electron microscopy (SEM), transmission electron microscopy (TEM), optical microscopy, and similar imaging techniques frequently need to measure feature dimensions, particle sizes, angular relationships, and area distributions directly from micrographs. While powerful desktop tools such as ImageJ [@schneider2012nih] and Fiji [@schindelin2012fiji] exist for this purpose, they require local software installation, Java runtime environments, and non-trivial configuration — creating friction for new users, students in laboratory courses, and researchers working across different machines or operating systems.

MicroMeasure addresses this gap by providing an immediately accessible, installation-free alternative that runs in any modern web browser. It is designed to serve:

- **Researchers** who need rapid, calibrated measurements from microscopy images without setting up a software environment
- **Graduate and undergraduate students** in materials science, physics, chemistry, biology, and engineering courses who need a low-barrier tool for image-based laboratory work
- **Educators and instructors** who require a consistent, cross-platform tool for classroom demonstrations and laboratory assignments

The tool is particularly well-suited for SEM micrograph analysis in materials characterization workflows, where fast, visual measurement of particle or fiber dimensions is a common analytical step. Two additional capabilities further distinguish MicroMeasure from existing desktop tools. First, measurement labels are rendered directly on the image canvas as interactive overlays — showing measurement IDs and calibrated values in real units — and the annotated image can be exported as a PNG in a single click. Producing an equivalent labeled export in ImageJ requires macro scripting or manual annotation steps, which adds overhead that is impractical in teaching or rapid-characterization contexts. Second, MicroMeasure automatically generates a histogram of measurement values once a minimum of four data points are available for a given feature type (length, area, angle, ECD), eliminating the need to export raw data to a separate application just to visualize a distribution.

# State of the Field

ImageJ [@schneider2012nih] and its distribution Fiji [@schindelin2012fiji] are the most widely adopted open-source tools for scientific image analysis, offering extensive plugin ecosystems and scripting capabilities. However, both require desktop installation and a Java runtime environment. CellProfiler [@stirling2021cellprofiler] provides sophisticated automated cell image analysis but is domain-specific and also requires installation. QuPath [@bankhead2017qupath] is designed for digital pathology and whole-slide imaging, with a focus on tissue analysis. MIPAR [@sosa2014mipar] is a commercial tool targeting materials science image segmentation.

MicroMeasure occupies a distinct niche: it prioritizes immediate accessibility and ease of use over extensibility. Unlike ImageJ or Fiji, it requires no installation and no configuration. Unlike CellProfiler or QuPath, it is domain-agnostic and suitable for general-purpose measurement tasks across scientific imaging contexts. The design decision to implement MicroMeasure as a static web application rather than contributing to existing tools was deliberate: the goal was to eliminate the installation barrier entirely, making the tool usable in educational settings, shared computing environments, and resource-constrained laboratories where software installation may not be permitted.

Beyond installation, MicroMeasure also differs from ImageJ in workflow integration. ImageJ does not natively render calibrated measurement labels as persistent visual overlays on the image during the measurement session, nor does it provide one-click export of a fully annotated image with all labels intact without additional macro or plugin steps. MicroMeasure renders each measurement — labeled with its ID and calibrated value in real units — directly on the image canvas as the user works, and exports the complete annotated image as a PNG in a single action. Similarly, while ImageJ provides histogram functionality through menu navigation and plugins, MicroMeasure triggers histogram generation automatically as soon as sufficient data (n ≥ 4) are present, keeping the distribution visualization immediately visible alongside the measurement workflow without requiring the user to navigate away.

# Software Design

MicroMeasure is implemented as a single-page static web application using HTML, CSS, and vanilla JavaScript. All image processing and measurement computations are performed entirely client-side using the browser's Canvas API, with no server-side processing. This architecture ensures data privacy (images are never uploaded), offline usability (the tool can be served locally via a simple HTTP server), and broad platform compatibility.

**Calibration.** Users define a scale by clicking two endpoints of a known reference feature (e.g., a scale bar embedded in a micrograph) and entering the corresponding real-world length and unit. The scaling factor $s$ is computed as:

$$s = \frac{L_{\text{real}}}{L_{\text{pixel}}}$$

where $L_{\text{real}}$ is the known physical length and $L_{\text{pixel}}$ is the pixel distance between the selected endpoints. All subsequent measurements are converted to real-world units by multiplying pixel measurements by $s$.

**Distance measurement.** Point-to-point distances are computed using the Euclidean formula:

$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

**Angle measurement.** Three-point angles are computed using the dot product of vectors from the vertex:

$$\theta = \cos^{-1}\!\left(\frac{\mathbf{A} \cdot \mathbf{B}}{|\mathbf{A}||\mathbf{B}|}\right)$$

where $\mathbf{A}$ and $\mathbf{B}$ are vectors from the vertex point to each of the two arm endpoints.

**Area measurement.** Polygon areas are computed using the Shoelace formula:

$$A = \frac{1}{2}\left|\sum_{i=0}^{n-1}(x_i y_{i+1} - x_{i+1} y_i)\right|$$

Circular areas are computed as $A = \pi r^2$ from a user-defined radius.

**Particle and hole analysis.** Automated detection of particles or voids follows an image processing pipeline: (1) optional background subtraction and smoothing (Gaussian or median filter); (2) morphological opening to suppress small artifacts; (3) thresholding via Otsu's method [@otsu1979threshold], manual threshold, or adaptive thresholding; and (4) connected-component labeling. Detected regions are characterized by their area $A$, from which the Equivalent Circular Diameter (ECD) is derived:

$$\text{ECD} = \sqrt{\frac{4A}{\pi}}$$

Region of Interest (ROI) selection allows analysis to be confined to a user-defined sub-region of the image.

**Measurement overlay and annotated export.** All measurements are rendered as persistent visual overlays directly on the image canvas. Each annotation displays a unique measurement ID and its calibrated value in real-world units (e.g., "L3: 84.54 nm"), positioned at the measurement site. The overlay is composited onto the original image using the Canvas 2D API and can be exported as a single PNG file with all labels intact. This one-click annotated export is available for all measurement types — length, angle, area, particle, and hole — and does not require any scripting or plugin configuration. The exported image is suitable for direct inclusion in reports and publications.

**Automatic histogram generation.** Once four or more measurements are recorded for a given feature type, MicroMeasure automatically generates a histogram of the distribution. For particle and hole analysis, the histogram can be plotted by either ECD or area, selectable via a metric toggle. Bin count is adjustable interactively within the histogram window. This in-situ visualization eliminates the need to export raw data to external software simply to inspect a measurement distribution during an active analysis session.

**Statistics and tabular export.** Descriptive statistics (mean, median, standard deviation, quartiles) accompany the histogram when sufficient data are available. Measurement tables can be copied as tab-separated values (TSV) or downloaded as a TSV file, compatible with spreadsheet and statistical software for further analysis.

All computations are deterministic; given the same image and user inputs, results are fully reproducible across sessions.

# Research Impact Statement

MicroMeasure was developed at the i-Nano Research Facility, De La Salle University Manila, to support ongoing nanomaterials characterization research and undergraduate laboratory instruction. The tool has been used internally for SEM micrograph analysis in materials science research workflows, including fiber diameter measurement and particle size distribution analysis. The software is publicly accessible at [https://jamesolarve.github.io/micromeasure/](https://jamesolarve.github.io/micromeasure/) and is archived with a persistent DOI via Zenodo [@olarve2026micromeasure]. It is designed to produce measurements consistent with those obtained from standard tools such as ImageJ under equivalent calibration conditions.

# AI Usage Disclosure

Claude AI (Anthropic) was used to assist in drafting documentation and supporting materials for this submission. In software development, Canva AI Code was used to generate an initial prototype of the user interface, focusing on front-end layout and design. The resulting code was subsequently reviewed, substantially revised, extended, and completed by the author using Visual Studio Code, with all final architectural decisions, algorithm implementations, and scientific logic authored entirely by the human developer. All AI-assisted outputs were validated by the author prior to release.

# Acknowledgements

The author acknowledges the i-Nano Research Facility, De La Salle University Manila, for institutional support in the development of MicroMeasure.

# References

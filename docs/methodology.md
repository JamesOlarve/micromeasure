Scientific Methodology - MicroMeasure

MicroMeasure performs quantitative image analysis based on calibrated pixel-to-real-world unit conversion and geometric measurement algorithms. The methodology ensures that measurements obtained from digital images correspond to physically meaningful values.

------------------------------------------------------------

1. Pixel-to-Real-World Calibration

A known reference length (e.g., a scale bar in a microscopy image) is used to compute a scaling factor:

scale = real_length / pixel_length

Where:
- real_length = known physical length (e.g., nm, µm, mm)
- pixel_length = corresponding length in pixels

All measurements are converted using:

real_measurement = pixel_measurement × scale

------------------------------------------------------------

2. Distance Measurement Algorithm

Distances are computed using the Euclidean distance formula:

d = √[(x2 - x1)^2 + (y2 - y1)^2]

Where:
- (x1, y1) and (x2, y2) are pixel coordinates

------------------------------------------------------------

3. Angle Measurement Algorithm

Angles are computed using three points, where the middle point is the vertex.

cos(θ) = (A · B) / (|A||B|)

Where:
- A and B are vectors from the vertex
- θ is reported in degrees

------------------------------------------------------------

4. Area Measurement Algorithm

Polygon areas are computed using the Shoelace formula:

A = 1/2 |Σ (xi yi+1 - xi+1 yi)|

Circular areas:

A = πr^2

------------------------------------------------------------

5. Particle and Hole Analysis

Preprocessing:
- Background subtraction
- Smoothing (Gaussian / Median)
- Morphological opening

Segmentation:
- Otsu thresholding
- Manual threshold
- Adaptive threshold

Measurement:
Equivalent Circular Diameter (ECD):

ECD = √(4A / π)

------------------------------------------------------------

6. Statistical Analysis

- Mean
- Median
- Standard deviation
- Quartiles
- Histogram visualization

------------------------------------------------------------

Assumptions and Limitations

- Image is properly calibrated
- No significant geometric distortion
- Single measurement plane
- Adequate resolution

------------------------------------------------------------

Reproducibility

All computations are deterministic and performed in the browser. Given the same input, results are reproducible.

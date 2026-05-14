## 📐 Module: System Geometry and Reconstruction Principles

Understanding the geometry of the imaging system is paramount, as the mathematical accuracy of the image reconstruction—the transformation from raw measurement data into a usable digital image—depends entirely on precise knowledge of the physical setup.

This module details both the **Physical Geometry** (the physical arrangement of components) and the **Computational Geometry** (the geometric rules governing data acquisition and image reconstruction).

---

### I. The Physical Geometry of the Mammography System

The mammography system is designed as a controlled source-object-detector chain. Deviations in the assumptions made about this physical setup lead directly to image distortions (e.g., magnification errors or streaking artifacts).

#### A. Key Geometric Components and Variables

| Component | Definition | Standard Variables | Impact on Image |
| :--- | :--- | :--- | :--- |
| **X-ray Source** | Emits the diagnostic beam. The focal spot size defines the resolution limit. | $D_{source}$ (Distance to Object) | Determines source geometry and magnification. |
| **Object** | The breast tissue being imaged. Position, thickness, and shape are variable. | $D_{object}$ (Object position) | Subject to positioning errors and cupping artifacts. |
| **Detector** | Captures the attenuated X-ray signal. Modern detectors are solid-state arrays (e.g., Direct Conversion Detectors). | $D_{detector}$ (Detector position) | Defines the geometric path length and image plane. |
| **Beam Path** | The path taken by the X-rays from source to detector. | $d_{source-detector}$ (Source-to-Detector distance) | The actual path length measured. |

#### B. The Geometry of Attenuation and Signal Path

The fundamental physical principle governing the image formation is the **Beer-Lambert Law**:

$$I = I_0 \cdot e^{-\mu x}$$

*   $I$: Intensity detected at the detector plane.
*   $I_0$: Initial intensity at the object plane.
*   $\mu$: Linear attenuation coefficient ($\text{cm}^{-1}$), which represents the material properties (density, composition).
*   $x$: The total path length ($\text{cm}$) through the material.

**Critical Geometric Consideration: Path Length ($x$)**
The recorded signal $I$ is not solely dependent on the object's attenuation coefficient ($\mu$) but on the *physical path length* ($x$) the beam traverses through the tissue. For any point $(x, y)$ in the image plane, the path length $x$ must be accurately calculated using the Pythagorean theorem based on the source-object and object-detector distances.

---

### II. The Computational Geometry of Image Acquisition

In modern digital radiography, the image is not formed by a single X-ray beam, but by an array of measurements (projection data) taken at various angles. This moves the system into the realm of **Tomography**.

#### A. Projection Data and Source-Detector Geometry

1.  **Projection:** A projection is a 1D measurement of how much the X-ray intensity is attenuated along a single, defined path (a ray).
2.  **Detector Array:** The detector captures thousands of these projections simultaneously across its active area.
3.  **Angular Sampling:** The system acquires multiple views (projections) taken at different angles ($\theta$) around the object. Each view provides constraints on the object's unknown density map.

#### B. The Math of Reconstruction: From Projection to Image

The central challenge is **Image Reconstruction**: deriving a 2D or 3D map ($\mu(x, y)$) from the measured line integrals ($\int \mu(x, y) dl$).

**1. Radon Transform (The Forward Model):**
The measurement process can be modeled by the Radon Transform ($\mathcal{R}$). It maps the unknown 2D object function $\mu(x, y)$ into the measured line integral $P(r, \theta)$:

$$P(r, \theta) = \int_{\text{line}} \mu(x, y) \, dl$$

*   $P(r, \theta)$: The measured intensity profile at a distance $r$ and angle $\theta$.
*   $\mu(x, y)$: The unknown density map (what we want to solve for).

**2. Inverse Problem (The Reconstruction):**
The goal of computational geometry in imaging is to invert the Radon Transform—to calculate $\mu(x, y)$ from $P(r, \theta)$. The most famous algorithm for this inversion is the **Filtered Backprojection (FBP)**:

$$\mu(x, y) = \frac{1}{2\pi} \int_{0}^{\pi} \left[ \frac{1}{\text{distance}(\text{line}, (x, y))} \int P(r, \theta) \, dr \right] d\theta$$

*   **The "Filtering" Step:** The measured projections $P(r, \theta)$ must be mathematically "filtered" (e.g., using a Ram-Lak filter) to correct for the inherent smoothing effect of the Radon transform itself.
*   **The "Backprojection" Step:** The filtered data is then smeared (backprojected) across the 2D plane along the path of measurement, allowing the individual measured lines to add up constructively to reveal the underlying 2D density map $\mu(x, y)$.

---

### III. Geometric Errors and Artifacts

Any discrepancy between the *assumed* geometry (the parameters input into the reconstruction algorithm) and the *actual* physical geometry introduces artifacts:

1.  **Magnification Error:** If the true source-to-object distance ($D_{source-object}$) is inaccurately measured, the final image will have incorrect magnification, appearing stretched or shrunk.
2.  **Beam Hardening:** If the source beam is not perfectly monochromatic (i.e., it contains multiple energies), the assumption of uniform attenuation breaks down, causing streaks or cupping artifacts that look like geometric errors.
3.  **Detector Misalignment:** If the measured projection angle $(\theta)$ is slightly off, the backprojection will not perfectly overlap the data, leading to faint streaks or 'ghost' artifacts.

**Conclusion:** A robust imaging system requires not only high-quality X-ray generation but also an extremely precise characterization of the source, object, and detector positions, allowing the mathematical reconstruction process to flawlessly invert the Radon Transform.
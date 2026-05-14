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

---


The geometry discussed till now is mainly for CT systems.

In Digital Breast Tomosynthesis (DBT), the geometry is different because the X-ray tube moves across a **limited arc** (usually 15° to 50°) rather than a full circle.
---

### 📐 System Geometry and Digital Breast Tomosynthesis (DBT) Reconstruction

#### 📝 Theory: Limited-Angle Geometry and Slice Reconstruction
In mammography, the physical arrangement of the source, breast, and detector is critical. While standard mammography uses a fixed geometry, DBT introduces motion to capture depth information.

##### 1. The Mammography Gantry Geometry
The system is a controlled chain designed to maximize resolution and minimize magnification errors.
*   **Focal Spot Size:** Mammography uses a very small focal spot (typically 0.1 to 0.3 mm) to maintain high spatial resolution for detecting tiny microcalcifications.
*   **Source-to-Image Distance (SID):** Usually fixed around 65-70 cm. Any deviation in the assumed SID during reconstruction leads to scaling artifacts and incorrect sizing of lesions.
*   **Object-to-Detector Distance (ODD):** Minimized through compression to reduce geometric magnification, which would otherwise blur the image (penumbra effect).

---

##### 2. DBT Acquisition: Limited-Angle Tomography
Unlike CT, which acquires data from 360° around the patient, DBT is **Limited-Angle Tomography**.
*   **The Scan Arc:** The X-ray tube moves in a small arc over the compressed breast, taking a discrete number of low-dose projections (e.g., 15-25 projections).
*   **Angular Sampling:** Because we do not have a "complete" dataset (missing angles), the reconstruction cannot use the standard Radon Transform found in CT without significant modification.

---

##### 3. The Math of DBT Reconstruction: Beyond Simple Backprojection
The goal is to convert these 2D projections into thin 1 mm slices that allow the radiologist to "see through" overlapping tissue.

*   **Filtered Backprojection (FBP) for DBT:** In DBT, the "filtering" step is not a standard Ram-Lak filter but is optimized to handle the limited data and reduce the **"out-of-plane" artifacts** (where a dense object in one slice appears as a ghost in others).
*   **Iterative Reconstruction:** Modern systems often use iterative algorithms. These start with an estimate of the breast volume and "correct" it repeatedly by comparing it to the actual measured projections until the error is minimized. This provides better noise reduction than FBP.
*   **Voxel Resolution:** In DBT, pixels become **voxels** (3D pixels). While the X-Y resolution is very high (determined by the detector), the Z-axis (depth) resolution is lower due to the limited acquisition angle.

---

##### 4. Specific Artifacts in Breast Imaging Geometry
*   **Limited-Angle Artifacts:** Because X-rays don't pass through the sides of the breast at 90°, the reconstruction of vertical borders is less sharp than horizontal ones.
*   **Detector Misalignment:** If the detector and the moving tube are not perfectly synchronized, microcalcifications may appear as "double images" or "smears".
*   **Breast Motion:** Even slight patient movement during the 5-10 second DBT scan arc creates "rippling" artifacts in the reconstructed slices.

---

##### 🖥️ Implementation: Simulating Depth Separation
The computational focus is on demonstrating how moving the source across an arc allows us to mathematically "focus" on a specific plane while blurring out the tissues above and below it.

---

### Next Steps: New Modules
Now that we have cleaned up the existing modules, we can proceed to create the **new modules** we discussed. Which one would you like to build first?

1.  **Mean Glandular Dose (MGD) & Dosimetry:** Deep dive into how we calculate the actual radiation risk to breast tissue.
2.  **Radiological Anatomy & Tissue Physics:** Understanding the $\mu$ (attenuation) differences between glandular, adipose, and cancerous tissues.
3.  **Synthetic Mammography (C-View):** How AI and geometry are used to create a 2D image from 3D DBT data to save dose.

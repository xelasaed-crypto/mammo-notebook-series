# 🧬 Mammography Physics and Computational Imaging Workflow

Welcome to the **Mammography Imaging Workflow Notebook Series**. This educational project provides an end-to-end exploration of the physics, mathematics, and computational techniques that power modern breast imaging, from X-ray generation to AI-driven diagnostics.

## 🎯 Series Objectives
The goal of this curriculum is to move beyond simple image acquisition and foster a **Clinician Mindset**. Students will learn to:
*   Understand the **physical origin of contrast** in breast tissue.
*   Master the mathematics of **volumetric reconstruction** in Digital Breast Tomosynthesis (DBT).
*   Implement **computational workflows** for image enhancement and pathology segmentation.
*   Apply **clinical stewardship** through dose optimization (ALARA) and quality assurance.

---

## 📚 Curriculum Structure

### Phase 1: The Physics of the Beam
1.  **X-ray Generation and Beam Physics:** The origin of photons via Bremsstrahlung and Characteristic radiation.
2.  **The Physics of Attenuation:** A deep dive into the **Beer-Lambert Law** ($I = I_0 e^{-\mu x}$) and how $\mu$ creates the signal.
3.  **Beam-Matter Interactions:** Understanding why the **Photoelectric Effect** is the primary source of contrast and how **Compton Scattering** introduces noise.
4.  **Radiological Anatomy and Tissue Physics (New):** Analyzing the specific attenuation coefficients of glandular tissue, adipose tissue, and microcalcifications (Note: Added during recent revision).

### Phase 2: Signal Capture and System Geometry
5.  **Detector Physics:** How flat-panel detectors convert X-ray quanta into digital pixels and the importance of **DQE**.
6.  **System Geometry and DBT Acquisition:** The physical arrangement of the source, breast, and detector, focusing on **limited-angle tomography** (Revised to remove CT-specific 360° concepts).
7.  **Digital Breast Tomosynthesis (DBT):** Using multiple projections to mitigate **tissue superimposition** and reconstruct depth information.

### Phase 3: Image Processing and Advanced Techniques
8.  **Image Filtering and Enhancement:** Applying Gaussian, Laplacian, and **Unsharp Masking (USM)** filters to sharpen diagnostic details.
9.  **Synthetic Mammography (New):** Generating 2D maps from 3D data to reduce patient dose while maintaining visibility of microcalcifications (Note: Added during recent revision).
10. **Advanced Techniques and Artifact Recognition:** Troubleshooting clinical images, identifying motion blur, and managing high-density artifacts (e.g., surgical clips or deodorants) (Revised to remove Hounsfield Units).
11. **Post-Processing and AI Diagnostics:** Moving from pixels to metadata through **Semantic Segmentation** (U-Net) and texture analysis.

### Phase 4: Clinical Stewardship
12. **MGD and Radiation Dosimetry (New):** Calculating the **Mean Glandular Dose (MGD)** as the primary safety metric for breast imaging (Note: Added during recent revision).
13. **Quality Assurance and Ethics:** Equipment calibration, phantom testing, and the legal responsibilities of the radiologic technologist.

---

## 🛠️ Key Technical Concepts

### The Foundation: Differential Attenuation
Contrast is not created by the detector; it is created by the varying $\mu$ values of tissues. The system must maximize **Photoelectric Absorption** while minimizing **Compton Scattering** through the use of anti-scatter grids.

### The Innovation: Tomosynthesis
DBT solves the problem of "hidden" pathologies by acquiring projections at varying angles. This allows the mathematical separation of overlapping structures that would otherwise be blurred into a single 2D plane.

### The Responsibility: ALARA
Radiation safety is paramount. Every technical choice—from $kVp$ selection to the use of synthetic 2D views—must be weighed against the **Mean Glandular Dose** to ensure the lowest possible risk to the patient.

---

## 💻 Computational Implementation
Each module includes a Python-based simulation to demonstrate these concepts:
*   **Signal Simulation:** Mapping physical density to pixel intensity.
*   **Reconstruction Simulation:** Visualizing how depth separation occurs in a reconstructed volume.
*   **AI Pipeline:** Segmenting features and calculating confidence scores for clinical validation.

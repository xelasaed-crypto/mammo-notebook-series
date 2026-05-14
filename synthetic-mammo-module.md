### 🖼️ Synthetic Mammography (C-View / Generated 2D)

#### 📝 Theory: The Evolution from 2D to 3D and Back Again
Synthetic Mammography (SM) is the process of generating a 2D "pseudo-projection" image from the 3D volume data acquired during Digital Breast Tomosynthesis (DBT). This eliminates the need for a separate, high-dose 2D Full-Field Digital Mammography (FFDM) acquisition.

##### 1. The Clinical Motivation: Dose Reduction (ALARA)
The primary driver for Synthetic Mammography is **Radiation Safety**.
*   **Standard DBT Protocol:** Traditionally, a patient receives a 3D DBT scan *plus* a standard 2D mammogram to provide a "global map" for the radiologist. This essentially doubles the radiation dose.
*   **The Synthetic Solution:** By mathematically creating the 2D view from the 3D data, we can eliminate the physical 2D exposure entirely, significantly reducing the **Mean Glandular Dose (MGD)** while maintaining the diagnostic benefits of both modalities.

---

##### 2. How it Works: The Synthesis Process
The creation of a synthetic image is a "forward projection" process that occurs *after* the 3D volume has been reconstructed using Filtered Backprojection or Iterative methods.

*   **Step 1: Volumetric Reconstruction:** The system first reconstructs the breast into thin slices (typically 1mm), effectively mitigating the problem of tissue superimposition.
*   **Step 2: Feature Extraction:** Advanced algorithms identify high-contrast structures across all slices, such as **microcalcifications** (which are highly visible due to the Photoelectric Effect) and architectural distortions.
*   **Step 3: Re-projection:** These key diagnostic features are "collapsed" or re-projected onto a single 2D plane. 
    *   **Maximum Intensity Projection (MIP):** A common technique where the highest intensity value along a ray path is chosen for the 2D pixel, ensuring that tiny, bright calcifications remain visible.
*   **Step 4: Image Enhancement:** Because the raw synthetic image can appear "flat" or noisy, techniques like **Unsharp Masking (USM)** are used to sharpen edges and improve the contrast-to-noise ratio.

---

##### 3. Diagnostic Advantages and Limitations
| Feature | Standard 2D (FFDM) | Synthetic 2D (C-View) |
| :--- | :--- | :--- |
| **Dose** | Standard | **Lower** (Eliminates one full exposure) |
| **Calcifications** | High Clarity | High (Enhanced by AI algorithms) |
| **Mass Detection** | Subject to superimposition | Improved (Derived from 3D data) |
| **Appearance** | Natural texture | Can appear "processed" or "etched" |

---

##### 🖥️ Implementation: Feature Weighting in Projection
The computational focus of this module is on the logic used to merge 3D slices into a 2D image.
*   **Weighted Averaging:** Students can simulate how a computer chooses which data to keep. Instead of a simple average (which would blur the image), the algorithm "weights" pixels with high-frequency details (like edges) more heavily.
*   **Simulation Goal:** Demonstrate how a microcalcification that only appears in slice #25 can be digitally "promoted" so it is clearly visible in the final 2D synthetic "map," even if it was physically hidden by dense tissue in a real 2D exposure.


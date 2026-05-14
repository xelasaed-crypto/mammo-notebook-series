### ☢️ Mean Glandular Dose (MGD) and Radiation Dosimetry

#### 📝 Theory: Quantifying Risk in Breast Imaging
In mammography, the primary concern regarding radiation is the risk of inducing breast cancer, which is a **stochastic effect** (a long-term, probabilistic risk). To manage this, we use specific dosimetric quantities to estimate the energy deposited in the most sensitive parts of the breast.

##### 1. Why Mean Glandular Dose (MGD)?
Unlike general radiography, which often measures the **Entrance Skin Dose**, mammography uses **Mean Glandular Dose (MGD)** as the gold standard for safety. 
*   **Radiosensitivity:** The glandular tissue (fibroglandular tissue) is significantly more radiosensitive than the surrounding adipose (fatty) tissue. 
*   **Definition:** MGD represents the average absorbed dose to the glandular tissues within the breast volume during a single exposure.
*   **Clinical Significance:** It is the reference parameter for establishing **Diagnostic Reference Levels (DRLs)** and for balancing image quality against radiation risk.

---

##### 2. Factors Influencing Dose
The amount of radiation a patient receives is not just a function of the machine's settings; it is heavily dependent on the interaction between the beam and the specific anatomy.

*   **Breast Composition and Thickness:** 
    *   **Path Length ($x$):** The total path length the beam traverses through the tissue directly affects attenuation. A thicker breast requires more photons (higher mAs) to achieve an adequate signal at the detector.
    *   **Density:** Denser breasts (higher $\mu$) absorb more radiation through the **photoelectric effect**, increasing the MGD.
*   **Beam Quality (kVp and Filtration):**
    *   **kVp:** Higher tube voltage increases the average energy of the photons, making the beam more "penetrating" and potentially reducing the dose for thicker breasts, though it may decrease contrast.
    *   **Filtration:** Physical filters (e.g., Rhodium or Silver) are used to "harden" the beam by absorbing low-energy photons that would otherwise be absorbed by the skin without contributing to the image.
*   **Geometry:** The **Source-to-Image Distance (SID)** and magnification factors also play a role in dose distribution due to the inverse square law of radiation.

---

##### 3. The ALARA Principle and Optimization
The guiding philosophy in mammography dosimetry is **ALARA (As Low As Reasonably Achievable)**.

*   **Optimization Strategies:**
    *   **Collimation:** Using the narrowest field of view to avoid exposing unnecessary tissue.
    *   **Grid Management:** While anti-scatter grids improve contrast by removing Compton scattering, they require an increase in dose to maintain the signal-to-noise ratio.
    *   **Digital Tomosynthesis (DBT) Dose:** While DBT provides volumetric data, the total dose is typically higher than a single 2D view, necessitating careful management of the number of projections and angular range.

---

##### 🖥️ Implementation: Dose Estimation Modeling
The computational focus of this module is on estimating the MGD based on exposure parameters ($kVp$, $mAs$) and patient-specific factors (thickness, breast density). 
*   **Dose Conversion Factors ($g$, $c$, $s$):** In practice, MGD is calculated by multiplying the measured air kerma (radiation intensity in air) by various conversion factors that account for breast thickness ($g$), glandular fraction ($c$), and X-ray spectrum ($s$).
*   **Simulation Goal:** Students can simulate how changing the breast thickness from 4cm to 8cm requires an exponential increase in dose to keep the detector signal constant, highlighting the importance of adequate compression.


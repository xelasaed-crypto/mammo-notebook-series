### 🧬 Radiological Anatomy and Tissue Physics

#### 📝 Theory: The Physical Basis of Breast Contrast
In mammography, the image is essentially a map of how different tissues attenuate the X-ray beam. Contrast arises because breast tissues—primarily fat, glandular tissue, and pathologies—have different atomic compositions and densities.

##### 1. The Two-Component Model: Fat vs. Gland
For simplicity, the breast is often modeled as a mixture of two primary tissues:
*   **Adipose Tissue (Fat):** Composed mostly of lipids. It has a lower effective atomic number ($Z$) and lower density. Consequently, it has a **lower $\mu$**, allowing more X-rays to pass through to the detector.
*   **Fibroglandular Tissue (The "Gland"):** Composed of water-rich proteins and connective tissue. It has a higher density and a **higher $\mu$** compared to fat, absorbing more X-rays and appearing brighter on a processed image.

##### 2. The Clinical Challenge: The Dense Breast
Breast density refers to the relative amount of glandular tissue versus fatty tissue.
*   **Low Contrast Environment:** Pathologies (like carcinomas) often have a linear attenuation coefficient ($\mu$) very similar to that of healthy glandular tissue. 
*   **Masking Effect:** In a "dense" breast (high glandular content), a tumor may be "hidden" because the differential attenuation between the tumor and the surrounding gland is nearly zero. This is why DBT and CEDM are used to add depth or chemical contrast to separate these structures.

##### 3. Microcalcifications: High-Z Contrast
Microcalcifications are tiny deposits of calcium hydroxyapatite. 
*   **The Photoelectric Advantage:** Calcium has a much higher atomic number ($Z \approx 20$) than soft tissue ($Z \approx 7$). 
*   **Appearance:** According to the $\sigma_{PE} \propto Z^3/E^3$ relationship, calcium absorbs significantly more radiation via the photoelectric effect, making even microscopic grains appear as high-contrast, bright spots.

---

##### 4. Tissue Physics Summary Table
| Tissue Type | Relative Density | Dominant Interaction | Image Appearance |
| :--- | :--- | :--- | :--- |
| **Adipose (Fat)** | Low | Low Attenuation | Dark (Radiolucent) |
| **Glandular** | Medium | Medium Attenuation | Bright (Radiopaque) |
| **Carcinoma** | Medium-High | Medium-High Attenuation | Bright (Often similar to Gland) |
| **Calcification** | Very High | **Photoelectric Absorption** | Brilliant White |

---

##### 🖥️ Implementation: Simulating Tissue Contrast Ratios
The computational goal of this module is to calculate the **Contrast-to-Noise Ratio (CNR)** between different tissue types.
*   **Simulation Task:** Use the Beer-Lambert Law ($I = I_0 e^{-\mu x}$) to calculate the remaining intensity behind 1cm of fat vs. 1cm of glandular tissue.
*   **Energy Optimization:** Demonstrate how lowering the $kVp$ (decreasing photon energy $E$) increases the difference in $\mu$ between these tissues, thereby increasing contrast, but at the cost of increasing the patient's radiation dose.

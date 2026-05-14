# 🔬  Digital Breast Tomosynthesis (DBT)

## 📐 Theory: Volumetric Reconstruction and Geometric Imaging

DBT is fundamentally a tomographic technique. Unlike standard 2D mammography, which collapses a 3D volume into a single 2D plane, DBT acquires multiple projections of the breast at varying angles and then uses complex mathematical reconstruction algorithms to generate thin, pseudo-3D slices.

### 1. Key Physical Concepts
*   **Principle of Tomosynthesis:** Acquiring multiple X-ray projections at different angles ($\theta_1, \theta_2, ..., \theta_n$) of the same object. The mathematical reconstruction process reconstructs the depth (Z-axis) information lost in the 2D projection.
*   **Superimposition Mitigation:** By acquiring multiple views, features that are stacked or superimposed in a single 2D image (e.g., overlapping ducts) can be separated in depth space, leading to clearer diagnostic views.
*   **Data Acquisition:** Requires precise geometric knowledge of the source-to-detector distance and object-to-detector distance.

### 💻 Computational Goal: Volume Reconstruction
The objective is to take a collection of 2D Sinogram data (the projections) and reconstruct a volume representation.

### 👨‍💻 Implementation Focus: Simulation of Tomographic Reconstruction
We simulate the *effect* of the reconstruction process on simple data points to demonstrate how depth separation occurs.

```python
import numpy as np
import matplotlib.pyplot as plt

# --- Simulation Parameters ---
N_angles = 10  # Number of angles (projections)
Depth_Depth = 10 # Depth axis
Width = 20       # Width of the breast volume

# 1. Simulate a 3D object (simplified cross-section with depth information)
# Assume an area of interest: a cluster of ducts at different simulated depths
X, Z = np.meshgrid(np.linspace(-Width/2, Width/2, Width), np.linspace(0, Depth_Depth, Depth_Depth))
Depth_Map = np.zeros((Depth_Depth, Width))

# Simulate a cluster of objects at different depths (e.g., ducts)
Depth_Map[3, 5:10] = 1.0  # Shallow feature
Depth_Map[6, 12:17] = 1.0 # Deep feature
Depth_Map[1, 3:8] = 0.5  # Very superficial feature

# 2. Simulate the 2D Projection (Averages the depth)
# For simplicity, we average the depth map across all depths (Simulated Sinogram)
Projected_View = np.mean(Depth_Map, axis=0)

print("--- Module 1: Projection Simulation ---")
print(f"Simulated Depth Map Shape (Z, X): {Depth_Map.shape}")
print(f"Simulated 2D Projection Shape (X): {Projected_View.shape}")

# 3. Simulate Reconstruction (The "Unstacking" Effect)
# In reality, this is done via Fourier Transforms (e.g., Filtered Backprojection).
# Here, we visualize the depth separation:
depth_slices = [Depth_Map[i*0.1: (i+1)*0.1:1, :, :] for i in range(N_angles)]

# Visualization
fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# Plot 1: The Single 2D Projection (Loss of depth information)
axes[0].imshow(Projected_View, aspect='auto', cmap='viridis')
axes[0].set_title('Simulated 2D Projection (Superimposed)')
axes[0].set_xlabel('Width (X)')
axes[0].set_ylabel('Depth (Z)')

# Plot 2: Reconstructed 3D Volume (Separated depth information)
axes[1].imshow(Depth_Map, aspect='auto', cmap='viridis')
axes[1].set_title('Reconstructed Volume (Depth Separated)')
axes[1].set_xlabel('Width (X)')
axes[1].set_ylabel('Depth (Z)')

plt.tight_layout()
plt.show()
```

**Explanation:**
1.  **The Limitation (Left Plot):** The single 2D projection averages the signal intensity across the entire depth axis. If two structures are close in X and Z, their signals blend together, making accurate measurement difficult.
2.  **The Benefit (Right Plot):** The reconstruction process (simulated by showing the raw Depth\_Map) separates the signal based on the *physical* depth dimension (Z). This allows clinicians to differentiate structures that were previously superimposed in the 2D view.

***

## Module 2: Enhanced Imaging and Image Filtering

This module simulates the application of image enhancement techniques, such as filtering, to improve the clarity and contrast of the raw data.

### Concept: Filtering
In medical imaging, filtering is used to:
1.  **Edge Enhancement (High-Pass Filter):** Sharpening borders (e.g., ductal borders).
2.  **Noise Reduction (Low-Pass Filter):** Smoothing out random electronic noise.
3.  **Directional Filtering:** Improving contrast along specific paths.

### 💻 Implementation: Gaussian and Laplacian Filters

```python
from scipy.ndimage import gaussian_filter, laplace

# We use the Depth_Map from the previous module as the input data
Input_Image = Depth_Map.copy()

# 1. Noise Simulation (Adding random noise)
Noise = np.random.normal(0, 0.05, Input_Image.shape)
Noisy_Image = Input_Image + Noise

# 2. Low-Pass Filtering (Noise Reduction)
# Blurs the image to reduce random high-frequency noise
LPS_Image = gaussian_filter(Noisy_Image, sigma=1.0)

# 3. High-Pass Filtering (Edge Sharpening)
# Laplacian filter highlights rapid changes in intensity (edges)
HPS_Image = laplace(Noisy_Image)

# Visualization
fig, axes = plt.subplots(1, 3, figsize=(18, 6))

# Plot 1: Noisy Input
axes[0].imshow(Noisy_Image, aspect='auto', cmap='viridis')
axes[0].set_title('1. Noisy Input (Random Noise)')

# Plot 2: Low-Pass Filtered (Smoothed)
axes[1].imshow(LPS_Image, aspect='auto', cmap='viridis')
axes[1].set_title('2. Low-Pass Filtered (Noise Reduction)')

# Plot 3: High-Pass Filtered (Edges Emphasized)
# We use absolute value here because the Laplacian output can be negative
axes[2].imshow(np.abs(HPS_Image), aspect='auto', cmap='hot')
axes[2].set_title('3. High-Pass Filtered (Edge Sharpening)')

plt.tight_layout()
plt.show()
```

**Conclusion:**
By combining the principles of **Depth Reconstruction (Tomosynthesis)** with **Advanced Signal Processing (Filtering)**, imaging systems can move beyond simple 2D views to provide cross-sectional, high-contrast, and depth-aware images crucial for accurate diagnosis in mammography.
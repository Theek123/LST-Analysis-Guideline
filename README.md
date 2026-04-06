# LST-Analysis-Guideline
Guideline for calculating Land Surface Temperature (LST) around a water body using QGIS and Landsat 8 imagery.

---
## Overview
This repository provides a **step-by-step guideline** for estimating Land Surface Temperature (LST) around a lake using QGIS. The workflow includes:

- Digitizing the lake boundary  
- Calculating the lake centroid  
- Generating cardinal (N, S, E, W) and intercardinal (NE, NW, SE, SW) lines  
- Creating points and buffers along these lines  
- Calculating TOA radiance, NDVI, Land Surface Emissivity (LSE), and LST  
- Extracting average LST per buffer  
- Visualizing trends using scatter plots

> This repo focuses on **methodology and workflow**. Actual results and rasters can be added in a separate repository.

---

## Tools & Software
- **QGIS** (Geometry Tools, Zonal Statistics, DataPlotly plugin)  
- **Landsat 8 bands 4,5, and 10**    

---

## Workflow

### Step 1: Load Data
- Load the **lake boundary shapefile** and **Landsat satellite imagery** into QGIS.

### Step 2: Find the Centroid
- Select the boundary layer.  
- Use **Centroids Tool** in the **Geometry Tools** tab to calculate the centroid (geometric center) of the lake.

### Step 3: Create Direction Lines
- Add X and Y coordinates of the centroid to the attribute table.  
- Use **Geometry by Expression** tool to create lines extending from the centroid in cardinal directions (N, S, E, W).  
- Example expression for North line:
```sql
make_line(
    make_point("X_coor", "Y_coor"),
    make_point("X_coor", "Y_coor" + [distance])
)
```
### Step 4: Create Points Along Direction Lines
- Create lines for intercardinal directions (e.g., NW at 45°).  
- Generate points along each line using **Points Along Geometry** tool with fixed spacing (e.g., 50 m).

### Step 5: Create Buffers
- Create small buffers around each point (e.g., 3x3 pixel buffers).  
- Buffer distance = pixel size × 1.5.

### Step 6: Calculate LST
1. **TOA Radiance (L𝝀)**:  
   L𝝀 = ML * QCAL + AL  
   - ML = Radiance multiplicative Band (B10)  
   - AL = Radiance additive Band (B10)  
   - QCAL = DN (digital number)

2. **Brightness Temperature (BT in °C)**:  

   BT = (K2 / ln(K1 / L𝝀 + 1)) – 273.15

4. **NDVI**:  
   NDVI = (Band 5 – Band 4) / (Band 5 + Band 4)

5. **Land Surface Emissivity (LSE)**:  
   Pv = ((NDVI - NDVIₘᵢₙ) / (NDVIₘₐₓ - NDVIₘᵢₙ))²

   E = 0.004 * Pv + 0.986
   
   Pv = Proportion of vegetation
   E = Land Surface Emissivity

7. **LST (Kelvin)**:  
   LST = BT / (1 + (λ * BT / C2) * ln(E))  
   - λ = 10.8 µm for Band 10  
   - C2 = 1.438 × 10⁻² m·K

### Step 7: Extract Average LST for Buffers
- Use **Zonal Statistics** tool in QGIS.  
- Input: buffer layer and LST raster.  
- Select **Mean** statistic to compute average LST for each buffer.

### Step 8: Visualize LST
- Install the **DataPlotly** plugin.  
- Create a scatter plot using distance from centroid as X-axis and average LST as Y-axis.

---

## Notes
- The lake boundary is **manually digitized** from satellite imagery (e.g., Landsat or Sentinel-2).  
- This guideline is **independent of actual data**; can apply it to any water body.  
- LST calculation formulas are based on standard Landsat 8 Thermal Infrared Bands.

---

## References
- USGS Landsat 8 Handbook  
- QGIS Documentation (Geometry Tools, Zonal Statistics, DataPlotly)
--- 
Author : Theekshana Pathirana
BSc in Geographical Information Science

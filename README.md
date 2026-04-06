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
)```

### Step 4 

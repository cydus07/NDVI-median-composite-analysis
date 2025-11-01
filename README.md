# NDVI Analysis for Jhansi Region Using Sentinel-2 Data (2024)

## Authors and Contact
- Author: Soumyajeet Sahu (jeetsahu1999@gmail.com)
- Date: October 30, 2025

## Study Overview
This repository contains the code and documentation for calculating Normalized Difference Vegetation Index (NDVI) for the Jhansi region in India using Sentinel-2 satellite imagery from 2024. The analysis produces:
1. A median composite NDVI layer for the entire year
2. Individual quarterly NDVI scenes for temporal analysis


## Files and Descriptions

### Code Files
- jhansi_ndvi_analysis.js: Main Google Earth Engine JavaScript code for:
  - Loading Sentinel-2 Surface Reflectance imagery
  - Filtering by cloud cover (<2%)
  - Cloud masking using QA60 band
  - NDVI calculation (NIR-Red)/(NIR+Red)
  - Creating median composite from all available 2024 images
  - Selecting best quarterly scenes
  - Exporting results to Google Drive

### Data Files (Generated Outputs)
- Jhansi_NDVI_Median_2024.tif: Median composite NDVI for entire AOI covering 2024
  - Format: GeoTIFF
  - Projection: EPSG:4326 (WGS84)
  - Resolution: 10m
  - Band: Single band (NDVI values ranging from -1 to 1)
  - Missing data: Areas with no valid observations

- Jhansi_NDVI_Scene_1_YYYY-MM-DD.tif: Q1 (January-March) best scene
- Jhansi_NDVI_Scene_2_YYYY-MM-DD.tif: Q2 (April-June) best scene
- Jhansi_NDVI_Scene_3_YYYY-MM-DD.tif: Q3 (July-September) best scene
- Jhansi_NDVI_Scene_4_YYYY-MM-DD.tif: Q4 (October-December) best scene
  - Format: GeoTIFF for each
  - Same specifications as median composite
  - Each represents single date acquisition

### Input Data
  - Satellite Imagery: Sentinel-2 Surface Reflectance (Harmonized)
  - Collection: COPERNICUS/S2_SR_HARMONIZED
  - Time period: January 1 - December 31, 2024
  - Cloud cover filter: <2%
  - Bands used:
    - B4 (Red): 665nm, 10m resolution
    - B8 (NIR): 842nm, 10m resolution
    - QA60: Cloud mask band

## Software Requirements

### Google Earth Engine
- Platform: Google Earth Engine Code Editor
- Account: Requires registered GEE account
- API Version: Earth Engine JavaScript API

### No Additional Packages Required
This script runs entirely within the Google Earth Engine environment and does not require local software installation.

## Workflow Instructions

### Step 1: Setup
1. Create a Google Earth Engine account at https://earthengine.google.com/
2. Access the Code Editor at https://code.earthengine.google.com/
3. Upload or ensure access to the Jhansi AOI asset

### Step 2: Running the Script
1. Open `jhansi_ndvi_analysis.js` in GEE Code Editor
2. Update the AOI path if using a different asset:
   ```javascript
   var aoi = ee.FeatureCollection('YOUR_ASSET_PATH');
   ```
3. Click "Run" button in the Code Editor
4. View the median composite NDVI on the map
5. Toggle individual quarterly scenes in the Layers panel

### Step 3: Exporting Data
1. After running the script, navigate to the "Tasks" tab (right panel)
2. You will see 5 export tasks:
   - 1x Median Composite
   - 4x Quarterly Scenes (if data available)
3. Click "RUN" on each task
4. Configure Google Drive export settings (default: GEE_Exports folder)
5. Click "Run" to start export
6. Files will appear in your Google Drive upon completion (may take several minutes)

### Step 4: Accessing Results
1. Navigate to your Google Drive > GEE_Exports folder
2. Download the GeoTIFF files
3. Open in GIS software (QGIS, ArcGIS, etc.) for further analysis

## Configuration Parameters

The following parameters can be modified in the script:

```javascript
var startDate = '2024-01-01';      // Analysis start date
var endDate = '2024-12-31';        // Analysis end date
var cloudThreshold = 2;            // Maximum cloud cover (%)
var exportScale = 10;              // Export resolution (meters)
var exportFolder = 'GEE_Exports';  // Google Drive folder name
var exportCRS = 'EPSG:4326';       // Output projection
```

### Choosing the Right Cloud Threshold

The cloud threshold significantly affects image availability and quality:

| Threshold | Quality | Coverage | Typical # Images | Best For |
|-----------|---------|----------|------------------|----------|
| <5% | Excellent | Limited | 3-10 | Small AOIs, dry seasons |
| <15% | Very Good | Good | 10-30 | **Recommended default** |
| <20% | Good | Excellent | 20-50 | Large AOIs, monsoon areas |
| <30% | Fair | Complete | 50+ | Ensuring full coverage |

Recommendation: Start with 15% and adjust based on your results. Check the console output for "Total images" count.

## NDVI Calculation

NDVI (Normalized Difference Vegetation Index) is calculated as:

```
NDVI = (NIR - Red) / (NIR + Red)
```

For Sentinel-2:
- NIR = Band 8 (842nm)
- Red = Band 4 (665nm)

NDVI Values Interpretation:
- -1 to 0: Water, snow, clouds, bare soil
- 0 to 0.2: Sparse vegetation, bare rock
- 0.2 to 0.5: Moderate vegetation (grasslands, shrubs)
- 0.5 to 1: Dense vegetation (forests, crops)

## Processing Steps

1. Data Loading: Filter Sentinel-2 imagery by AOI, date range, and cloud cover (<2%)
2. Cloud Masking: Apply QA60 band mask to remove remaining clouds and cirrus
3. NDVI Calculation: Compute NDVI for all images
4. Median Composite: Calculate pixel-wise median across all valid observations
5. Quarterly Selection: Select best (lowest cloud) scene per quarter
6. Export: Save results as GeoTIFF files

## Quality Control

- Cloud filtering: Only images with <2% cloud cover included
- Cloud masking: Per-pixel cloud masks applied using QA60 band for additional cloud removal
- Multiple observations: Median composite reduces noise and fills gaps
- Visual inspection: Review outputs in GEE map viewer before export
- Data availability check: Verify sufficient images in console output

## Known Limitations

1. Data Gaps: Some areas may have no valid observations due to persistent cloud cover
2. Temporal Coverage: Quarterly scenes represent single dates, not seasonal averages
3. Image Availability: Number of available images depends on cloud threshold setting
   - Too strict (e.g., <5%): May result in insufficient images (3-10) and incomplete coverage
   - Recommended: 15% provides good balance between quality and quantity
   - If coverage is incomplete, increase threshold to 20-30%
4. Atmospheric Correction: Uses Sentinel-2 Level-2A (atmospherically corrected) but residual effects may exist
5. Edge Effects: AOI boundaries may have reduced data coverage due to satellite swath patterns
6. Monsoon Season: Cloud cover is typically higher June-September, reducing available imagery


## Contact for Questions
For questions about this analysis, please contact at [jeetsahu1999@gmail.com]

---
Last updated: October 30, 2025

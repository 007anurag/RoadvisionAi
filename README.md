# Automatic Road Extraction from Sentinel-2 (New Delhi, India)

##  Project Overview

This project implements an **end-to-end, reproducible pipeline** for **automatic road extraction** from **Sentinel-2 Level-2A satellite imagery** using image-based segmentation and geospatial post-processing techniques. The study area is **New Delhi, India**.

The workflow covers satellite data acquisition, AOI cropping, preprocessing, road segmentation, post-processing, vectorization, and road naming using **OpenStreetMap (OSM)** data.

---

##  Dataset

* **Satellite:** Sentinel-2 Level-2A (Surface Reflectance)
* **Spatial Resolution:** 10 m
* **Bands Used:** B02 (Blue), B03 (Green), B04 (Red), B08 (NIR)
* **Temporal Coverage:** 2023
* **Cloud Filtering:** < 10% (Scene Classification Layer masking)
* **AOI:** New Delhi, India

---

## Methodology

**Pipeline:**

```
Sentinel-2 Acquisition (GEE)
        ↓
AOI Cropping
        ↓
Preprocessing (Normalization)
        ↓
Road Segmentation (NDVI + Edge Detection)
        ↓
Post-processing (Cleaning + Skeletonization)
        ↓
Vectorization (Raster → Lines)
        ↓
Road Naming (OSM Nearest-Neighbour Matching)
```

### Key Techniques

* NDVI-based vegetation suppression
* Canny edge detection
* Morphological operations
* Skeletonization for centerlines
* Geometry-based nearest-neighbour matching with OSM

---

##  Repository Structure

```
road_ai/
├── img/
│   ├── S2_Delhi_2023.tif
│   ├── S2_AOI.tif
│   ├── S2_Preprocessed.tif
│   ├── road_mask.tif
│   ├── road_clean.tif
│   └── road_skeleton.tif
│
├── osm/
│   └── osm_roads_delhi.geojson
│
├── outputs/
│   ├── roads_raw.geojson
│   └── final_named_roads.geojson
│
├── scripts/
│   ├── phase1_gee.js
│   ├── phase2_crop.py
│   ├── phase3_preprocess.py
│   ├── phase4_segmentation.py
│   ├── phase5_postprocess.py
│   ├── phase6_vectorize.py
│   └── phase7_naming.py
│
├── report.md
└── README.md
```

---

##  How to Run

### 1. Environment Setup

```bash
conda create -n road_ai python=3.10
conda activate road_ai
pip install rasterio geopandas numpy opencv-python scikit-image shapely
```

### 2. Data Acquisition

* Run `phase1_gee.js` in **Google Earth Engine**
* Download exported GeoTIFF

### 3. Execute Pipeline (in order)

```bash
python phase2_crop.py
python phase3_preprocess.py
python phase4_segmentation.py
python phase5_postprocess.py
python phase6_vectorize.py
python phase7_naming.py
```

---

##  Outputs

* **Road Mask:** `road_mask.tif`
* **Centerlines:** `road_skeleton.tif`
* **Vector Roads:** `roads_raw.geojson`
* **Final Named Roads:** `final_named_roads.geojson`

Each road feature contains:

* `road_id`
* `road_name`
* `confidence_score`
* geometry

---

##  Notes on CRS

Due to environment-level PROJ limitations, the final GeoJSON is exported **without embedded CRS metadata**. Geometries are in WGS84 and can be assigned **EPSG:4326** in GIS software (e.g., QGIS).

---

##  Future Work

* Use higher-resolution imagery
* Deep learning-based segmentation
* Quantitative accuracy assessment
* Interactive web mapping

---

##  Author

**Anurag Paliwal**
M.Sc. Geoinformatics

---

##  License

This project is for research purposes.

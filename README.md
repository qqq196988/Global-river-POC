# Global river POC (1984–2018) — analysis-ready data & reproducible figures

This repository supports the analysis and figure reproduction for our study on four-decade changes in global river particulate organic carbon (POC) export. We provide **analysis-ready outputs** (river geometries + annual POC flux time series) and an example notebook to reproduce key results (including Fig. 2) **from the analysis stage onward**.

> Repository: `Global-river-POC`  
> GitHub: https://github.com/qqq196988/Global-river-POC

---

## What is included

### 1) Spatial data (for mapping and plotting)
- `land_shp/`  
  Global land polygon shapefile used for basemap plotting.

- `river_shpfile/`  
  Shapefiles for the **2,409 river mouths** analyzed in this study:
  - `river_point.shp`: river mouth points (main plotting/selection layer)
  - `river_line.shp`: river polylines (used for visualizing river geometry)

#### Key attributes in `river_point.shp` / `river_line.shp`
- `ID`: unique river identifier used across all files
- `width_mean`: mean river width (from GRWL)
- `COMID`: HydroRIVERS reach identifier used to match GRDR discharge
- `slope`: POC flux change rate (as reported in the manuscript)
- `uparea`: upstream area (from GRWL)
- `trend`: final trend category based on Monte Carlo uncertainty propagation (POC concentration + discharge); see Methods
- `trend2`: deterministic trend (no uncertainty propagation); see Methods
- `ave_flux`: four-decade mean POC flux (1984–2018)
- `continent`: continent label
- `ord_stra`: Strahler stream order
- `image_num`: number of valid satellite observations at each mouth
- `OWT`: optical water type classification

---

### 2) Annual POC flux time series (1984–2018)
- `Global river POC data.xlsx`  
  Annual land–ocean POC flux for each of the **2,409 rivers** (rows = rivers; columns = years 1984–2018; matched by `ID`).

#### Notes on missing values
Some rivers have missing annual values because systematic Landsat observations are not consistently available over time in specific regions (e.g., parts of the Russian Arctic, where Landsat 5/7 Level-2 Tier-1 surface reflectance becomes consistently available mainly after ~2000).  
For these rivers, when quantifying changes/trends, we do not perform generic gap filling; instead we use **3-, 5-, or 10-year running means** of annual POC flux depending on the gap length (see Methods).

---

### 3) Code to reproduce key figures/statistics
- `Code.ipynb`  
  A Jupyter notebook that reproduces the core trend analysis plots (including **Fig. 2**) **end-to-end from the provided analysis-ready data**.  
  The notebook also contains supporting utilities for data processing and (optional) attribution analysis.

---

## Quick start

### Recommended environment
- Python 3.8+  
- Core packages: `numpy`, `pandas`, `geopandas`, `matplotlib`, `scipy`, `openpyxl`

### Reproduce Fig. 2
1. Download/clone the repository
2. Open `Code.ipynb`
3. Run all cells in order  
The notebook reads:
- `Global river POC data.xlsx` (annual flux time series)
- `river_shpfile/river_point.shp` and/or `river_shpfile/river_line.shp` (spatial plotting)

---

## About “end-to-end” reproducibility
We aim to make the study reproducible **from the analysis stage onward**, which covers:
- trend tests (including Monte Carlo-based uncertainty propagation),
- derived trend classifications,
- and the main/extended figure generation scripts.

The upstream satellite image-processing workflow relies on Google Earth Engine (GEE) and cloud-based data access/processing. In practice, re-running the full global workflow end-to-end requires:
- authenticated access to cloud-hosted archives,
- large-scale distributed computation,
- and workflow components that are not fully representable as standalone local scripts (e.g., server-side processing graphs, quotas, and intermediate products that are prohibitively large to bundle and version locally).

Accordingly, we provide **analysis-ready outputs** and the scripts to reproduce all key statistics and figures from these outputs.

---

## Citation
If you use these data/code, please cite our manuscript (and associated datasets referenced therein, e.g., GRWL/GRDR/HydroBASINS/Global Dam Watch/Hansen et al. as applicable).

---

## Contact
For questions or issues, please open a GitHub issue in this repository.

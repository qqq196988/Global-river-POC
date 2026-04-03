# Global river POC (1984–2018) — analysis-ready data, cloud-processing workflow, and reproducible figures

This repository supports the analysis and figure reproduction for our study on four-decade changes in global river particulate organic carbon (POC) export. We provide **analysis-ready outputs** (river geometries + annual POC flux time series), an example notebook to reproduce key results (including Fig. 2), and a **GEE/geemap-based notebook** that documents the Landsat cloud-processing workflow used to extract river reflectance metrics.

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

### 3) Code for downstream analysis and figure reproduction
- `Code.ipynb`  
  A Jupyter notebook that reproduces the core trend analysis plots (including **Fig. 2**) **end-to-end from the provided analysis-ready data**.  
  The notebook also contains supporting utilities for data processing and (optional) attribution analysis.

---

### 4) GEE/geemap notebook for Landsat cloud-based processing
- `Landsat_data_processing.ipynb`  
  A Jupyter notebook implementing the **Google Earth Engine (GEE) Python API via geemap** workflow used for Landsat-based river reflectance extraction.

#### What this notebook documents
- Landsat 5/7 Collection 2 Level-2 image access and merging
- QA-based masking of clouds, cloud shadows, snow, and radiometric saturation
- river-geometry buffering and per-river image filtering
- cloud/ice-snow coverage screening
- AWEIsh-based water masking (for simplicity, only the AWEIsh-based mask is shown in the shared notebook)
- batch extraction of median reflectance statistics for selected bands
- export of per-river reflectance summaries to CSV

#### Important notes
- The cloud-based workflow is implemented through the **Earth Engine Python API / geemap**, rather than standalone JavaScript scripts in the GEE Code Editor.
- Some external datasets are **not bundled in this repository** and must be downloaded separately by the user. For example:
  - **GRWL river network shapefile**: available from Zenodo  
    https://zenodo.org/records/1297434
- In the notebook, dataset paths are intentionally written as **user-defined placeholders** (e.g., `path_to_GRWL_shapefile.shp`, `path_to_output_directory`) rather than hardcoded local machine paths.
- For simplicity and portability, the shared notebook is intended to document the main processing logic rather than serve as a one-click end-to-end global rerun.

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

### Inspect the Landsat/GEE processing workflow
1. Open `Landsat_data_processing.ipynb`
2. Prepare required external inputs (e.g., GRWL shapefile)
3. Replace placeholder paths with user-defined local paths
4. Authenticate Earth Engine in your Python environment if needed
5. Run notebook cells in order

---

## About reproducibility

### Reproducible from the analysis stage onward
We aim to make the study reproducible **from the analysis stage onward**, which covers:
- trend tests (including Monte Carlo-based uncertainty propagation),
- derived trend classifications,
- and the main/extended figure generation scripts.

### Transparency of the cloud-based workflow
To improve transparency of the upstream remote-sensing workflow, we also provide `Landsat_data_processing.ipynb`, which documents the **GEE/geemap-based** Landsat processing steps used for river reflectance extraction.

### Why full one-click global reruns remain difficult
Re-running the full global workflow end-to-end still requires:
- authenticated access to cloud-hosted archives and Earth Engine resources,
- large-scale distributed computation,
- user-supplied external datasets (e.g., GRWL),
- and workflow components that are difficult to package as a universal local pipeline (e.g., server-side processing graphs, quotas, and large intermediate products).

Accordingly, this repository provides:
1. **analysis-ready outputs** for direct reproduction of the main statistics and figures;  
2. **documented GEE/geemap notebook** to improve transparency of the upstream Landsat processing workflow.

---

## Citation
If you use these data/code, please cite our manuscript (and associated datasets referenced therein).
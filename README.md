# Healthcare Deserts in Texas: A Spatial Data Engineering Analysis

A reproducible spatial ETL and GIS pipeline that finds Texas census tracts with poor hospital access and scores them with a composite Healthcare Deficit Index.

*Texas A&M University, DAEN 489 Spatial Data Engineering, Final Project, Spring 2026. Two-person team project.*

## Data Sources (all public)
- **HIFLD Open Hospitals:** 8,000+ U.S. hospitals, filtered to **810** open Texas facilities
- **U.S. Census ACS 2023** total population (DP05) for all **6,896** Texas census tracts, pulled through the Census API
- **2024 TIGER/Line** census tract boundaries

## Spatial ETL Pipeline
- **Extract:** GeoPandas shapefile loads, programmatic TIGER download, and an ACS API request.
- **Transform:** Filtered to open Texas hospitals, joined ACS data to tract geometries on GEOID, derived tract centroids and population density (people per km²), and removed invalid geometries.
- **CRS:** Reprojected everything to **EPSG:3081** (Texas State Mapping System) for accurate distance measurement.
- **Load:** Exported a final GeoDataFrame to GeoPackage and CSV.

## Methods
- **Nearest-hospital distance** from each tract centroid using a SciPy **cKDTree**.
- **10, 20, and 30-mile service-area buffers** with point-in-polygon coverage tests.
- **Healthcare desert rule:** more than 30 miles from the nearest hospital and more than 500 residents.
- **Healthcare Deficit Index (HDI):** a composite of normalized distance, population sparsity, hospital supply, and buffer coverage, grouped into severity tiers.
- **Raster-vector workflow:** rasterized population density, with zonal statistics extracted back to tracts (Rasterio, rasterstats).

## Findings
- Hospitals cluster in Dallas-Fort Worth, Houston, San Antonio, Austin, and the Rio Grande Valley, which have short travel distances and low deficit scores.
- Healthcare deserts and high HDI tracts are concentrated in **West Texas, the Texas-Mexico border, and rural interior counties**. The 15 worst counties average more than 30 miles to the nearest hospital, and some approach 45 miles.
- Most tracts and residents fall in the low-deficit tier, but severe gaps are significant where they occur.

## Reproducibility & Ethics
The pipeline is an end-to-end Jupyter notebook that runs from a fresh environment. Census data is fetched programmatically, the only local input is the hospital shapefile, and random seeds are fixed. Results use tract centroids, so they approximate access rather than measure individual travel. No personally identifiable data is used.

## Tech Stack
Python, GeoPandas, pandas, NumPy, SciPy (cKDTree), Rasterio, rasterstats, Matplotlib, Census API

> The notebook will be added.

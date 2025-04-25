# NbS_tool
This repository is for 2025 NYU CUSP Capstone project: A Decision Support Tool for Enhancing Resilience to Urban Overheating with Nature-Based Solutions

# Heat Risk Index
## 1. Land Surface Temperature Downscaling
**Automated pipeline for downloading and processing Sentinel-3 Land Surface Temperature data**
🔗 [GitHub Repository](https://github.com/Seab0t/Sentinel3_Download_Process_Python)

**Pixel Block Intensity Modulation (PBIM) Downscaling**
For model details, check this paper: https://doi.org/10.1016/j.rse.2009.07.017 [^1] 

## 2. Heat Risk Index
**Definition**  
Heat Risk Index: Conceptualized using Crichton's Risk Triangle framework, which defines risk as the interaction of hazard, exposure, and vulnerability components. The index is calculated as the sum of these three components:
     ```
     HRI = Hazard + Exposure + Vulnerability
     ```
  
---

### Study Period & Criteria
- **Date range**: Heat-wave days from **2017–2023**  
  Chosen because NYC land-use change was < 10 % during this period ([USFS LCMS](https://developers.google.com/earth-engine/datasets/catalog/USFS_GTAC_LCMS_v2023-9#description)).  
- **Heat wave**: ≥ 3 consecutive days with daily highs ≥ 90 °F ([NOAA OKX](https://www.weather.gov/okx/excessiveheat)).  
  
---

### Component Datasets

1. **Hazard**  
   - **LST**: Mean Land Surface Temperature during heat waves (radiative surface heat).

2. **Exposure**  
   - **PopDen**: Population density (people /km²)  
   - **RoadD**: Pedestrian demand × road length per grid cell  

3. **Vulnerability**  
   - **Vegetation & Built Environment**  
     - **NDVI**: Normalized Difference Vegetation Index  
     - **NDBI**: Normalized Difference Built-up Index  
     - **NDWI**: Normalized Difference Water Index  
     - **ParkArea**: Park area (km²)  
   - **Socioeconomic**  
     - **MedianInc**: Median household income  
     - **Race_Black**, **Race_Latin**, **Race_Other**: % of each group  
     - **Home_AC**: % of households with at least one functioning AC  
   - **Health & Social Support**  
     - **Hos_D**: Hospital density (/km²)  
     - **Hypertensi**: % of adults diagnosed with hypertension  
     - **Helpful_Ne**: % of adults reporting neighbor support  
   - **Age Vulnerability**  
     - **Pop5P**: % of population under 5 years  
     - **Pop65P**: % of population over 65 years  

> _All remote-sensing indices (NDVI, NDBI, NDWI) are derived from Landsat 8 averages over heat wave days (2017–2023)._

---

### Methodology

1. **Normalization**  
   - Scaled to the range [0.1, 0.9] using MinMaxScaler[^2].  
   - **Negative** variables (higher → lower risk):  
     NDVI, NDWI, MedianInc, ParkArea, Hos_D, Home_AC, Helpful_Ne  
   - **Positive** variables (higher → higher risk):  
     LST, PopDen, RoadD, NDBI, Pop5P, Pop65P, Race_Black, Hypertensi  

2. **Dimensionality Reduction**  
   - PCA on the 14 vulnerability metrics, retaining ≥ 85 % of total variance (Pareto principle).  

3. **Aggregation**
     ```
     HRI = Hazard + Exposure + Vulnerability
     ```
[^1]: Stathopoulou, M., and C. Cartalis. (2009). *Downscaling AVHRR Land Surface Temperatures for Improved Surface Urban Heat Island Intensity Estimation.* Remote Sensing of Environment, 113 (12): 2592–2605. https://doi.org/10.1016/j.rse.2009.07.017
[^2]: Chen, B., et al. (2022). *Diurnal Heat Exposure Risk Mapping and Related Governance Zoning: A Case Study of Beijing, China.* Sustainable Cities and Society, 81:103831. https://doi.org/10.1016/j.scs.2022.103831

# TARGET model
- For model details, check this paper: https://gmd.copernicus.org/articles/12/785/2019/gmd-12-785-2019.html

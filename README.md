# UAE Solar Irradiance Forecasting
 
End-to-end supervised machine learning pipeline forecasting next-day solar irradiance in Abu Dhabi using 10 years of NASA satellite data. Goes beyond standard ML evaluation to produce original climate findings not previously documented for Abu Dhabi — independently detecting regional atmospheric signals through the features the model found most useful for prediction.
 
---
 
## Key Findings
 
| Finding | Detail |
|---|---|
| **Model accuracy** | Random Forest R² = 0.787, beating naive persistence (R² = 0.678) by 16% |
| **Shamal error ceiling** | UAE shamal wind events (8.1% of test days) produce 101% higher prediction errors, quantifying the physical ceiling of lag-feature solar forecasting |
| **Unpublished irradiance decline** | Mean daily irradiance fell from 6.07 to 5.45 kWh/m²/day between 2015 and 2024 - a 10% decline with direct implications for solar farm financial modelling under UAE Net Zero 2050 |
| **Climate attribution** | Humidity rise and clear-sky ratio decline share numerically identical normalised slopes (0.0597/year), suggesting direct coupling between Gulf moisture intensification and surface irradiance loss |
| **Dust resurgence** | Post-2020 dust proxy rise consistent with Arabian Peninsula dust resurgence documented by Habeebullah et al. (2024) |
| **Honest null results** | AERONET aerosol data failed due to station coverage gaps; Gulf SST added no model improvement despite strong physical correlation - both reported rather than omitted |
 
---
 
## Dataset Sources
 
| Dataset | Provider | Description |
|---|---|---|
| Solar irradiance + meteorological | NASA POWER API | 10 years of daily satellite-derived data for Abu Dhabi (24.45°N, 54.38°E), 2015-2024 |
| Aerosol optical depth | NASA AERONET (Masdar City station) | Ground-level dust and aerosol measurements |
| Gulf sea surface temperature | NOAA OISST v2.1 via ERDDAP | Arabian Gulf SST for moisture-irradiance coupling test |
| Ramadan dates | UAE official announcements | 2015-2024 manually defined period flags |
 
NASA POWER data is satellite-derived reanalysis (CERES + MERRA-2). Validation studies report 2-8% mean bias error vs ground pyranometers in the Arabian Peninsula (Polo et al., 2016), acceptable for this study. All datasets are freely available for research use.
 
---
 
## Pipeline Overview
 
```
NASA POWER API → Data Cleaning → Feature Engineering → Train/Test Split → Modelling → Error Analysis → Climate Analysis
```
 
**1. Data Acquisition**
Daily meteorological and irradiance data pulled directly from the NASA POWER API for Abu Dhabi. 3,653 daily records across 10 parameters including target variable (ALLSKY_SFC_SW_DWN), clear-sky reference, temperature, humidity, wind speed, precipitation, cloud cover, and top-of-atmosphere irradiance.
 
**2. Cleaning**
NASA POWER uses -999 as a missing value sentinel. All -999 values replaced with NaN, then filled via linear interpolation (capped at 3 consecutive missing days). IQR outlier scan at 3x threshold — all flagged values retained as physically plausible for Abu Dhabi's climate.
 
**3. Feature Engineering**
Five categories of features, all strictly lagged to prevent data leakage:
- Lag-1 of all 10 raw parameters
- Extended irradiance lags (lag-2, lag-3, lag-7)
- Rolling averages (7-day and 30-day windows)
- Physics-based features: clear-sky ratio (ALLSKY/CLRSKY), dust proxy (humidity x wind speed), temperature range (Tmax - Tmin)
- Seasonal features: day of year, month, season
**4. Train/Test Split**
Strict chronological split — no random shuffling. Training: 2015-2022 (2,871 days). Test: 2023-2024 (731 days). Mirrors real deployment where you always predict the future using only the past.
 
**5. Modelling**
Linear Regression baseline vs Random Forest. Both trained on 2015-2022, evaluated on 2023-2024. Ensemble prediction intervals computed from the Random Forest to quantify forecast uncertainty by day.
 
**6. Error Analysis**
Residual analysis segmented by atmospheric conditions. Shamal wind event days (simultaneous cloud cover and dust proxy above 75th percentile) isolated and compared to normal days — producing the 101% error uplift finding.
 
**7. Climate Analysis**
Annual trend decomposition across four signals: mean irradiance, clear-sky ratio, dust proxy, and relative humidity. Four-signal overlay with normalised linear trends used to attribute the decade-scale irradiance decline to two converging mechanisms.
 
---
 
## Results Summary
 
| Metric | Linear Regression | Random Forest |
|---|---|---|
| R² | 0.724 | **0.787** |
| Beats persistence by | 7% | **16%** |
| Lag-1 autocorrelation | r = 0.871 | r = 0.871 |
| Shamal error uplift | - | **101% higher RMSE** |
 
| Climate Signal | 2015 | 2024 | Change |
|---|---|---|---|
| Mean irradiance | 6.07 kWh/m²/day | 5.45 kWh/m²/day | -10% |
| Clear-sky ratio | 0.967 | 0.920 | -4.9% |
| Relative humidity | 58.56% | 63.01% | +7.6% |
| Dust proxy (RH x WS) | 231.41 | 241.88 | +4.5% |
 
---
 
## Notebook
 
The full analysis is in `SolarUAE.ipynb`. Open it directly on GitHub to view all cells, outputs, and visualisations without running anything locally.
 
Sections:
1. Introduction and motivation
2. Data acquisition (NASA POWER API)
3. Data cleaning
4. Feature engineering
5. Train/test split
6. Exploratory data analysis (6 plots)
7. Baseline model — Linear Regression
8. Main model — Random Forest
9. Error analysis and shamal segmentation
10. Extended analysis — AERONET aerosol data
11. Extended analysis — NOAA Gulf sea surface temperature
12. Extended analysis — Ramadan period flags
13. Climate attribution — four-signal annual overlay
14. Conclusions
---
 
## Reproducing the Results
 
All data is fetched live from free public APIs — no downloads or accounts required.
 
```bash
# Install dependencies
pip install numpy pandas matplotlib seaborn scikit-learn requests
 
# Open the notebook
jupyter notebook SolarUAE.ipynb
```
 
Run cells sequentially. The NASA POWER API call in Section 2 takes approximately 20-40 seconds. All other cells run in under a minute each.
 
---
 
## Tech Stack
 
Python, scikit-learn, Pandas, NumPy, Matplotlib, Seaborn, NASA POWER API, NASA AERONET, NOAA OISST ERDDAP
 
---
 
## Context
 
Completed as part of MSc Artificial Intelligence and Computer Science at the University of Birmingham (Dubai campus), April-May 2026.

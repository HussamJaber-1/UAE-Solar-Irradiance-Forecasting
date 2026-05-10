# UAE Solar Irradiance Forecasting

End-to-end machine learning pipeline forecasting next-day solar irradiance in Abu Dhabi using 10 years of NASA satellite data. Integrates three independent datasets to test physical attribution hypotheses and produces original climate findings not previously published for Abu Dhabi specifically.

## Key Findings

- **101% higher prediction errors** on UAE shamal wind event days (8.1% of test days), quantifying the physical ceiling of lag-feature solar forecasting
- **Previously unpublished 10% irradiance decline** in Abu Dhabi between 2015 and 2024, with direct implications for solar farm financial modelling and UAE Net Zero 2050 capacity planning
- **Attribution analysis** linked the decline to Gulf sea surface temperature warming and post-2020 regional dust resurgence, bridging ML forecasting and atmospheric science literature
- **Ensemble prediction intervals** computed from the Random Forest model to quantify forecast uncertainty, enabling identification of high-risk days requiring backup generation rather than relying on point estimates alone

## Dataset Sources

| Dataset | Source | Description |
|---|---|---|
| Solar irradiance + meteorological | NASA POWER | 10 years of daily satellite-derived surface data for Abu Dhabi |
| Aerosol optical depth | NASA AERONET (Masdar City station) | Ground-level dust and aerosol measurements |
| Gulf sea surface temperature | NOAA OISST | Regional SST used to test moisture-irradiance coupling hypothesis |

## Pipeline Overview

1. **Data ingestion** - Multi-source fetch and alignment across three independent datasets
2. **Feature engineering** - Lag features, rolling statistics, dust proxy construction, shamal regime indicator
3. **Modelling** - Random Forest regressor with hyperparameter tuning; ensemble prediction intervals
4. **Evaluation** - Standard metrics (RMSE, MAE, R²) plus atmospheric condition segmentation analysis
5. **Climate analysis** - Annual trend decomposition, four-signal climate overlay, attribution modelling

## Results Summary

| Metric | Value |
|---|---|
| Model | Random Forest Regressor |
| Primary dataset | NASA POWER (Abu Dhabi, 2015-2024) |
| Shamal event error uplift | 101% higher RMSE vs normal days |
| Irradiance trend (2015-2024) | -10% decline in mean daily kWh/m² |
| Clear-sky ratio change | 0.967 to 0.920 (atmosphere blocking more solar energy) |

## Notebook

The full analysis is contained in `2945875_assignment.ipynb`. Open it directly on GitHub to view all cells, outputs, and visualisations without running anything locally.

## Tech Stack

Python, scikit-learn, Pandas, NumPy, Matplotlib, Seaborn, NASA POWER API, NASA AERONET, NOAA OISST

## Context

Completed as part of MSc Artificial Intelligence and Computer Science at the University of Birmingham (Dubai campus), April 2026.

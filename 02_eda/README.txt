# 02_eda - Exploratory Data Analysis

This folder contains the exploratory analysis of solar cell performance data for cells M0 and P12.

## Notebooks

EDA_MPP.ipynb
Analysis of maximum power point (MPP) and efficiency.
- Processes CAMS satellite irradiance and local measurements.
- Calculates daily solar energy received vs. energy produced (kWh/m2).
- Visualizes conversion efficiency percentages and daily performance curves.

EDA_JV.ipynb
Analysis of current-voltage (J-V) sweeps.
- Identifies and validates individual curves based on voltage span.
- Distinguishes between forward and reverse sweep directions.
- Extracts key photovoltaic parameters: Voc, Jsc, Vmpp, Jmpp, and Fill Factor (FF).
- Tracks the daily evolution of cell performance metrics.

## Key Metrics

- Irradiance (mW/cm2)
- Power Density (mW/cm2)
- Conversion Efficiency (%)
- Fill Factor (FF)
- Open-circuit Voltage (Voc) and Short-circuit Current (Jsc)

## Requirements

Python libraries: pandas, numpy, matplotlib, seaborn, and pyarrow.
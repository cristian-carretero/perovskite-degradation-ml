# Explainable Machine Learning for Outdoor Perovskite Solar Cell Degradation

Bachelor's Thesis — Double Degree in Physics and Materials Engineering  
**Author:** Cristian Carretero Fernández  
**Supervisors:** Dr. Miguel Anaya Martín (Dept. of Condensed Matter Physics) · Dr. Manuel Jesús Jiménez Navarro (Dept. of Computer Languages and Systems)  
**Institution:** Universidad de Sevilla, 2026

---

## Overview

Metal halide perovskite solar cells (PSCs) offer transformative photovoltaic efficiencies, yet their commercialization remains bottlenecked by long-term environmental instability. Traditional indoor static testing fails to capture the synergistic, non-linear effects of fluctuating outdoor stressors, often conflating **reversible thermodynamic losses** with **permanent structural fatigue**.

This thesis introduces a data-driven framework to evaluate the operational stability of two triple-mesoscopic architectures deployed outdoors for 50 days:

- **M0** — commercial hybrid formulation
- **P12** — standard MAPbI₃ formulation

Two complementary machine learning approaches are used:

- **Unsupervised K-Medoids clustering** on dynamically normalized J-V sweeps, to map pure morphological failure archetypes independently of absolute power.
- **XGBoost ensembles interpreted via SHAP**, to decouple reversible diurnal environmental quenching from path-dependent cumulative structural decay.

**Key finding:** initial baseline efficiency does not dictate operational longevity. Despite superior initial efficiency, M0 irreversibly succumbed to a terminal interfacial extraction barrier driven by cumulative photo-thermal stress. P12 exhibited high temperature-elastic resilience, sustaining a stable asymptotic power plateau through dynamic diurnal recovery.

---

## Machine Learning Workflow

![ML Workflow](https://github.com/user-attachments/assets/3630b20a-7041-46ae-b201-663423a29226)

Sequential pipeline of data curation and predictive modeling for PSC degradation characterization.

**Four stages:**

1. **Data Sources** — Operational MPPT stream, local sensors (G, T_c, T_amb, RH), CAMS satellite irradiance proxy, and raw J-V sweeps (reverse + forward).
2. **Preprocessing** — Performance filtering, causal time alignment, physical cleaning (night mask + artifacts), feature imputation, and feature engineering.
3. **Modeling** — Thermodynamic SQ envelope calibration, non-stationary detrending (State of Health), MLR baseline, physics-constrained XGBoost with monotonic constraints, and path-dependent cumulative stress model.
4. **Diagnostics** — SHAP interactions, degradation diagnostics with stress channel attribution, failure mode decoupling (extrinsic vs intrinsic), and morphological archetypes via PCA + K-Medoids.

---

## Repository Structure

**notebooks/**

- `01_preprocess_mpp.ipynb` — MPPT telemetry cleaning & alignment
- `02_preprocess_jv.ipynb` — J-V sweep cleaning & normalization
- `03_eda_mpp.ipynb` — Exploratory analysis of MPPT stream
- `04_eda_jv.ipynb` — Exploratory analysis of J-V curves
- `05_modeling_theory.ipynb` — Shockley-Queisser envelope & calibration
- `06_modeling_linear_regression.ipynb` — MLR baseline for reversible thermodynamics
- `07_modeling_xgboost.ipynb` — XGBoost with monotonic constraints
- `08_modeling_xgboost_standalone.ipynb` — Path-dependent XGBoost (cumulative stress)
- `09_clustering_kmedoids.ipynb` — PCA + K-Medoids morphological archetypes
- `10_sq.ipynb` — Final SQ limit comparison

**Root files:**

- `.gitignore`
- `README.md`

---

## Methodology Summary

### Morphological tracking (unsupervised)

Voltage and current density sweeps are locally normalized (Ṽ, J̃), interpolated onto a 50-node grid, and projected into a 2D latent space via PCA. Trajectories are partitioned into four physical archetypes using K-Medoids (PAM), validated by the Silhouette coefficient.

**Four archetypes identified:**

- **Type 0** — Standard hysteresis (optimal baseline functional state)
- **Type 1** — Resistive collapse (catastrophic loss of diode rectification)
- **Type 2** — Severe hysteresis with S-shape (interfacial charge-extraction barrier)
- **Type 3** — Reverse hysteresis (anomalous J-V dynamics, terminal interfacial decomposition)

### Thermodynamic decoupling (supervised)

Two parallel modeling tracks:

1. **Reversible thermodynamics:** an asymptotic exponential decay is fitted to hourly peak power to define a bounded State of Health H(t). MLR and XGBoost (with monotonic constraints ∂P/∂G ≥ 0, ∂P/∂T_c ≤ 0, ∂P/∂H_A ≤ 0) are then trained on the stationary target.
2. **Irreversible degradation:** a regularized XGBoost is trained directly on raw P_mp using cumulative stress integrals (G_accum, H_A,accum, T_accum) as path-dependent features.

Feature attributions are extracted with SHAP to isolate the marginal impact of each stress channel.

---

## Key Results

| Metric | M0 (Hybrid) | P12 (MAPbI₃) |
|---|---|---|
| Initial PCE | 7.16 % | 5.26 % |
| Asymptotic health plateau H∞ | 3.96 % | 30.49 % |
| Dominant degradation driver (SHAP) | Cumulative photo-thermal (54.8 %) | Synergistic multi-factorial |
| Terminal archetype | Type 3 (irreversible lock-in) | Type 0 (recovered) |

**Conclusion:** continuous morphological tracking and path-dependent predictive modeling are essential to benchmark authentic perovskite survivability in the field — static STC evaluations are insufficient.

---

## Stack

Python 3.10 · pandas · SciPy · scikit-learn · XGBoost · SHAP · NumPy · Matplotlib · Jupyter

---

## Data Source & Acknowledgments

The telemetry data analyzed in this thesis was provided by the **ParaSol platform** at the **Open Solar Stability (OSS) Lab**, University of Zaragoza (Spain), led by **Dr. Emilio J. Juárez-Pérez**. Data was shared with the **University of Seville** for collaborative research.

**ParaSol platform details:**

- Outdoor testing facility for perovskite solar cell stability under real environmental conditions
- MPPT tracking: Perovskino galvanostatic tracker (open-source, high-hysteresis capable)
- IV sweeps: Reverse + Forward scan directions
- Sensors: POA reference cell, ambient/module thermistors, capacitive humidity sensor
- Platform: [www.emiliojuarez.es](https://www.emiliojuarez.es)
- OSS Lab GitHub: [github.com/ej-jp/perovskino](https://github.com/ej-jp/perovskino)

**Special thanks to:**

- Dr. Miguel Anaya Martín and Dr. Manuel Jesús Jiménez Navarro, for their supervision and mentorship.
- Dr. Emilio J. Juárez-Pérez, for providing the high-quality telemetry data, the PSCs, and the technical information of the ParaSol platform.
- Guadalupe Vega Morrone, for her continuous support and constructive feedback.
- The entire team at SMS Lab (Instituto de Ciencia de Materiales de Sevilla, ICMS) for their welcome and scientific discussions.

---

## Citation

If you use this code or the methodology in your research, please cite:

Carretero Fernández, C. (2026). *Explainable Machine Learning for Outdoor Perovskite Solar Cell Degradation*. Bachelor's Thesis, Double Degree in Physics and Materials Engineering, Universidad de Sevilla.

---

## Author

**Cristian Carretero Fernández**  
Data Scientist & ML Researcher | Physicist & Materials Engineer

- GitHub: [@cristian-carretero](https://github.com/cristian-carretero)
- LinkedIn: [cristian-carretero-fernandez](https://www.linkedin.com/in/cristian-carretero-fernandez)

---

## Appendix: Artificial Intelligence Usage Declaration

AI tools (Google Gemini Pro) were used strictly as an iterative linguistic and coding support tool. All scientific content, data interpretation, system architecture, and scientific contributions are the sole work of the author. Full details are available in the thesis document (Appendix, page 45).

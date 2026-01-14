# Turbofan HPC Efficiency Prediction (EDA + Multicollinearity Analysis)

- **Goal:** explore whether we can predict **isentropic High-Pressure Compressor (HPC) efficiency** from turbofan operating/performance variables.
- **Type:** regression / tabular data.
- **Current status:** exploratory analysis + multicollinearity diagnostics (correlation + VIF). No final ML model shipped yet.

---

## What’s inside

- `data/Turbofan_HPC_Efficiency.csv` from kaggle https://www.kaggle.com/datasets/nicolascaparroz/turbofan-hpc-efficiency 
  - **11,971 rows × 17 columns**
  - **No missing values**
  - All variables are **float**
- `src/main.ipynb`
  - EDA (distributions, boxplots)
  - Outlier capping (IQR method)
  - Correlation heatmap
  - VIF-based feature elimination
- `variable_glossary.pdf`
  - Short descriptions of each variable
- `requirements.txt`
  - Environment snapshot (`pip freeze`)

---

## Dataset

- **Target (y):** `Isentr.HPCEfficiency`
- **Features (X):**
  - `NetThrust_kN`
  - `CoreNozzleGrossThrust_kN`
  - `BypassNozzleGrossThrust_kN`
  - `Sp.FuelConsumption_g/(kN*s)`
  - `SpecificThrust_m/s`
  - `CoreNozzleVel.V8_m/s`
  - `CoreNozzlePressureRatio`
  - `BypassNozzleVel.V18_m/s`
  - `BypassNozzlePressureRatio`
  - `BurnerEfficiency`
  - `EnginePressureRatioP5/P2`
  - `HPSpoolSpeed_RPM`
  - `LPSpoolSpeed_RPM`
  - `FuelFlow_kg/s`
  - `LPTExitPressureP5_kPA`
  - `LPTExitTemperatureT5_K`

See `variable_glossary.pdf` for a quick explanation of each feature.

---

## Quickstart

### 1) Create and activate a virtual environment
```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

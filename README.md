#  Physics-Informed Wind Power Forecasting

> Short-term wind power forecasting using physics-informed feature engineering on real SCADA turbine data — achieving **R² = 0.9868** with XGBoost.

 **Presented at ICTEAH-2026** | KJ Somaiya School of Engineering, Somaiya Vidyavihar University, Mumbai
 **Innovation Award** — CiiA-5, Nehru Science Centre, Mumbai (Feb 2026)

---

##  Problem

Wind power output is highly nonlinear — it scales with the **cube of wind speed**. Most forecasting methods are either:
- **Pure data-driven** → flexible but ignore physical constraints
- **Manufacturer power curves** → physically grounded but can't adapt to real-world degradation

This project bridges that gap with **physics-informed feature engineering** that encodes aerodynamics and turbine health directly into the ML pipeline.

---

##  Approach

Three custom physics-based features were engineered on top of raw SCADA data:

| Feature | Formula | What it Captures |
|---|---|---|
| **Wind Power Density (WPD)** | `WPD_t = (v_t)³` | Nonlinear cubic relationship between wind speed and power |
| **Turbulence Intensity** | Rolling std dev of wind speed over 30-sample window | Short-term wind variability |
| **Lagged Efficiency Proxy (η)** | `η_(t-1) = P_actual(t-1) / (P_theoretical(t-1) + ε)` | Turbine operational health — detects faults, blade soiling, pitch mismatch |

A strict **80:20 chronological train-test split** was used to prevent look-ahead bias — unlike random k-fold CV which leaks future data into training.

Four models were compared: **Random Forest, LightGBM, XGBoost, and ANN** — both with and without physics features.

---

##  Results

All metrics normalised to turbine rated capacity of 3618.7 kW.

| Model | Baseline RMSE | Physics RMSE | Baseline R² | Physics R² |
|---|---|---|---|---|
| Random Forest | 0.1412 | 0.0326 | 0.8559 | 0.9923 |
| LightGBM | 0.0620 | **0.0293** | 0.9702 | **0.9934** |
| ANN | 0.1249 | 0.0634 | 0.8816 | 0.9695 |
| **XGBoost (chosen)** | 0.1299 | **0.0417** | 0.8719 | **0.9868** |

**XGBoost was selected as the recommended model** — not because of best raw RMSE (LightGBM wins there), but because:
- Sequential boosting mirrors how fault events appear as residual spikes
- Feature importance is interpretable in terms of cause-and-effect
- More robust when turbine performance distribution shifts over time

> Physics-informed features reduced XGBoost RMSE by **70%** over baseline (0.1299 → 0.0417)

---

##  Dataset

**T1 Wind Turbine Dataset** — [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/785/wind+power+forecasting)

- Source: Single onshore wind turbine, Turkey (2018)
- Size: **50,530 records**, 10-minute SCADA intervals
- Features: Wind speed, wind direction, theoretical power, active power output
- Task: Predict active power at `t+1` (10-minute ahead forecast)

---

##  Tech Stack

![Python](https://img.shields.io/badge/Python-0d1117?style=for-the-badge&logo=python&logoColor=58A6FF)
![XGBoost](https://img.shields.io/badge/XGBoost-0d1117?style=for-the-badge&logo=python&logoColor=58A6FF)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-0d1117?style=for-the-badge&logo=scikit-learn&logoColor=58A6FF)
![Pandas](https://img.shields.io/badge/Pandas-0d1117?style=for-the-badge&logo=pandas&logoColor=58A6FF)
![NumPy](https://img.shields.io/badge/NumPy-0d1117?style=for-the-badge&logo=numpy&logoColor=58A6FF)
![Matplotlib](https://img.shields.io/badge/Matplotlib-0d1117?style=for-the-badge&logo=python&logoColor=58A6FF)

---

---

##  Citation / Paper

 **Presented at ICTEAH-2026**
*Full paper available upon request — publication pending.*

> Salvi, A., Pandagare, A., **Survase, A.**, Varangaonkar, P., & Gupta, S. (2026).
> *Physics-Informed Feature Engineering: Enhancing Short-Term Wind Power Forecasting.*
> Presented at ICTEAH-2026, KJ Somaiya School of Engineering, Mumbai.

[Request a preprint](mailto:ankitasurvase134@gmail.com)

---

##  Authors

- Anjnney Salvi
- Aarushee Pandagare
- **Ankita Survase** · [GitHub](https://github.com/ankitas134) · [Kaggle](https://www.kaggle.com/ankitasurvasee)
- Payal Varangaonkar
- Sudha Gupta (Guide)

---

##  License

MIT License — see [LICENSE](LICENSE) for details.

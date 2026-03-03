# 🇺🇿 Poverty Prediction — Uzbekistan DHS 1996

<div align="center">

[![Python Version](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Data: DHS Program](https://img.shields.io/badge/data-DHS%20Program-blue)](https://dhsprogram.com/)
[![Status](https://img.shields.io/badge/status-complete-success.svg)]()

**Benchmarking 4 ML models for household poverty classification using Uzbekistan DHS survey data**

[Overview](#-overview) • [Results](#-results) • [Dataset](#-dataset) • [Features](#-features) • [Reproduce](#-reproduce) • [Limitations](#-limitations)

</div>

---

## 🎯 Overview

This project replicates and extends World Bank methodology for ML-based poverty prediction, applying it to Uzbekistan's 1996 Demographic and Health Survey (DHS). Four supervised learning models are trained and compared to classify household poverty status based on demographic and socioeconomic indicators.

A key motivation is the near-absence of ML research on Central Asian poverty data. Uzbekistan in 1996 was navigating a severe post-Soviet economic transition — understanding what predicted household poverty during this period has both historical and policy significance.

---

## 📊 Results

Evaluated on a held-out test set of 741 households (20%):

| Model | Accuracy | AUC-ROC | Poor F1 | Not Poor F1 |
|-------|----------|---------|---------|-------------|
| Logistic Regression | 93.00% | **0.9856** | 0.89 | 0.95 |
| Decision Tree | 92.00% | 0.9395 | 0.87 | 0.94 |
| Random Forest | 92.00% | 0.9784 | 0.87 | 0.94 |
| **Gradient Boosting** | **94.00%** | 0.9817 | **0.89** | **0.95** |

**Best overall: Gradient Boosting (94% accuracy). Best AUC-ROC: Logistic Regression (0.9856).**

### Key Findings

**Urban/rural location** was the single strongest predictor — urban poverty rate was 6.2% versus 58.0% in rural areas, reflecting the severe urban-rural divide in post-Soviet Uzbekistan.

**Water source** was the second most important feature group. Households relying on public taps, wells, rivers, or streams were significantly more likely to be poor, while piped water into the residence was a strong wealth signal — consistent with the state of rural water infrastructure following the Soviet collapse.

**Appliance ownership** (refrigerator, television) ranked highly, reflecting that durable goods were reliable proxies for household wealth in 1996 Uzbekistan.

**Household size** showed a modest but consistent effect — poor households averaged 5.9 members versus 4.9 for non-poor households.

---

## 🗃️ Dataset

| Property | Value |
|----------|-------|
| **Source** | Uzbekistan Standard DHS, 1996 — [DHS Program](https://dhsprogram.com) |
| **Survey type** | Standard DHS (Phase III) |
| **Households** | 3,703 |
| **Poverty rate** | 29.17% (lowest + second wealth quintile) |
| **Target variable** | Binary poverty label derived from DHS Wealth Index (`wlthind5`) |
| **Splits** | 80% train / 20% test (stratified) |

### Poverty Label Construction

The DHS Wealth Index (`wlthind5`) assigns each household to one of five quintiles. Households in the **lowest** or **second quintile** are labeled **poor (1)**; all others are labeled **not poor (0)**. This gives a poverty rate of 29.17%, consistent with Uzbekistan's economic conditions in 1996.

> ⚠️ The raw DHS data files are not included in this repository due to DHS Program terms of use. Access requires free registration at [dhsprogram.com](https://dhsprogram.com). The processed feature matrix cannot be shared due to DHS Program terms of use. Please follow the reproduction steps above to generate it from the raw data.

---

## 🔧 Features

| Variable | Description | Type |
|----------|-------------|------|
| `hv009` | Household size | Numeric |
| `hv025` | Urban / rural residence | Binary |
| `hv219` | Sex of household head | Binary |
| `hv201` | Primary water source | Categorical (one-hot) |
| `hv205` | Toilet facility type | Categorical (one-hot) |
| `hv206` | Electricity access | Binary |
| `hv207` | Radio ownership | Binary |
| `hv208` | Television ownership | Binary |
| `hv209` | Refrigerator ownership | Binary |
| `hv210` | Bicycle ownership | Binary |
| `hv211` | Motorcycle ownership | Binary |

Final feature matrix: **23 columns** after one-hot encoding. No missing values in any feature.

---

## 🛠️ Tech Stack

| Category | Details |
|----------|---------|
| **Language** | Python 3.10+ |
| **Libraries** | pandas, numpy, scikit-learn, matplotlib, seaborn |
| **Data format** | Stata (.dta) via `pd.read_stata` |
| **Environment** | Jupyter Notebook |

---

## 🔁 Reproduce

### 1. Clone the repo

```bash
git clone https://github.com/sssplash6/poverty-prediction-uzbekistan.git
cd poverty-prediction-uzbekistan
```

### 2. Install dependencies

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

### 3. Get the data

Register for free at [dhsprogram.com](https://dhsprogram.com) and request the Uzbekistan 1996 Standard DHS dataset. Download:
- `UZHR31DT.dta` — Household Recode
- `UZWI31DT.dta` — Wealth Index

Place them in the `data/` directory.

Alternatively, use the pre-processed feature matrix already in the repo:
```python
import pandas as pd
df = pd.read_csv('uzbekistan_1996_processed.csv')
```

### 4. Run the notebook

```bash
jupyter notebook 01_eda.ipynb
```

---

## ⚠️ Limitations

- **Data age**: The most recent DHS survey available for Uzbekistan dates to 2002 (Health Examination Survey), with no standard poverty-focused survey after 1996. Results reflect economic conditions during post-Soviet transition and should not be extrapolated to present-day Uzbekistan.
- **No education variable**: The household head's education level (`hv106`) was not available in this dataset — a known strong poverty predictor that would likely improve model performance.
- **Wealth index as proxy**: The DHS Wealth Index is a relative, asset-based measure — not a direct income or consumption poverty measure. It captures asset inequality rather than absolute deprivation.
- **Single year**: With only one time point available, longitudinal analysis was not possible.

---

## 🗺️ Future Work

- [ ] Incorporate 2002 Health Examination Survey to enable longitudinal comparison
- [ ] Build a proxy wealth index for 2002 from asset ownership variables
- [ ] Compare findings with neighboring Central Asian countries (Kyrgyzstan, Tajikistan)
- [ ] Apply SHAP values for more interpretable feature importance

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgments

- **DHS Program** — for providing open access to survey data for research purposes
- **World Bank ML Poverty Prediction research** — for the methodological framework
- **Virginia Tech** — for prior collaboration on Uzbek economic research that motivated this project

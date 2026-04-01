# 🇮🇳 causal-impact-digital-india

> **Causal ML analysis of Digital India's effect on literacy, employment, and GDP across Indian states**

---

## 📌 Overview

This project applies **heterogeneous treatment effect estimation** to quantify the causal impact of India's telecom infrastructure expansion (Digital India initiative) on three socioeconomic outcomes: literacy rate, employment rate, and GDP per capita — at the state level.

Unlike simple correlation studies, this analysis uses modern **Causal Machine Learning** methods to answer: *does expanding internet and telecom access actually cause improvements in socioeconomic outcomes, and which states benefit the most?*

---

## 🔬 Research Question

**Does high telecom penetration under Digital India causally improve literacy, employment, and economic growth — and does the effect vary across Indian states?**

| Component | Variable | Description |
|-----------|----------|-------------|
| **Treatment (T)** | Telecom subscribers (binary) | High vs. low telecom access — median split |
| **Outcome Y₁** | Literacy rate (%) | NFHS-5 state-level literacy |
| **Outcome Y₂** | Employment rate (%) | Derived from CMIE unemployment data |
| **Outcome Y₃** | GDP per capita (₹) | State-level GDP |
| **Confounders (X)** | Electricity, water access, youth population % | Pre-existing conditions influencing both T and Y |

---

## 🛠️ Methods

| Method | Purpose |
|--------|---------|
| **T-Learner (XGBoost)** | Baseline CATE estimator — trains separate models for treated/control |
| **Doubly Robust (DR) Estimator** | Combines outcome regression + IPW; robust to model misspecification |
| **Causal Forest (EconML CausalForestDML)** | Heterogeneous treatment effects with state-level confidence intervals |
| **Propensity Score Matching (PSM)** | Nearest-neighbour and caliper matching for quasi-experimental comparison |
| **Rosenbaum Bounds** | Sensitivity analysis for hidden confounding |

---

## 📦 Datasets

| # | Dataset | Source | Role |
|---|---------|--------|------|
| 1 | Year-wise Telecom Subscribers (2008–2022) | TRAI via Kaggle: `hmnshudhmn24/year-wise-telecom-subscribers-in-india-20082022` | Treatment variable |
| 2 | NFHS-5 Rural & Urban State-wise Data | IIPS via Kaggle: `nitishsinghal/indian-rural-and-urban-statewise-family-data` | Literacy + confounders |
| 3 | India State-wise GDP | MoSPI via Kaggle: `siddheshmahajan/indias-gdp-statewise` | GDP outcome |
| 4 | Unemployment in India | CMIE via Kaggle: `gokulrajkmv/unemployment-in-india` | Employment outcome |

> **Recommended upgrade**: Replace with World Bank WDI (`IT.NET.USER.ZS`), RBI DBIE state GSDP, and MoSPI PLFS for publication-quality analysis.

---

## 🗂️ Repository Structure

```
causal-impact-digital-india/
│
├── causal-impact-digital-india-analysis.ipynb   # Main analysis notebook
├── README.md                                     # This file
│
├── data/                                         # (gitignored) Downloaded datasets
│   ├── telecom/
│   ├── nfhs/
│   ├── gdp/
│   └── unemployment/
│
├── outputs/                                      # Generated results
│   ├── digital_india_causal_results.csv          # Full state-level CATE estimates
│   ├── ate_comparison_table.csv                  # ATE comparison across estimators
│   └── covariate_balance.csv                     # Balance diagnostics
│
└── figures/                                      # Saved plots
    ├── fig_eda_distributions.png
    ├── fig_propensity_overlap.png
    ├── fig_balance.png
    ├── fig_ate_comparison.png
    ├── fig_cate_distributions.png
    ├── fig_statewise_cate_literacy.png
    ├── fig_feature_importance.png
    └── fig_dose_response.png
```

---

## 🚀 Getting Started

### Run in Google Colab (Recommended)

1. **Open the notebook** in Google Colab

2. **Set up Kaggle API** (run this before Cell 1):
```python
from google.colab import files
files.upload()  # Upload your kaggle.json
!mkdir -p ~/.kaggle && mv kaggle.json ~/.kaggle/ && chmod 600 ~/.kaggle/kaggle.json
```

3. **Run all cells** — datasets are downloaded automatically

### Run Locally

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/causal-impact-digital-india.git
cd causal-impact-digital-india

# Install dependencies
pip install econml xgboost pandas numpy seaborn matplotlib scikit-learn openpyxl kagglehub

# Launch notebook
jupyter notebook causal-impact-digital-india-analysis.ipynb
```

---

## 📊 Key Results

The notebook produces:

- **ATE Comparison Table** — Average Treatment Effect from T-Learner, DR, and Causal Forest across all three outcomes
- **State-level CATE Bar Chart** — which states benefit most from telecom expansion
- **Propensity Score Overlap Plot** — validates the common support assumption
- **Covariate Balance Love Plot** — before/after IPW reweighting
- **Dose-Response Curves** — continuous treatment intensity vs. outcomes
- **Rosenbaum Bounds** — robustness to hidden confounders

---

## ⚠️ Limitations

1. **Observational data** — causal estimates may be affected by unobserved confounders despite using causal ML methods
2. **Cross-sectional design** — panel data with DiD or synthetic control would be stronger
3. **Small N** (~28–36 states) — estimates are directional, not precise
4. **Median binarization** — loses treatment intensity information (addressed via continuous treatment sensitivity check)
5. **Data quality** — Kaggle-sourced data may have year mismatches across sources

> These findings constitute **suggestive causal evidence**, not proof. They motivate further investigation via natural experiments or RCT designs.

---

## 🧰 Tech Stack

![Python](https://img.shields.io/badge/Python-3.10-blue)
![EconML](https://img.shields.io/badge/EconML-CausalForest-green)
![XGBoost](https://img.shields.io/badge/XGBoost-meta--learner-orange)
![scikit-learn](https://img.shields.io/badge/scikit--learn-PSM-lightgrey)
![Seaborn](https://img.shields.io/badge/Seaborn-visualization-teal)

- **Causal Inference**: `econml`, custom T-Learner, Doubly Robust estimator
- **ML Models**: `xgboost`, `sklearn` (Logistic Regression, GBM, NearestNeighbors)
- **Data**: `pandas`, `numpy`
- **Visualization**: `matplotlib`, `seaborn`

---

## 📄 License

MIT License — free to use, modify, and distribute with attribution.

---

## 🙋 Author

Built as a causal inference capstone project exploring Digital India's socioeconomic impact using state-of-the-art heterogeneous treatment effect methods.

*Feedback and contributions welcome — open an issue or submit a PR.*

<div align="center">

# 🇮🇳 Causal Impact of Digital India

### Heterogeneous Treatment Effect Analysis Using Causal Machine Learning

*A causal ML study investigating whether internet expansion under Digital India is associated with improvements in literacy, employment, and GDP per capita — with a focus on heterogeneous state-level effects.*

![Python](https://img.shields.io/badge/python-3.9+-blue.svg)
![Causal ML](https://img.shields.io/badge/Causal%20ML-EconML-orange.svg)
![XGBoost](https://img.shields.io/badge/XGBoost-T--Learner-red.svg)
![Data](https://img.shields.io/badge/data-Indian%20government%20sources-green.svg)
![Status](https://img.shields.io/badge/status-research%20analysis-purple.svg)

</div>

---

## 📊 At a Glance

|                          |                                                                                                      |
| ------------------------ | ---------------------------------------------------------------------------------------------------- |
| 🎯 **Research question** | Does expanding internet access under Digital India improve literacy, employment, and GDP per capita? |
| 🧪 **Treatment**         | Internet penetration (%) at the state level                                                          |
| 📈 **Outcomes**          | Literacy rate · Employment rate · GDP per capita                                                     |
| 🧠 **Methods**           | T-Learner · Doubly Robust · Causal Forest · PSM · Rosenbaum Bounds                                   |
| 🗺️ **Unit of analysis** | Indian states/UTs, n ≈ 30                                                                            |
| 🔬 **Core finding**      | Suggestive positive effects for literacy and GDP per capita; employment effects remain ambiguous     |
| ⚠️ **Identification**    | Observational cross-sectional design; results are directional, not causal proof                      |

---

## 🎯 1. Research Question

> Does expanding internet access under Digital India causally improve literacy, employment, and GDP per capita — and does the effect vary across Indian states?

The analysis treats **internet penetration** as the treatment variable and estimates both average and heterogeneous treatment effects after adjusting for observable socioeconomic and infrastructure characteristics.

---

## 🏗️ 2. Causal Architecture

```text
State-level socioeconomic data
                │
                ▼
       ┌──────────────────────┐
       │ Treatment Definition  │
       │ Internet penetration  │
       └──────────────────────┘
                │
                ▼
       ┌──────────────────────┐
       │ Confounder Adjustment │
       │                      │
       │ Electricity          │
       │ Piped water          │
       │ Youth share          │
       │ Urbanisation         │
       │ Log income           │
       └──────────────────────┘
                │
                ▼
      ┌─────────────────────────┐
      │ Causal ML Estimators     │
      │                         │
      │ T-Learner              │
      │ Doubly Robust          │
      │ Causal Forest DML      │
      │ PSM                   │
      └─────────────────────────┘
                │
                ▼
       ┌──────────────────────┐
       │ Treatment Effects     │
       │                      │
       │ ATE / ATT            │
       │ State-level CATE     │
       │ Confidence intervals │
       └──────────────────────┘
                │
                ▼
       ┌──────────────────────┐
       │ Robustness &          │
       │ Sensitivity Analysis  │
       │                      │
       │ Covariate balance    │
       │ Propensity overlap   │
       │ Rosenbaum bounds     │
       │ Dose-response        │
       └──────────────────────┘
```

> **Important:** The analysis estimates causal effects under the assumption of conditional unconfoundedness. Because the data are observational and cross-sectional, the results should be interpreted as **suggestive directional evidence rather than definitive causal estimates**.

---

## 📁 3. Project Structure

<details>
<summary>Click to expand</summary>

```text
causal-impact-digital-india/
│
├── causal_impact_digital_india_publishable.ipynb
│                                      Main analysis notebook
│
├── outputs/
│   ├── digital_india_cate_results.csv
│   │                              State-level CATE estimates
│   │
│   ├── table3_ate_comparison.csv
│   │                              ATE comparison across estimators
│   │
│   ├── table2_covariate_balance.csv
│   │                              IPW covariate balance diagnostics
│   │
│   ├── table_sensitivity_thresholds.csv
│   │                              Multi-threshold robustness results
│   │
│   ├── fig1_eda_distributions.*
│   │                              Variable distributions by treatment group
│   │
│   ├── fig2_propensity_overlap.*
│   │                              Propensity score overlap diagnostics
│   │
│   ├── fig3_love_plot.*
│   │                              Covariate balance before/after IPW
│   │
│   ├── fig4_ate_comparison.*
│   │                              ATE comparison across estimators
│   │
│   ├── fig5_cate_distributions.*
│   │                              CATE distribution histograms
│   │
│   ├── fig6_statewise_cate.*
│   │                              State-level CATEs with 95% CIs
│   │
│   ├── fig7_feature_importance.*
│   │                              XGBoost feature importance
│   │
│   ├── fig8_cate_heterogeneity.*
│   │                              CATE moderation by baseline characteristics
│   │
│   └── fig9_dose_response.*
│                                  Internet penetration vs outcomes
│
└── README.md
```

</details>

---

## ⚙️ 4. Setup

All datasets used in the analysis are embedded directly in the notebook, so no manual dataset downloads or API credentials are required.

```bash
# 1. Clone the repository
git clone https://github.com/your-username/your-repo.git
cd causal-impact-digital-india

# 2. Install dependencies
pip install econml xgboost pandas numpy seaborn matplotlib scikit-learn scipy
```

The notebook is designed to run in:

* Google Colab
* Kaggle
* Local Jupyter environments

---

## 🚀 5. Running It

Open:

```text
causal_impact_digital_india_publishable.ipynb
```

Then run the notebook from top to bottom.

```text
1. Load embedded state-level data
2. Define treatment and outcome variables
3. Prepare confounders
4. Perform exploratory analysis
5. Estimate propensity scores
6. Check treatment overlap
7. Run causal estimators
8. Estimate ATE / ATT
9. Estimate state-level CATEs
10. Analyse treatment-effect heterogeneity
11. Run robustness and sensitivity checks
12. Generate publication-quality figures
13. Export tables and results
```

All randomised components use:

```python
RANDOM_SEED = 42
```

---

## 🧪 6. Causal Methods

The project uses multiple complementary estimators rather than relying on a single causal ML model.

| Method                           | Purpose                                                        | Main output           |
| -------------------------------- | -------------------------------------------------------------- | --------------------- |
| 🧠 **T-Learner**                 | Separate outcome models for treated/control groups             | CATE                  |
| ⚖️ **Doubly Robust**             | Combines outcome regression and inverse-probability weighting  | ATE / CATE            |
| 🌳 **Causal Forest DML**         | Learns heterogeneous treatment effects using orthogonalisation | State-level CATE      |
| 🔗 **Propensity Score Matching** | Compares observationally similar treated/control states        | ATT                   |
| 🛡️ **Rosenbaum Bounds**         | Tests sensitivity to hidden confounding                        | Sensitivity threshold |

### T-Learner

An XGBoost-based T-Learner estimates separate response functions:

```text
T = 1  →  E[Y | X, T=1]
T = 0  →  E[Y | X, T=0]

CATE(X) = μ₁(X) - μ₀(X)
```

Bootstrap confidence intervals are used to quantify uncertainty.

### Doubly Robust Estimation

The DR estimator combines:

* outcome regression
* propensity-score weighting

This provides robustness when either the treatment model or outcome model is correctly specified.

### Causal Forest

`EconML.CausalForestDML` is used to estimate heterogeneous treatment effects while controlling for observed covariates.

The resulting CATE estimates allow the analysis to ask:

> Which types of states benefit more from increased internet penetration?

---

## 🗂️ 7. Data & Variables

| Role               | Variable                 | Source                    | Period   |
| ------------------ | ------------------------ | ------------------------- | -------- |
| 🎯 **Treatment**   | Internet penetration (%) | TRAI / World Bank         | Dec 2021 |
| 📚 **Outcome Y₁**  | Literacy rate (%)        | NFHS-5 / IIPS             | 2019–21  |
| 💼 **Outcome Y₂**  | Employment rate (%)      | PLFS / MoSPI              | 2020–21  |
| 💰 **Outcome Y₃**  | GDP per capita (USD)     | MoSPI State GSDP          | 2020–21  |
| ⚡ **Confounder**   | Electricity access       | NFHS-5 / IIPS             | 2019–21  |
| 🚰 **Confounder**  | Piped water access       | NFHS-5 / IIPS             | 2019–21  |
| 👥 **Confounder**  | Youth share              | NFHS-5 / IIPS             | 2019–21  |
| 🏙️ **Confounder** | Urbanisation             | NFHS-5 / IIPS             | 2019–21  |
| 💵 **Confounder**  | Log income               | State-level economic data | 2020–21  |

### Data sources

* TRAI Telecom Subscription Reports
* NFHS-5 / International Institute for Population Sciences
* Ministry of Statistics and Programme Implementation
* PLFS Annual Reports
* World Bank

All data used by the notebook is embedded inline for reproducibility.

---

## 📈 8. Key Findings

| Outcome                | Direction | Estimator consistency | Interpretation                                                                                                     |
| ---------------------- | --------- | :-------------------: | ------------------------------------------------------------------------------------------------------------------ |
| 📚 **Literacy rate**   | Positive  |          High         | High-internet states show higher literacy conditional on observed infrastructure and socioeconomic characteristics |
| 💼 **Employment rate** | Mixed     |          Low          | No robust employment effect; labour-market channels appear heterogeneous                                           |
| 💰 **GDP per capita**  | Positive  |          High         | High-internet states show higher estimated per-capita income                                                       |

### Estimated effect patterns

```text
Literacy
    │
    ├── Positive effect
    └── approximately +2 to +5 percentage points


GDP per capita
    │
    ├── Positive effect
    └── approximately +$200 to +$600


Employment
    │
    ├── Mixed estimates
    └── No robust directional effect
```

> These magnitudes are **model-based estimates from an observational state-level sample** and should not be interpreted as policy-causal effect sizes without stronger identification.

---

## 🔬 9. Treatment-Effect Heterogeneity

The analysis does not assume that Digital India's effects are identical across states.

### ⚡ Electricity infrastructure

States with stronger baseline electricity access show **larger estimated treatment effects**.

This suggests that digital connectivity may be more effective when complementary physical infrastructure is already available.

### 🏙️ Urbanisation

More urbanised states exhibit larger GDP CATEs, consistent with possible agglomeration and complementary economic effects.

### 📚 Baseline literacy

States with very low baseline literacy — including Bihar and Uttar Pradesh — show near-zero estimated literacy CATEs from internet expansion alone.

This suggests that internet access may not be sufficient by itself to overcome deeper educational constraints.

### 📉 Diminishing returns

The dose-response analysis suggests diminishing returns at very high levels of internet penetration, with effects becoming less pronounced above approximately **65% penetration**.

---

## 📊 10. Outputs & Diagnostics

The notebook produces both statistical tables and publication-quality visualisations.

| Output                             | Purpose                                |
| ---------------------------------- | -------------------------------------- |
| `digital_india_cate_results.csv`   | State-level treatment-effect estimates |
| `table3_ate_comparison.csv`        | Comparison of ATE estimates            |
| `table2_covariate_balance.csv`     | IPW balance diagnostics                |
| `table_sensitivity_thresholds.csv` | Sensitivity analysis                   |
| `fig1_eda_distributions`           | Treatment/outcome distributions        |
| `fig2_propensity_overlap`          | Propensity-score overlap               |
| `fig3_love_plot`                   | Covariate balance                      |
| `fig4_ate_comparison`              | Cross-estimator ATE comparison         |
| `fig5_cate_distributions`          | CATE distributions                     |
| `fig6_statewise_cate`              | State-level CATEs with 95% CIs         |
| `fig7_feature_importance`          | XGBoost feature importance             |
| `fig8_cate_heterogeneity`          | CATE moderation analysis               |
| `fig9_dose_response`               | Dose-response relationship             |

Figures are exported in both **PNG** and **PDF** formats.

---

## 🛡️ 11. Robustness & Sensitivity Analysis

Several diagnostics are used to assess whether the findings are stable.

### Propensity-score overlap

Checks whether treated and control states have sufficiently comparable propensity scores.

### IPW covariate balance

Evaluates whether weighting reduces observable differences between treatment groups.

### Cross-estimator comparison

ATE estimates are compared across:

```text
T-Learner
     │
     ├──► Doubly Robust
     │
     ├──► Causal Forest DML
     │
     └──► Propensity Score Matching
```

Agreement across estimators provides stronger directional evidence than relying on one estimator alone.

### Rosenbaum Bounds

Sensitivity analysis evaluates how strong an unobserved confounder would need to be to alter the conclusions.

---

## ⚠️ 12. Known Limitations

* **Observational design** — unconfoundedness is assumed rather than experimentally established.
* **Small sample** — approximately 30 states/UTs is small for flexible causal ML estimation.
* **Cross-sectional identification** — a single time slice cannot fully rule out reverse causality.
* **Treatment binarisation** — the median split discards some information about treatment intensity.
* **Temporal mismatch** — outcomes from 2019–21 partially overlap with the Digital India rollout period.
* **Unobserved confounding** — governance quality, geography, terrain, education quality, and other factors may remain uncontrolled.
* **Treatment measurement error** — telecom service areas do not perfectly correspond to state boundaries.
* **State-level aggregation** — state averages may hide substantial within-state heterogeneity.
* **CATE uncertainty** — individual state-level effects should be treated as exploratory rather than definitive estimates.

> A stronger future design would use panel data with **Difference-in-Differences**, event-study specifications, or a synthetic-control framework.

---

## 📚 13. References

* Athey, S., & Wager, S. (2019). *Estimating treatment effects with causal forests*. Observational Studies, 5(2), 37–51.
* Austin, P. C. (2011). *An introduction to propensity score methods*. Multivariate Behavioral Research, 46(3), 399–424.
* Chernozhukov et al. (2018). *Double/debiased machine learning for treatment and causal parameters*. The Econometrics Journal, 21(1), C1–C68.
* Rosenbaum, P. R. (2002). *Observational Studies*. Springer.
* Government of India, IIPS. (2021). *NFHS-5 India Report*.
* Government of India, MoSPI. (2021). *PLFS Annual Report 2020–21*.
* TRAI. (2022). *Telecom Subscription Reports — December 2021*.

---

## 📋 14. More Info

|                           |                                                   |
| ------------------------- | ------------------------------------------------- |
| 🔗 **Repository**         | https://github.com/your-username/your-repo        |
| 📓 **Main notebook**      | `causal_impact_digital_india_publishable.ipynb`   |
| 🧠 **Primary ML library** | `EconML`                                          |
| 🌳 **Tree-based models**  | `XGBoost`                                         |
| 🌐 **Geographic scope**   | Indian states / UTs                               |
| 📊 **Sample size**        | n ≈ 30                                            |
| ⚠️ **Interpretation**     | Suggestive directional evidence, not causal proof |

---

## 📝 Citation

If you use this analysis or code, please cite:

```bibtex
@misc{digital_india_causal,
  title   = {Causal Impact of Digital India on Socioeconomic Outcomes:
             Heterogeneous Treatment Effect Analysis Using Causal Machine Learning},
  year    = {2024},
  url     = {https://github.com/your-username/your-repo}
}
```

---

<div align="center">

*Built for causal inference research on India's digital transformation.*

</div>

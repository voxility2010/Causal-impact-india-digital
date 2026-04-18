# Causal Impact of Digital India on Socioeconomic Outcomes
### Heterogeneous Treatment Effect Analysis Using Causal Machine Learning

---

## Overview

This project examines whether India's **Digital India programme** (launched 2015) causally improves state-level **literacy**, **employment**, and **GDP per capita** — and whether these effects vary across states. Using internet penetration as the treatment variable and three complementary causal ML estimators, the analysis finds suggestive positive effects on literacy and GDP per capita, with heterogeneity concentrated in states with stronger baseline electricity infrastructure. Employment effects remain ambiguous.

> **Note**: All findings carry the caveats of observational cross-sectional analysis with a small state-level sample (n ≈ 30). Results constitute *suggestive directional evidence*, not causal proof.

---

## Research Question

> Does expanding internet access under Digital India causally improve literacy, employment, and GDP per capita — and does the effect vary by state?

---

## Causal Framework

| Role | Variable | Description |
|------|----------|-------------|
| **Treatment** | Internet penetration (%) | % of population using internet — TRAI/WDI |
| **Outcome Y₁** | Literacy rate (%) | NFHS-5 state-level literacy |
| **Outcome Y₂** | Employment rate (%) | 100 − unemployment rate (PLFS) |
| **Outcome Y₃** | GDP per capita (USD) | State-level GSDP per capita (MoSPI) |
| **Confounders** | Electricity, piped water, youth share, urbanisation, log income | Pre-existing conditions predicting both treatment and outcomes |

**Core identification assumption**: Conditional on confounders, treatment assignment (high vs. low internet penetration) is as-good-as-random (unconfoundedness/strong ignorability).

---

## Methods

The analysis uses five complementary approaches:

1. **T-Learner** — XGBoost meta-learner with bootstrap 95% CIs; flexible CATE estimator
2. **Doubly Robust (DR)** — combines outcome regression + IPW; consistent under partial model misspecification
3. **Causal Forest (EconML CausalForestDML)** — captures heterogeneous state-level treatment effects with honest splitting and confidence intervals
4. **Propensity Score Matching (PSM)** — nearest-neighbour and caliper matching for ATT estimation
5. **Rosenbaum Bounds** — formal sensitivity test for unobserved confounding

---

## Data Sources

All data is **embedded inline** — no manual downloads or account sign-ups required. The notebook is fully self-contained and reproducible.

| Dataset | Source | Role | Year |
|---------|--------|------|------|
| Internet penetration (%) | [TRAI Subscription Reports](https://www.trai.gov.in/release-publication/reports/telecom-subscription-reports) | Treatment variable | Dec 2021 |
| Literacy, electricity, water, demographics | [NFHS-5 Factsheets (IIPS)](https://rchiips.org/nfhs/NFHS-5Reports/NFHS-5_INDIA_REPORT.pdf) | Outcomes + confounders | 2019–21 |
| GDP per capita | [MoSPI State GSDP](https://mospi.gov.in) | Outcome | 2020–21 |
| Employment rate | [PLFS Annual Report (MoSPI)](https://mospi.gov.in/documents/213904/0/) | Outcome | 2020–21 |

---

## Key Findings

| Outcome | Direction | Cross-Estimator Consistency | Interpretation |
|---------|-----------|-----------------------------|----------------|
| **Literacy Rate** | Positive | All three agree | ~+2–5 %p; high-internet states show higher literacy, conditional on infrastructure confounders |
| **Employment Rate** | Mixed | Estimators disagree | No robust employment effect; labour market channels are heterogeneous |
| **GDP per Capita** | Positive | All three agree | ~+USD 200–600; high-internet states show higher per-capita income |

**Heterogeneity findings:**
- States with higher baseline **electricity access** show amplified Digital India effects
- More **urbanised** states exhibit larger GDP CATEs (agglomeration dynamics)
- States with very low baseline **literacy** (e.g., Bihar, UP) show near-zero literacy CATEs from internet expansion alone
- **Diminishing returns** appear above ~65% internet penetration

---

## Repository Structure

```
├── causal_impact_digital_india_publishable.ipynb   # Main analysis notebook
├── outputs/
│   ├── digital_india_cate_results.csv              # State-level CATE estimates (all outcomes)
│   ├── table3_ate_comparison.csv                   # ATE comparison across estimators
│   ├── table2_covariate_balance.csv                # IPW covariate balance diagnostics
│   ├── table_sensitivity_thresholds.csv            # Multi-threshold robustness results
│   ├── fig1_eda_distributions.*                    # EDA: variable distributions by group
│   ├── fig2_propensity_overlap.*                   # Propensity score overlap diagnostics
│   ├── fig3_love_plot.*                            # Covariate balance before/after IPW
│   ├── fig4_ate_comparison.*                       # ATE comparison across three estimators
│   ├── fig5_cate_distributions.*                   # CATE distribution histograms
│   ├── fig6_statewise_cate.*                       # State-level CATEs with 95% CIs
│   ├── fig7_feature_importance.*                   # XGBoost feature importance
│   ├── fig8_cate_heterogeneity.*                   # CATE moderation by baseline characteristics
│   └── fig9_dose_response.*                        # Dose-response: log(internet) vs outcomes
└── README.md
```

Figures are saved as both PDF (print-quality) and PNG (web).

---

## Requirements

```bash
pip install econml xgboost pandas numpy seaborn matplotlib scikit-learn
```

| Package | Role |
|---------|------|
| `econml` | CausalForestDML implementation |
| `xgboost` | T-Learner and DR base learners |
| `scikit-learn` | Propensity model, preprocessing, LOO-CV |
| `pandas`, `numpy` | Data manipulation |
| `matplotlib`, `seaborn` | Visualisation |
| `scipy` | Rosenbaum bounds, statistical tests |

---

## Reproducibility

- Random seed is set globally (`RANDOM_SEED = 42`) at the top of the notebook
- All data is embedded inline; no external API calls or file downloads are needed
- Compatible with Google Colab, Kaggle, and local Jupyter environments
- Output directory is configurable via the `OUTPUT_DIR` environment variable

---

## Limitations

1. **Observational design**: Unconfoundedness is assumed, not tested. Unobserved confounders (terrain, governance quality) may still bias estimates despite causal ML adjustment.
2. **Small sample (n ≈ 30)**: All estimators are designed for larger datasets. State-level CATEs should be treated as exploratory and directional.
3. **Cross-sectional identification**: A single time slice cannot rule out reverse causality. A DiD or synthetic control design would provide stronger identification.
4. **Treatment binarisation**: The median split discards intensity variation (partially addressed via dose-response analysis in Section 15b).
5. **Temporal mismatch**: Outcomes measured 2019–21 partially overlap with the rollout period; long-run effects may be underestimated.
6. **Treatment measurement error**: TRAI service areas do not perfectly align with state boundaries, introducing attenuation bias.

---

## References

- Athey, S., & Wager, S. (2019). Estimating treatment effects with causal forests. *Observational Studies*, 5(2), 37–51.
- Austin, P. C. (2011). An introduction to propensity score methods. *Multivariate Behavioral Research*, 46(3), 399–424.
- Chernozhukov et al. (2018). Double/debiased machine learning. *The Econometrics Journal*, 21(1), C1–C68.
- Rosenbaum, P. R. (2002). *Observational Studies* (2nd ed.). Springer.
- Government of India, IIPS. (2021). *NFHS-5 India Report*. IIPS, Mumbai.
- Government of India, MoSPI. (2021). *PLFS Annual Report 2020–21*.
- TRAI. (2022). *Telecom Subscription Reports — December 2021*.

---

## Citation

If you use this code or analysis, please cite:

```
@misc{digital_india_causal,
  title   = {Causal Impact of Digital India on Socioeconomic Outcomes: 
             Heterogeneous Treatment Effect Analysis Using Causal Machine Learning},
  year    = {2024},
  url     = {https://github.com/your-username/your-repo}
}
```

---

## License

This project is released for academic and research use. Data sourced from Indian government agencies (TRAI, NFHS-5/IIPS, MoSPI) and the World Bank — all publicly accessible.

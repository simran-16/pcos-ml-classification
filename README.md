# PCOS Classification via SHAP-Guided Minimal Feature Selection

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Research%20In%20Progress-orange.svg)]()

## Overview

This project investigates whether a minimal, clinically interpretable 
feature set can match full-feature ensemble models for PCOS classification 
within a hospital evaluation cohort — without requiring ultrasound imaging.

**Central finding:** A SHAP-selected set of 10 features achieves 
ROC-AUC = 0.9984 (10-fold CV), representing a 72% reduction in feature 
count with less than 0.001 AUC loss compared to the full 36-feature model.


## Key Results

| Model | Accuracy | F1-Score | ROC-AUC | PR-AUC |
|---|---|---|---|---|
| Logistic Regression | 0.838 | 0.750 | 0.906 | 0.841 |
| Random Forest | 0.989 | 0.982 | 0.9996 | 0.9991 |
| XGBoost | 0.984 | 0.973 | 0.998 | 0.994 |
| AdaBoost | 0.962 | 0.937 | 0.991 | 0.981 |

*All pairwise differences statistically significant (McNemar's test, p < 0.05)*

**Feature Reduction:** 10 SHAP-selected features → AUC = 0.9984  
**Calibration:** Brier Score = 0.022 (well-calibrated)  
**Optimal Threshold:** 0.64 (Sensitivity = 0.983, Specificity = 0.978)

## Methodology Highlights

- **Data Audit:** Systematic identification of Rotterdam criterion features,
  data entry errors, and spectrum bias — documented before any modelling
- **Leakage-free pipeline:** SMOTE applied exclusively within CV training 
  folds using `imblearn.Pipeline`
- **Explainability:** SHAP TreeExplainer for global feature ranking, 
  beeswarm analysis, and per-patient local explanations
- **Statistical rigour:** McNemar's test for all pairwise model comparisons,
  bootstrapped 95% confidence intervals on AUC
- **Calibration:** Isotonic regression calibration with Brier score evaluation
- **Threshold optimisation:** Clinical screening vs. balanced decision 
  boundaries with PPV/NPV reporting

## Important Note on Dataset Context

This dataset originates from a hospital evaluation cohort (Kerala, India, 
10 centres) where all patients were under clinical evaluation for PCOS. 
Results reflect performance within this population and should not be 
interpreted as general population screening accuracy. See 
`docs/research_framing.md` for full discussion.

## Repository Structure

```
pcos-ml-classification/
├── notebooks/          # 8 numbered notebooks (audit → calibration)
├── outputs/figures/    # 16 publication-quality figures
├── data/sample/        # 5-row schema sample (full data via Kaggle)
├── docs/               # Research framing and audit findings
└── paper/              # Manuscript (in preparation)
```

## Getting Started

```bash
git clone https://github.com/[username]/pcos-ml-classification.git
cd pcos-ml-classification
pip install -r requirements.txt
```

Download the dataset from [Kaggle](https://www.kaggle.com/datasets/prasoonkottarathil/polycystic-ovary-syndrome-pcos) 
and place `PCOS_extended_dataset.csv` in `data/raw/`.

Run notebooks in order, starting with `01_data_audit_cleaning.ipynb`.

## Technologies

Python · scikit-learn · XGBoost · SHAP · imbalanced-learn · 
statsmodels · matplotlib · seaborn · pandas · NumPy

## Status

- [x] Data audit and cleaning
- [x] Exploratory data analysis  
- [x] Model comparison (4 models, 10-fold CV)
- [x] SHAP explainability analysis
- [x] Feature reduction study
- [x] Calibration and threshold analysis

## License

MIT License. See [LICENSE](LICENSE).

## Citation

If you use this work, please cite:  
> [Simran Devkumar]. (2026). *PCOS Classification via SHAP-Guided Minimal 
> Feature Selection*. GitHub Repository. 
> https://github.com/[simran-16]/pcos-ml-classification
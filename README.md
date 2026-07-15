# Diabetes Risk Classification: Multinomial vs. Binary Logistic Regression

Compared two classification strategies on **253,680 CDC health survey records** to predict diabetes risk. The binary logistic regression model won decisively — **84.7% accuracy and an AUC of 0.81** versus 64.8% accuracy for the three-class multinomial approach — quantifying the trade-off between diagnostic granularity and predictive power.

## Business Problem

Diabetes affects more than 38 million U.S. adults, and early identification of at-risk individuals — especially those in the transitional prediabetes state — is a public health priority where intervention can prevent progression to Type 2 diabetes. This project asks a practical modeling question with real screening implications: does separating prediabetes into its own class (multinomial model) add enough insight to justify its cost in accuracy, or is a simpler at-risk/not-at-risk classification (binary model) the better screening tool?

## Data

- **Source:** [Diabetes Health Indicators — BRFSS 2015 (Kaggle)](https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset) — CDC Behavioral Risk Factor Surveillance System
- **Size:** 253,680 complete observations (no missing values), nationally representative survey
- **Features:** BMI, age, high blood pressure, high cholesterol, physical activity, smoking, general/physical/mental health ratings, difficulty walking, education, income
- **Target:** diabetes status — No Diabetes / Prediabetes / Diabetes (collapsed to at-risk vs. not for the binary model)

## Approach

1. **Literature grounding:** Reviewed three peer-reviewed BRFSS studies (Okwechime et al. 2015; Yang et al. 2010; Ullah et al. 2022) to justify predictor selection and the multinomial-vs-binary comparison design.
2. **EDA:** Descriptive statistics, frequency tables, and a correlation heatmap confirmed meaningful predictor variability and no multicollinearity (all correlations < 0.60), supporting full-model estimation without stepwise selection.
3. **Class imbalance handling:** The prediabetes class was severely underrepresented; up-sampling the training data rebalanced multinomial performance across classes.
4. **Modeling:** Multinomial logistic regression (`nnet`) vs. binary logistic regression, evaluated on accuracy, McFadden's pseudo R², and AUC (`pROC`).

| Model | Accuracy | Pseudo R² | AUC |
|---|---|---|---|
| Multinomial (3-class, up-sampled) | 0.648 | 0.128 | — |
| **Binary (at-risk vs. not)** | **0.847** | **0.199** | **0.813** |

## Key Findings

- **The binary model is the better screening tool.** Collapsing prediabetes and diabetes into one at-risk class traded diagnostic detail for a 20-point accuracy gain and substantially better model fit — the right trade for a population-level screening context.
- **Risk factors replicated the epidemiology literature.** Odds ratios showed BMI, age, high blood pressure, high cholesterol, difficulty walking, and worse general health all increased diabetes risk, while physical activity, education, and income were protective — consistent across both models, which strengthens confidence in the estimates.
- **Class imbalance is the honest limitation.** The binary model detects non-diabetic individuals with 97% sensitivity but struggles on the minority at-risk class — a known challenge with imbalanced health data that motivates the resampling and ensemble methods listed in future work.

**BMI varies widely across respondents, making it a strong candidate predictor:**

![BMI distribution](images/fig1_bmi_distribution.png)

**No multicollinearity among predictors (all correlations below 0.60):**

![Correlation heatmap](images/fig2_correlation_heatmap.png)

**ROC curve for the binary model (AUC = 0.813):**

![ROC curve for binary logistic regression](images/fig3_roc_curve_binary.png)

## Tools

**R** — tidyverse (ggplot2, dplyr), caret, nnet, pROC, pscl, car, corrplot, janitor, gridExtra

## Repository Contents

- [`diabetes_project_script.Rmd`](diabetes_project_script.Rmd) — full analysis code (EDA → modeling → evaluation)
- [`diabetes_health_indicators_dataset.csv`](diabetes_health_indicators_dataset.csv) — dataset
- [`diabetes_project_final_report.pdf`](diabetes_project_final_report.pdf) — complete written report with literature review
- [`images/`](images/) — figures referenced above

## Future Work

Alternative resampling strategies (SMOTE), interaction terms, and benchmarking logistic regression against tree-based and ensemble methods to improve minority-class recall.

---

*Jack Griffin · B.S.B.A. Economics, Statistics Minor · University of Central Florida*

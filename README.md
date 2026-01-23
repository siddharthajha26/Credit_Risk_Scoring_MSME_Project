\# Credit Risk Scoring — MSME Portfolio



\## 🚀 Executive Summary



This repository presents an \*\*end-to-end Credit Risk Scoring framework for an MSME loan portfolio\*\*, built using \*\*Python and industry-standard statistical and machine learning techniques\*\*. The project mirrors \*\*real-world banking and consulting engagements\*\*, covering the full lifecycle from \*\*data preparation and feature engineering\*\* to \*\*PD modeling, calibration, validation, and ensemble learning\*\*, aligned with \*\*Basel / IFRS 9 expectations\*\*.



\## 📌 Project Overview



This project demonstrates the use of \*\*Python and Machine Learning\*\* to build \*\*Credit Scoring models\*\* for an \*\*MSME (Micro, Small \& Medium Enterprise) loan portfolio\*\* using a \*\*synthetic dataset of 1,000 loans\*\*. The objective is to estimate \*\*Probability of Default (PD)\*\*, develop a \*\*logistic regression scorecard\*\*, and enhance predictive performance using \*\*advanced ML models and ensemble (meta-learning) techniques\*\*.



The repository is designed as a \*\*step-by-step, inspection-friendly pipeline\*\*, closely aligned with \*\*banking industry practices under Basel / IFRS 9\*\*, and is suitable for \*\*credit risk modeling, validation, and analytics portfolios\*\*.



\*\*Repository:\*\* `siddharthajha26/Credit\_Risk\_Scoring\_MSME\_Project`



---



\## ⭐ Key Takeaways



\* Built and validated \*\*Probability of Default (PD) models\*\* for an MSME portfolio using Logistic Regression, Random Forest, and XGBoost

\* Applied \*\*regulatory-aligned model calibration and validation\*\* techniques (Gini, AUROC, KS, PSI, HHI, Anchor Point tests)

\* Demonstrated \*\*model interpretability\*\* using IV, SHAP, and scorecard concepts

\* Enhanced predictive robustness through \*\*ensemble / meta-learning frameworks\*\*

\* Designed the project with \*\*banking governance, auditability, and consulting use cases\*\* in mind



---



\## 🎯 Objectives



\* Prepare and clean MSME loan-level data for credit risk modeling

\* Engineer predictive and stable risk features

\* Train and evaluate PD models using \*\*Logistic Regression, Random Forest, and XGBoost\*\*

\* Build a \*\*scorecard-style PD model\*\*

\* Perform \*\*model calibration, validation, and performance monitoring\*\*

\* Combine model predictions using an \*\*ensemble / meta-learner\*\*



---



\## 🧱 Repository Structure (Master View)



This repository is organized as a \*\*progressive modeling pipeline\*\*, where each notebook builds on the previous one:



1\. \*\*Data Preparation \& EDA\*\* → `Part1\_Data\_prep\_clean.ipynb`

2\. \*\*Feature Engineering \& Baseline PD Modeling\*\* → `Part2\_DataSplit\_Feature\_Eng.ipynb`

3\. \*\*Advanced ML (XGBoost)\*\* → `Part3\_CRM\_MSME\_XGBOOST.ipynb`

4\. \*\*PD Calibration (Regulatory View)\*\* → `Part4\_LR\_Score\_Calib.ipynb`

5\. \*\*Model Validation \& Monitoring\*\* → `Part5\_Model\_Perform\_validation.ipynb`

6\. \*\*Ensemble / Meta-Learning\*\* → `Par



\### 📁 Dataset/



\* \*\*MSME\_data\_file.csv\*\* — Raw MSME loan dataset

\* \*\*MSME\_clean\_trasformed.csv\*\* — Cleaned \& feature-engineered dataset used for modeling

\* \*\*MSME\_Data\_Dictionary.xlsx\*\* — Feature definitions and descriptions



\### 📁 Python\_Code/ (Notebooks)



\#### \*\*Part1\_Data\_prep\_clean.ipynb\*\*



Covers end-to-end \*\*data preparation and exploratory analysis\*\*, including:



\* Data import and preliminary inspection

\* Missing value treatment

\* Concentration analysis and correlation checks

\* Outlier detection, skewness, kurtosis

\* Variability, stability, and risk distribution analysis

\* Standardization, normalization, and transformation techniques

\* Preprocessing for downstream modeling



---



\#### \*\*Part2\_DataSplit\_Feature\_Eng.ipynb\*\*



Focuses on \*\*feature engineering and baseline PD modeling\*\*:



\* Train-test split and stratified sampling

\* Validation set creation using cutoff-date logic

\* Handling correlated features

\* Information Value (IV) and Univariate Gini analysis

\* Hazard rate modeling for PD estimation

\* Logistic Regression PD modeling (train, test, validation)

\* Confusion matrix, AUROC, Gini, classification report

\* K-fold cross-validation (AUROC-based)

\* Feature importance via Random Forest

\* SHAP analysis (dependency plots, beeswarm plots) for interpretability



---



\#### \*\*Part3\_CRM\_MSME\_XGBOOST.ipynb\*\*



Demonstrates \*\*tree-based ML modeling\*\* using XGBoost:



\* PD prediction using gradient-boosted decision trees

\* Feature importance interpretation (Weight, Gain, Cover)

\* Hyperparameter tuning using Grid Search CV

\* Performance comparison against baseline models



---



\#### \*\*Part4\_LR\_Score\_Calib.ipynb\*\*



Dedicated to \*\*PD score calibration\*\* for Logistic Regression:



\* Calibration assessment using log loss

\* Probability diagnostics and calibration plots

\* Alignment of predicted PDs with observed portfolio default rates

\* Regulatory-relevant calibration concepts under \*\*Basel / IFRS 9\*\*



---



\#### \*\*Part5\_Model\_Perform\_validation.ipynb\*\*



Covers \*\*model validation and performance monitoring\*\*, including:



\* Gini, AUROC, ROC curve analysis

\* KS statistic and score separation

\* Confusion matrix and classification report on calibrated PDs

\* Data drift detection using \*\*PSI and SSI\*\*

\* Variance Inflation Factor (VIF) for multicollinearity

\* Portfolio concentration analysis using \*\*HHI and Adjusted HHI\*\*

\* Anchor Point testing (TTC to PIT PD comparison)

\* Statistical tests: \*\*Chi-Squared, Binomial, Adjusted Binomial\*\*



---



\#### \*\*Part6\_Meta\_Learn\_Ensemble.ipynb\*\*



Implements an \*\*ensemble (meta-learning) framework\*\*:



\* Combining predictions from multiple base models

\* Meta-learner construction

\* Performance comparison vs individual models

\* Improvement in ranking power and predictive robustness

\* Practical application of ensemble learning in regulated banking environments



---



\## 📊 Dataset – Brief Evaluation



\### MSME\_data\_file.csv (Raw)



\* Date fields in `dd.mm.yyyy` format

\* Mix of categorical (sector, purpose) and numerical financial ratios

\* Target variable: `default\_flag` (0/1)

\* Missing values and placeholders present



\### MSME\_clean\_trasformed.csv



\* Cleaned and feature-engineered modeling dataset

\* Includes capped variables, encodings (mean / WOE), and derived features

\* Used as the canonical dataset for training and evaluation



\### MSME\_Data\_Dictionary.xlsx



\* Definitions for original features

\* Should be extended to include engineered variables for full auditability



---



\## 📈 Outputs \& Artifacts (Expected)



\* Clean dataset: `Dataset/MSME\_clean\_trasformed.csv`

\* Trained models (recommended):



&nbsp; \* `artifacts/models/xgb\_model.pkl`

&nbsp; \* `artifacts/models/lr\_scorecard.pkl`

&nbsp; \* `artifacts/models/meta\_model.pkl`

\* Encoders \& mappings:



&nbsp; \* Mean / WOE encodings

&nbsp; \* Score bin-to-point mappings

\* Evaluation plots:



&nbsp; \* ROC curves, calibration plots, Gini over time



---



\## 📌 Key Model Metrics



\* Reported AUROC across models: \*\*~0.94 – 0.96\*\*

\* Corresponding Gini: \*\*~0.88 – 0.92\*\*



> ⚠️ Note: Some monthly Gini values reach 1.0 due to small sample sizes or separation effects and should be treated as \*\*indicative, not definitive\*\*.



---



\## ⚠️ Limitations \& Assumptions



\* Notebooks rely on in-memory state (`%store`) — convert to persisted artifacts for production use

\* Some PDs missing in validation sets (later imputed); root cause should be investigated

\* WOE / scorecard bins may be unstable for sparse segments; smoothing is recommended

\* Data leakage risk must be carefully reviewed for time-based splits



---



\## 🏦 Intended Audience



\* Credit Risk \& Model Validation Analysts

\* Risk Consulting Professionals

\* Banking \& Financial Analytics Recruiters

\* Aspiring Credit Risk Modelers building practical portfolios



---



\## 📘 Notes



This repository is part of a broader \*\*Credit Risk Modeling \& Validation portfolio\*\*, covering the full lifecycle from \*\*data preparation to advanced ensemble modeling\*\*.




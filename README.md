# Credit Risk Scoring — MSME Portfolio

The project demonstrates the use of Python data‑science tools to build credit scoring models (Probability of Default, scorecard, and ensemble) for an MSME portfolio.

Repository: siddharthajha26/Credit_Risk_Scoring_MSME_Project  
Primary artifacts: Jupyter notebooks (Python_Code/) and datasets (Dataset/)

Table of contents
- About
- Quick start
- Repository structure
- Dataset (files & brief evaluation)
- Python_Code notebooks (purpose & run order)
- Outputs & artifacts produced
- Key model metrics (summary)
- Limitations & assumptions
- Reproducibility & governance
- Suggested next steps
- Contributing, license & contact

About
This repository contains a step‑by‑step pipeline to prepare MSME loan data, engineer features, train/evaluate predictive models (XGBoost, logistic regression), produce a scorecard and combine predictions via an ensemble/meta‑learner. The work is organized as Jupyter notebooks to allow inspection of intermediate steps and results.

Quick start

1. Clone the repo
   git clone https://github.com/siddharthajha26/Credit_Risk_Scoring_MSME_Project.git

2. Create and activate an environment (recommended)
   - Using venv:
     python -m venv .venv
     source .venv/bin/activate    # macOS/Linux
     .venv\Scripts\activate       # Windows
   - Or using conda:
     conda create -n msme-scoring python=3.9
     conda activate msme-scoring

3. Install dependencies
   pip install -r requirements.txt
   (If requirements.txt is not present, install core libs:)
   pip install pandas numpy scikit-learn xgboost matplotlib seaborn openpyxl joblib

4. Place Dataset files in Dataset/ (required)
   - Dataset/MSME_data_file.csv
   - Dataset/MSME_Data_Dictionary.xlsx

5. Recommended run order (notebooks assume prior state or saved artifacts)
   - Python_Code/Part1_Data_prep_clean.ipynb
   - Python_Code/Part2_DataSplit_Feature_Eng.ipynb
   - Python_Code/Part3_CRM_MSME_XGBOOST.ipynb
   - Python_Code/Part5_LR_Score_Calib.ipynb
   - Python_Code/Part4_Model_Perform_validation.ipynb
   - Python_Code/Part6_Meta_Learn_Ensemble.ipynb

Repository structure (high level)
- Dataset/
  - MSME_data_file.csv — raw source CSV
  - MSME_clean_trasformed.csv — cleaned + engineered dataset (used in modeling)
  - MSME_Data_Dictionary.xlsx — column definitions and descriptions
- Python_Code/
  - Part1_Data_prep_clean.ipynb
  - Part2_DataSplit_Feature_Eng.ipynb
  - Part3_CRM_MSME_XGBOOST.ipynb
  - Part4_Model_Perform_validation.ipynb
  - Part5_LR_Score_Calib.ipynb
  - Part6_Meta_Learn_Ensemble.ipynb
  - .ipynb_checkpoints/
- (recommended) artifacts/ (not present — suggested)
  - models/, encoders/, score_maps/, figures/

Dataset — brief evaluation
- MSME_data_file.csv (raw)
  - Contains date fields in dd.mm.yyyy style, many categorical fields (sector, purpose), numeric financial ratios, and target default_flag (0/1).
  - Mixed formatting and placeholders found in some fields; missing values present.
- MSME_clean_trasformed.csv
  - Cleaned and feature-engineered dataset used in modeling. Includes many engineered columns (mean/WOE encodings, capped features).
  - Useful as the canonical dataset for model training and evaluation.
- MSME_Data_Dictionary.xlsx
  - Contains definitions for original features. Confirm it includes engineered/derived columns for full auditability.

Python_Code notebooks — purpose & notes
- Part1_Data_prep_clean.ipynb
  - Data ingestion, cleaning, date parsing, missing-value handling, initial transformations.
  - Recommendation: produce a column-wise data quality summary and save cleaned dataset (Dataset/MSME_clean_trasformed.csv).
- Part2_DataSplit_Feature_Eng.ipynb
  - Feature engineering (mean encoding, WOE, capping), feature selection, and creation of train/validation/test splits (time-aware splits included).
  - Recommendation: persist encoding maps and final feature list.
- Part3_CRM_MSME_XGBOOST.ipynb
  - Train XGBoost models, predict PD_xgboost, inspect feature importances.
  - Recommendation: add hyperparameter tuning and model artifact persistence.
- Part4_Model_Perform_validation.ipynb
  - Calculate ROC/AUC, Gini, KS, temporal Gini by month, and validation plots.
  - Recommendation: add calibration checks and sample-size guards on period metrics.
- Part5_LR_Score_Calib.ipynb
  - Build logistic regression scorecard, map PDs to scores with odds-to-score transformation and binning.
  - Recommendation: clip PDs, smooth WOE, merge sparse bins to avoid unstable scores.
- Part6_Meta_Learn_Ensemble.ipynb
  - Combine PDs via average, weighted-average, and stacking (meta-learner). Handles missing PDs via indicators and imputation.
  - Recommendation: persist meta model and OOF predictions for audit.

Outputs & artifacts (what to expect)
- Cleaned dataset: Dataset/MSME_clean_trasformed.csv
- Trained model artifacts: (recommended) artifacts/models/xgb_model.pkl, lr_scorecard.pkl, meta_model.pkl
- Encoders & mappings: (recommended) artifacts/encoders/mean_encoding_*.json, woe_maps.json
- Score mapping tables (bins -> score)
- Evaluation plots: ROC curves, calibration plots, Gini/month charts

Key model metrics (reported in notebooks)
- Reported AUCs across models ~ 0.94–0.96 (Gini ≈ 0.88–0.92) — promising but should be validated on holdout/back‑test.
- Note: some monthly Gini values reach 1.0 (likely due to small sample sizes or separation); treat these as indicative, not definitive.

Limitations & assumptions
- Notebooks use %store and in-memory state between parts — fragile for automation. Convert to saved artifacts for robust runs.
- Some PD predictions missing in validation sets (imputed later). Investigate root cause (feature mismatch or unseen categories).
- Scorecard bins and WOE calculations can be unstable for sparse cells; smoothing and minimum cell sizes are recommended.
- Data leakage risk must be assessed (ensure features are not derived using future information for a given split).

Reproducibility & governance (recommended)
- Add requirements.txt and optionally environment.yml
- Persist:
  - trained model files (joblib/pickle)
  - encoder mappings (JSON/pickle)
  - final feature list and schema (CSV/JSON)
  - a MODEL_REGISTRY or artifacts/ directory with versions and metadata
- Add a README cell at the top of each notebook describing inputs, outputs, and runtime.

Suggested next steps
1. Add requirements.txt and an environment specification.
2. Convert the pipeline to a master notebook or python scripts to run end-to-end.
3. Persist all encoders and model artifacts; add simple scoring script.
4. Add model governance docs (see MDD) and a CHANGELOG.
5. Add unit tests for key functions (scoring, gini calculation, master_scaling).

Contributing
- Open to pull requests. Please follow repo conventions and include tests for new code.

License
- No LICENSE file in repo currently. Add an appropriate license (e.g., MIT) if you wish to open-source.

Contact
- Repository owner: @siddharthajha26
- For questions or edits to these documents, reply here and I can prepare the files and optionally commit them to the repo with your approval.
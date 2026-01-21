# Model Development Document — MSME Credit Risk Scoring

Repository: siddharthajha26/Credit_Risk_Scoring_MSME_Project
Author/Owner: siddharthajha26
Date: 2026-01-21 (rev. 0)

## 1. Executive summary
- Purpose: Build and validate a credit risk scoring system (PD and scorecard) for MSME customers using historical lending/performance data.
- Models developed: Tree-based PD (XGBoost), Logistic Regression scorecard, and an ensemble meta-learner for final PD.
- Primary deliverables: trained models, score mapping, feature encoders, model performance metrics, validation reports.

## 2. Intended use & scope
- Use: Evaluate borrower default risk, support underwriting, portfolio monitoring, and decisioning.
- Scope: MSME customers in the dataset; not validated for other geographies/products without retraining.

## 3. Data sources & target
- Primary file: Dataset/MSME_data_file.csv
- Cleaned dataset: Dataset/MSME_clean_trasformed.csv (used to train and evaluate models)
- Data dictionary: Dataset/MSME_Data_Dictionary.xlsx
- Target: `default_flag` (binary; 1 = default, 0 = non-default)
- Time coverage: (refer to date column in dataset) — ensure temporal split uses observation date, not creation date.

## 4. Data quality & preprocessing
- Date parsing: original dd.mm.yyyy strings converted to pandas datetime.
- Missing values: identified and handled; categorical placeholders normalized.
- Outliers: numeric fields capped at the 0.01 and 0.99 quantiles (or via business rules) — implemented as column_name_capped_0.99 in cleaned dataset.
- Transformations: log transforms, ratio computations (e.g., saving_ratio, installment_as_income_perc), and financial ratios normalized.
- Recommendation: maintain a column-level data quality table: dtype, %missing, unique samples, imputations applied.

## 5. Feature engineering (summary)
- Mean encoding / target encoding for high-cardinality categorical variables (e.g., sector).
- WOE binning for scorecard features.
- Capped numeric features (e.g., sector_risk_capped_0.99).
- Binned features used for scorecard and bin-level summaries.
- Derived financial ratios: ROA, ROE, Net_Profit_Margin, EBITDA-related features.
- Missing indicators added for important features/predictions where applicable.

## 6. Feature selection
- Univariate Gini/AUC used to rank features.
- Correlation checks and VIF analysis to reduce multicollinearity.
- Business rationale included for retaining features with domain importance.
- Recommendation: persist final feature list as artifacts/feature_list_final.csv.

## 7. Modeling approach
- XGBoost
  - Purpose: produce a strong PD predictor; handles interactions and missingness.
  - Typical hyperparameters used in notebooks: n_estimators ~ 100–500, learning_rate ~ 0.01–0.1, max_depth ~ 3–8.
  - Recommendation: incorporate cross‑validation and hyperparameter tuning (RandomizedSearchCV or Optuna).
- Logistic Regression
  - Purpose: interpretable benchmark and scorecard conversion.
  - Built on WOE/linearized features for score stability and interpretability.
  - Use L2 regularization and check coefficients for sign stability.
- Ensemble / Stacking
  - Average & Weighted average: governance-driven weights.
  - Meta-learner: logistic regression trained on out-of-fold predictions (stacking). Include missingness indicators and median imputation for missing model outputs.

## 8. Model training & validation
- Data splits:
  - Time split for validation to simulate production (train on earlier period, validate on later period).
  - Holdout / test set reserved for final evaluation.
- Cross-validation:
  - Out-of-fold approach used for stacking; recommend K-fold with time-aware folds for temporal validation.
- Metrics:
  - Discrimination: ROC-AUC, Gini (Gini = 2*AUC - 1), KS
  - Calibration: reliability diagrams, Brier score (recommended)
  - Stability: monthly/periodic Gini, PSI (Population Stability Index)
- Observed performance:
  - AUC ~ 0.94–0.96 (noted in notebooks) — exercise caution and confirm with holdout/back-test.

## 9. Scorecard & calibration
- Score scaling:
  - Base formula used in notebooks: Score ≈ -ln(PD/(1-PD)) * factor + offset; PDO and Factor calculations used for scaling.
  - Typical practice: choose a reference point (e.g., Score = 600 at Odds = 50:1) and PDO (points to double odds).
- Binning:
  - Bins created by Sturges' rule in notebooks (approx. 10 bins). Ensure min observations per bin and merge sparse bins.
- Stability:
  - Clip PDs to [eps, 1-eps] before log-odds to avoid extreme scores.
  - Add smoothing to WOE to avoid infinite values when 0 events occur.

## 10. Deployment & scoring
- Scoring pipeline requirements:
  - Apply same data cleaning, capping, and encoders (mean/WOE maps).
  - Align features before model.predict_proba.
- Artifacts to deploy:
  - artifacts/models/xgboost_model.pkl
  - artifacts/models/lr_scorecard.pkl
  - artifacts/encoders/mean_encode_maps.json
  - artifacts/encoders/woe_maps.json
  - scoring script: scripts/score_msme.py (recommended)
- Real-time vs batch:
  - Prefer batch scoring for portfolio monitoring; support real-time scoring if encoders and pipelines are available as APIs.

## 11. Monitoring & maintenance
- Metrics to monitor monthly/quarterly:
  - AUC/Gini, KS, Brier score, PSI per feature/score, population composition changes.
- Trigger thresholds:
  - PSI > 0.25 (suggests distribution shift), drop in AUC by > 0.03 vs baseline, sustained calibration drift.
- Retraining cadence:
  - Periodic: every 6–12 months or triggered by monitoring alerts based on data drift or performance degradation.

## 12. Risks & mitigations
- Risk: Data leakage — mitigate by confirming no forward-looking features and using strict temporal splits.
- Risk: Small-sample instability in monthly metrics — mitigate with minimum sample-size filters and confidence intervals.
- Risk: Unseen categories at scoring — store and use fallback encodings and document behavior.
- Risk: Extreme score values from zero-event bins — mitigate with smoothing, clipping, and minimum cell sizes.

## 13. Governance & audit artifacts (what to store)
- datasets/: raw and cleaned CSVs, data dictionary
- artifacts/models/: model pickles with metadata (training dates, hyperparams, versions)
- artifacts/encoders/: mean/WOE mapping files
- artifacts/reports/: evaluation plots and CSV metrics
- docs/MODEL_DEVELOPMENT_DOCUMENT.md (this document)
- CHANGELOG.md with model version history

## 14. Reproducibility checklist
- [ ] requirements.txt / environment.yml present
- [ ] Fixed random seed specified (e.g., seed=42) for all model training steps
- [ ] All intermediate artifacts saved to disk (no reliance on %store only)
- [ ] Final feature list saved
- [ ] All encoders and model artifacts saved with metadata (training date, sample size, AUC)
- [ ] Notebook cell at top documenting inputs and outputs

## 15. Appendix
- File references:
  - Dataset/MSME_data_file.csv (raw)
  - Dataset/MSME_clean_trasformed.csv (cleaned)
  - Dataset/MSME_Data_Dictionary.xlsx (dictionary)
  - Python_Code/Part1..Part6 ipynb files
- Suggested file names for artifacts:
  - artifacts/models/xgb_v1_YYYYMMDD.pkl
  - artifacts/models/lr_scorecard_v1_YYYYMMDD.pkl
  - artifacts/encoders/mean_enc_map_v1.json
  - artifacts/score_map/scorecard_table_v1.csv
- Example hyperparameter suggestions:
  - XGBoost: n_estimators=200, learning_rate=0.05, max_depth=6, subsample=0.8, colsample_bytree=0.8, random_state=42
  - LR (meta): C=1.0, penalty='l2', solver='liblinear' (tune if needed)
- Contact / authorship
  - Owner: siddharthajha26
  - For questions and follow-ups: open an issue or contact the owner via GitHub


---

## 🧩 Step-by-Step Workflow

### 🟦 Member 1 — Data Understanding & Cleaning
- Loaded 4 raw CSVs: Beneficiary, Inpatient, Outpatient, Labels  
- Checked data types, missing values, and duplicates  
- Fixed inconsistencies (e.g., `RenalDiseaseIndicator` Y/N → 1/0)  
- Exported cleaned versions for later use  
- Deliverables:
  - `cleaned_beneficiary.csv`, `cleaned_inpatient.csv`, `cleaned_outpatient.csv`, `cleaned_labels.csv`
  - Notebook: `01_data_exploration_and_feature_engineering.ipynb`

---

### 🟩 Member 2 — Feature Engineering
- Merged all datasets using `BeneID` and `Provider`
- Aggregated claim-level to provider-level
- Created over 50 features:
  - total_claims, avg_inpatient_cost, chronic condition ratios, reimbursement sums, physician diversity, inpatient/outpatient ratios
- Final dataset: `provider_level_features.csv`
- Notebook: `02_modeling.ipynb`

---

### 🟨 Member 3 — Modeling & Imbalance Handling
- Split dataset (train/test = 80/20)
- Fraud ratio ≈ **9.3%** (high imbalance)
- Applied **SMOTE** oversampling to balance classes  
- Trained multiple models:
  - Logistic Regression (baseline)
  - Decision Tree
  - Gradient Boosting
- Evaluated on ROC-AUC, PR-AUC, Precision, Recall, F1-score  

#### 🔢 Model Comparison

| Model | ROC-AUC | PR-AUC | Recall (Fraud) | Precision (Fraud) | F1 (Fraud) |
|--------|----------|--------|----------------|--------------------|-------------|
| Logistic Regression | 0.9495 | 0.7328 | 0.8811 | 0.4045 | 0.5545 |
| Decision Tree | 0.7727 | 0.3679 | 0.5940 | 0.5555 | 0.5741 |
| Gradient Boosting | 0.9533 | 0.7362 | 0.5544 | 0.7000 | 0.6187 |
| Random Forest | 0.9453 | 0.7216 | 0.5346 | 0.7013 | 0.6067 |

---

### 🟧 Member 4 — Evaluation, Error Analysis & Reporting

#### ✅ Confusion Matrix (Gradient Boosting)
|        | Predicted No | Predicted Yes |
|--------|---------------|---------------|
| **Actual No** | 957 | 24 |
| **Actual Yes** | 45 | 56 |

- **True Negatives (TN):** 957  
- **False Positives (FP):** 24  
- **False Negatives (FN):** 45  
- **True Positives (TP):** 56  

#### ✅ Error Analysis
- **False Positives:** High outpatient volume, high reimbursement, legitimate providers misclassified as fraud.  
- **False Negatives:** Small claims, low volume fraud, subtle patterns missed.  
- **Impact:** FP wastes investigation cost; FN misses true fraud.  
- **Recommendation:** Adjust thresholds or add temporal features.

#### ✅ Visualization Summary
- ROC Curves and Precision–Recall Curves for 3 models  
- Gradient Boosting had highest PR-AUC = **0.736**, meaning better fraud ranking ability  
- PR-AUC is preferred over accuracy due to imbalance.

---

## 💡 Business Insight
- Gradient Boosting model can flag **high-risk providers** for further manual audit.
- PR-AUC > 0.70 indicates reliable fraud prioritization.
- With optimized thresholds, system can **reduce manual audit load by ~50%** while maintaining fraud recall.

---

## 🧠 Future Work
- Add time-based claim frequency patterns  
- Introduce cost-sensitive learning  
- Implement SHAP explainability for model transparency  
- Expand dataset with provider specialties and regional data  

---

## ⚙️ Reproduction Instructions

1. Clone repository:
   ```bash
   git clone https://github.com/mahmoudamr25/fraud_detection_project.git
   cd fraud_detection_project

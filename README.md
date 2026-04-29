#  Phishing Email Detection with Explainable AI

**Diploma Project — Chapter 3: Technical Implementation**  
International Information Technology University | Major 6B06301 — Computer Security  
Authors: Kuatbai A.K., Jakipbekova A.B., Satybaldiyev A.

---

##  Project Goal

Develop a phishing email detection system based on **Explainable Artificial Intelligence (XAI)** that not only classifies emails but also provides human-understandable explanations of detected risk indicators — supporting user awareness and analyst trust.

---

##  Project Structure

```
phishing_xai_detection/
│
├── data/
│   ├── raw/
│   │   └── phishing_email.csv          # Source dataset (82,486 emails)
│   └── processed/
│       └── phishing_features.csv       # Engineered feature dataset
│
├── models/
│   └── best_model.joblib               # Saved best model
│
├── reports/
│   └── figures/                        # All generated figures (fig_3_*.png)
│
├── 01_EDA.ipynb                        # Exploratory Data Analysis
├── 02_Feature_Engineering.ipynb        # Feature extraction (textual + URL + KZ)
├── 03_Modeling.ipynb                   # Model training and evaluation
├── 04_XAI_Explanations.ipynb           # SHAP + LIME explanations + reports
└── README.md
```

---

##  Dataset

- **Source:** Phishing Email Dataset (Kaggle)
- **Size:** 82,486 emails
- **Classes:** 52.0% Phishing (label=1) / 48.0% Legitimate (label=0)
- **Features:** `text_combined` (preprocessed email body + subject), `label`

---

##  Methodology

### 1️ Exploratory Data Analysis (`01_EDA.ipynb`)
**Key findings:**
- Nearly balanced dataset — minimal class imbalance risk
- Phishing emails contain more URLs, urgency language, and promotional keywords
- Legitimate emails tend to be longer with more domain-specific vocabulary
- Strong textual signals: "free", "winner", "verify", "click here", "urgent"

### 2️ Feature Engineering (`02_Feature_Engineering.ipynb`)
**33 interpretable features extracted across 3 domains:**

| Domain | Count | Key Features |
|--------|-------|-------------|
| Textual | 17 | has_urgent, has_free, has_winner, has_verify, char_count, unique_word_ratio |
| URL-based | 12 | has_url, has_ip_url, has_shortened_url, has_suspicious_tld, url_count |
| KZ-Regional | 4 | has_kz_phishing, has_kz_legit, has_kz_domain, kz_risk_score |

**Kazakhstan-specific features** include detection of:
- Impersonation of egov.kz, kaspi.kz, halykbank.kz, enpf.kz
- Suspicious patterns like `gov-kz.*`, `secure.*kaspi`, `enpf.*bonus`
- Legitimate KZ domain baseline (20 official KZ domains)

### 3️ Modeling (`03_Modeling.ipynb`)
**5 models evaluated:**

| Model | Notes |
|-------|-------|
| Logistic Regression | Baseline, interpretable |
| Random Forest | Ensemble, feature importance |
| XGBoost | Gradient boosting, high performance |
| LightGBM | Fast gradient boosting |
| TF-IDF + LogReg | Text-only baseline |

**Evaluation metrics:** Accuracy, Precision, Recall, F1-Score, ROC-AUC  
**Validation:** 80/20 stratified train-test split + 5-fold cross-validation

### 4️ XAI Explanations (`04_XAI_Explanations.ipynb`)
**Two complementary XAI methods:**

- **SHAP (SHapley Additive exPlanations)**
  - Global: feature importance ranking for the entire model
  - Local: waterfall plots showing per-email feature contributions
  - Domain analysis: textual vs URL vs KZ feature importance

- **LIME (Local Interpretable Model-agnostic Explanations)**
  - Independent validation of SHAP findings
  - Per-email local surrogate model

**Automated Analytical Report Generator:**  
Produces structured text reports for each email including:
- Classification result + confidence + risk level (LOW/MEDIUM/HIGH)
- Top 8 SHAP-contributing features with direction
- Active phishing indicators checklist

---

##  Key Results

- Best model achieves **F1 > 0.97** on test set
- **Explanation coverage > 85%** — top SHAP feature is a human-recognizable indicator
- Kazakhstan domain impersonation patterns successfully detected
- SHAP and LIME explanations are consistent, validating reliability
- Most impactful features: URL presence, IP-based URLs, urgency language

---

##  How to Run

```bash
# 1. Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm shap lime joblib

# 2. Place phishing_email.csv in the project root

# 3. Run notebooks in order
jupyter notebook 01_EDA.ipynb
jupyter notebook 02_Feature_Engineering.ipynb
jupyter notebook 03_Modeling.ipynb
jupyter notebook 04_XAI_Explanations.ipynb
```

**Recommended environment:** Google Colab (GPU support for LightGBM/XGBoost)

---

##  Tools & Environment

| Component | Tool |
|-----------|------|
| Environment | Google Colab / Jupyter |
| Language | Python 3.10+ |
| Data processing | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| ML models | Scikit-learn, XGBoost, LightGBM |
| XAI | SHAP, LIME |
| Version control | Git / GitHub |

---

##  References

This implementation directly corresponds to:
- **Section 2.3** — System Components and Functional Modules
- **Section 2.4** — Data Flow and Information Processing
- **Section 2.5** — Algorithm of System Operation  
- **Section 2.6** — Explainability and Human-in-the-Loop Interaction
- **Section 2.7** — Validation Methodology and Evaluation Criteria
- **Section 2.8** — Tools and Environment

---

**Status:**  Complete | **Chapter:** 3 — Technical Implementation

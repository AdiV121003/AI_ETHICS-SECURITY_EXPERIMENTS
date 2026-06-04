# Credit Risk Prediction via Logistic Regression
### AI Ethics & Security Experiments — Experiment 2

This project applies Logistic Regression to the German Credit Dataset to predict 
credit risk (good/bad borrowers) and analyse the model from ethical and security 
perspectives. It is the second experiment in the 
[AI Ethics & Security Experiments](https://github.com/AdiV121003/AI_ETHICS-SECURITY_EXPERIMENTS)) series.

<img width="2752" height="1536" alt="Gemini_Generated_Image_ws7sriws7sriws7s" src="https://github.com/user-attachments/assets/b86c35aa-200e-42ac-9466-0c6b3e96c89a" />

---

## 🎯 Objectives
- Understand logistic regression in a real-world fintech context
- Handle class imbalance via SMOTE and evaluate its impact
- Analyse model coefficients for bias and fairness concerns
- Explore the interpretability vs performance trade-off in credit scoring

---

## 📂 Dataset
**German Credit Dataset** via KaggleHub (`jumpingdino/german-credit-dataset`)
- 1,000 records, 21 features
- Target: `good` (700) / `bad` (300) — 70:30 class imbalance
- Features include financial behavior, demographics, and loan purpose

---

## 🔬 Experiment Steps
| Step | Description |
|---|---|
| 1 | Data Loading |
| 2 | Exploratory Data Analysis |
| 3 | Data Preprocessing |
| 4 | Baseline Logistic Regression |
| 5 | Class Imbalance Handling via SMOTE |
| 6 | Ethical & Security Analysis |

---

## 📊 Key Results

| Metric | Baseline | Post-SMOTE |
|---|---|---|
| Accuracy | 0.69 | 0.68 |
| Bad Credit Recall | 0.43 | 0.50 |
| Good Credit Recall | 0.79 | 0.75 |
| AUC-ROC | 0.75 | ~0.74 |

---

## 🔍 Key Findings

**Performance:**
Logistic regression achieves acceptable AUC (0.75) but critically misses 50%+ 
of bad credit customers even after SMOTE — a direct consequence of its linear 
decision boundary being unable to capture complex feature interactions.

**Security:**
- High-magnitude coefficients on sensitive features represent adversarial surfaces
- If model behaviour is exposed via model inversion or membership inference attacks,
  protected attributes could be manipulated to systematically disadvantage specific 
  applicant groups

**Core Trade-off:**
Interpretability without sufficient performance produces explainable failure, not 
trustworthy decisions. Logistic regression makes its biases visible — but visibility 
only helps if someone is actively auditing.

---
## 🔎 Disparate Impact Analysis

| Group | Test Samples | Baseline Accuracy | Post-SMOTE Accuracy |
|---|---|---|---|
| Foreign Workers | 191 | 68.1% | 65.4% |
| Non-Foreign Workers | 9 | 88.9% | 88.9% |


**~20 percentage point accuracy gap persists across both models.**

Key findings from error analysis:

- SMOTE reduced `is_foreign_worker` coefficient by 55% (-0.857 → -0.386)
  but did not close the accuracy gap — coefficient improvement ≠ 
  fairness improvement
- Non-foreign worker comparison group (n=9) is statistically unreliable,
  itself a finding — 96.3% of the dataset is foreign workers, making 
  `is_foreign_worker` a near-zero variance feature with no legitimate 
  predictive value
- A legally protected characteristic with near-zero variance carrying 
  a non-trivial negative coefficient represents both a fairness violation 
  and an adversarial surface
- False positive rate for foreign workers (18.3%) vs non-foreign workers 
  (11.1%) suggests creditworthy foreign applicants face higher wrongful 
  rejection rates
- Unlike is_foreign_worker (96.3% single value), purpose_car has sufficient variance (23.4% positive)
  making its coefficient a genuine learned signal, not a statistical artifact. However,
  whether car loan purpose legitimately predicts credit risk or reflects historical lending
  bias warrants further investigation.

## 📎 Related
- 📝 Blog Post (TBA)
- 🔗 [Experiment 1 — Decision Trees for Loan Approval](https://medium.com/ai-ethics-and-security-experiments/is-your-ai-powered-loan-approval-model-fair-and-secure-e68ab859d34c)

---

## ⚠️ Disclaimer
This project is for educational purposes. The German Credit Dataset contains 
demographic features (nationality, gender) that would be legally impermissible 
in real credit scoring systems in many jurisdictions. Their inclusion here is 
purely for demonstrating algorithmic bias detection.

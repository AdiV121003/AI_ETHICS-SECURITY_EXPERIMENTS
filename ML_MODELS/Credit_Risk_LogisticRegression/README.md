# Credit Risk Scoring Audit (Fairness & Security Analysis)

## Project Overview
This project analyzes a credit risk scoring model from a **Responsible 
AI and AI Security perspective**.

Instead of focusing only on accuracy, the goal is to understand:
* Whether a highly interpretable model is sufficient for high-stakes 
  financial decisions
* How class imbalance affects both performance and fairness
* Whether technical fixes like SMOTE translate into real-world 
  fairness improvements
* Which features carry ethically or legally problematic signals

The model used in this project is a **Logistic Regression classifier** 
trained on the German Credit Dataset.

---

## Project Objectives
* Train a credit risk scoring model using Logistic Regression
* Handle class imbalance via **SMOTE** and evaluate its impact
* Perform **coefficient analysis** for model interpretability
* Evaluate **fairness across demographic groups**
* Investigate **legally sensitive features** and their influence 
  on predictions
* Explore the **interpretability vs performance trade-off** in 
  regulated AI systems

This project is part of my **AI Ethics & Security Experiments** series 
exploring Responsible AI and AI Security in real-world domains.

---

## Model Summary

| | Baseline Model | Post-SMOTE Model |
|---|---|---|
| Model | Logistic Regression | Logistic Regression |
| Accuracy | 0.69 | 0.68 |
| Bad Credit Recall | 0.43 | 0.50 |
| Good Credit Recall | 0.79 | 0.75 |
| AUC-ROC | 0.75 | ~0.74 |

Key features influencing predictions (baseline):
* `status_account_no checking account` — strongest positive predictor
* `is_foreign_worker_yes` — strongest negative predictor (baseline)
* `purpose_car (new)` — strongest negative predictor (post-SMOTE)
* `secondary_obligor_guarantor` — strong positive predictor
* `status_savings_< 100 DM` — negative predictor

---

## Fairness Analysis

Fairness was evaluated through **coefficient analysis** and **disparate 
impact testing** across foreign worker status.

### Coefficient Analysis
Comparing baseline vs post-SMOTE coefficients revealed:
* `is_foreign_worker_yes` carried a coefficient of **-0.857** in the 
  baseline model, decreasing odds of good credit prediction by 57.6%
* `purpose_car (new)` coefficient amplified from -0.780 to -1.862 
  after SMOTE — model shifted reliance from demographic to financial 
  behavior features

### Disparate Impact Analysis
Error analysis grouped by foreign worker status revealed:
* Non-foreign workers: **88.9%** prediction accuracy
* Foreign workers: **65-68%** prediction accuracy
* A **~20 percentage point accuracy gap** persisting across both models

### Critical Finding: Near-Zero Variance Problem
* 963/1000 samples (96.3%) are foreign workers
* `is_foreign_worker` has near-zero variance — making its coefficient 
  a statistical artifact rather than genuine learned signal
* The feature is legally impermissible in credit decisions under EU 
  anti-discrimination frameworks regardless of its statistical impact
* I should have removed it during feature selection — its presence 
  represents a data governance failure at ingestion stage

### Two Types of Problematic Coefficients

| | is_foreign_worker | purpose_car (new) |
|---|---|---|
| Variance | Near-zero (96.3%) | Sufficient (77/23) |
| Coefficient type | Statistical artifact | Genuine learned signal |
| Legal status | Protected characteristic | Neutral feature |
| Accuracy gap | ~20 points | Negligible |
| Fix | Remove before training | Investigate bias source |

---

## Security Analysis

### Adversarial Surface via Sensitive Coefficients
* High-magnitude coefficients on sensitive features represent 
  exploitable vulnerabilities
* If model behavior is exposed through **model inversion** or 
  **membership inference attacks**, an adversary could identify 
  which features most influence predictions
* `is_foreign_worker` coefficient of -0.857 could be manipulated 
  to systematically disadvantage specific applicant groups
* Removing protected characteristics before training reduces both 
  legal liability and adversarial attack surface simultaneously

---

## Key Insights
* Logistic regression achieves acceptable AUC (0.75) but critically 
  misses 57% of bad credit customers. This is due to its 
  linear decision boundary
* SMOTE improved minority class recall marginally but didn't improve
   accuracy gap for foreign workers — **coefficient 
  improvement ≠ fairness improvement**
* `is_foreign_worker` is simultaneously a legal violation, a 
  statistical artifact, and an adversarial surface — yet would have 
  shipped undetected without post-hoc auditing
* Interpretability made every bias visible — but visibility only 
  helps if someone is actively auditing
* Fairness auditing must begin at data ingestion, not after modeling

---

## What I Would Do Differently
* Conduct a **fairness-aware feature audit** before modeling by
  removing protected characteristics and near-zero variance features 
* Remove `is_foreign_worker` and `status_and_sex` before training
* Try `class_weight='balanced'` before SMOTE for logistic regression 
  specifically
* Treat fairness as a mandatory checkpoint at every pipeline stage, 
  not a post-hoc audit

---

## Future Work
* Retrain without protected characteristics and compare results
* Apply Random Forest to the same dataset — same ethics lens, 
  higher performance, less transparency (Experiment 3)
* Test class_weight='balanced' and compare with SMOTE results

---

## Related
* 📝 [Blog Post]([link](https://medium.com/ai-ethics-and-security-experiments/when-xai-isnt-enough-logistic-regression-credit-risk-and-the-limits-of-simplicity-37e509a68c05))
* 🔗 [Experiment 1 — Decision Tree Loan Approval Audit]([link](https://github.com/AdiV121003/AI_ETHICS-SECURITY_EXPERIMENTS/tree/main/ML_MODELS/DecisionTree_LoanApproval_Audit))

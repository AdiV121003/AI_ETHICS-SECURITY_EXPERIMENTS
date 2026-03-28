# Loan Approval Model Audit (Fairness & Security Analysis)

## Project Overview

This project analyzes a loan approval prediction model from a **Responsible AI and AI Security perspective**.
Instead of focusing only on accuracy, the goal is to understand:

* Which features influence the model the most
* Whether the model behaves fairly across different groups
* How robust the model is to input manipulation

The model used in this project is a **Decision Tree classifier** trained on a loan dataset.


## Project Objectives

* Train a loan approval prediction model
* Perform **feature importance analysis**
* Evaluate **fairness across various groups**
* Test **security risks and model vulnerabilities**

This project is part of my practice work while exploring the **AI + Cybersecurity (AI Security / Responsible AI)** field.


## Model Summary

Model used: Decision Tree
Accuracy: ~0.88

Key features influencing predictions:

* Previous loan defaults
* Loan interest rate
* Loan percent of income
* Income
* Home ownership status

The feature `previous_loan_defaults_on_file` had a very high influence on predictions.

## Fairness Analysis

Fairness was evaluated across multiple groups:

* Age groups
* Gender
* Education level

Findings:

* Approval rates were similar across gender groups.
* Education levels showed small variations in approval rate.
* The **50–80 age group had a slightly higher approval rate** compared to other groups.
* Overall, no severe bias was observed, but group differences were observed

## Security & Robustness Analysis

This project also explores potential **AI security risks** in the model. I conducted educational experiments to test the potential vulnerabilities in the model. 

### Feature Manipulation Test

Here, I simulated manipulation of a key feature:

Previous loan default history was flipped in test samples.

Result:

* Predictions changed in **49% of tested cases**

This indicates the model is **sensitive to this feature**, which could be a potential vulnerability if data is manipulated.

### Feature Dependency Test

The most important feature was removed and the model was retrained.

Result:

* Accuracy remained similar (~0.89)

This suggests that while the feature strongly influences individual predictions, the model can still rely on other signals.

### Input Sensitivity Test

Small changes were applied to numerical features such as income and interest rate.

Result:

* The predictions of the decision tree remained stable unless values crossed decision thresholds, which is expected behavior for decision trees.

---

## Key Insights

* The model performs well in terms of accuracy.
* Some features have strong influence on predictions.
* The model shows moderate sensitivity to manipulated inputs.
* Fairness differences across groups exist but are not extreme.
* Security testing helps identify potential risks before deployment.

---

## Future Improvements

I want to try and: 

* Compare with Random Forest or other ensemble models
* Apply fairness-aware ML techniques
* Add adversarial testing
* Implement model monitoring strategies

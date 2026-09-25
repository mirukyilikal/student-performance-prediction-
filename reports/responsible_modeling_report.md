# Responsible AI and Limitations Report

## Purpose

This project explores whether machine learning can identify patterns associated with student mathematics performance and academic failure risk.

The model is intended for educational research and portfolio purposes only. It is not designed to make automated decisions about individual students.

---

## Prediction Timing

Two prediction settings were evaluated:

### Early-Warning Scenario

Grades `G1` and `G2` were excluded.

### Later-Term Scenario

Grades `G1` and `G2` were included.

Model performance improved substantially when these grades became available, demonstrating the importance of prediction timing.

---

## Key Results

### Early-Warning Regression

* MAE: 2.989
* RMSE: 3.760
* R²: 0.310

### Later-Term Regression

* MAE: 1.187
* RMSE: 1.995
* R²: 0.806

### Early-Warning Classification (5-Fold CV)

* Accuracy: 0.693 ± 0.039
* ROC-AUC: 0.655 ± 0.055
* Fail Recall: 0.232 ± 0.081
* Fail F1: 0.325 ± 0.085

The model identified only a limited proportion of students who actually failed.

---

## Fairness Analysis

Fail Recall was evaluated across:

* Sex
* School
* Residential Address

| Group Type | Group  | Mean Fail Recall |
| ---------- | ------ | ---------------- |
| Sex        | Female | 0.190            |
| Sex        | Male   | 0.310            |
| School     | GP     | 0.226            |
| School     | MS     | 0.283            |
| Address    | Rural  | 0.237            |
| Address    | Urban  | 0.228            |

These differences should be interpreted cautiously because subgroup sizes are small and results vary across cross-validation folds.

---

## Limitations

* Dataset contains only 395 students.
* Data comes from a specific educational context in Portugal.
* Results may not generalize to other countries or schools.
* Feature importance does not imply causation.
* Fairness conclusions are limited by subgroup sample sizes.
* Student data may contain sensitive information.

---

## Appropriate Use

The model may be considered as a supporting signal for educational analysis.

Predictions should always be reviewed by humans and supplemented with additional context.

---

## Inappropriate Use

The model should not be used to:

* Automatically label students as failures
* Deny educational opportunities
* Make admission decisions
* Determine scholarships automatically
* Replace teachers or counselors
* Make high-stakes decisions without human oversight

---

## Final Statement

This project demonstrates that machine learning can identify predictive patterns in student performance data. However, predictive accuracy alone is not sufficient for real-world deployment.

The model should be treated as an educational research artifact and decision-support experiment rather than an automated decision-making system.

# Student Performance Prediction: Accurate, Fair, and Responsible Machine Learning

## Project Overview

This project develops and evaluates machine learning models for predicting student mathematics performance using the **UCI Student Performance dataset**.

The project goes beyond model accuracy by examining:

* Exploratory data analysis
* Regression for final grade prediction
* Classification for Pass/Fail prediction
* Early-warning vs. later-term prediction
* Cross-validation
* Feature importance and permutation importance
* Subgroup fairness
* Responsible AI
* Privacy and deployment limitations

The central question is not simply:

> **"Can machine learning predict student performance?"**

It is also:

> **"How accurate, reliable, fair, interpretable, and responsible is the prediction?"**

---

## Objectives

The project investigates five main questions:

1. How accurately can student characteristics and academic history predict the final mathematics grade (`G3`)?
2. How well can an early-warning model identify students who ultimately fail?
3. How does prediction performance change when previous grades (`G1` and `G2`) become available?
4. Which variables contribute most to model predictions?
5. Do model errors and Fail Recall differ across selected student groups?

---

## Dataset

The project uses the **UCI Student Performance dataset**, specifically the mathematics dataset:

```text
student-mat.csv
```

The dataset contains:

* **395 students**
* **33 original variables**
* Student demographic information
* Family and socioeconomic variables
* School-related variables
* Study habits
* Previous academic performance
* Absences
* Final mathematics grade (`G3`)

The final grade ranges from **0 to 20**.

### Classification Target

For the classification task, the project defines:

```text
G3 >= 10 → Pass
G3 < 10  → Fail
```

This threshold is a project-level modeling assumption.

---

# Project Structure

```text
student-performance-prediction/
│
├── README.md
├── LICENSE
├── requirements.txt
│
├── data/
│   └── README.md
│
├── notebooks/
│   └── student_performance_prediction.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── modeling.py
│   └── evaluation.py
│
├── models/
│   └── README.md
│
├── reports/
│   └── responsible_modeling_report.md
│
└── figures/
    ├── grade_distribution.png
    ├── pass_fail_distribution.png
    ├── correlation_matrix.png
    ├── feature_importance.png
    ├── permutation_importance.png
    └── fairness_analysis.png
```

---

# Methodology

## 1. Exploratory Data Analysis

The analysis examined:

* Final-grade distribution
* Pass/Fail distribution
* Correlations between numerical variables
* Previous failures
* Study time
* Absences
* Parental education
* School support
* Family support
* Internet access
* Family relationships
* Other student characteristics

EDA findings were treated as **descriptive associations rather than causal relationships**.

---

## 2. Feature Engineering

Two prediction scenarios were created.

### Early-Warning Scenario

`G1` and `G2` were excluded.

This represents a prediction setting where later-period grades are not yet available.

### Later-Term Scenario

`G1` and `G2` were included.

These variables represent previous mathematics grades that may be available later in the academic period.

This distinction is important because the information available at prediction time strongly affects model performance.

---

## 3. Preprocessing

The preprocessing pipeline includes:

* Numerical feature scaling using `StandardScaler`
* Categorical feature encoding using `OneHotEncoder`
* `handle_unknown="ignore"` for unseen categorical values
* Scikit-learn `Pipeline`
* Scikit-learn `ColumnTransformer`

Using a pipeline helps ensure that preprocessing is performed consistently during model training and evaluation.

---

# Machine Learning Models

The project evaluates:

### Regression

* Linear Regression
* Random Forest Regressor

### Classification

* Logistic Regression
* Decision Tree
* Random Forest Classifier

The Random Forest model was used for the main early-warning and later-term comparisons.

---

# Results

## Regression Results

| Scenario             | Model         |   MAE |  RMSE |    R² |
| -------------------- | ------------- | ----: | ----: | ----: |
| Early Warning        | Random Forest | 2.989 | 3.760 | 0.310 |
| Later Term (G1 + G2) | Random Forest | 1.187 | 1.995 | 0.806 |

The later-term model performed substantially better when `G1` and `G2` were available.

This demonstrates the importance of **prediction timing** and available information.

---

## Classification Results

| Scenario                  | Model         | Accuracy | ROC-AUC | Fail Recall | Fail F1 |
| ------------------------- | ------------- | -------: | ------: | ----------: | ------: |
| Early Warning             | Random Forest |    0.658 |   0.622 |       0.192 |   0.270 |
| Later Term (G1 + G2)      | Random Forest |    0.873 |   0.935 |       0.920 |   0.830 |
| Early Warning — 5-Fold CV | Random Forest |    0.693 |   0.655 |       0.232 |   0.325 |

The **5-fold cross-validation result is the primary evaluation for the early-warning risk model**.

The early-warning model's average Fail Recall was:

```text
0.232 ± 0.081
```

This means that the model identified only a limited proportion of students who actually failed.

---

# Why Fail Recall Matters

The classification problem was evaluated from a risk-oriented perspective:

```text
1 = Fail
0 = Pass
```

Fail Recall measures:

> Of the students who actually failed, how many did the model identify?

The cross-validation result:

```text
Fail Recall = 0.232 ± 0.081
```

indicates that the model detected approximately 23% of actual Fail cases on average in this evaluation.

Therefore, the model should **not** be treated as a standalone early-warning system.

---

# Feature Importance

The Random Forest model identified several variables as important for its predictions.

Important variables included:

* `failures`
* `absences`
* `goout`
* `age`
* `Fedu`
* `health`
* `Medu`
* `freetime`
* `famrel`
* `Walc`
* `studytime`

Permutation importance particularly highlighted:

```text
failures
```

as an important predictive variable.

### Important interpretation

Feature importance does **not** establish causation.

For example:

> A variable being important to the model does not mean that changing that variable would directly cause a change in academic performance.

The results describe **predictive patterns**, not causal effects.

---

# Fairness Analysis

Fairness was examined using **Fail Recall** across:

* Sex
* School
* Residential address

## Sex

| Group  | Mean Fail Recall |   Std |
| ------ | ---------------: | ----: |
| Female |            0.190 | 0.097 |
| Male   |            0.310 | 0.149 |

## School

| Group | Mean Fail Recall |   Std |
| ----- | ---------------: | ----: |
| GP    |            0.226 | 0.116 |
| MS    |            0.283 | 0.183 |

## Residential Address

| Group | Mean Fail Recall |   Std |
| ----- | ---------------: | ----: |
| Rural |            0.237 | 0.070 |
| Urban |            0.228 | 0.131 |

### Fairness interpretation

The model's Fail Recall differed across some groups.

However, subgroup performance also varied across cross-validation folds, and some groups contained relatively few actual Fail cases.

Therefore, these results should be interpreted as **preliminary fairness signals**, not evidence of systematic discrimination.

A larger and more representative dataset would be necessary for stronger fairness conclusions.

---

# Responsible AI

This project treats student prediction as a **decision-support research problem**, not an automated decision-making problem.

The model should not be used to:

* Automatically label students as failures
* Punish students
* Deny educational opportunities
* Make automatic admission decisions
* Automatically determine scholarships
* Make disciplinary decisions
* Replace teachers or counselors
* Make high-stakes decisions without human review

A responsible conceptual workflow would be:

```text
Student Information
        ↓
Machine Learning Model
        ↓
Risk Signal
        ↓
Human Review
        ↓
Additional Context
        ↓
Appropriate Support
```

The prediction should be treated as **one supporting signal, not a final verdict**.

---

# Limitations

## Dataset Size

The dataset contains only 395 students.

This is suitable for an educational machine learning project but is small for making strong claims about real-world populations.

## Generalization

The dataset represents a specific educational context in Portugal.

The results should not automatically be generalized to:

* Ethiopia
* other African countries
* other educational systems
* different schools
* different subjects
* different populations

## Observational Data

The dataset is observational.

Therefore, the model can identify predictive associations but cannot establish causal relationships.

## Fairness

Some demographic and contextual groups are relatively small.

This creates uncertainty in subgroup metrics.

## Privacy

Student educational information can be sensitive.

Any real-world system would require appropriate privacy protection, access control, governance, and purpose limitation.

---

# Future Work

Potential improvements include:

* Larger and more representative datasets
* External validation
* Repeated cross-validation
* Hyperparameter tuning
* Probability calibration
* Classification threshold analysis
* Additional fairness metrics
* Confidence intervals for subgroup metrics
* Additional demographic evaluations
* Temporal validation
* Testing on different educational contexts
* Further investigation of potential proxy variables

---

# Technologies

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Google Colab
* Jupyter Notebook
* GitHub

---

# Key Lessons

This project demonstrates several important machine learning principles:

1. **Prediction timing matters.**
2. **Accuracy alone is not enough.**
3. **Class-specific metrics can reveal problems hidden by accuracy.**
4. **Cross-validation provides a more robust evaluation than a single split.**
5. **Feature importance does not imply causation.**
6. **Fairness analysis requires subgroup-level evaluation.**
7. **Small subgroup sizes can make fairness metrics unstable.**
8. **Removing one sensitive feature does not automatically remove bias.**
9. **Responsible ML requires considering privacy and deployment context.**
10. **A prediction should not automatically become a decision.**

---

# Conclusion

This project demonstrates an end-to-end approach to student performance prediction while considering accuracy, prediction timing, interpretability, fairness, and responsible use.

The models were able to identify predictive patterns in the dataset, and later-term prediction became substantially more accurate when previous grades were available.

However, the early-warning model showed limited ability to identify students who ultimately failed, with a cross-validation Fail Recall of `0.232 ± 0.081`.

The fairness analysis also showed differences in subgroup performance, although these differences were not stable enough to establish systematic discrimination.

The main lesson is that a useful machine learning project should evaluate more than whether a model produces accurate predictions. It should also ask:

> **When is the prediction being made?**

> **Which students might be affected?**

> **How reliable are the predictions?**

> **Are errors distributed differently across groups?**

> **What are the risks of using the model in practice?**

For these reasons, this project treats the model as an **experimental research and decision-support artifact**, not an automated system for making high-stakes decisions about students.

---

## Responsible AI Report

For a detailed discussion of fairness, privacy, limitations, prediction timing, and responsible deployment:

`reports/responsible_modeling_report.md`

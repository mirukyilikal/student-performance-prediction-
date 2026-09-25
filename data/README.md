# Dataset

## Dataset Used

This project uses the **Student Performance Dataset** from the UCI Machine Learning Repository.

The primary dataset used in this project is:

```text
student-mat.csv
```

It contains student information and academic performance data for mathematics.

## Dataset Characteristics

* **Number of students:** 395
* **Number of original variables:** 33
* **Prediction target:** `G3`
* **Target meaning:** Final mathematics grade
* **Grade range:** 0–20

The project also creates a derived classification target:

```text
pass_fail = 1 if G3 >= 10
pass_fail = 0 if G3 < 10
```

The threshold of 10 is a project-defined assumption used to demonstrate binary classification.

## Prediction Scenarios

Two prediction scenarios are evaluated.

### Early-Warning Scenario

The variables `G1` and `G2` are excluded because these grades represent later academic-period information.

This scenario investigates whether student information available before these grades can provide useful predictions.

### Later-Term Scenario

`G1` and `G2` are included because they may be available later in the academic term.

This scenario demonstrates how prediction performance changes when recent academic performance is available.

## How to Obtain the Dataset

Download the Student Performance dataset from the official UCI Machine Learning Repository and place:

```text
student-mat.csv
```

inside this `data/` directory when running the notebook locally.

The notebook expects the dataset to be available at:

```text
/content/student_data/student-mat.csv
```

when running in Google Colab, although the path can be changed to match your local environment.

## Data Privacy and Responsible Use

The dataset contains information about students and includes demographic, family, educational, and behavioral variables.

This project is intended for **educational and research purposes**.

Model predictions should not be treated as definitive judgments about individual students. In a real educational setting, predictions should be considered alongside human review, additional context, and appropriate privacy protections.

## Important Limitation

The dataset represents a specific educational context and should not automatically be assumed to represent students in other countries, schools, or populations.

In particular, model performance from this dataset should not be interpreted as evidence that the model will perform similarly in Ethiopia or other educational systems.

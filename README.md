# Breast Cancer Classification with Machine Learning in R

**An exploratory machine learning project comparing a neural network, ridge logistic regression, and random forest on the Wisconsin Diagnostic Breast Cancer (WDBC) dataset.**

![R](https://img.shields.io/badge/Language-R-276DC3?logo=r&logoColor=white) ![R Markdown](https://img.shields.io/badge/Report-R%20Markdown-75AADB) ![Machine Learning](https://img.shields.io/badge/Focus-Classification-555555)

## Overview

Can measurements of cell nuclei help distinguish **benign** from **malignant** breast tumors? This project explores that question using the WDBC dataset, with data exploration, feature scaling, model training, and evaluation in **R**.

The main model is a feedforward **neural network** with three hidden layers. Its results are compared with **ridge-regularized logistic regression** and a **random forest**.

> **Educational project:** These models are not clinically validated and must not be used for medical diagnosis.

## Dataset

The [Wisconsin Diagnostic Breast Cancer dataset](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic) contains **569 observations** and **30 numeric predictors** derived from images of fine-needle aspirates of breast masses.

| Characteristic | Details |
|---|---|
| Cases | 569 |
| Benign (`B`) | 357 |
| Malignant (`M`) | 212 |
| Predictors | 30 numeric features |
| Original fields | ID, diagnosis, and 30 predictors |
| Measurements | Radius, texture, perimeter, area, smoothness, compactness, concavity, concave points, symmetry, and fractal dimension |

Each measurement is represented by its **mean**, **standard error**, and **worst** value. The identifier is excluded from the classification features.

The included `wdbc.names.txt` documents the dataset and its original attribution.

## Workflow

1. **Explore the data:** Inspect structure, summary statistics, class distribution, and feature distributions.
2. **Visualize relationships:** Create boxplots and correlation heatmaps.
3. **Preprocess:** Encode diagnosis, remove the ID, and apply min–max feature scaling.
4. **Split the data:** Use a 70% training / 30% testing split (`set.seed(42)`).
5. **Train models:** Fit a neural network, ridge logistic regression, and random forest.
6. **Evaluate:** Examine confusion matrices and classification metrics, including accuracy, sensitivity, and specificity.

### Models

| Model | Implementation | Configuration in the report |
|---|---|---|
| Neural network | `neuralnet` | Three hidden layers: **15 → 10 → 5** neurons; learning rate 0.01 |
| Ridge logistic regression | `glmnet` | Binomial model, `alpha = 0` |
| Random forest | `randomForest` | **100 trees** |

## Results

The **confusion matrices printed in the supplied PDF report** give the following test-set accuracies:

| Model | Test accuracy | Correct predictions / test cases |
|---|---:|---:|
| Neural network | **97.08%** | 166 / 171 |
| Ridge logistic regression | **97.66%** | 167 / 171 |
| Random forest | **96.49%** | 165 / 171 |

On this particular split, ridge logistic regression has the highest reported test accuracy. All three models perform well on the held-out examples, but one split is not enough to establish general performance.

**Reporting note:** The final performance-comparison table and interpretation in the original report contain numbers that do not agree with its printed confusion matrices. The table above uses the confusion-matrix counts, rather than the inconsistent summary table. Sensitivity and specificity printed by `caret` use **Benign** as the positive class in this report; they should not be interpreted as malignant-case sensitivity.

## Repository contents

```text
breast-cancer-classification/
├── Breast_Cancer_Classification.Rmd   # R Markdown analysis and code
├── Breast_Cancer_Classification.pdf   # Knitted project report
├── wdbc.csv                           # Input dataset
├── wdbc.names.txt                     # Dataset description and citations
└── README.md                          # This page
```

*If your local report files are still named `Assignment11.Rmd` and `Assignment11.pdf`, rename them to match the structure above before publishing, or update the names here.*

## Run the project

**Prerequisites:** R and RStudio, with an R Markdown/PDF knitting setup (such as a LaTeX distribution or TinyTeX) if you want to recreate the PDF.

1. Clone the repository:

   ```bash
   git clone https://github.com/s4sharif/breast-cancer-classification.git
   cd breast-cancer-classification
   ```

2. Install the packages used by the analysis:

   ```r
   install.packages(c(
     "ggplot2", "dplyr", "neuralnet", "glmnet", "randomForest",
     "corrplot", "caret", "pROC", "car", "reshape2", "rmarkdown"
   ))
   ```

3. Open `Breast_Cancer_Classification.Rmd` in RStudio, set the working directory to the repository folder, and click **Knit** to build the report. Keep `wdbc.csv` beside the `.Rmd` file.

**Before knitting:** The uploaded source file contains a typo in its package-installation line: `install.packages("ggplot2e")` should be `install.packages("ggplot2")`. Other sections may also need review before the entire analysis runs cleanly from a fresh R session.

## Limitations and opportunities to improve

- The dataset is relatively small and the evaluation uses a single random train/test split.
- The source analysis normalizes features **before** splitting the data, which allows test-set information to influence preprocessing; a stronger workflow would fit scaling on the training set only.
- Class-specific performance for **malignant** cases should be reported explicitly, since false negatives have important consequences.
- Repeated cross-validation, hyperparameter tuning, and external validation would make model comparisons more reliable.

## Dataset attribution

Wolberg, W. H., Street, W. N., and Mangasarian, O. L. **Wisconsin Diagnostic Breast Cancer (WDBC)** dataset, University of Wisconsin. See the included [`wdbc.names.txt`](wdbc.names.txt) for feature definitions and references to the original research.

---

**Project by Mohammed Sharif** · Built with R, R Markdown, and open research data.

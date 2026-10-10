# Prostate-Specific Antigen (PSA) Prediction and Classification Using Machine Learning in R

**Healthcare Analytics | Machine Learning | Predictive Modeling | Classification | R Programming**

## Project Overview

Prostate-specific antigen (PSA) is a biomarker commonly used in the clinical evaluation of prostate conditions. Elevated PSA levels may be associated with prostate cancer, although noncancerous conditions can also cause PSA levels to increase.

This project applies statistical learning and machine learning techniques to investigate factors associated with PSA levels and develop predictive models for continuous, binary, and multiclass outcomes.

Using the **Prostate Cancer dataset**, the study evaluates three major modeling problems:

1. **Continuous Prediction:** Predicting the logarithm of PSA levels using regularized regression.
2. **Binary Classification:** Classifying observations into normal and abnormal PSA categories.
3. **Multiclass Classification:** Classifying observations into low-risk, at-risk, and high-risk PSA categories.

The analysis compares multiple models based on predictive accuracy, sensitivity, specificity, interpretability, and variable importance.

The project was implemented in **R**.

## 1. Business and Analytical Problem

Early identification of abnormal PSA levels may help healthcare professionals determine whether additional diagnostic evaluation is warranted.

However, predictive models must balance the ability to identify abnormal PSA observations with the risk of incorrectly classifying normal observations.

The challenge is not simply to maximize overall prediction accuracy but also to evaluate the consequences of false positives and false negatives.

### Central Analytical Question

Which statistical learning and machine learning models can effectively predict PSA levels and classify abnormal PSA observations while maintaining acceptable predictive performance and interpretability?

### Potential Stakeholders

- Healthcare researchers
- Clinical decision-support developers
- Medical data analysts
- Healthcare administrators
- Public health researchers

The models are developed for academic research and methodological comparison, not for clinical diagnosis.

## 2. Project Objectives

1. Explore the characteristics, distributions, and correlations of prostate cancer-related variables.
2. Develop Ridge, LASSO, and Elastic Net regression models for continuous PSA prediction.
3. Compare regularized regression models using predictive error metrics and cross-validation.
4. Develop Logistic Regression, Naive Bayes, and Linear Discriminant Analysis models for binary PSA classification.
5. Evaluate binary classifiers using sensitivity, specificity, accuracy, F1-score, and AUC.
6. Develop a classification decision tree and compare it with ensemble learning approaches.
7. Identify the most important predictors across different models.
8. Recommend an appropriate modeling approach based on predictive performance, interpretability, and the analytical objective.

## 3. Dataset

**Dataset:** Prostate Cancer Dataset

**Original Study:** Stamey et al. (1989), *Prostate Specific Antigen in the Diagnosis and Treatment of Adenocarcinoma of the Prostate. II. Radical Prostatectomy Treated Patients.*

The dataset contains **97 observations and 9 primary variables**, representing patients who underwent radical prostatectomy.

### Data Dictionary

| Variable | Description | Type |
|---|---|---|
| `lpsa` | Logarithm of prostate-specific antigen | Continuous response |
| `lcavol` | Logarithm of cancer volume | Continuous |
| `lweight` | Logarithm of prostate weight | Continuous |
| `age` | Patient age | Continuous |
| `lbph` | Logarithm of benign prostatic hyperplasia amount | Continuous |
| `svi` | Seminal vesicle invasion | Binary |
| `lcp` | Logarithm of capsular penetration | Continuous |
| `gleason` | Gleason score | Discrete |
| `pgg45` | Percentage of Gleason scores classified as grade 4 or 5 | Continuous |

### Response Variables

**Continuous Response**

`lpsa` – Logarithm of PSA levels.

**Binary Response**

`psa_binary` – PSA classification based on the study's threshold of 4.0 ng/mL.

- Low PSA (0): 14 observations (14%)
- High PSA (1): 83 observations (86%)

**Multiclass Response**

The study also created three PSA risk categories:

- Low-risk: 14 observations (14.4%)
- At-risk: 27 observations (27.8%)
- High-risk: 56 observations (57.7%)

These are study-defined categories and should not be interpreted as validated clinical cancer-risk classifications.

## 4. Exploratory Data Analysis

The exploratory analysis examined the distribution and relationships of the predictors.

### Techniques Used

- Summary statistics
- Box plots and outlier identification
- Correlation matrices
- Scatter plot matrices
- Estimated variable distributions
- Examination of potential multicollinearity

### Initial Findings

**Cancer volume and PSA**

The logarithm of cancer volume (`lcavol`) exhibited a strong positive correlation with the logarithm of PSA levels (`lpsa`).

**Correlated predictors**

Several predictor pairs showed notable correlations, including:

- Cancer volume and capsular penetration
- Seminal vesicle invasion and capsular penetration
- Gleason score and percentage of high-grade Gleason scores

These correlations suggested potential multicollinearity, supporting the use of regularized regression techniques.

**Potential outliers**

Box plots identified potential outliers in prostate weight, capsular penetration, Gleason score, and patient age.

## 5. Machine Learning Methodology

### Part I: Continuous PSA Prediction

Three regularized regression methods were evaluated.

#### Ridge Regression

Ridge regression applies L2 regularization to reduce coefficient magnitudes and improve model stability.

Hyperparameter tuning was performed using 10-fold cross-validation.

**Optimal lambda:** 0.385

#### LASSO Regression

LASSO applies L1 regularization, which can shrink some regression coefficients to exactly zero.

This provides both regularization and automatic feature selection.

Hyperparameter tuning was performed using 10-fold cross-validation.

**Optimal lambda:** 0.060

#### Elastic Net Regression

Elastic Net combines L1 and L2 regularization.

The model was tuned using repeated 5-fold cross-validation with five repetitions.

**Optimal hyperparameters:**

- Alpha: 0.288
- Lambda: 0.129

### Continuous Model Selection

The three models were compared using:

- Bias
- Normalized bias
- Mean Squared Prediction Error (MSPE)
- Root Mean Squared Error (RMSE)
- Normalized RMSE
- Root Mean Squared Prediction Error (RMSPE)
- Mean Absolute Error (MAE)
- Mean Absolute Percentage Error (MAPE)
- Symmetric MAPE

**Selected Model: LASSO Regression**

LASSO achieved the lowest reported prediction errors among the three regularized regression models.

It also provided a more interpretable specification through feature selection.

The most prominent predictors included:

- Seminal vesicle invasion (`svi`)
- Prostate weight (`lweight`)
- Cancer volume (`lcavol`)

The model reduced the coefficients of several less influential or redundant predictors.

---

### Part II: Binary PSA Classification

The second modeling task classified observations as having low or high PSA levels.

Three classification methods were evaluated.

#### Logistic Regression

Logistic Regression was developed to estimate the probability of high PSA classification.

Stepwise variable selection based on AIC identified the following predictors:

- `lcavol`
- `lweight`
- `lcp`

Model diagnostics and statistical tests included:

- Variance Inflation Factor
- Analysis of Deviance
- Wald Test
- Likelihood Ratio Test
- Decision Boundary Visualization

The model was also evaluated using confusion matrices and ROC analysis.

#### Naive Bayes

Naive Bayes was evaluated using multiple combinations of predictors.

The selected predictor set included:

- `lcavol`
- `lweight`
- `lcp`
- `age`

The selected model achieved the following reported cross-validation results:

| Metric | Result |
|---|---:|
| Accuracy | 0.82 |
| Sensitivity | 0.82 |
| Specificity | 0.86 |
| Precision | 0.97 |
| F1-score | 0.89 |
| AUC | 0.93 |

Permutation-based variable importance analysis identified prostate weight and cancer volume as important predictors.

#### Linear Discriminant Analysis

Linear Discriminant Analysis (LDA) was developed and evaluated using multiple predictor combinations.

The analysis examined:

- Multivariate normality
- Homogeneity of covariance matrices
- Bartlett's Test
- Box's M-Test
- Henze-Zirkler Test

Following the assumption checks, the model using `lcavol` and `lweight` was selected.

### Binary Classification Model Comparison

The classifiers were evaluated using:

- Accuracy
- Sensitivity
- Specificity
- Precision
- F1-score
- AUC
- Balanced Accuracy
- Cohen's Kappa
- Youden's Index
- G-Mean
- False Negative Rate

A 70% training and 30% testing split was used, alongside cross-validation.

**Selected Model: Logistic Regression**

Logistic Regression was selected because of its favorable sensitivity, interpretability, and comparatively flexible assumptions.

The model achieved approximately **96% sensitivity** on the test dataset.

The selected model also correctly classified 24 of 25 high-PSA observations in the reported test confusion matrix.

Its odds ratios provided an interpretable way to communicate the relationships between predictors and high PSA classification.

---

### Part III: Multiclass PSA Classification

The third modeling task classified observations into three study-defined PSA risk categories.

#### Classification Decision Tree

A recursive partitioning decision tree was developed to identify classification rules.

The primary splitting variables included:

- Cancer volume (`lcavol`)
- Prostate weight (`lweight`)
- Percentage of high-grade Gleason scores (`pgg45`)

**Predictive Performance**

| Metric | Result |
|---|---:|
| Training Accuracy | 70% |
| Test Accuracy | 73% |
| High-risk Sensitivity (Test) | 95% |
| At-risk Sensitivity (Test) | 0% |

The model demonstrated relatively strong identification of high-risk observations but struggled to classify the intermediate at-risk group.

#### Ensemble Learning

Three ensemble learning techniques were compared:

1. Bagging
2. Random Forest
3. AdaBoost

Random Forest hyperparameter tuning used repeated k-fold cross-validation.

### Ensemble Model Comparison

| Model | CV Accuracy | Test Accuracy |
|---|---:|---:|
| Bagging | 63.83% | 73.33% |
| Random Forest | 65.09% | 76.67% |
| AdaBoost | 70.44% | 70.00% |

**Selected Ensemble Model: Random Forest**

The selected Random Forest configuration used:

- `mtry = 3`
- `ntree = 1000`

Random Forest achieved the highest test accuracy among the evaluated ensemble methods.

The most important predictors were:

- Prostate weight (`lweight`)
- Cancer volume (`lcavol`)
- Patient age (`age`)

Random Forest was preferred over the single decision tree because it provided better overall test accuracy and a more balanced classification performance across risk categories.

## 6. Summary of Selected Models

| Prediction Task | Selected Model | Primary Reason |
|---|---|---|
| Continuous PSA prediction | LASSO | Lower prediction errors and feature selection |
| Binary PSA classification | Logistic Regression | High sensitivity and interpretability |
| Multiclass PSA classification | Random Forest | Higher test accuracy and better class balance than the single tree |

### Overall Recommended Model

**Logistic Regression** was recommended as the most appropriate model for the project's stated screening-oriented objective.

The selection emphasized:

- High sensitivity in detecting elevated PSA observations
- Lower risk of false-negative classification
- Interpretable odds ratios
- Relatively straightforward model assumptions
- Ease of communicating predictor relationships

This recommendation applies to the academic modeling exercise and does not establish clinical screening validity.

## 7. Key Findings and Insights

### Finding 1: Prostate weight and cancer volume consistently emerged as important predictors.

Across LASSO, Logistic Regression, and Random Forest, `lweight` and `lcavol` repeatedly appeared among the most influential variables.

This consistency suggests that both variables contain important predictive information about PSA levels in the analyzed dataset.

### Finding 2: Regularization can improve model simplicity.

LASSO achieved favorable predictive performance while shrinking selected coefficients to zero.

This demonstrated how regularization can help address correlated predictors and produce a more parsimonious regression model.

### Finding 3: Logistic Regression provided a useful balance between classification performance and interpretability.

The selected Logistic Regression model achieved high sensitivity for elevated PSA classification.

Its odds ratios also provided interpretable estimates of the associations between predictor values and the odds of high PSA classification.

### Finding 4: Random Forest improved multiclass prediction compared with a single decision tree.

The Random Forest model achieved a test accuracy of 76.67%, compared with approximately 73% for the single classification tree.

The ensemble approach also improved classification performance for the intermediate at-risk category.

### Finding 5: Important predictors can vary depending on the modeling technique.

Different algorithms emphasized different predictors because of their underlying learning mechanisms.

For example:

- LASSO emphasized seminal vesicle invasion.
- Logistic Regression emphasized prostate weight, cancer volume, and capsular penetration.
- Random Forest emphasized prostate weight, cancer volume, and age.

This illustrates why variable importance should be interpreted within the context of the model being used.

### Finding 6: Model selection should consider the analytical objective.

A model with high overall accuracy may still perform poorly for an important minority class.

For screening-oriented classification, sensitivity and false-negative rates deserve particular attention.

The project demonstrates why evaluation metrics should be selected based on the intended decision problem rather than overall accuracy alone.

## 8. Tools and Techniques

**Programming Language**

- R

**Regression Techniques**

- Ridge Regression
- LASSO Regression
- Elastic Net Regression

**Classification Techniques**

- Logistic Regression
- Naive Bayes
- Linear Discriminant Analysis
- Classification Decision Trees
- Random Forest
- Bagging
- AdaBoost

**Model Optimization**

- Hyperparameter Grid Search
- K-Fold Cross-Validation
- Repeated Cross-Validation
- Stepwise Variable Selection
- Feature Selection

**Model Evaluation**

- Regression Error Metrics
- Confusion Matrix
- ROC Curve and AUC
- Sensitivity and Specificity
- Precision and F1-score
- Balanced Accuracy
- Cohen's Kappa
- Variable Importance Analysis

**R Packages Referenced in the Analysis**

- `caret`
- `stats`
- `vip`

Other package requirements should be confirmed against the original R source code.

## 9. How to Reproduce the Analysis

1. Clone or download the repository.
2. Install R and RStudio.
3. Open the project's R source file or R Markdown notebook.
4. Install the required packages.
5. Load the prostate cancer dataset.
6. Run the exploratory data analysis.
7. Train and tune the regression and classification models.
8. Evaluate the models using the specified validation procedures.
9. Compare model performance and interpret the results.

The original source code should be consulted for exact data preprocessing, package dependencies, and random seed settings.

## 10. Limitations

- The dataset contains only 97 observations, limiting the stability and generalizability of predictive estimates.
- The data originate from patients who underwent radical prostatectomy and are not representative of a general screening population.
- The binary classification task has an imbalanced outcome distribution.
- The risk categories were defined for this modeling exercise and are not validated clinical risk classifications.
- The small dataset may contribute to unstable estimates, particularly for minority classes.
- PSA levels are not sufficient on their own to diagnose prostate cancer.
- The analysis evaluates statistical associations and predictive performance rather than causal relationships.
- No external clinical validation was performed.

## 11. Future Improvements

Potential extensions include:

- External validation using larger and more representative datasets.
- Nested cross-validation for hyperparameter selection and performance estimation.
- Probability calibration for classification models.
- Evaluation of alternative classification thresholds.
- Class imbalance handling techniques.
- Additional interpretability methods such as SHAP.
- Comparison with other classification algorithms.
- Development of interactive dashboards for model exploration.

## 12. Conclusion

This project demonstrates the application of statistical learning and machine learning methods to continuous prediction, binary classification, and multiclass classification using a healthcare dataset.

LASSO was selected for continuous PSA prediction because of its favorable predictive performance and automatic feature selection.

Logistic Regression was recommended for binary PSA classification because of its high sensitivity and interpretable predictor effects.

Random Forest achieved the strongest test accuracy among the evaluated ensemble methods for multiclass PSA classification.

Across the models, prostate weight and cancer volume consistently emerged as important predictors.

The project emphasizes that effective machine learning requires not only model development and predictive evaluation but also careful consideration of the problem context, class imbalance, interpretability, and limitations of the available data.

## 13. Author

**Dominique Pocholo P. de Lara**

Academic Machine Learning and Statistical Modeling Project

**Focus Areas:**
- Predictive Analytics
- Machine Learning
- Classification Models
- Statistical Learning
- Healthcare Data Analytics
- R Programming

## 14. References

Agresti, A. (2019). *An Introduction to Categorical Data Analysis* (3rd ed.). Wiley.

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2013). *An Introduction to Statistical Learning: With Applications in R*. Springer.

Leal, J., et al. (2018). Estimating the sensitivity of a prostate cancer screening programme for different PSA cut-off levels: A UK case study. *Cancer Epidemiology, 52*, 99–105.

Stamey, T. A., et al. (1989). Prostate specific antigen in the diagnosis and treatment of adenocarcinoma of the prostate. II. Radical prostatectomy treated patients. *The Journal of Urology, 141*(5), 1076–1083.

Stamey, T. A., et al. (2004). The prostate specific antigen era in the United States is over for prostate cancer: What happened in the last 20 years? *The Journal of Urology, 172*(4), 1297–1301.

---

**Disclaimer:** This project is intended for academic research and machine learning education. The developed models are not clinically validated and should not be used for medical diagnosis, patient screening, or treatment decisions.

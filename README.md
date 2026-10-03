# Wine Quality Prediction: Classification vs. Regression

## Overview

This project investigates whether the quality of wine can be predicted from
physicochemical measurements such as acidity, pH, density, sulphates, and
alcohol content.

Because wine quality is measured on an **ordinal scale**, the project explores
two different ways of modeling the target:

- **Regression**, which preserves the ordering and distance between quality scores
- **Classification**, which treats each quality score as a separate class

The goal was to compare these approaches and determine which was more effective
for predicting wine quality from the available measurements.

## Dataset

The project uses the Wine Quality dataset from the UCI Machine Learning
Repository. The data contains physicochemical measurements of Portuguese wine
samples along with quality scores assigned through sensory evaluation.

The analysis used 1,599 observations and measurements including:

- Fixed acidity
- Volatile acidity
- Citric acid
- Residual sugar
- Chlorides
- Free sulfur dioxide
- Total sulfur dioxide
- Density
- pH
- Sulphates
- Alcohol
- Quality

`quality` was used as the target variable.

## Exploratory Data Analysis

Initial analysis identified several challenges in the dataset.

Most features contained substantial outliers and several were strongly skewed.
The quality target was also highly imbalanced, with most observations belonging
to qualities 5 and 6 and very few observations at the extremes.

Multivariable analysis revealed relationships between several features.
For example, alcohol, sulphates, and citric acid generally increased with
quality, while volatile acidity and density tended to decrease.

Permutation testing found evidence of an association between quality and most
of the investigated variables, with residual sugar being a notable exception.

Multicollinearity was also present among several predictors and was considered
when constructing the regression models.

## Regression Models

Regression was considered because the quality score is ordinal. Predicting a
quality of 4 when the true value is 5 should intuitively represent a smaller
error than predicting a quality of 8.

The regression approaches included:

### Random Forest Regressor

Random Forest was selected because it can model nonlinear relationships and is
relatively robust to skewed distributions and outliers.

Hyperparameters were selected using randomized cross-validation.

### Multiple Linear Regression

Ordinary Least Squares regression was used as a simpler and more interpretable
baseline.

Variance Inflation Factor (VIF) was used to identify multicollinearity, with
high-VIF features removed iteratively. Ridge regression was also tested to
reduce the effects of multicollinearity.

The linear models underperformed Random Forest, suggesting that the
relationships between the physicochemical measurements and wine quality were
not adequately represented by a purely linear model.

## Classification Models

The same target was then treated as a multiclass classification problem.

### K-Nearest Neighbors

KNN was trained using standardized features, with the number of neighbors
selected through cross-validation and observations weighted by distance.

The model achieved an overall ROC-AUC of approximately **0.73**.

Performance was substantially weaker for rare quality classes, demonstrating
the effect of the target's class imbalance.

### Random Forest Classifier

Random Forest classification was also evaluated. Balanced class weights were
used to increase the penalty associated with misclassifying minority classes.

The model achieved approximately:

- **ROC-AUC: 0.77**
- **Accuracy: 0.66**
- **MAE: 0.38**

Random Forest generally performed better than KNN, although estimates for the
rarest classes were unstable because very few examples were available for
evaluation.

## Results

When regression and classification models were evaluated using comparable
prediction errors, the classification models performed better.

| Model | Approach | Approx. MAE |
|---|---|---:|
| Random Forest Classifier | Classification | 0.38 |
| KNN | Classification | 0.385 |
| Random Forest Regressor | Regression | 0.73 |

When predictions were converted to discrete quality scores, Random Forest
Classifier achieved the highest observed accuracy at approximately **66%**.

An important limitation is the substantial class imbalance in the dataset.
Most samples belong to qualities 5 and 6, while extreme scores contain very few
observations. As a result, overall accuracy primarily represents performance on
the common quality classes rather than balanced performance across the entire
quality scale.

## Key Findings

1. **Classification performed better than regression in this experiment.**
   Despite the ordinal structure of wine quality, allowing models to learn
   separate decision boundaries for each quality class produced better
   predictive results under the metrics used.

2. **The relationship between wine characteristics and quality appears
   substantially nonlinear.**
   Random Forest models performed better than the linear regression approaches.

3. **Class imbalance was a major limitation.**
   Models struggled with rare quality scores because there were too few
   observations to reliably learn or evaluate those classes.

4. **Several physicochemical measurements were associated with quality.**
   Alcohol, sulphates, and citric acid generally increased with quality, while
   volatile acidity and density generally decreased.

## Conclusion

The results supported the original hypothesis that classification would
outperform regression for this dataset. Random Forest Classifier produced the
strongest overall results among the models tested.

However, the strong imbalance in wine quality scores limits how broadly the
results can be interpreted. Performance was substantially better for the
common quality classes than for rare scores.

Future work could address this by collecting additional observations for
underrepresented quality levels or reframing the target into broader categories
such as **low, medium, and high quality**.

## Technologies

- Python
- Pandas
- NumPy
- Scikit-learn
- Statsmodels
- Matplotlib
- Seaborn

## Methods

- Exploratory Data Analysis
- Permutation Testing
- Variance Inflation Factor (VIF)
- Multiple Linear Regression
- Ridge Regression
- Random Forest Regression
- K-Nearest Neighbors Classification
- Random Forest Classification
- Cross-Validation
- ROC-AUC
- F1 Score
- Mean Absolute Error

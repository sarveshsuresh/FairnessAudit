# Fairness Audit Package

This package does the following 
- Automated Pre-Processing
- Automated Modelling
- Calculates accuracy and various Fairness-Metrics on unseen data


## Automated Pre-Processing
- All numeric columns with levels < 10 are converted to categorical
- All numeric columns with levels > 10 remain the same
- All categorical columns with levels > 10 are removed
- All categorical columns with levels < 10 remain the same
- Impute missing values with mean for numeric columns and with 'missing' for categorical columns

## Model Codes
- '**logreg**': Logistic Regression with [default model-parameters](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
- '**dt**': Decision Tree with [default model-parameters](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)
- '**xgb**': XGBoost using grid search
```
params = {
        'min_child_weight': [1, 5, 10],
        'gamma': [0.5, 1, 1.5, 2, 5],
        'subsample': [0.6, 0.8, 1.0],
        'colsample_bytree': [0.6, 0.8, 1.0],
        'max_depth': [3, 4, 5]
        }

```
## Metrics
![This is an image](https://myoctocat.com/assets/images/base-octocat.svg)

- **Proportional parity (prop_parity)**

Proportional parity is achieved if the proportion of positive predictions in the subgroups are close to each other. This measure does not depend on the true labels. 

Formula: (TP + FP) / (TP + FP + TN + FN) 

- **Equalized odds (equalized_odds)**

Equalized odds are achieved if the sensitivities in the subgroups are close to each other. The group-specific sensitivities indicate the number of the true positives divided by the total number of positives in that group. 

Formula: TP / (TP + FN) 

- **Predictive rate parity (pred_rate_parity)**

Predictive rate parity is achieved if the precisions (or positive predictive values) in the subgroups are close to each other. The precision stands for the number of the true positives divided by the total number of examples predicted positive within a group. 

Formula: TP / (TP + FP) 

- **Accuracy parity (accuracy_parity)**

Accuracy parity is achieved if the accuracies (all accurately classified examples divided by the total number of examples) in the subgroups are close to each other. 

Formula: (TP + TN) / (TP + FP + TN + FN) 

- **False negative rate parity (false_negrate_parity)**

False negative rate parity is achieved if the false negative rates (the ratio between the number of false negatives and the total number of positives) in the subgroups are close to each other. 

Formula: FN / (TP + FN) 

- **False positive rate parity (false_posrate_parity)**

False positive rate parity is achieved if the false positive rates (the ratio between the number of false positives and the total number of negatives) in the subgroups are close to each other. 

Formula: FP / (TN + FP) 

- **Negative predictive value parity (neg_predval_parity)**

Negative predictive value parity is achieved if the negative predictive values in the subgroups are close to each other. The negative predictive value is computed as a ratio between the number of true negatives and the total number of predicted negatives. This function can be considered the ‘inverse’ of the predictive rate parity. 

Formula: TN / (TN + FN) 

- **Specificity parity (specificity_parity)**

Specificity parity is achieved if the specificities (the ratio of the number of the true negatives and the total number of negatives) in the subgroups are close to each other. This function can be considered the ‘inverse’ of the equalized odds. 

Formula: TN / (TN + FP) 

## Functions
### Build Model Check Fairness Multiclass
Does pre-processing, builds required model, makes prediction on unseen data and evaluates fairness metric required.
```
build_model_check_fairness_multiclass(dfr,target,model,test_size,sens,metric,reference,threshold)

```
#### Parameters
- **dfr**: Raw DataSet
- **target**: Target Variable (Variable that needs to be predicted)
- **model**: Model Code of model desired
- **test_size**: Proportion of rows assigned to the **Test Set** while performing train-test-split
- **sens**: Name of the sensitive variable
- **metric**: Desired metric
- **reference**: The level of the target variable that is considered to be the positive class. All other classes are considered negative.
- **threshold** : The level of significance to be considered for the statistical test of proportions (metrics) to evaluate fairness.

### All Metric Fairness
Does pre-processing, builds required model, makes prediction on unseen data and evaluates all the fairness metrics.
```
all_metric_fairness(dfr,target,model,test_size,sens,reference,threshold)

```
#### Parameters
- **dfr**: Raw DataSet
- **target**: Target Variable (Variable that needs to be predicted)
- **model**: Model Code of model desired
- **test_size**: Proportion of rows assigned to the **Test Set** while performing train-test-split
- **sens**: Name of the sensitive variable
- **reference**: The level of the target variable that is considered to be the positive class. All other classes are considered negative.
- **threshold** : The level of significance to be considered for the statistical test of proportions (metrics) to evaluate fairness.

## Exception Handling
'**Model too Simplistic**' is returned when the model makes constant prediction (For example, All test cases are predicted as 'Yes') for the entire test set or there are no metrics to be compared at all (For example, metric for Males can be calculated but model makes constant prediction for Females).


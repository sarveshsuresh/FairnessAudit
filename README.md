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

##Exception Handling
'**Model too Simplistic**' is returned when the model makes constant prediction (For example, All test cases are predicted as 'Yes') for the entire test set or there are no metrics to be compared at all (For example, metric for Males can be calculated but model makes constant prediction for Females).


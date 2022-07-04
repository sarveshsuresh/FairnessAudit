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
## Functions
### Build Model Check Fairness Multiclass
```
build_model_check_fairness_multiclass(dfr,target,model,test_size,sens,metric,reference,threshold)

```
#### Parameters
- **dfr**: Raw DataSet
- **target

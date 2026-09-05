# Machine Learning Project 1: Breast-Cancer Diagnosis Classification

## Goal

Build and evaluate a reproducible binary-classification pipeline using the public **Breast Cancer Wisconsin (Diagnostic)** dataset from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic). The goal is to predict the diagnostic label from the provided cell-nuclei measurements while emphasizing validation, class imbalance, and interpretability.

## Requirements

Import the data, remove the identifier column, encode the diagnosis label, inspect missing values, and create a stratified training/test split. Standardize predictors using training-set statistics only. Train at least three models, such as logistic regression, a linear or Gaussian-kernel SVM, and a bagged tree ensemble. Use five-fold cross-validation on the training set for model selection.

Report accuracy, balanced accuracy, precision, recall, F1 score, ROC-AUC, confusion matrix, and a calibration or threshold analysis. Plot ROC curves for the selected models in `breast_cancer_model_comparison.png`. Discuss why accuracy alone can be misleading in medical classification and state clearly that this educational model is not a diagnostic tool.

## Suggested MATLAB workflow

```matlab
T = readtable('wdbc.data', 'ReadVariableNames', false);
labels = categorical(T.Var2);
X = T{:, 3:end};

cv = cvpartition(labels, 'HoldOut', 0.20, 'Stratify', true);
XTrain = X(training(cv), :); yTrain = labels(training(cv));
XTest = X(test(cv), :); yTest = labels(test(cv));

mu = mean(XTrain, 1); sigma = std(XTrain, [], 1);
sigma(sigma == 0) = 1;
XTrain = (XTrain - mu)./sigma;
XTest = (XTest - mu)./sigma;

model = fitcsvm(XTrain, yTrain, 'KernelFunction', 'linear', ...
    'Standardize', false, 'ClassNames', categories(labels));
predicted = predict(model, XTest);
confusionchart(yTest, predicted);
```

Use `fitclinear`, `fitcsvm`, or `fitcensemble` as appropriate. Store the preprocessing parameters with the final model and ensure that no test-set information influences training or tuning.

## Validation checklist

Use a fixed random seed, report class counts in every split, and compare cross-validation performance with final held-out performance. Inspect false positives and false negatives separately. Confirm that the same feature transformations are applied at inference time.

## Extensions

Use nested cross-validation, probability calibration, permutation feature importance, cost-sensitive learning, or a compact report that compares model performance with and without feature selection.

## References

1. [UCI Machine Learning Repository: Breast Cancer Wisconsin (Diagnostic)](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic)
2. [MathWorks: Machine Learning in MATLAB](https://www.mathworks.com/help/stats/machine-learning-in-matlab.html)

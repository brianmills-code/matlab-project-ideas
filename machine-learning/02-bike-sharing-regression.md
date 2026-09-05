# Machine Learning Project 2: Bike-Sharing Demand Regression

## Goal

Predict hourly bike-sharing demand from calendar and weather variables using the public **Bike Sharing Dataset** from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset). Compare regularized linear regression with tree-based regression and evaluate generalization using a time-aware split.

## Requirements

Import the hourly data, parse the date and hour fields, inspect missing values, and avoid target leakage by excluding the casual and registered counts when predicting total count. Create calendar features such as hour, weekday, month, and working-day status. Treat categorical variables appropriately and document how the `count` target is transformed, if at all.

Use the earliest portion of the data for training and the latest portion for testing so the evaluation reflects forecasting rather than random interpolation. Train at least two models, such as regularized linear regression and a bagged regression ensemble. Select hyperparameters using validation data from the training period only.

Report MAE, RMSE, normalized RMSE, and R-squared. Plot actual versus predicted demand, residuals over time, and error by hour of day in `bike_demand_regression.png`. Examine performance separately for working days, non-working days, and high-demand periods.

## Suggested MATLAB workflow

```matlab
T = readtable('hour.csv');
T.dteday = datetime(T.dteday);
T.hour = categorical(T.hr);
T.weekday = categorical(T.weekday);
T.workingday = categorical(T.workingday);

% Keep time order; do not randomly shuffle the final evaluation split.
splitDate = datetime(2012, 7, 1);
train = T.dteday < splitDate;
test = ~train;

predictorNames = {'season','yr','mnth','hour','holiday', ...
    'weekday','workingday','weathersit','temp','atemp','hum','windspeed'};
XTrain = T(train, predictorNames);
yTrain = T.cnt(train);
XTest = T(test, predictorNames);
yTest = T.cnt(test);

model = fitrensemble(XTrain, yTrain, 'Method', 'Bag');
yHat = predict(model, XTest);
mae = mean(abs(yTest - yHat));
rmse = sqrt(mean((yTest - yHat).^2));
```

Adapt categorical encoding to the selected MATLAB release. Do not use future weather or target-derived fields. If a logarithmic target transform is used, invert predictions before computing metrics and document the bias correction choice.

## Validation checklist

Confirm that test dates occur after training dates, baseline performance is reported, and metrics are computed on the same target scale. Compare errors across hours and weather conditions, inspect the largest residuals, and explain whether the model underpredicts peaks.

## Extensions

Use rolling-origin evaluation, add lagged demand features without leakage, compare quantile regression for prediction intervals, or deploy the model as a function that accepts one future hourly record.

## References

1. [UCI Machine Learning Repository: Bike Sharing Dataset](https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset)
2. [MathWorks: Machine Learning in MATLAB](https://www.mathworks.com/help/stats/machine-learning-in-matlab.html)

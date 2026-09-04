# Intermediate Project 1: Temperature Trend and Anomaly Analysis

## Goal

Analyze a year of daily temperature observations, identify seasonal behavior and unusual days, and communicate the findings with multiple coordinated plots.

## Learning outcomes

This project practices importing data, handling missing values, moving averages, anomaly thresholds, date-aware plotting, and reproducible analysis. It also introduces a clean separation between data preparation, analysis, and visualization.

## Dataset format

Create or obtain a CSV named `daily_temperature.csv` with columns `Date` and `TemperatureC`. The date column should contain one date per row, while temperature values are measured in degrees Celsius. A generated dataset is acceptable for the first version.

## Requirements

Read the CSV with `readtable`. Convert the date column to `datetime` if needed. Detect missing or non-finite temperatures and report how many were found. Fill short gaps with linear interpolation, while documenting the choice. Calculate a 7-day moving average and the daily anomaly relative to the overall mean.

Define an anomaly as an observation more than two standard deviations from the mean. Report the warmest day, coldest day, mean temperature, standard deviation, and number of anomalies. Create a figure with three panels: the raw series and moving average, the anomaly series with zero reference, and a histogram of temperatures. Save the figure as `temperature_analysis.png`.

## Suggested workflow

```matlab
T = readtable('daily_temperature.csv');
T.Date = datetime(T.Date);
T.TemperatureC = fillmissing(T.TemperatureC, 'linear');
assert(all(isfinite(T.TemperatureC)), 'Temperature values must be finite.');

meanTemp = mean(T.TemperatureC);
stdTemp = std(T.TemperatureC);
T.MovingAverage = movmean(T.TemperatureC, 7);
T.Anomaly = T.TemperatureC - meanTemp;
T.IsAnomaly = abs(T.Anomaly) > 2*stdTemp;

figure('Color', 'w');
tiledlayout(3, 1);
nexttile;
plot(T.Date, T.TemperatureC, '-', T.Date, T.MovingAverage, 'LineWidth', 1.2);
grid on; ylabel('Temperature (C)'); legend('Daily', '7-day mean');
nexttile;
plot(T.Date, T.Anomaly); yline(0, 'k--'); grid on; ylabel('Anomaly (C)');
nexttile;
histogram(T.TemperatureC); grid on; xlabel('Temperature (C)'); ylabel('Count');
```

## Validation checklist

Verify that dates are ordered and unique, the number of rows is unchanged after cleaning, and the moving average is smoother than the raw series. Manually inspect at least three flagged anomalies and explain whether they might be data errors or real events.

## Extensions

Compare monthly means, add confidence bands, fit a linear trend, compare two locations, or build a function that accepts any table with a date and measurement column.

# Data Analysis Project 1: Wine Quality Exploration

## Goal

Perform a reproducible exploratory analysis of physicochemical measurements and quality ratings using the public **Wine Quality** dataset from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/186/wine+quality). Use either the red-wine or white-wine file, and record which file and license information you used.

## Learning outcomes

This project practices importing delimited data, descriptive statistics, correlation analysis, grouped summaries, outlier inspection, and clear visual communication without making causal claims from observational data.

## Requirements

Import the dataset with `readtable`, inspect variable names and missing values, and report the number of observations and predictors. Calculate mean, median, standard deviation, and interquartile range for every numeric predictor. Produce a correlation heatmap or labeled matrix and identify the three strongest absolute correlations with quality.

Group observations by quality score and compare average alcohol, acidity, and residual sugar. Create a figure containing a quality-score histogram, an alcohol-versus-quality box chart, and a scatter plot of alcohol versus density colored or grouped by quality. Save it as `wine_quality_exploration.png`.

## Suggested workflow

```matlab
T = readtable('winequality-red.csv', 'Delimiter', ';');
summary(T);
missingCount = sum(ismissing(T));

numericNames = T.Properties.VariableNames;
X = T{:, numericNames};
correlation = corrcoef(X, 'Rows', 'pairwise');
quality = T.quality;
[groupMean, groupID] = groupsummary(T, 'quality', 'mean', ...
    {'alcohol', 'density', 'residual_sugar'});

figure('Color', 'w');
tiledlayout(2, 2);
nexttile; histogram(quality); xlabel('Quality score'); ylabel('Count');
nexttile; boxchart(categorical(quality), T.alcohol);
xlabel('Quality score'); ylabel('Alcohol');
nexttile; scatter(T.alcohol, T.density, 18, quality, 'filled');
xlabel('Alcohol'); ylabel('Density'); colorbar;
```

If `boxchart` is unavailable, use grouped scatter points or `boxplot`. Discuss correlations as associations only and state that the dataset does not establish causation.

## Validation checklist

Verify that the imported row count matches the source file, the quality variable is treated as an ordinal rating rather than a continuous physical measurement without justification, and all plot labels are readable. Check whether conclusions change when extreme observations are examined separately.

## Extensions

Compare red and white wine files, build a simple baseline quality classifier with a held-out test set, use principal component analysis for visualization, or write a function that generates the report for either file.

## References

1. [UCI Machine Learning Repository: Wine Quality Dataset](https://archive.ics.uci.edu/dataset/186/wine+quality)

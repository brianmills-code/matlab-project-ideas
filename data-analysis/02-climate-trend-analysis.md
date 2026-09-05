# Data Analysis Project 2: Daily Climate Trend Analysis

## Goal

Analyze a multi-year daily temperature record from NOAA Climate Data Online, quantify seasonal and long-term patterns, and communicate uncertainty and data-quality decisions clearly.

## Data source

Download daily observations for one station from the [NOAA Climate Data Online](https://www.ncei.noaa.gov/cdo-web/) service. Select a station with several years of daily maximum and minimum temperature data, and save the downloaded file as `daily_climate.csv`. Record the station identifier, date range, variables, and download date in a project notes file.

## Learning outcomes

This project practices date parsing, quality checks, aggregation by month and year, missing-data handling, linear regression, confidence intervals, and honest interpretation of trends.

## Requirements

Import the daily file and convert dates to `datetime`. Identify missing values and impossible records, documenting whether you exclude or impute them. Derive daily mean temperature from maximum and minimum temperatures when both are present. Calculate monthly means, annual means, and climatological monthly means.

Fit a linear trend to annual mean temperature and report the slope in degrees Celsius per year, the coefficient of determination, and a confidence interval for the slope. Do not interpret a short station record as a global climate estimate. Create a three-panel figure showing the daily series with a moving average, annual means with fitted trend, and climatological monthly means. Save it as `climate_trend_analysis.png`.

## Suggested workflow

```matlab
T = readtable('daily_climate.csv');
T.DATE = datetime(string(T.DATE), 'InputFormat', 'yyyyMMdd');
T.TMean = (T.TMAX + T.TMIN)/2;
valid = isfinite(T.TMAX) & isfinite(T.TMIN) & ...
    T.TMAX >= T.TMIN;
T = T(valid, :);
T.Year = year(T.DATE);
T.Month = month(T.DATE);

annual = groupsummary(T, 'Year', 'mean', 'TMean');
annual.Year = double(annual.Year);
coefficients = polyfit(annual.Year, annual.mean_TMean, 1);
trend = polyval(coefficients, annual.Year);

monthly = groupsummary(T, 'Month', 'mean', 'TMean');
```

Use the exact column names from the downloaded NOAA file after inspecting `T.Properties.VariableNames`. For confidence intervals, use a regression model or bootstrap annual means, and explain the method in the report.

## Validation checklist

Check for duplicate dates, confirm units, inspect the number of valid observations per year, and verify that annual means are not dominated by years with very different coverage. Compare a trend computed from daily observations with a trend computed from annual means and explain why the results may differ.

## Extensions

Compare two nearby stations, analyze heatwave days using a documented threshold, examine precipitation alongside temperature, or test whether the trend is sensitive to excluding years with substantial missingness.

## References

1. [NOAA Climate Data Online](https://www.ncei.noaa.gov/cdo-web/)
2. [NOAA Daily Climate Records](https://www.ncdc.noaa.gov/cdo-web/datatools/records)

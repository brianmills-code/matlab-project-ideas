# Beginner Project 1: Gradebook Analyzer

## Goal

Build a MATLAB script that analyzes a small class gradebook, reports student and class statistics, identifies students who need support, and creates a readable visualization.

## Learning outcomes

This project practices matrices, tables, logical indexing, summary statistics, formatted output, and basic plotting. It also introduces input validation and reproducible calculations.

## Dataset

Start with the following data:

```matlab
names = ["Ava" "Ben" "Chloe" "Diego" "Emma" "Fatima"]';
quiz = [78; 92; 65; 88; 71; 95];
assignment = [85; 89; 72; 91; 68; 94];
exam = [80; 87; 60; 84; 75; 90];
```

## Requirements

Calculate a weighted final grade using 25% quiz, 25% assignment, and 50% exam. Create a table with each name, the three scores, final grade, letter grade, and status. Define status as `Pass` for grades of at least 60 and `Needs support` otherwise. Report the class mean, median, highest grade, lowest grade, and number of passing students.

Create a bar chart of final grades. Add a horizontal line at 60, label both axes, add a title, and use student names as tick labels. Save the figure as `gradebook_summary.png`.

## Suggested implementation

```matlab
weights = [0.25 0.25 0.50];
finalGrade = weights(1)*quiz + weights(2)*assignment + weights(3)*exam;

letter = strings(size(finalGrade));
letter(finalGrade >= 90) = "A";
letter(finalGrade >= 80 & finalGrade < 90) = "B";
letter(finalGrade >= 70 & finalGrade < 80) = "C";
letter(finalGrade >= 60 & finalGrade < 70) = "D";
letter(finalGrade < 60) = "F";

status = repmat("Needs support", size(finalGrade));
status(finalGrade >= 60) = "Pass";
results = table(names, quiz, assignment, exam, finalGrade, letter, status);
```

Use `mean`, `median`, `max`, `min`, and `sum(finalGrade >= 60)` for the summary. Use `assert` to confirm that all score vectors have the same number of rows and that every score is between 0 and 100.

## Validation checklist

The weighted grade should remain between 0 and 100. The number of table rows should equal the number of names. Confirm that every student with a final grade below 60 is marked `Needs support`, and check that the chart contains one bar per student.

## Extensions

Add an input function that accepts a new gradebook table. Compare weighted grades with an unweighted mean. Add a histogram of final grades and export the summary table to `gradebook_results.csv`.

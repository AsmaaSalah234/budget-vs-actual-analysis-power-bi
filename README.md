# budget-vs-actual-analysis-power-bi
## Project Overview

This Power BI project analyzes the difference between Budgeted values and Actual performance across products and time periods.
The goal of this analysis is to track how closely the organization meets its financial targets, identify months with significant variance, and highlight products that either exceeded or underperformed against the budget.
The dashboard enables users to monitor budget performance, variance trends, and product-level performance through interactive visualizations.

## Business Questions

• How does the actual performance compare to the planned budget?

• Which products exceeded the budget and which underperformed?

• What are the monthly trends in budget variance?

• Which months contributed the most to positive or negative variance?

• How does budget performance change across years?

## Data Understanding

The project uses two main datasets:

Budget Table

• Contains monthly budget values for each product.
• The table was not originally structured in a standard tabular format.
• Therefore, it required data transformation (Unpivoting) to make it suitable for analysis.

Actual Table

• Contains daily actual performance values.
• The table was already in a standard tabular format.

Because of the difference in granularity (monthly vs daily), additional dimension tables were required.

Data Model Design

To build a proper Star Schema, two additional dimension tables were created:

• dCalendar – for time intelligence analysis.
• dProduct – to store unique product names.

The final model includes:

• Fact Tables

fActual

fBudget

• Dimension Tables

dCalendar

dProduct

## Data Preparation (Power Query)

The following transformations were performed in Power Query:

1. Data Loading

• Imported both Budget and Actual tables from the Excel data source.

2. Table Renaming

To follow data modeling standards, the tables were renamed:

Budget → fBudget

Actual → fActual

3. Budget Table Transformation

The Budget table was not in a proper tabular format, so it was transformed using:

• Unpivot Only Selected Columns

This step converted the product columns into rows to make the data suitable for analysis.

4. Data Type Adjustments

Ensured correct data types for:
• Dates
• Numeric values
• Text fields

5. Creating the Product Dimension

A Product table was created by referencing the Budget table:

Steps:

Reference the Budget table.

Keep only the Product column.

Remove all other columns.

Remove duplicates.

This created the dProduct dimension table.

Finally, the transformations were applied using Close & Apply.

## DAX Measures
Total Budget

This measure calculates the total planned budget from the budget fact table.
```DAX
Total Budget = SUM(fBudget[Budget])
```
Total Actual

This measure calculates the total actual sales/performance from the transactions table.
```DAX
Total Actual = SUM(fTransactions[Sales])
```
Budget Variance

This measure calculates the difference between actual performance and the planned budget.
```DAX
Budget Variance = [Total Actual] - [Total Budget]
```
Budget Variance %

This measure calculates the percentage difference between actual and budget values.
```DAX
Budget Variance % = DIVIDE([Budget Variance], [Total Budget])
```
Budget SPLY (Same Period Last Year)

This measure calculates the budget for the same period in the previous year, which helps in comparing performance year-over-year.
```DAX
Budget SPLY = CALCULATE([Total Budget], SAMEPERIODLASTYEAR(dCalendar[Date]))
```
## Data Model Relationships

Relationships were established between the tables in Model View:

dCalendar → connected to both fActual and fBudget via Date.

dProduct → connected to both fact tables via Product.

This structure follows a Star Schema model, improving performance and analytical flexibility.

Foreign keys in the fact tables were hidden to keep the model clean.

## Dashboard Visualizations
The dashboard includes multiple visuals to analyze performance:
KPI Cards
Show high-level metrics:

• Total Budget
• Total Actual
• Budget Variance
• Budget Variance %

Budget vs Actual by Month (Line & Column Chart)

Displays:

• Monthly Budget
• Monthly Actual values
• Budget Variance %

This helps identify periods of overperformance or underperformance.

Product Performance Table

Compares each product:

• Budget
• Actual performance
• Budget variance

## Example insights:

• Carlota exceeded the budget with a positive variance.
• Aspen slightly underperformed relative to the budget.

Budget Variance by Month (Bar Chart)

Shows which months contributed the most to performance differences.

Example observations:

• November and December show strong positive variance.
• June and August show negative variance.

## Key Insights
1. Overall Budget Performance
The total actual performance is very close to the planned budget, indicating relatively accurate forecasting.

2. Product Performance
• Carlota performed above expectations.
• Quad also slightly exceeded the budget
• Aspen underperformed compared to the planned target.

4. Seasonal Trends
Some months demonstrate higher variance than others, suggesting seasonal demand or operational factors influencing performance.

## Recommendations
Improve Budget Accuracy

Investigate months with high negative variance to improve forecasting accuracy.

Analyze Product-Level Performance

Focus on why Aspen underperformed compared to other products.

Possible factors:

• pricing strategy
• demand fluctuations
• supply constraints

Leverage High-Performing Periods

Months with strong positive variance could indicate successful promotions or seasonal demand. These periods should be analyzed and replicated.

Tools Used

• Power BI – Data Modeling & Dashboard Development
• Power Query – Data Cleaning & Transformation
• DAX – Measures and Time Intelligence
• Excel – Data Source

## Dashboard
<img width="1451" height="807" alt="variance analysis" src="https://github.com/user-attachments/assets/7ae23b33-c648-46f8-92d5-90d04523f9ff" />
<img width="1450" height="811" alt="sales Dashboard 2026" src="https://github.com/user-attachments/assets/e5d52b0f-34ef-4050-a048-d28e0abb524d" />


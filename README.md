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

## Calendar Table Creation (DAX)

To support time-based analysis, a Calendar table was created using DAX.
'''DAX
dCalendar = CALENDARAUTO()
'''

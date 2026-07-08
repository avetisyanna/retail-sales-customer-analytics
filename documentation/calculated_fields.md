# Calculated Fields

This project uses Tableau calculated fields to support sales performance analysis, customer analytics, and period-over-period comparison.

The calculations were used to clean the dataset, define valid sales, build KPI cards, compare time periods, and support customer-level analysis.

## Core Calculations

### Sales

```tableau
[Quantity] * [Unit Price]
```

This calculation creates the revenue value for each transaction row.

### Is Cancelled

```tableau
LEFT(STR([Invoice No]), 1) = "C"
```

Some invoice numbers include the letter `C`, which indicates cancelled orders. This calculation helps identify cancelled transactions.

### Valid Sale

```tableau
NOT [Is Cancelled]
AND [Quantity] > 0
AND [Unit Price] > 0
AND NOT ISNULL([Customer ID])
```

This filter keeps only valid sales transactions for the main dashboard analysis.

### Max Order Date

```tableau
{ FIXED : MAX(DATE([Invoice Date])) }
```

This LOD expression identifies the latest order date in the dataset and supports dynamic period-based calculations.

## Period-Based KPI Calculations

The sales dashboard includes current-period and previous-period calculations for:

- Sales
- Orders
- Customers
- Average invoice value

These calculations make it possible to compare business performance across selected periods such as last week, last month, last quarter, and last year.

## Period-over-Period Metrics

The dashboard uses period-over-period comparison metrics such as:

- Sales growth %
- Orders growth %
- Customer growth %
- Average invoice growth %

Directional indicators are also used to show whether performance increased or decreased.

Example indicators:

```tableau
IF ZN([Sales PoP %]) = 0 THEN ""
ELSEIF [Sales PoP %] > 0 THEN "▲"
ELSEIF [Sales PoP %] < 0 THEN "▼"
END
```

## Time-Based Calculations

The sales dashboard includes time-based purchasing behavior analysis.

### Order Hour

```tableau
DATEPART('hour', [Invoice Date])
```

### Order Weekday

```tableau
DATENAME('weekday', [Invoice Date])
```

These fields help analyze purchasing patterns by weekday and hour.

## Product Performance Calculations

Product-level calculations are used to analyze product demand and revenue contribution.

### Product Orders

```tableau
{ FIXED [Stock Code] : COUNTD([Invoice No]) }
```

### Product Quantity Sold

```tableau
{ FIXED [Stock Code] : SUM([Quantity]) }
```

### Product Revenue

```tableau
{ FIXED [Stock Code] : SUM([Sales]) }
```

These calculations help compare products by revenue, order count, and quantity sold.

## Customer Analytics Calculations

The customer dashboard includes calculations related to customer behavior and customer value.

Main customer metrics:

- Total customers
- Average revenue per customer
- Purchase frequency
- Recency
- Frequency
- Monetary value
- Customer segments
- Retention rate

## RFM Analysis

RFM analysis evaluates customers using three dimensions:

| Metric | Meaning |
|---|---|
| Recency | How recently a customer purchased |
| Frequency | How often a customer purchases |
| Monetary | How much revenue a customer generates |

RFM analysis helps identify high-value customers, loyal customers, inactive customers, and customers at risk of churn.

## Tableau Concepts Used

- Calculated fields
- Parameters
- Level of Detail expressions
- Table calculations
- Conditional formatting
- KPI cards
- Dashboard actions
- Dynamic period comparison
- RFM analysis
- Cohort retention analysis

# 📊 Sales Performance & Revenue Analytics

An end-to-end **Data Analytics project** focused on analyzing large-scale sales transaction data to understand **revenue, profitability, customer behavior, product performance, regional trends, returns, and target achievement**.

The project demonstrates a complete analytics workflow using **SQL, Excel, Power Query, and Power BI**, starting from raw transactional data and ending with an interactive business intelligence dashboard.

---

## 🎯 Business Problem

A large retail/e-commerce business generates hundreds of thousands of transactions across multiple regions, customers, and product categories.

Management wants to answer important business questions such as:

- How much revenue is the company generating?
- Which products and categories generate the most revenue?
- Which regions are performing well?
- Which products are most profitable?
- What is the monthly revenue trend?
- How does actual revenue compare with targets?
- Who are the highest-value customers?
- What is the customer return rate?
- Which categories have high sales but low profitability?
- Which regions require business attention?
- How are discounts affecting profitability?

This project converts raw transactional data into **actionable business insights** using SQL, Excel, and Power BI.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **SQL / PostgreSQL** | Data querying, transformation and KPI analysis |
| **Excel** | Data validation, Pivot Tables and business analysis |
| **Power Query** | Data cleaning and transformation |
| **Power BI** | Interactive dashboards and visualization |
| **DAX** | Measures, KPIs and calculations |
| **Python** | Dataset generation and preprocessing |
| **Git & GitHub** | Version control and project documentation |

---

## 📦 Dataset

The project uses a large-scale synthetic retail dataset designed to simulate a realistic business environment.

### Dataset Size

- **500K+ sales transactions**
- **50K+ customers**
- **5K+ products**
- Multiple regions and cities
- Multiple product categories
- Historical sales data
- Customer returns
- Monthly sales targets

### Data Model

The project follows a relational/star-schema structure.

```text
                    ┌─────────────────┐
                    │   dim_customers │
                    │─────────────────│
                    │ customer_id     │
                    │ customer_name   │
                    │ gender          │
                    │ age             │
                    │ city            │
                    │ state           │
                    │ segment         │
                    └────────┬────────┘
                             │
                             │
┌──────────────┐       ┌─────▼──────┐       ┌───────────────┐
│ dim_products │──────►│ fact_sales │◄──────│   dim_date    │
│──────────────│       │────────────│       │───────────────│
│ product_id   │       │ order_id   │       │ date          │
│ product_name │       │ customer_id│       │ year          │
│ category     │       │ product_id │       │ quarter       │
│ subcategory  │       │ order_date │       │ month         │
│ brand        │       │ quantity   │       │ week          │
│ cost_price   │       │ sales      │       └───────────────┘
└──────────────┘       │ cost       │
                       │ profit     │
                       │ discount   │
                       └─────┬──────┘
                             │
                    ┌────────▼────────┐
                    │ fact_returns    │
                    │─────────────────│
                    │ return_id       │
                    │ order_id        │
                    │ product_id      │
                    │ return_date     │
                    │ return_qty      │
                    │ return_reason   │
                    └─────────────────┘
```

---

# 📁 Project Structure

```text
Sales-Performance-Revenue-Analytics/
│
├── data/
│   ├── raw/
│   │   ├── fact_sales.csv
│   │   ├── dim_customers.csv
│   │   ├── dim_products.csv
│   │   ├── dim_date.csv
│   │   ├── dim_region.csv
│   │   ├── fact_returns.csv
│   │   └── fact_targets.csv
│   │
│   └── processed/
│       └── cleaned_sales_data.csv
│
├── sql/
│   ├── 01_data_validation.sql
│   ├── 02_basic_analysis.sql
│   ├── 03_revenue_analysis.sql
│   ├── 04_product_analysis.sql
│   ├── 05_customer_analysis.sql
│   ├── 06_regional_analysis.sql
│   ├── 07_profitability_analysis.sql
│   ├── 08_return_analysis.sql
│   └── 09_advanced_kpis.sql
│
├── excel/
│   └── Sales_Analysis.xlsx
│
├── powerbi/
│   └── Sales_Performance_Dashboard.pbix
│
├── python/
│   ├── generate_dataset.py
│   └── data_validation.py
│
├── screenshots/
│   ├── executive_dashboard.png
│   ├── product_dashboard.png
│   ├── customer_dashboard.png
│   └── regional_dashboard.png
│
└── README.md
```

---

# 📊 Key Business KPIs

The project analyzes several important business metrics.

### Revenue KPIs

- Total Revenue
- Net Revenue
- Monthly Revenue
- Year-over-Year Revenue Growth
- Month-over-Month Growth
- Average Order Value
- Revenue by Category
- Revenue by Region

### Profitability KPIs

- Total Profit
- Gross Profit
- Profit Margin %
- Profit per Order
- Profit by Category
- Profit by Product
- Profit by Region
- Discount vs Profitability

### Customer KPIs

- Total Customers
- New Customers
- Returning Customers
- Customer Revenue
- Average Customer Value
- Top Customers
- Customer Segment Performance

### Product KPIs

- Units Sold
- Top-Selling Products
- Bottom-Selling Products
- Category Performance
- Subcategory Performance
- Product Profitability

### Operational KPIs

- Return Rate
- Returned Quantity
- Returned Revenue
- Return Reasons
- Target Achievement
- Regional Performance

---

# 🔍 SQL Analysis

SQL is used extensively to extract business insights from the transactional database.

### SQL Concepts Used

- SELECT
- WHERE
- GROUP BY
- HAVING
- ORDER BY
- CASE WHEN
- INNER JOIN
- LEFT JOIN
- CTEs
- Subqueries
- Aggregate Functions
- Window Functions
- RANK()
- DENSE_RANK()
- ROW_NUMBER()
- LAG()
- LEAD()
- Running Totals
- Date Functions
- Conditional Aggregation

### Example: Revenue by Category

```sql
SELECT
    p.category,
    SUM(s.sales) AS total_revenue,
    SUM(s.profit) AS total_profit
FROM fact_sales s
JOIN dim_products p
    ON s.product_id = p.product_id
GROUP BY p.category
ORDER BY total_revenue DESC;
```

### Example: Product Ranking

```sql
SELECT
    product_id,
    product_name,
    category,
    revenue,
    RANK() OVER (
        PARTITION BY category
        ORDER BY revenue DESC
    ) AS category_rank
FROM product_sales;
```

### Example: Monthly Revenue Growth

```sql
WITH monthly_sales AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(sales) AS revenue
    FROM fact_sales
    GROUP BY 1
)

SELECT
    month,
    revenue,
    LAG(revenue) OVER (
        ORDER BY month
    ) AS previous_month_revenue,

    ROUND(
        (
            revenue -
            LAG(revenue) OVER (ORDER BY month)
        )
        /
        NULLIF(
            LAG(revenue) OVER (ORDER BY month),
            0
        ) * 100,
        2
    ) AS mom_growth_percentage

FROM monthly_sales
ORDER BY month;
```

---

# 📗 Excel Analysis

Excel is used for data validation, exploratory analysis, and business reporting.

### Excel Features Used

- XLOOKUP
- SUMIFS
- COUNTIFS
- IF
- IFS
- IFERROR
- INDEX/MATCH
- Date Functions
- Pivot Tables
- Pivot Charts
- Conditional Formatting
- Power Query
- Data Validation

### Excel Analysis Includes

- Monthly revenue analysis
- Product performance
- Category performance
- Customer segmentation
- Regional sales
- Profitability analysis
- Return analysis
- Target vs actual performance

---

# 📈 Power BI Dashboard

The final Power BI solution contains multiple interactive dashboard pages.

## 1️⃣ Executive Overview

The executive dashboard provides a high-level view of business performance.

### KPIs

```text
Total Revenue
Total Profit
Profit Margin %
Total Orders
Total Customers
Units Sold
Return Rate
Average Order Value
```

### Visualizations

- Monthly Revenue Trend
- Revenue vs Target
- Revenue by Region
- Revenue by Category
- Profit Margin by Category
- Top 10 Products

---

## 2️⃣ Product Performance

This dashboard focuses on product and category performance.

### Analysis

- Top 10 Products
- Bottom 10 Products
- Revenue by Category
- Profit by Category
- Units Sold
- Product Contribution %
- Discount vs Profit Margin

---

## 3️⃣ Customer Analysis

This page analyzes customer purchasing behavior.

### Analysis

- New vs Returning Customers
- Customer Segments
- Top Customers
- Customer Revenue
- Average Order Value
- Purchase Frequency
- Revenue by Customer Segment

---

## 4️⃣ Regional Performance

This dashboard analyzes geographic performance.

### Analysis

- Revenue by State
- Revenue by City
- Profit by Region
- Orders by Region
- Target Achievement
- Return Rate by Region

---

# 📐 Important DAX Measures

### Total Revenue

```DAX
Total Revenue =
SUM(fact_sales[sales])
```

### Total Profit

```DAX
Total Profit =
SUM(fact_sales[profit])
```

### Profit Margin

```DAX
Profit Margin % =
DIVIDE(
    [Total Profit],
    [Total Revenue],
    0
)
```

### Total Orders

```DAX
Total Orders =
DISTINCTCOUNT(fact_sales[order_id])
```

### Average Order Value

```DAX
Average Order Value =
DIVIDE(
    [Total Revenue],
    [Total Orders],
    0
)
```

### Revenue Growth

```DAX
Revenue Growth % =
VAR CurrentRevenue = [Total Revenue]

VAR PreviousRevenue =
    CALCULATE(
        [Total Revenue],
        DATEADD(
            dim_date[date],
            -1,
            YEAR
        )
    )

RETURN
DIVIDE(
    CurrentRevenue - PreviousRevenue,
    PreviousRevenue,
    0
)
```

---

# 💡 Key Business Questions

The analysis attempts to answer:

### Revenue

1. What is the total revenue?
2. What is the monthly revenue trend?
3. Which months generated the highest revenue?
4. What is the YoY revenue growth?
5. Which regions contribute the most revenue?

### Products

6. Which categories generate the most revenue?
7. Which products generate the most profit?
8. Which products have high sales but low margins?
9. Which products are underperforming?
10. Which categories have the highest unit sales?

### Customers

11. Who are the highest-value customers?
12. What percentage of revenue comes from returning customers?
13. Which customer segments generate the most revenue?
14. What is the average order value?

### Regions

15. Which states generate the most revenue?
16. Which regions have the highest profit margin?
17. Which regions are failing to meet their targets?
18. Which regions have higher return rates?

### Operations

19. What is the overall return rate?
20. Which products have the highest return rate?
21. What are the most common return reasons?
22. How do discounts affect profitability?

---

# 🧠 Business Insights

The final analysis focuses not only on reporting numbers but also on identifying actionable patterns.

Examples of insights that can be generated from the analysis:

- High-revenue categories may not always have the highest profit margins.
- Heavy discounting can increase sales volume while reducing profitability.
- A small percentage of customers may contribute a significant share of total revenue.
- Regional performance can vary significantly across markets.
- Products with high sales volume can still have low profitability.
- High-return products may require investigation into pricing, quality, or customer expectations.
- Monthly trends can reveal seasonal changes in customer demand.

> **Note:** Final insights should be based on the actual calculated results from the dataset rather than predetermined conclusions.

---

# 🔄 End-to-End Workflow

```text
Raw Transaction Data
        ↓
Data Cleaning
        ↓
Data Validation
        ↓
SQL Database
        ↓
SQL Analysis
        ↓
Excel Analysis
        ↓
Power BI Data Model
        ↓
DAX Measures
        ↓
Interactive Dashboard
        ↓
Business Insights
        ↓
Business Recommendations
```

---

# 🎯 Skills Demonstrated

### Data Analysis

- Exploratory Data Analysis
- Data Cleaning
- Data Transformation
- Data Validation
- KPI Analysis
- Business Analysis

### SQL

- Joins
- CTEs
- Subqueries
- Aggregations
- Window Functions
- Ranking
- Time-Series Analysis

### Excel

- Advanced Formulas
- Pivot Tables
- Power Query
- Data Validation
- Business Reporting

### Power BI

- Data Modeling
- DAX
- KPI Cards
- Interactive Visualizations
- Drill-Down
- Filters & Slicers
- Dashboard Design

### Business Intelligence

- Revenue Analysis
- Profitability Analysis
- Customer Analytics
- Product Analytics
- Regional Analytics
- Target vs Actual Analysis

---

# 📸 Dashboard Preview

### Executive Dashboard

Add your Power BI dashboard screenshot here:

```text
screenshots/executive_dashboard.png
```

### Product Performance

```text
screenshots/product_dashboard.png
```

### Customer Analysis

```text
screenshots/customer_dashboard.png
```

### Regional Performance

```text
screenshots/regional_dashboard.png
```

---

# 🚀 How to Use This Project

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/Sales-Performance-Revenue-Analytics.git
```

### 2. Load the dataset

Import the CSV files from:

```text
data/raw/
```

into PostgreSQL or another compatible SQL database.

### 3. Run SQL scripts

Execute the scripts from:

```text
sql/
```

in the recommended order.

### 4. Open Excel

Open:

```text
excel/Sales_Analysis.xlsx
```

to explore the spreadsheet analysis.

### 5. Open Power BI

Open:

```text
powerbi/Sales_Performance_Dashboard.pbix
```

and refresh the data connection.

---

# 📌 Project Outcomes

This project demonstrates the ability to:

- Work with large datasets
- Clean and validate raw data
- Write advanced SQL queries
- Calculate business KPIs
- Perform profitability analysis
- Analyze customer behavior
- Analyze product performance
- Build interactive Power BI dashboards
- Use Excel for business analysis
- Translate data into actionable business insights

---

# 👨‍💻 Author

**Karan Purkait**

Data Analyst | SQL | Excel | Power BI | Python

---

## ⭐ If you find this project useful

Feel free to explore the repository, use the analysis approach for learning, and connect with me for discussions around **Data Analytics, SQL, Excel, Power BI, and Business Intelligence**.

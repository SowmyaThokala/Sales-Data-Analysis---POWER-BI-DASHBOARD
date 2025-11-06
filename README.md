Dashboard Link: https://app.powerbi.com/groups/me/reports/6ddbbe39-34f0-4659-8001-14012978a657/978b4a88ab811c76c219?experience=power-bi

## Dashboard Objective:
This Power BI report was developed for ElectroHub, a multi-category retail company that sells products across electronics, footwear, clothing, home appliances, accessories, kitchenware, bags, and personal care.

The goal of this project is to help the management understand sales trends, profitability, customer buying patterns, and discount performance across different product categories and time periods — ultimately aiding in data-driven business decisions.

## Problem Statement:

ElectroHub’s management team wanted to gain deeper insights into their sales and profitability. They needed a dynamic and interactive dashboard that would answer key business questions such as:

Which are the Top 5 and Bottom 5 products by Sales, Profit, and Quantity Sold?

How do Sales trends vary over time (yearly, quarterly, monthly, and daily)?

What is the relationship between Sales and Profit?

How can users compare sales/profit/quantity between any two selected periods?

What is the Average Discount offered for each promotion category?

What is the Total Number of Orders?

Can we view all transaction details (Sales, Profit, Discount, Net Sales, etc.) filtered by Product, Date, Customer, or Promotion Category?

What are the Sales by City?

## Steps Followed:

### Step 1: Data Import and Model Design

Imported the following datasets into Power BI Desktop:

Dim_Customers

Dim_Product

Dim_Promotion

Fact_Sales

Created relationships between tables using Model View:

One-to-Many relationships between Fact table and each Dimension table (Customer ID, Product ID, Promotion ID).

Ensured relationship integrity with referential integrity enabled.

### Step 2: Data Cleaning & Transformation

In Dim_Promotion, the Discount column had a mix of text and numbers.Created a conditional column to extract numeric discount values and calculate Discount %.

In Fact Table, Price per Unit had null values.Performed a Left Outer Join between Fact Table and Dim_Product on Product ID to fill missing prices.Replaced null values in Discount % with 0.

### Step 3: Calculated Columns & Measures
Custom Columns

(a) Total Sales

Total Sales = [Units Sold] * [Price per Unit]


(b) Profit (Assumed 10% of Net Sales)

Profit = [Net Sales] * 0.1

Calculated Measures

(a) Total Orders

Total Orders = COUNTROWS('Fact_Sales')


(b) Total Profit

Total Profit = SUM('Fact_Sales'[Profit])


(c) Total Quantity Sold

Total Quantity Sold = SUM('Fact_Sales'[Quantity])


(d) Total Net Sales

Total Net Sales = SUM('Fact_Sales'[Net Sales])


(e) Average Discount

Average Discount = AVERAGE('Dim_Promotion'[Discount %])


(f) Compare Between Two Date Ranges

Created two separate Date Tables using CALENDARAUTO():

DateTable1 = CALENDARAUTO()
DateTable2 = CALENDARAUTO()


Created relationships:

DateTable1[Date] → Fact_Sales[Order Date]

DateTable2[Date] → inactive relationship with Fact_Sales[Order Date]

Used USERELATIONSHIP to activate the second date filter dynamically:

Total Sales (Date2) =
CALCULATE(
    SUM('Fact_Sales'[Net Sales]),
    USERELATIONSHIP('DateTable2'[Date], 'Fact_Sales'[Order Date])
)

### Step 4: Visualizations
Requirement 1: Top/Bottom 5 Products

Visual: Stacked Bar Chart

Applied Top N Filter to show Top 5 and Bottom 5 Products by:

Sales

Profit

Quantity Sold

Customized formatting (removed X-axis title, centered titles, added legends).

Requirement 2: Sales Trend Over Time

Visual: Line Chart

Used drill-down feature to view by Year → Quarter → Month → Day.

Requirement 3: Sales vs Profit Relationship

Visual: Scatter Chart

X-axis = Sales

Y-axis = Profit

Bubble size = Quantity Sold

Color = Product Category

Requirement 4: Compare Two Periods

Created two slicers (Date Filter 1 and Date Filter 2).

Linked them to the respective Date Tables.

Used measures with USERELATIONSHIP for side-by-side comparison of Sales, Profit, and Quantity.

Requirement 5: Average Discount by Promotion Category

Visual: Stacked Bar Chart

Y-axis = Promotion Category

X-axis = Average Discount %

Requirement 6: Total Orders

Visual: Card Visual showing Total Orders.

Requirement 7: Order-Level Detail View

Visual: Table Visual

Added columns: Sales, Profit, Discount, Net Sales, etc.

Added slicers for:

Product Name

Customer Name

Date Filter

Promotion Category

To make slicers interact dynamically, created:

Sum Dim = SUM('Fact_Sales'[Net Sales])


Added to slicer filter pane → “is not blank” option enabled for dynamic interaction.

Requirement 8: Sales by City

Visual: Map or Filled Map

Location = City

Size = Sales Amount

Findings and Insights
1) Sales and Profit has linear relationship with each other. As the sales are increasing profits are also increasing and vice versa.
2) Weekend Flash sale is offering high discounts compared to other categories.
3)Highest number of sales recorded on Novemeber 25,2022.


### Tools & Techniques Used:
Category	      Tool/Concept
Data Visualization -	Power BI Desktop
Data Cleaning -	Power Query Editor
Data Modeling - Star Schema (Fact + Dimensions)
DAX Functions -	CALCULATE, USERELATIONSHIP, ALL, SUM, AVERAGE, COUNTROWS, TOPN
Interactivity -	Slicers, Drill-downs, Dynamic Filters
Publishing -	Power BI Service

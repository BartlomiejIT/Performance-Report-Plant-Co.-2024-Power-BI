# Performance Report – Plant Co. (2024)

## Power BI | Sales Analysis | DAX | YTD vs PYTD | Customer Segmentation

---

## Table of Contents

- [Objective](#objective)  
- [User Story](#user-story)  
- [Data Sources](#data-sources)  
- [Tools](#tools)  
- [Design](#design)  
- [Dashboard Mockup](#dashboard-mockup)  
- [Development](#development)  
  - [Pseudocode](#pseudocode)  
  - [Data Review](#data-review)  
  - [Data Preparation (Power Query)](#data-preparation-power-query)  
  - [DAX Measures](#dax-measures)  
- [Testing](#testing)  
- [Visualization](#visualization)  
- [Analysis](#analysis)  
  - [Insights](#insights)  
  - [Findings](#findings)  
  - [Recommendations](#recommendations)  
  - [Action Plan](#action-plan)  
- [Business Value](#business-value)  
- [Use Case Scenario](#use-case-scenario)  
- [Role in the Project](#role-in-the-project)  
- [Conclusion](#conclusion)

---

## Objective

This dashboard simulates a real-world use case for a mid-size B2B retail company in the horticulture sector, designed to support data-informed sales and operations planning.

The goal of this report is to evaluate Plant Co.'s sales and profitability performance in 2024 and compare it to the same period in the previous year (PYTD). By analyzing year-to-date trends across countries, products, and customer segments, the report highlights key drivers of growth and underperformance.

It supports strategic decisions by enabling:

- Identification of regional and product-level performance gaps  
- Insights into customer profitability and account potential  
- Data-driven planning for sales, marketing, and operations teams

The report equips stakeholders with a clear, timely view of business health and actionable priorities.

---

## User Story

As a business manager at Plant Co., I want to understand how current sales and profitability compare to the previous year, so I can:

- Spot negative trends early and take corrective action  
- Identify high-performing regions, products, and accounts  
- Prioritize resources and align initiatives with revenue goals  

This enables smarter, faster decisions across sales, operations, and marketing functions based on reliable and timely data.

---

## Data Sources

What is data coming from? The data is sourced from Kaggle (an Excel extract), [see here to find it]().

| Table         | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| Fact_Sales    | Transactional data including quantity, sales (USD), and cost of goods sold  |
| Dim_Product   | Product attributes: family, size, and type                                  |
| Dim_Account   | Customer location, country, and segmentation info                           |
| Dim_Date      | Calendar table used for time intelligence (YTD, PYTD)                       |
| Slic_Values   | Parameter table for dynamic metric switching (Sales, GP, Quantity)          |

---

## Tools

| Tool       | Purpose                                               |
|------------|-------------------------------------------------------|
| Power BI   | Interactive reporting, DAX modeling, data visualization |
| Excel      | Initial data exploration                              |
| DAX        | Time-based calculations, KPI definitions, SWITCH logic |

---

## Design

The dashboard was designed with the following key questions in mind:

1. What is the company’s current YTD performance vs previous year?
2. Which countries or customer segments are underperforming?
3. What is the profitability profile across product types and accounts?
4. How does performance evolve monthly, and what are the drivers?

---

## Dashboard Mockup

Visual components include:

- KPI cards
- Waterfall charts
- Bar-line combo charts
- Treemaps and scatter plots

![image](https://github.com/user-attachments/assets/0f380a5e-f9e1-4bea-b2ed-0684e8371337)

---

## Development

### Pseudocode

1. Review transactional and dimension tables  
2. Define base measures (Sales, Quantity, Gross Profit)  
3. Create time intelligence metrics (YTD, PYTD)  
4. Add comparison measures (YTD vs PYTD)  
5. Create SWITCH logic for metric selection  
6. Build visuals with filters and interactions  
7. Add tooltips, sorting, labels, and formatting  
8. Test calculations for business logic  
9. Present findings, insights, and recommendations

---

### Data Review

- Sales data includes revenue and COGS at a transactional level  
- Product table contains product categorization into Landscape, Indoor, Outdoor  
- Dim_Date includes full calendar range required for time intelligence  
- No major data quality issues detected – data is clean and analytical columns are consistent

---

### Data Preparation (Power Query)

In the Power Query layer, several preprocessing steps were applied to ensure data quality and usability:

- **Removed duplicates** from `Dim_Product` and `Dim_Account` to maintain key uniqueness  
- **Renamed columns** in `Dim_Account` for better consistency and readability  
- Applied standard steps such as `Promoted Headers` and `Changed Type` to align schema with data model requirements  

These transformations ensured clean and reliable dimension tables for relationship building and accurate aggregation in Power BI.

---

### DAX Measures

#### Base Measures

```sql
Sales = SUM(Fact_Sales[Sales_USD])

Quantity = SUM(Fact_Sales[Quantity])

Gross Profit = SUM(Fact_Sales[Sales_USD]) - SUM(Fact_Sales[COGS_USD])

GP% = DIVIDE([Gross Profit], [Sales])
```

#### YTD & PYTD

```sql
YTD_Sales = TOTALYTD([Sales], Dim_Date[Date])
PYTD_Sales = CALCULATE([YTD_Sales], SAMEPERIODLASTYEAR(Dim_Date[Date]))

YTD_GrossProfit = TOTALYTD([Gross Profit], Dim_Date[Date])
PYTD_GrossProfit = CALCULATE([YTD_GrossProfit], SAMEPERIODLASTYEAR(Dim_Date[Date]))

YTD_Quantity = TOTALYTD([Quantity], Dim_Date[Date])
PYTD_Quantity = CALCULATE([YTD_Quantity], SAMEPERIODLASTYEAR(Dim_Date[Date]))
```

#### Difference

```sql
YTD vs PYTD Sales = [YTD_Sales] - [PYTD_Sales]
YTD vs PYTD GP = [YTD_GrossProfit] - [PYTD_GrossProfit]
YTD vs PYTD Quantity = [YTD_Quantity] - [PYTD_Quantity]
```

#### Dynamic SWITCH

```sql
Selected YTD = 
SWITCH(
    SELECTEDVALUE(Slic_Values[Metric]),
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Gross Profit", [YTD_GrossProfit]
)

Selected PYTD = 
SWITCH(
    SELECTEDVALUE(Slic_Values[Metric]),
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Gross Profit", [PYTD_GrossProfit]
)

YTD vs PYTD (Selected) = [Selected YTD] - [Selected PYTD]
```

---

## Testing

- Verified YTD and PYTD match monthly calendar definitions  
- Validated total vs breakdown in visuals (e.g., Sales by Month = sum of Sales by Country)  
- SWITCH logic tested by toggling slicer between metrics  
- Measures return correct values even with slicers applied

---

## Visualization

The dashboard includes:
- KPI cards for YTD, PYTD, and GP%  
- Waterfall chart showing contribution by month, country and product  
- Combo chart for product type performance  
- Treemap highlighting worst-performing countries  
- Scatter plot for customer GP% vs volume segmentation

![image](https://github.com/user-attachments/assets/a1a71197-cd0d-49d3-be48-8729fea41c4c)

---

## Analysis

### Insights

This analysis provides a multidimensional view of performance using sales revenue, quantity sold, and gross profit across two full years (2023 vs 2024). Insights are driven by time series trends, country-level contribution, product type breakdown and account-level segmentation.

Key insights include:

- **Sales, Quantity, and Gross Profit all declined in 2024 vs 2023**, with Sales showing the sharpest drop:
  - **Sales** YTD: ↓ from **13M to 3.57M**
  - **Quantity** YTD: ↓ from **555K to 148K**
  - **Gross Profit** YTD: ↓ from **5.15M to 1.40M**  
  Despite these drops, **GP% remained stable at ~39%**, suggesting good margin control and cost management.

- **April is the weakest month in both years across all metrics**, pointing to possible seasonal or operational disruption.

- **Landscape products remain stable** in both years and all metrics.  
  **Indoor and Outdoor** product types are more volatile and fluctuate significantly, especially in Q2.

- **Persistent underperformance in key countries**:  
  - **Canada, Colombia, Germany, Croatia** repeatedly appear in the bottom YTD vs PYTD contributors for both 2023 and 2024.  
  - Their presence across all three KPIs signals systemic issues rather than random shortfalls.

- **Customer profitability patterns remained largely unchanged** year over year.  
  Scatterplots reveal consistent clustering in the high-GP% but low-YTD-value quadrant.  
  This indicates limited success in account growth or upsell efforts between years.

---

### Findings

- **Performance drops are consistent across all KPIs**, confirming a widespread contraction rather than metric-specific anomalies.

- **April acts as a breaking point** in both years, showing steep declines.  
  This month could reflect seasonality, supply constraints, or market behavior worth further investigation.

- **The consistent GP% (~39%)** across both years despite lower volume and sales suggests the company preserved profitability at a unit level and avoided cost erosion or aggressive discounting.

- **Product performance diverges by category**:  
  - Landscape is a strong performer across metrics and years.  
  - Indoor and Outdoor categories face volatility, suggesting inconsistent demand, channel misalignment, or lack of targeted campaigns.

- **Low migration between customer profitability segments**:  
  Scatterplots show few accounts moving from low-value to high-value, implying stagnation in account development.

---

### Recommendations

#### 1. Regional Sales Optimization

- Conduct a deep-dive root cause analysis in **Canada and Colombia** to identify whether the decline is caused by distribution issues, competitive pressure, or customer attrition.
- Initiate **localized incentive programs** and **reactivation campaigns** for dormant or declining accounts in those countries.
- Implement **monthly monitoring KPIs** for YTD delta to catch emerging issues earlier.

#### 2. Product Portfolio Strategy

- Reinvest in **Landscape products**, which demonstrate high stability and customer retention.
- Perform **customer preference analysis** for Indoor and Outdoor categories to inform redesign, repackaging, or repositioning efforts.
- Introduce **bundle pricing or promotional offers** to drive volume without sacrificing gross margin.

#### 3. Customer Account Segmentation

- Segment all accounts by **GP% and sales volume**, and prioritize **"high-margin / low-volume" customers** for targeted upselling campaigns.
- Deploy **predictive modeling** (e.g., churn prediction or LTV estimation) for accounts with declining performance.
- Offer **relationship-based incentives** (e.g., loyalty pricing, strategic discounts) to retain top-value accounts.

#### 4. Operational Adjustments

- Evaluate supply chain responsiveness for Q2/Q3, as volatility in April may point to fulfillment or forecasting gaps.
- Integrate this report into **monthly operations meetings** to align logistics and production with actual demand trends.
- Use **scatter plot quadrant analysis** to define client segments for growth vs retention vs reactivation.

---

## Action Plan

- Prepare follow-up presentation for Sales and Ops teams  
- Schedule deep-dives into target countries and product families  
- Share segmented customer lists with Account Managers  
- Track post-recommendation KPIs over the next 3–6 months

---

## Business Value

- Provides clear visibility on year-over-year performance
- Enables strategic interventions across teams
- Supports profitability-driven account and product decisions
- Acts as foundation for scalable, KPI-based monitoring

---

## Use Case Scenario

- Sales managers use this dashboard weekly to track regional performance and trigger follow-ups on underperforming accounts.  
- Product managers rely on product-type performance trends to adjust campaigns and align production with demand.  
- Executives use KPI comparisons to benchmark progress and allocate resources strategically.

---

## Role in the Project

- Designed and implemented the complete data model in Power BI  
- Developed all DAX measures, including dynamic switching logic  
- Cleaned and transformed source data in Power Query (deduplicated, renamed)  
- Designed dashboard layout for stakeholders with final dark theme  
- Conducted insight generation and documented strategic recommendations

---

## Conclusion

This report consolidates key performance indicators into a dynamic and accessible format, offering a year-over-year comparison of Plant Co.'s sales, quantity, and gross profit.

Stakeholders are equipped to:

- Detect early signs of commercial underperformance
- Make region- and product-specific decisions
- Take action on customer segmentation insights
  
With consistent GP% and visible drops in volume, strategic focus should shift toward recovering quantity while retaining profit margins. 

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

This analysis provides a multidimensional view of sales and profitability using both transactional and dimensional data. The primary insights were obtained by leveraging time intelligence (YTD vs PYTD), product hierarchy, geographic distribution, and customer account data.

Key insights include:

- **Sales volume has slightly declined** compared to the same period last year, with a negative YTD vs PYTD delta of approximately $135,000.
- **Gross profit margin remains strong (39.15%)**, indicating sound pricing and cost management practices despite the slight revenue drop.
- **April represents a significant turning point** with the sharpest drop in revenue and profitability, disrupting an otherwise stable trend.
- The **Landscape product family demonstrates consistent contribution** to revenue and profit across all months.
- **Indoor and Outdoor categories show volatility**, likely influenced by seasonal demand or inconsistent customer interest.
- The **bottom 10 countries** (by YTD vs PYTD delta) include Canada, Colombia, Germany, and Croatia, signaling region-specific challenges.

---

### Findings

The findings go beyond surface metrics and suggest structural opportunities and risks:

- **Canada and Colombia experienced steep revenue drops**, despite no apparent changes in customer base, which could point to macroeconomic or logistical issues.
- **Some customer accounts exhibit high GP% but generate relatively low revenue**, suggesting underutilized premium client relationships.
- **Product performance is unequally distributed**, with Landscape being stable and predictable, while Indoor/Outdoor suffer from demand fragmentation.
- **Monthly performance variability is non-random**: April and Q2 in general show signs of seasonal or operational disruption worth further investigation.
- **Price vs Quantity analysis indicates price sensitivity** in some markets — higher-priced items yield strong margins but with much lower volumes.

---

### Recommendations

To convert these findings into strategic business value, the following recommendations are proposed:

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

## Recommendations

1. **Region Focus**  
   - Run audits in Canada and Colombia  
   - Explore root causes (distribution, pricing, economic factors)  
   - Test localized marketing incentives  

2. **Product Strategy**  
   - Promote Landscape lines with consistent performance  
   - Conduct surveys for Indoor/Outdoor seasonal preferences  
   - Consider bundling and pricing optimization by segment  

3. **Account Growth Plans**  
   - Prioritize low-volume, high-margin customers for tailored offers  
   - Segment clients by GP% and create scorecards for retention/churn risk  
   - Monitor accounts with negative growth for proactive outreach

---

## Action Plan

- Prepare follow-up presentation for Sales and Ops teams  
- Schedule deep-dives into target countries and product families  
- Share segmented customer lists with Account Managers  
- Track post-recommendation KPIs over the next 3–6 months

---

## Business Value

- Enables real-time monitoring of performance vs prior year  
- Supports focused action by geography and product line  
- Helps identify and develop customer growth opportunities  
- Aligns cross-functional decisions through clear reporting

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

This Power BI report provides a clear, data-driven assessment of Plant Co.'s year-to-date performance in 2024, enabling comparison with the previous year across multiple dimensions such as geography, product category, and customer profitability.

The insights uncovered in this report allow stakeholders to:
- Detect early warning signs in underperforming regions  
- Focus on high-potential customer segments and product lines  
- Drive operational and strategic alignment across business units

By leveraging dynamic metrics, time intelligence, and interactive visuals, the report acts as both a monitoring tool and a foundation for continuous performance improvement.

Moving forward, integrating forecast data, customer satisfaction metrics, and drill-through analytics can further enhance the report's role in proactive decision-making.

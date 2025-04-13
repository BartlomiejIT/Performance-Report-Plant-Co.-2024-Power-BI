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
  - [DAX Measures](#dax-measures)  
- [Testing](#testing)  
- [Visualization](#visualization)  
- [Analysis](#analysis)  
  - [Insights](#insights)  
  - [Findings](#findings)  
  - [Recommendations](#recommendations)  
  - [Action Plan](#action-plan)  
- [Business Value](#business-value)  
- [Future Enhancements](#future-enhancements)

---

## Objective

To assess the year-to-date (YTD) sales and profitability performance of Plant Co. in 2024 across countries, products, and customer accounts, and compare it against the previous year-to-date (PYTD), in order to identify performance drivers, trends, and growth opportunities.

---

## User Story

As a business unit manager, I want to analyze year-over-year performance trends in sales and gross profit, broken down by region, product type, and customer segments, so I can:
- identify top- and bottom-performing areas,
- plan corrective actions for underperforming regions,
- and align sales and marketing efforts to revenue growth.

---

## Data Sources

| Table         | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| Fact_Sales    | Transactional data including quantity, sales (USD), and cost of goods sold |
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

Visual components include:
- KPI cards
- Waterfall charts
- Bar-line combo charts
- Treemaps and scatter plots

---

## Dashboard Mockup

The report includes:
- **Light version (mockup)** – designed as a clean wireframe for layout planning
- **Final version** – implemented in dark theme for stakeholder visibility and visual focus

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

## Data Review

- Sales data includes revenue and COGS at a transactional level  
- Product table contains product categorization into Landscape, Indoor, Outdoor  
- Dim_Date includes full calendar range required for time intelligence  
- No major data quality issues detected – data is clean and analytical columns are consistent

---

## DAX Measures

### Base Measures

```dax
Sales = SUM(Fact_Sales[Sales_USD])
Quantity = SUM(Fact_Sales[Quantity])
Gross Profit = SUM(Fact_Sales[Sales_USD]) - SUM(Fact_Sales[COGS_USD])
GP% = DIVIDE([Gross Profit], [Sales])
```

### YTD & PYTD

```dax
YTD_Sales = TOTALYTD([Sales], Dim_Date[Date])
PYTD_Sales = CALCULATE([YTD_Sales], SAMEPERIODLASTYEAR(Dim_Date[Date]))

YTD_GrossProfit = TOTALYTD([Gross Profit], Dim_Date[Date])
PYTD_GrossProfit = CALCULATE([YTD_GrossProfit], SAMEPERIODLASTYEAR(Dim_Date[Date]))

YTD_Quantity = TOTALYTD([Quantity], Dim_Date[Date])
PYTD_Quantity = CALCULATE([YTD_Quantity], SAMEPERIODLASTYEAR(Dim_Date[Date]))
```

### Difference

```dax
YTD vs PYTD Sales = [YTD_Sales] - [PYTD_Sales]
YTD vs PYTD GP = [YTD_GrossProfit] - [PYTD_GrossProfit]
YTD vs PYTD Quantity = [YTD_Quantity] - [PYTD_Quantity]
```

### Dynamic SWITCH

```dax
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
- Waterfall chart showing contribution by month and country  
- Combo chart for product type performance  
- Treemap highlighting worst-performing countries  
- Scatter plot for customer GP% vs volume segmentation

---

## Analysis

### Insights

- Overall sales and profit are down slightly vs PYTD (approx. -135k USD)  
- Gross profit margin remains strong (~39%), indicating cost control is effective  
- Key drops occur in April and from specific regions

### Findings

- Canada, Colombia, and Germany are the bottom 3 countries in terms of YTD vs PYTD performance  
- Landscape products are most stable, while Indoor/Outdoor show seasonal variation  
- Several customers have strong GP% but generate low sales volumes – possible upsell targets

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

## Future Enhancements

- Integrate targets and forecasts for vs-budget analysis  
- Add satisfaction and NPS metrics for qualitative insight  
- Enable drill-through to detailed product and account performance  
- Use DAX bookmarks and smart tooltips for guided navigation


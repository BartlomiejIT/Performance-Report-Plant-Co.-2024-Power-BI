# Performance Report – Plant Co. (2024)

## Power BI | Sales Analysis | DAX | YTD vs PYTD | Customer Segmentation

---

## Table of Contents

- [Objective](#objective)
- [Business Context](#business-context)    
- [User Story](#user-story)  
- [Data Sources](#data-sources)  
- [Tools](#tools)  
- [Design](#design)  
- [Dashboard Mockup](#dashboard-mockup)  
- [Development Process](#development-process)  
  - [Pseudocode](#pseudocode)  
  - [Data Review](#data-review)  
  - [Data Preparation (Power Query)](#data-preparation-power-query)
  - [Supporting Tables Created in Power BI](#supporting-tables-created-in-power-bi)  
  - [DAX Measures](#dax-measures)  
- [Testing](#testing)  
- [Visualization](#visualization)  
- [Analysis](#analysis)  
  - [Insights](#insights)   
  - [Recommendations](#recommendations)  
  - [Action Plan](#action-plan)  
- [Business Value](#business-value)  
- [Use Case Scenario](#use-case-scenario)  
- [Role in the Project](#role-in-the-project)  
- [Conclusion](#conclusion)

---

## Objective

This Power BI report delivers a comprehensive performance assessment for **Plant Co.**, a mid-sized B2B horticulture company operating in international markets. The report focuses on **YTD vs PYTD comparisons for 2024 vs 2023**, measuring Sales, Quantity, and Gross Profit at various levels of granularity: region, product family, and customer account.

Through interactive dashboards and dynamic KPI selection, the report empowers decision-makers to uncover underperformance, assess customer profitability, and act on key opportunities to drive growth, retention, and operational efficiency.

---

## Business Context

Plant Co. operates in a highly seasonal and margin-sensitive industry, distributing horticultural goods such as indoor, outdoor, and landscape products. The company faces fluctuations in demand due to weather cycles, pricing pressures, and shifting consumer priorities across regions.

Given the competitive nature of the industry and the increasing need for data-driven planning, this dashboard simulates a scenario where Plant Co. needs to monitor short-term commercial performance while aligning long-term resource allocation with real profitability insights.

---

## User Story

> *As a Sales & Operations Manager at Plant Co., I need a real-time report that shows how we’re performing against last year across revenue, volume, and margin metrics. I want to understand where we are losing momentum (countries, products, accounts), and I need clear indicators on which levers to pull – whether it’s pricing, marketing, or customer outreach – to hit our targets for the rest of the year.*

---

## Data Sources

The dataset used in this analysis is a simulated dataset created for educational purposes. It includes tables such as:

## Data Sources

The dataset used in this analysis is a simulated dataset created for educational purposes. It includes the following tables:

## Data Sources

The dataset used in this analysis is a simulated dataset created for educational purposes. It includes the following tables (imported from Excel):

| Table             | Description                                                        |
|-------------------|--------------------------------------------------------------------|
| `Plant_FACT`       | Transaction-level data with Quantity, Sales (USD), and COGS (USD) |
| `Accounts`         | Customer details including country, region, and segmentation      |
| `Plant_Hierarchy`  | Product categorization: family, group, and type                   |

---

## Tools

| Tool        | Purpose                                                                      |
|-------------|------------------------------------------------------------------------------|
| Power BI    | Interactive dashboard, relationship modeling, user-driven analysis           |
| Power Query | ETL: deduplication, schema alignment, column renaming                        |
| DAX         | Time intelligence (YTD, PYTD), KPI definitions, dynamic SWITCH logic         |
| Excel       | Initial exploration and QA of raw dataset                                    |

---

## Design

The dashboard was designed to answer the following key business questions:

1. How does current YTD performance compare to PYTD across Sales, Quantity, and Gross Profit?
2. Which countries, product categories, or customer segments are underperforming?
3. Are gross profit margins being maintained despite volume and revenue declines?
4. What is causing repeated drops in specific months (e.g., April)?
5. Which accounts are profitable but underutilized in terms of volume?
   
---

## Dashboard Mockup

Visual components include:

- KPI cards
- Waterfall chart
- Bar-line combo chart
- Treemap and scatter plot

![image](https://github.com/user-attachments/assets/0f380a5e-f9e1-4bea-b2ed-0684e8371337)

---

## Development Process

### Pseudocode

1. Load Excel-based tables (`Plant_FACT`, `Accounts`, `Plant_Hierarchy`) into Power BI  
2. In Power Query:  
   - Remove duplicates  
   - Rename columns for consistency  
   - Change data types and promote headers  
3. Create `Dim_Date` and `Slc_Values` directly in Power BI  
4. Define base and time intelligence measures in DAX (e.g., YTD, PYTD, Gross Profit Margin)  
5. Implement dynamic metric selection using `SWITCH` and slicer table  
6. Design dashboard visuals (KPI cards, charts, treemaps, scatter plots)  
7. Test logic with known values and ensure expected results across all filters

---

### Data Review

- Checked for missing/null values: none found  
- Verified product categories: Indoor, Outdoor, Landscape  
- Time granularity: full daily range for 2023 and 2024  
- Keys: clean joins between fact and dimensions

---

### Data Preparation (Power Query)

- Cleaned `Dim_Product`, `Dim_Account` for duplicates  
- Applied consistent naming and column typing

  ![image](https://github.com/user-attachments/assets/07a929c4-474c-4677-a0d7-1baed92c587a)

  ![image](https://github.com/user-attachments/assets/e3f8cc7c-ec5f-4d0e-b6d3-6c8334537b8a)

---

### Supporting Tables Created in Power BI

- Two additional tables were manually created to support time intelligence and interactive metric selection:

  | Table         | Purpose                                                                 |
  |---------------|-------------------------------------------------------------------------|
  | `Dim_Date`     | Calendar table used for YTD and PYTD DAX calculations. Created using the `CALENDAR` DAX function or imported as a prebuilt date dimension. |
  | `Slc_Values`   | Slicer table containing metric labels (Sales, Quantity, Gross Profit), used in `SWITCH` logic for dynamic KPI selection. |

---

### Ensured proper table relationships in model view

   ![image](https://github.com/user-attachments/assets/9fc068ff-59a6-4ef7-89f8-6b40d7298282)

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
    SELECTEDVALUE(Slc_Values[Values]),
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Gross Profit", [YTD_GrossProfit]
)

Selected PYTD = 
SWITCH(
    SELECTEDVALUE(Slc_Values[Values]),
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Gross Profit", [PYTD_GrossProfit]
)

YTD vs PYTD (Selected) = [Selected YTD] - [Selected PYTD]
```

---

## Testing

| Test Scenario                               | Expected Result                          | Status |
|---------------------------------------------|------------------------------------------|--------|
| YTD Sales for Jan 2024                      | Sum of daily Sales in Jan 2024           | ✅     |
| GP% = GP / Sales                            | Returns consistent ~39% across periods   | ✅     |
| PYTD for March 2023 matches March 2023 data | Slicer accurately retrieves PYTD         | ✅     |
| Metric toggle to Quantity updates visuals   | Chart values switch correctly            | ✅     |

---

## Visualization

The dashboard includes:

- KPI cards for YTD, PYTD, and GP%  
- Waterfall chart showing contribution by month, country and product  
- Combo chart for product type performance  
- Treemap highlighting worst-performing countries  
- Scatter plot for customer GP% vs volume segmentation

### - Gross Profit (2023-2024):
  
  - 2023:     
  ![image](https://github.com/user-attachments/assets/1f59c2db-d16e-48d0-bbde-b701b6e77c5e)

  - 2024:
  ![image](https://github.com/user-attachments/assets/a1a71197-cd0d-49d3-be48-8729fea41c4c)

### - Quantity (2023-2024):

  - 2023:
  ![image](https://github.com/user-attachments/assets/12ea3558-40d9-42af-8d81-a9127d1506c1)

  - 2024:
  ![image](https://github.com/user-attachments/assets/19420c90-11cf-4991-ac23-f442dd5d48c7)

### - Sales (2023-2024):

  - 2023:
  ![image](https://github.com/user-attachments/assets/5e32d2cd-d312-442f-b1ca-6c78dfb6bfcb)

  - 2024:
  ![image](https://github.com/user-attachments/assets/4c06e0d0-4332-4808-bf3b-44ed8218da34)

---

## Analysis

## Insights:

This analysis provides evidence-based answers to five key business questions driving the performance review of Plant Co. between the current and prior year-to-date periods:

### 1. How does current YTD performance compare to PYTD across Sales, Quantity, and Gross Profit?

- **Sales Revenue** dropped significantly from **$13M in 2023** to **$3.57M in 2024**, indicating a major contraction in business activity (−72% YoY).
- **Quantity Sold** followed a parallel decline, from **555K to 148K units** (−73%), suggesting that the performance drop is demand-related rather than price-driven.
- **Gross Profit** decreased accordingly, from **$5.15M to $1.40M**, tracking closely with sales volume.
- Importantly, the **Gross Profit Margin (GP%) remained stable (~39%)**, which shows resilience in cost control and pricing discipline.

**Interpretation:** The company is maintaining unit profitability but suffering from systemic volume loss. This signals a need to stimulate demand, not to correct pricing or margin strategy.

### 2. Which countries, product categories, or customer segments are underperforming?

- **Underperforming Countries:**  
  - **Canada**, **Colombia**, and **Germany** consistently rank in the bottom quartile for Sales, Quantity, and Gross Profit. Their poor results appear across both 2023 and 2024, suggesting structural challenges rather than one-time dips.
  - **Croatia** also emerged as a weak performer in 2024, particularly in Gross Profit.

- **Product Category Performance:**  
  - **Landscape products** have remained stable in performance and margin, offering consistent contribution.
  - **Indoor and Outdoor categories** exhibit high volatility, with dramatic rises and falls in monthly performance—especially in Q2.
  - These categories seem highly sensitive to seasonality, distribution timing, or inventory planning.

- **Customer Segmentation:**  
  - Scatterplots indicate a large number of **high-GP%, low-volume clients**—a classic signal of underutilized account potential.
  - Few customers migrate between segments YoY, pointing to limited success in account development or upsell programs.

**Interpretation:** Plant Co.’s growth is being limited by repeated regional underperformance, volatile products with demand misalignment, and a customer base that is not being actively matured.


### 3. Are gross profit margins being maintained despite volume and revenue declines?

- Yes. **GP% has remained strong and steady (~39%)** across both years, despite the revenue and quantity drops.
- This suggests:
  - Stable pricing policies
  - Controlled costs (COGS)
  - A lack of aggressive discounting to chase volume

**Interpretation:** The company has retained pricing integrity and margin discipline. Therefore, recovery strategies should prioritize volume reactivation, not price cuts.


### 4. What is causing repeated drops in specific months (e.g., April)?

- **April stands out as a critical month**, showing the **lowest performance in both years** for all three KPIs.
- This trend is consistent regardless of product type or region, indicating a **systemic issue**—not a localized or seasonal blip.
- Likely causes include:
  - Poor sales campaign alignment
  - Inaccurate demand forecasting
  - Inventory or fulfillment delays
  - Lack of early-year customer reactivation efforts

**Interpretation:** April requires special attention in planning cycles. Forecasting, logistics, and promotional strategy should be adjusted specifically for this month to avoid repeat losses.


### 5. Which accounts are profitable but underutilized in terms of volume?

- A significant portion of customers (visible in GP% vs Volume scatter plots) fall into the **“high margin, low value”** quadrant.
- These clients generate strong per-unit profits but contribute very little to overall revenue.
- There is **no meaningful migration of accounts** from low-value to higher-value segments between 2023 and 2024.

**Interpretation:** These accounts represent a substantial opportunity. With targeted upsell campaigns, personalization, and engagement strategies, even modest increases in volume could have a major impact on overall profit growth.


**Summary:**  
Plant Co.’s core profitability is intact, but commercial performance is hindered by weak regional demand, product volatility, and passive account management. Addressing these weaknesses through targeted reactivation, better forecasting, and focused account development is key to restoring growth in 2024 and beyond.

---

## Recommendations

Based on the findings, the following strategic and operational recommendations are proposed to address root causes, recover performance, and unlock growth potential:

### 1. Reactivate Demand in Underperforming Regions

- Conduct regional deep-dives in **Canada, Colombia, Germany**, and **Croatia** to identify root causes:
  - Local market conditions
  - Distribution challenges
  - Competitive pricing dynamics
- Assign regional recovery targets and track via monthly KPIs.
- Launch **territory-specific marketing campaigns** tied to seasonal patterns and customer behavior.
- Reengage dormant accounts in those countries with time-limited incentives or bundled product offers.


### 2. Stabilize Product Portfolio with Data-Informed Planning

- Increase promotional focus and distribution efficiency around **Landscape products**, which have shown stable demand and margin performance.
- Conduct a **demand seasonality analysis** on **Indoor and Outdoor categories** to better align marketing and stock planning.
- Introduce **dynamic inventory allocation** based on Q2 volatility trends.
- Pilot **bundling strategies or cross-category promotions** to drive volume and smooth demand in off-peak months.


### 3. Improve Customer Segmentation and Targeted Growth

- Segment all accounts using a **GP% vs Volume matrix** to identify:
  - “Grow” accounts (high GP%, low volume)
  - “Defend” accounts (high GP%, high volume)
  - “Monitor” accounts (low GP%, low volume)
- Create tailored account development plans:
  - “Grow” → Upsell programs and relationship-building
  - “Monitor” → Evaluate for churn risk or margin erosion
- Use **predictive analytics** to estimate lifetime value (LTV) and guide strategic focus.
- Deploy account scorecards to track migration over time between segments.


### 4. Address Operational Bottlenecks in April

- Treat **April as a strategic risk window**:  
  - Introduce a proactive “April Readiness Playbook” that includes supply planning, sales readiness, and marketing alignment.
- Launch **pre-season promotions in Q1** to pull demand forward and stabilize early Q2 volume.
- Tighten coordination between sales forecasts, fulfillment schedules, and customer reactivation outreach.


### 5. Transition to Proactive, Scenario-Based Planning

- Shift from reactive reporting to **scenario-based planning using dynamic dashboards**:
  - Build what-if simulations for product and regional changes
  - Use variance tracking to anticipate rather than explain gaps
- Integrate performance alerts (e.g., “April GP% below threshold”) to guide weekly decision-making.
- Build feedback loops between actual sales vs planned volume to continuously refine forecasts.

These recommendations are designed not only to mitigate current performance gaps but to build the organizational capability needed to adapt to future volatility and segment complexity.

---

## Action Plan

A phased plan is recommended to ensure implementation and follow-up of the strategic insights revealed in this report:

1. **Internal Alignment**
   - Schedule stakeholder presentation with Sales, Product, and Operations teams
   - Share detailed insights and recommendations per region, product, and segment

2. **Account Activation**
   - Assign account managers to "Grow" segment accounts (high GP%, low volume)
   - Launch pilot reactivation campaigns with clear volume targets

3. **Product Strategy Execution**
   - Realign Q2 Indoor/Outdoor campaign timing based on seasonal volatility
   - Initiate pricing/bundling testing on volatile SKUs

4. **Operational Enhancements**
   - Create monthly KPI review cadence with alerts for April risk zone
   - Adjust inventory planning models using Q2 patterns

5. **Dashboard Enhancement**
   - Expand dashboard to include budget, forecast, and satisfaction (Net Promoter Score) metrics
   - Enable drill-through to account- and Stock Keeping Unit Level insights in next release

---

## Business Value

This report delivers cross-functional business value by serving as both a diagnostic and decision-making tool:

- **Risk Detection**: Enables early identification of structural performance issues
- **Growth Acceleration**: Highlights revenue opportunities in stable and underutilized segments
- **Strategic Focus**: Drives prioritization by product category, customer segment, and region
- **Operational Efficiency**: Supports forecasting and fulfillment with empirical trend data
- **Collaboration**: Unites sales, marketing, and operations teams around a common KPI framework

---

## Use Case Scenario

The report supports various roles across the organization:

- **Sales Teams**  
  Use the dashboard weekly to monitor declining accounts and regions, then trigger follow-ups.

- **Product Managers**  
  Review product-level KPIs to refine promotional plans and align production with actual demand.

- **Executives & Leadership**  
  Benchmark strategic performance vs previous year and align quarterly targets to regional/product trends.

- **Data & BI Teams**  
  Extend the model with forecast, budget, and LTV metrics for forward-looking business planning.

---

## Role in the Project

- Built complete Power BI model and visuals
- Designed final report layout (light blue theme)
- Created dynamic DAX measures (YTD, PYTD, SWITCH)
- Applied Power Query for data cleanup and normalization
- Led insight generation, analysis, and documentation 

---

## Conclusion

This performance report equips Plant Co. with a scalable, insight-driven tool for assessing YTD progress and identifying strategic levers for recovery and growth.

Although 2024 reflects a significant drop in both sales and volume, the company's ability to maintain margins points to strong fundamentals. More importantly, the segmentation and underperformance patterns revealed in this dashboard provide a clear roadmap for targeted interventions.

With actionable insights, consistent KPI logic, and modular DAX-driven architecture, this report forms a foundation for smarter, faster, and more aligned decision-making across the organization.

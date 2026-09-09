# Data-Analyst-Operational-Ecommerce-2
E-commerce operational data analysis using Python and Pandas to monitor order performance, fulfillment efficiency, cancellations, returns, shipping SLA, and operational data quality.

Looker Studio (Data Studio) Dashboard : https://bit.ly/Data_Analyst_Ecommerce_Report

# E-commerce Operational Performance Analysis

## Overview

This project presents an end-to-end operational analysis of a real operating e-commerce business, using actual transactional and operational data generated from day-to-day store activities.

The analysis focuses on evaluating the complete order lifecycle, including order creation, payment, fulfillment, shipping arrangement, completion, cancellation, and returns.

Using Python and Pandas, raw order-item-level transactional data is cleaned, validated, transformed, and aggregated into order-level operational datasets to monitor performance, identify bottlenecks, detect data inconsistencies, and generate actionable operational insights.

To protect business and customer confidentiality, sensitive or personally identifiable information should be removed or anonymized before the dataset is published.

---

## Business Objectives

The main objectives of this project are to:

* Monitor order completion and cancellation performance
* Measure fulfillment and order processing efficiency
* Identify slow-processing and critical orders
* Monitor shipping deadline compliance
* Analyze cancellation stages and reasons
* Measure return incidence
* Detect operational bottlenecks and anomalies
* Perform reconciliation and data consistency checks
* Prepare clean datasets for dashboard development

---

## Dataset

The dataset contains e-commerce order transactions from **October 2025**.

Initial dataset:

* **3,266 order-item rows**
* **3,153 unique orders**
* **50 original columns**

### Dataset Grain

The raw dataset is stored at the **order-item level**.

One row represents one product or variation within an order. Therefore, a single order can appear in multiple rows when customers purchase multiple products or variations.

This distinction is important because operational KPIs such as completion rate, cancellation rate, and processing time must be calculated at the **unique order level** to avoid double counting.

---

## Tools & Technologies

* Python
* Pandas
* NumPy
* Jupyter Notebook / Google Colab
* CSV / Excel

---

## Analysis Workflow

### 1. Data Understanding

The first stage focuses on understanding the structure and grain of the dataset.

The analysis includes:

* Dataset dimensions
* Column inspection
* Data type inspection
* Missing-value analysis
* Duplicate detection
* Unique order identification
* Investigation of multi-item orders

The investigation confirmed that repeated order IDs represent different products or variations within the same order rather than duplicate records.

---

### 2. Data Quality Assessment

Operational data quality was evaluated before transformation.

Checks include:

* Missing operational timestamps
* Missing tracking numbers
* Order status consistency
* Cancellation and return information
* Fulfillment information
* Datetime field validation
* Numerical sanity checks

Missing values were not automatically removed because several missing operational fields are structurally associated with cancelled or unfinished orders.

---

### 3. Data Cleaning & Standardization

The raw dataset was preserved while a separate working dataset was created.

Cleaning activities include:

* Standardizing column names
* Renaming important operational fields
* Converting datetime columns
* Validating datetime conversions
* Preserving legitimate missing values
* Preparing fields for operational analysis

Examples of standardized fields include:

`order_id`
`order_status`
`order_created_at`
`payment_at`
`shipping_arranged_at`
`shipping_deadline`
`order_completed_at`
`tracking_number`
`cancellation_reason`
`returned_quantity`

---

### 4. Feature Engineering

Several operational features were created to measure different stages of the order lifecycle.

These include:

* Payment lead time
* Processing time
* Order completion time
* Shipping deadline margin
* Cancellation indicator
* Fulfillment-start indicator
* Cancellation stage
* Processing category
* Return indicator

The order-item dataset was also aggregated into an **order-level dataset** to ensure that operational KPIs were calculated using unique orders.

---

## Key Operational KPIs

The project monitors several operational metrics, including:

| KPI                    | Description                                               |
| ---------------------- | --------------------------------------------------------- |
| Total Orders           | Number of unique customer orders                          |
| Completed Orders       | Orders successfully completed                             |
| Cancelled Orders       | Orders cancelled during the order lifecycle               |
| Completion Rate        | Percentage of orders successfully completed               |
| Cancellation Rate      | Percentage of orders cancelled                            |
| Median Processing Time | Typical time required to arrange fulfillment              |
| P90 Processing Time    | Processing time within which 90% of orders were handled   |
| Critical Order Rate    | Percentage of unusually slow-processing orders            |
| Late Shipment Rate     | Percentage of orders arranged after the shipping deadline |
| On-Time Rate           | Percentage of orders arranged before the deadline         |
| Return Order Rate      | Percentage of completed orders containing returned items  |
| Returned Item Rate     | Percentage of ordered items that were returned            |

---

## Operational Reconciliation

Reconciliation checks were implemented to identify logical inconsistencies in the operational data.

Examples include:

* Completed orders without shipping timestamps
* Completed orders without completion timestamps
* Shipping arranged before order creation
* Completion recorded before shipping arrangement
* Payment recorded before order creation
* Return status without returned items
* Returned items without a return indicator
* Cancelled orders incorrectly classified as non-cancelled
* Non-cancelled orders containing a cancellation stage

Each reconciliation check is classified as either:

* **PASS** — no inconsistency detected
* **REVIEW** — potential issue requiring investigation

---

## Key Business Insights

### Order Performance

A total of **3,153 unique orders** were recorded during October 2025.

* **2,650 completed orders**
* **503 cancelled orders**
* **84.05% completion rate**
* **15.95% cancellation rate**

---

### Fulfillment Performance

Among **2,712 orders that entered fulfillment**:

* Median processing time: **18.88 hours**
* P90 processing time: **32.10 hours**

This means that approximately 90% of processed orders had fulfillment arranged within 32.10 hours.

---

### Critical Processing Cases

A total of **26 orders (0.96%)** were classified as Critical based on processing times above the 99th-percentile monitoring threshold.

These orders can be prioritized for operational investigation.

---

### Shipping Deadline Compliance

Shipping deadline compliance was high.

* Late shipment arrangements: **9 orders**
* Late shipment rate: **0.33%**
* On-time shipping-arrangement rate: **99.67%**

---

### Cancellation Analysis

Of **503 cancelled orders**:

* **441 orders (87.67%)** were cancelled before fulfillment
* **62 orders (12.33%)** were cancelled after fulfillment had started

Post-fulfillment cancellations are particularly important operationally because warehouse or logistics activities may already have been initiated.

Cancellation reasons were also standardized into categories to support easier monitoring and dashboard reporting.

---

### Return Performance

Among completed orders:

* **36 orders** contained returned items
* Return-order rate: **1.36%**
* **36 of 3,513 ordered items** were returned
* Returned-item rate: **1.02%**

---

### Workload vs Processing Time

The correlation between daily order volume and median processing time was approximately **0.076**.

This indicates little linear association between daily order volume and processing time during the observed period.

Therefore, higher daily order volume alone does not appear to explain slower fulfillment performance in this dataset.

---

## Output Datasets

The analysis produces several cleaned and aggregated datasets:

* `vidrona_order_items_clean.csv` — cleaned order-item-level data
* `vidrona_orders_clean.csv` — aggregated order-level operational data
* `vidrona_daily_operations.csv` — daily operational performance
* `vidrona_cancellation_summary.csv` — cancellation analysis
* `vidrona_reconciliation_summary.csv` — operational consistency checks
* `vidrona_operational_kpi.csv` — summarized operational KPIs

These datasets can be used as data sources for further analysis or dashboard development.

---

## Project Structure

```text
ecommerce-operational-analysis/
│
├── Data_Analyst_Ecommerce.ipynb
│
├── data/
│   └── raw_data.xlsx
│
├── output/
│   ├── vidrona_order_items_clean.csv
│   ├── vidrona_orders_clean.csv
│   ├── vidrona_daily_operations.csv
│   ├── vidrona_cancellation_summary.csv
│   ├── vidrona_reconciliation_summary.csv
│   └── vidrona_operational_kpi.csv
│
└── README.md
```

---

## Skills Demonstrated

This project demonstrates practical skills in:

* Data cleaning and preprocessing
* Exploratory data analysis
* Data quality assessment
* Data aggregation
* Feature engineering
* Operational KPI development
* E-commerce order lifecycle analysis
* Fulfillment performance analysis
* Cancellation and return analysis
* SLA / shipping deadline monitoring
* Operational reconciliation
* Business insight generation
* Dashboard data preparation

---
## Business Insights

The analysis of actual e-commerce operational data from **October 2025** generated several key findings related to order completion, fulfillment efficiency, cancellations, shipping performance, and returns.

### 1. Order Completion Performance

A total of **3,153 unique orders** were recorded during the analysis period.

* **2,650 orders were completed**
* **503 orders were cancelled**
* **Completion Rate: 84.05%**
* **Cancellation Rate: 15.95%**

**Business Insight:**
Approximately **1 out of every 6 orders was cancelled**, indicating that cancellation represents a meaningful source of lost order conversion. Further analysis of cancellation stages and reasons is therefore important to identify which cancellations can potentially be reduced through operational improvements.

---

### 2. Fulfillment Processing Performance

Among **2,712 orders that entered the fulfillment process**:

* Median processing time: **18.88 hours**
* P90 processing time: **32.10 hours**

This means that half of processed orders entered shipping arrangement within approximately 19 hours, while 90% were processed within approximately 32 hours.

**Business Insight:**
Overall fulfillment performance was relatively consistent for most orders. However, using both median and P90 processing time provides better operational monitoring than relying only on averages, as unusually slow orders can be identified separately.

---

### 3. Critical Processing Cases Were Limited

A total of **26 orders (0.96% of processed orders)** were classified as `Critical` based on processing times above the 99th-percentile monitoring threshold.

**Business Insight:**
Severe processing delays affected only a small proportion of orders. Rather than treating processing speed as a system-wide issue, operational teams could focus investigation on these specific outlier orders to identify recurring causes such as stock availability, fulfillment handling, or other operational exceptions.

---

### 4. Shipping Deadline Compliance Was High

Only **9 processed orders** were arranged for shipping after their marketplace shipping deadline.

* **Late Shipment Rate: 0.33%**
* **On-Time Shipping Arrangement Rate: 99.67%**

**Business Insight:**
Shipping deadline compliance was very high, suggesting that fulfillment operations generally succeeded in arranging shipments within marketplace requirements.

The small number of late orders should still be monitored because repeated late shipments may negatively affect marketplace service performance.

---

### 5. Most Cancellations Happened Before Fulfillment

Of the **503 cancelled orders**:

* **441 orders (87.67%)** were cancelled before fulfillment started
* **62 orders (12.33%)** were cancelled after fulfillment had already been initiated

**Business Insight:**
Most cancellations occurred before warehouse or shipping activities began, meaning the majority did not directly consume fulfillment resources.

However, the **62 post-fulfillment cancellations** deserve greater operational attention because processing or logistics activities had already started, potentially creating unnecessary handling and operational costs.

---

### 6. Cancellation Reasons Reveal Improvement Opportunities

The largest cancellation categories were:

| Cancellation Reason | Cancelled Orders |  Share |
| ------------------- | ---------------: | -----: |
| Other               |              123 | 24.45% |
| Change Order        |              121 | 24.06% |
| Change Address      |              105 | 20.87% |
| Unpaid Order        |               76 | 15.11% |
| Delivery Failed     |               60 | 11.93% |

**Business Insight:**
`Change Order` and `Change Address` together accounted for approximately **44.93% of cancellations**, making customer order modification a major cancellation driver.

This suggests a potential opportunity to reduce cancellations by improving product information, checkout confirmation, address validation, or customer communication before fulfillment begins.

`Delivery Failed` is particularly important because these cancellations occurred after fulfillment initiation, meaning logistics resources had already been consumed.

---

### 7. Return Incidence Was Relatively Low

Among completed orders:

* **36 orders** contained returned items
* **Return Order Rate: 1.36%**
* **36 of 3,513 ordered items** were returned
* **Item Return Rate: 1.02%**

**Business Insight:**
Returns represented a relatively small portion of completed transactions during the observed period. This indicates that cancellations currently represent a considerably larger operational issue than product returns.

Operational improvement efforts may therefore generate greater impact by prioritizing cancellation reduction while continuing to monitor return trends.

---

### 8. Higher Daily Order Volume Did Not Significantly Slow Processing

The correlation between daily order volume and median processing time was approximately:

**0.076**

This indicates very little linear relationship between daily workload and processing time during the observed period.

**Business Insight:**
Higher order volume alone does not appear to explain slower fulfillment days.

This suggests that processing delays may be driven by other operational factors rather than simply order volume. Future analysis could investigate variables such as product mix, fulfillment method, order timing, stock availability, or staffing capacity.

---

### 9. Operational Data Passed Reconciliation Checks

The reconciliation process found no inconsistencies across the implemented validation rules, including:

* Completed orders without shipping timestamps
* Completed orders without completion timestamps
* Shipping before order creation
* Completion before shipping arrangement
* Payment before order creation
* Return-status inconsistencies
* Cancellation-stage inconsistencies

All implemented reconciliation checks returned **PASS**.

**Business Insight:**
The operational dataset showed strong logical consistency across the tested order lifecycle fields, increasing confidence that the resulting KPIs can be used for operational monitoring and dashboard development.

---

## Recommended Actions

Based on the analysis, several operational priorities can be considered:

1. **Reduce preventable cancellations** by investigating `Change Order`, `Change Address`, and `Unpaid Order` cases.
2. **Prioritize post-fulfillment cancellations** because these orders have already consumed operational resources.
3. **Investigate Critical processing orders individually** rather than treating fulfillment delays as a system-wide problem.
4. **Maintain the current shipping SLA performance**, while monitoring the small number of late-shipment cases.
5. **Continue monitoring returns**, although their current incidence is substantially lower than cancellations.
6. **Investigate additional drivers of processing time**, since daily order volume alone showed little association with fulfillment speed.

---

## Overall Business Conclusion

The analysis indicates that the store's **fulfillment and shipping operations were generally stable**, with **99.67% on-time shipping arrangement**, fewer than **1% Critical processing cases**, and a relatively low **1.36% return-order rate**.

The more significant improvement opportunity lies in **order cancellations**, which affected **15.95% of total orders**. Since most cancellations occurred before fulfillment and large portions were associated with order or address changes, cancellation prevention represents a potential area for improving overall order conversion without requiring major changes to the existing fulfillment process.

At the same time, post-fulfillment cancellations and isolated processing outliers should remain operational monitoring priorities because they represent cases where business resources may already have been consumed.


## Conclusion

This project demonstrates how raw e-commerce transactional data can be transformed into structured operational information for performance monitoring and decision-making.

By combining data cleaning, order-level aggregation, KPI development, fulfillment analysis, cancellation and return monitoring, and reconciliation checks, the analysis provides a foundation for identifying operational issues and building a scalable e-commerce operations dashboard.

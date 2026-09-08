# Data-Analyst-Operational-Ecommerce-2
E-commerce operational data analysis using Python and Pandas to monitor order performance, fulfillment efficiency, cancellations, returns, shipping SLA, and operational data quality.

# E-commerce Operational Performance Analysis

## Overview

This project analyzes e-commerce operational data to evaluate **order performance, fulfillment efficiency, cancellations, returns, shipping deadline compliance, operational bottlenecks, and data consistency**.

The analysis simulates a typical **E-commerce Operations Data Analyst** workflow, starting from raw transactional data and progressing through data quality assessment, cleaning, feature engineering, KPI development, operational reconciliation, and business insight generation.

The project uses **Python and Pandas** to transform order-item-level transactional data into order-level operational metrics that can be used for performance monitoring and dashboard development.

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

## Potential Development

The project can be extended by developing an interactive operational dashboard using tools such as **Power BI, Tableau, or Looker Studio**.

Potential dashboard components include:

* Operational KPI scorecards
* Daily order trends
* Completion and cancellation rates
* Processing-time monitoring
* Processing-category distribution
* Shipping deadline performance
* Cancellation reason analysis
* Return monitoring
* Operational anomaly tracking

---

## Conclusion

This project demonstrates how raw e-commerce transactional data can be transformed into structured operational information for performance monitoring and decision-making.

By combining data cleaning, order-level aggregation, KPI development, fulfillment analysis, cancellation and return monitoring, and reconciliation checks, the analysis provides a foundation for identifying operational issues and building a scalable e-commerce operations dashboard.

# 📓 Jupyter Notebooks: Analytical Workflow

This directory contains the six core Jupyter notebooks that power the data processing, database ingestion, business analytics, and statistical modeling for the Brazilian E-Commerce Analytics platform.

---

## 🗺️ Pipeline Architecture

The notebooks are organized into a clear, sequential pipeline:

```
┌──────────────────────────────────────────────┐
│  01_E-commerce_Data_Cleaning.ipynb           │  ──> Data Acquisition, Cleaning & Deduplication
└──────────────────────┬───────────────────────┘
                       │ Cleaned Tables
┌──────────────────────▼───────────────────────┐
│  02_Validation_and_Product_Analysis.ipynb    │  ──> PostgreSQL (NeonDB) Ingestion & Schema Reconciliation
└──────────────────────┬───────────────────────┘
                       │ Relational Database
       ┌───────────────┼───────────────┐
       ▼                               ▼
┌──────────────────────────────┐ ┌──────────────────────────────┐
│ 03_Sales_and_Product_        │ │ 04_Customer_Analytics.ipynb  │
│ Analysis.ipynb               │ │                              │
│ • Category Pareto (80/20)    │ │ • Repeat Purchase Rate       │
│ • Geographic State Demand    │ │ • RFM Behavioral Segmentation│
└──────────────┬───────────────┘ └──────────────┬───────────────┘
               │                                │
               └───────────────┬────────────────┘
                               ▼
┌──────────────────────────────────────────────┐
│  05_Customer_Experience.ipynb                │  ──> Logistics SLA, Lead Times & Review Score Analysis
└──────────────────────┬───────────────────────┘
                       │ Delivery & Feedback Data
┌──────────────────────▼───────────────────────┐
│  06_EDA_Statistical_Analysis.ipynb           │  ──> Skewness, Mann-Whitney U Test & Correlation
└──────────────────────────────────────────────┘
```

---

## 📑 Notebook Catalog

### [01_E-commerce_Data_Cleaning.ipynb](01_E-commerce_Data_Cleaning.ipynb)
**Purpose**: Raw data acquisition, initial exploratory audit, schema profiling, and data cleaning.

- **Inputs**: Raw CSV tables downloaded via `kagglehub` from Kaggle (`olistbr/brazilian-ecommerce`).
- **Key Operations**:
  - Inspected null counts, column datatypes, and duplicate rows across all 9 raw datasets.
  - Investigated missing delivery timestamps in `orders`: verified that missing `order_delivered_customer_date` values correspond to non-delivered statuses (`shipped`: 1,107, `canceled`: 619, `unavailable`: 609), with only 8 true null anomalies.
  - Resolved missing product metadata (category names, weights, dimensions). Dropped corrupted record index `18851` lacking physical attributes.
  - Converted string timestamps into standard ISO `datetime64[ns]` formats.
- **Outputs**: Verified, standardized datasets exported as `cleaned_data.zip`.

---

### [02_Validation_and_Product_Analysis.ipynb](02_Validation_and_Product_Analysis.ipynb)
**Purpose**: Relational database migration, SQL data validation, and core financial reconciliation.

- **Inputs**: Cleaned CSV files from `01`.
- **Database Target**: Cloud serverless PostgreSQL (NeonDB) via `SQLAlchemy` and `psycopg2`.
- **Key Operations**:
  - Programmatically created and populated 9 database tables (`orders`, `customers`, `order_items`, `order_payments`, `order_reviews`, `products`, `sellers`, `geolocation`, `product_category`).
  - Executed relational count validations against information schema.
  - Calculated financial benchmarks using SQL aggregation:
    - Total Merchandise Sales: **R$ 13,591,643.70**
    - Total Freight Value: **R$ 2,251,909.54**
    - Combined Merchandise + Freight: **R$ 15,843,553.24**
    - Average Order Value (Item-level): **R$ 137.75**
  - Mapped monthly order trends from September 2016 through October 2018.
  - Joined Portuguese-to-English translations directly into the database `products` table (`product_category_name_english`).
- **Outputs**: Fully modeled relational PostgreSQL warehouse ready for querying.

---

### [03_Sales_and_Product_Analysis.ipynb](03_Sales_and_Product_Analysis.ipynb)
**Purpose**: Product performance, category contribution, Pareto analysis, and regional sales distribution.

- **Key Operations**:
  - **Pareto Analysis (80/20 Rule)**: Ranked categories by cumulative sales. Found that the top 7 categories (`health_beauty`, `watches_gifts`, `bed_bath_table`, `sports_leisure`, `computers_accessories`, `furniture_decor`, `cool_stuff`) account for **49.76%** of revenue, while the top 8 categories exceed 50% (cumulative **53.33%** of merchandise sales).
  - **Top Product Performance**: Identified top-selling individual SKUs by revenue and volume sold.
  - **Geographic State Analysis**: Aggregated orders, revenue, and AOV by customer state.
    - São Paulo (`SP`): R$ 5.20M (38.28% share, 41,750 orders).
    - Rio de Janeiro (`RJ`): R$ 1.82M (13.42% share, 12,850 orders).
    - Minas Gerais (`MG`): R$ 1.59M (11.66% share, 11,640 orders).
  - **Order Status & Cancellations**: Quantified overall marketplace cancellation rate at **0.63%** (625 canceled orders).
- **Visuals Produced**: Plotly Pareto cumulative curve, sales contribution bar charts, state revenue maps.

---

### [04_Customer_Analytics.ipynb](04_Customer_Analytics.ipynb)
**Purpose**: Customer retention analysis, repeat purchase behavior, and RFM behavioral segmentation.

- **Key Operations**:
  - **Unique Customer Mapping**: Distinguished between `customer_id` (per-order token: 99,441) and `customer_unique_id` (real unique buyers: 96,096).
  - **Repeat Purchase Analysis**: Discovered that **96.9%** (93,099 buyers) purchased only once; only **3.1%** (2,997 buyers) returned for repeat orders (max: 16 orders).
  - **RFM Segmentation Model**:
    - *Recency (R)*: Days since last order relative to the maximum dataset timestamp.
    - *Frequency (F)*: Distinct order count per unique customer.
    - *Monetary (M)*: Total spend per unique customer.
    - Quintile scoring (1–5) applied across R, F, and M dimensions.
  - **Customer Tier Classification**:
    - **Champions** (R>=4, F>=4, M>=4): 6,651 customers — highest engagement and LTV.
    - **Loyal Customers** (R>=4, F>=3): 16,332 customers — consistent repeat purchasers.
    - **Recent Customers** (R>=4, F<=2): 15,300+ customers — newly acquired buyers.
    - **At Risk High Value** (R<=2, F>=3, M>=3): 13,486 customers — high historic spend, dormant recency.
    - **Inactive** (R<=2): 24,490 customers — largest dormant single-order pool.
    - **Developing** (Others): 19,126 customers — moderate activity.
- **Outputs**: Segment-level summary metrics (avg spend, customer counts, frequency) utilized directly in Power BI Page 2.

---

### [05_Customer_Experience.ipynb](05_Customer_Experience.ipynb)
**Purpose**: Logistics SLA tracking, delivery delay analysis, regional fulfillment performance, and review score correlation.

- **Key Operations**:
  - **Delivery Timelines**: Calculated elapsed days between purchase, carrier handover, customer delivery, and estimated delivery dates.
    - Average delivery time: **12.50 days** (Median: **10.00 days**).
  - **SLA Fulfillment Compliance**:
    - On-Time Orders: **89,000 (91.89%)**
    - Late Orders: **8,000 (8.11%)**
  - **Regional Disparity**: Quantified late delivery percentages across states. Northern/Northeastern states (`AL`: 24%, `MA`: 20%, `PI`, `CE`, `SE`, `BA`) suffer disproportionately higher delays than Southern/Southeastern states (`SP`: ~7%, `PR`, `SC`, `RS`).
  - **Review Rating Degradation**: Evaluated customer ratings across delivery delay bands:
    - Delivered On Time: **4.29★**
    - 1–3 Days Late: **3.30★**
    - 4–7 Days Late: **2.10★**
    - 8+ Days Late: **1.70★**
- **Visuals Produced**: Monthly late delivery rate trends, delay severity vs rating bar charts, regional SLA heatmaps.

---

### [06_EDA_Statistical_Analysis.ipynb](06_EDA_Statistical_Analysis.ipynb)
**Purpose**: Statistical distribution profiling, tail-risk assessment, non-parametric hypothesis testing, and correlation analysis.

- **Key Operations**:
  - **Distribution Profiling**: Confirmed strong right-skewness in order value and customer spending:
    - Order Value: Median **R$ 86.90** vs Mean **R$ 137.75** (Max: R$ 13,440).
    - Delivery Lead Time Percentiles: 50th (10.2d), 75th (15.7d), 90th (23.1d), 95th (29.3d), 99th (46.1d).
  - **Hypothesis Testing**:
    - $H_0$: There is no difference in the distribution of customer review scores between on-time and late deliveries.
    - $H_1$: There is a statistically significant difference in review score distributions between on-time and late deliveries.
    - Executed a two-sided **Mann-Whitney U Test**:
      - Statistic: **524,826,310.5**
      - $p$-value: **0.000 ($p < 0.001$)**
      - *Conclusion*: Reject $H_0$ with overwhelming statistical significance. The results provide strong evidence of a statistically significant negative association between delivery delays and review scores.
  - **Correlation Analysis**:
    - Calculated **Spearman Rank Correlation** between delivery delay (days) and customer review scores:
      - Spearman's $\rho$: **-0.176**
      - $p$-value: **0.000 ($p < 0.001$)**
      - *Conclusion*: Confirms a statistically significant negative monotonic relationship, demonstrating that delivery delays are significantly associated with lower customer review scores.
- **Outputs**: Statistical validation underpinning executive decision-making and Power BI dashboard design.

---

## 💻 How to Run the Notebooks

### Local Environment Setup

1. **Activate your environment**:
   ```bash
   cd "d:/E-Commerce Analytics"
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install required packages**:
   ```bash
   pip install pandas numpy scipy matplotlib seaborn plotly sqlalchemy psycopg2-binary kagglehub
   ```

3. **Launch Jupyter**:
   ```bash
   jupyter lab notebooks/
   # or
   jupyter notebook notebooks/
   ```

4. **Execution Order**:
   Run the notebooks in numeric sequence (`01` through `06`). Notebooks `02` through `06` query the database or use the cleaned data generated in notebook `01`.

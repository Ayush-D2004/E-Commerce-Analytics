# 📑 Executive Business Insights: Brazilian E-Commerce Analytics

An executive-level synthesis of market dynamics, customer behavior, fulfillment reliability, and operational performance derived from the Brazilian E-Commerce (Olist) marketplace dataset (~100,000 orders between 2016 and 2018).

---

## 1. Executive Summary

Olist operates a nationwide e-commerce marketplace connecting small-to-medium Brazilian merchants with consumers across all 27 federative units. This analytics project evaluated transactional records, customer feedback, and delivery logistics to uncover operational bottlenecks and growth opportunities.

### Key Headline Insights
- **Total Marketplace Volume**: Generated **R$ 13.59M** in merchandise sales (GMV) and **R$ 2.25M** in freight charges across **99,441 orders** (Total merchandise + freight value: **R$ 15.84M**).
- **The Retention Hurdle**: **96.9%** of buyers purchased only once. Repeat purchasers accounted for just **3.1%** of the customer base, representing a major untapped driver for customer lifetime value (LTV).
- **Category Concentration**: The marketplace follows an **80/20 Pareto pattern**—the top 7 categories (`health_beauty`, `watches_gifts`, `bed_bath_table`, `sports_leisure`, `computers_accessories`, `furniture_decor`, `cool_stuff`) account for **49.76%** of revenue, while the top 8 exceed 50% (generating **53.33%** of total merchandise revenue).
- **Geographic Dominance**: The Southeast region drives marketplace demand. São Paulo (`SP`) alone accounts for **38.28%** of revenue and **41.98%** of order volume.
- **Fulfillment vs. Satisfaction Link**: Marketplace fulfillment maintains a **91.89% on-time rate**. However, when orders are late, customer ratings drop precipitously from **4.29★ to 1.7★**.
- **Statistical Significance**: A non-parametric **Mann-Whitney U Test** ($p < 0.001$) and **Spearman Rank Correlation** ($\rho = -0.176, p < 0.001$) provide strong evidence of a statistically significant negative association between delivery delays and review scores.

---

## 2. Sales & Revenue Dynamics

### 2.1 Order & Revenue Trajectory
Marketplace gross merchandise value grew consistently throughout 2017 and 2018:
- **Starting Baseline**: Q4 2016 saw modest order volume (~300 orders/month) following initial platform rollout.
- **Scaling Phase**: Monthly orders scaled from ~800 orders in January 2017 to over 7,000 orders/month in early 2018.
- **Black Friday Surge**: November 2017 produced the platform's highest monthly volume with **7,451 orders** and **R$ 1.01M** in merchandise sales.

### 2.2 Average Order Value (AOV)
- **Mean vs. Median Disparity**: The mean order value was **R$ 137.75** (at the item level) and **R$ 120.65** (at the basket level), while the median order value was **R$ 86.90**.
- **Skewness**: Order values are heavily right-skewed. While typical purchases are under R$ 100, occasional high-value purchases (up to R$ 13,440) substantially elevate the average.
- **Freight Share**: Total freight charges (R$ 2.25M) represented **14.2%** of combined merchandise and shipping expenditure, indicating that shipping costs are a significant component of customer spend.

---

## 3. Product & Category Concentration (Pareto Analysis)

Marketplace demand is concentrated within a small subset of dominant merchandise categories:

| Rank | Category (English) | Revenue (R$) | Revenue Share (%) | Cumulative Share (%) |
|:---:|---|:---:|:---:|:---:|
| 1 | `health_beauty` | R$ 1.26M | 9.26% | 9.26% |
| 2 | `watches_gifts` | R$ 1.21M | 8.87% | 18.13% |
| 3 | `bed_bath_table` | R$ 1.04M | 7.63% | 25.76% |
| 4 | `sports_leisure` | R$ 0.99M | 7.27% | 33.03% |
| 5 | `computers_accessories` | R$ 0.91M | 6.71% | 39.74% |
| 6 | `furniture_decor` | R$ 0.73M | 5.37% | 45.11% |
| 7 | `cool_stuff` | R$ 0.63M | 4.65% | 49.76% |
| 8 | `housewares` | R$ 0.49M | 3.57% | 53.33% |

### Strategic Implications
- **Core Revenue Drivers**: The top 7 categories account for ~49.8% of merchandise revenue (and the top 8 exceed 50% at 53.3%), while the remaining ~63 categories account for the long tail.
- **Supply Stability**: Supply disruptions or seller churn in `health_beauty` and `watches_gifts` have a disproportionate impact on top-line platform revenue.

---

## 4. Geographic Distribution & Regional Disparities

Demand and supply are heavily concentrated in the industrialized Southeast region:

### State-by-State Revenue Contribution
- **São Paulo (`SP`)**: R$ 5.20M (38.28% of revenue, 41,750 orders).
- **Rio de Janeiro (`RJ`)**: R$ 1.82M (13.42% of revenue, 12,850 orders).
- **Minas Gerais (`MG`)**: R$ 1.59M (11.66% of revenue, 11,640 orders).
- **Southern Region (`RS`, `PR`, `SC`)**: Combined R$ 1.95M (~14.4% of revenue).
- **Top 3 States Total**: SP, RJ, and MG together account for **63.36%** of marketplace revenue.

### Regional Logistics Friction
While demand is concentrated in the Southeast, deliveries to the North (`AM`, `PA`, `RO`, `AC`) and Northeast (`AL`, `MA`, `PI`, `CE`, `BA`) suffer substantial friction:
- **Longer Transit Times**: Average delivery timelines to Northern states exceed 20–25 days, compared to 8–10 days within São Paulo.
- **Higher Freight Burden**: Customers in remote states pay up to **2.5× higher freight charges** relative to item price.

---

## 5. Customer Behavior & RFM Segmentation

### 5.1 The Repurchase Deficit
- **Single-Purchase Buyers**: **96.9%** (93,099 customers) placed only a single order.
- **Repeat Buyers**: Only **3.1%** (2,997 customers) ordered more than once.
- **Mean Order Frequency**: 1.03 orders per customer (maximum observed: 16 orders).
- **Core Problem**: The business relies almost exclusively on continuous, costly customer acquisition rather than recurring lifetime value.

### 5.2 RFM Behavioral Segmentation Model
Using Recency (days since last purchase), Frequency (distinct orders), and Monetary value (total spend), customers were segmented into six behavioral tiers using quintile-based RFM scoring:

| Segment | Customers | Avg. Recency (Days) | Avg. Frequency | Avg. Spend (R$) | Total Revenue (R$) | Strategic Focus |
|---|:---:|:---:|:---:|:---:|:---:|---|
| **Champions** | 6,651 | 138.9 | 1.17 | R$ 277.03 | R$ 1.84M | VIP loyalty perks, early access |
| **Loyal Customers** | 16,332 | 148.2 | 1.02 | R$ 178.50 | R$ 2.91M | Cross-selling, subscription offers |
| **Recent Customers** | 15,310 | 142.1 | 1.00 | R$ 102.60 | R$ 1.57M | Onboarding nurture sequences |
| **At Risk High Value** | 13,486 | 443.7 | 1.07 | R$ 216.87 | R$ 2.92M | Win-back discounts, dedicated service |
| **Inactive** | 24,490 | 445.7 | 1.00 | R$ 96.38 | R$ 2.36M | Low-cost re-engagement / churn |
| **Developing** | 19,126 | 269.7 | 1.04 | R$ 134.39 | R$ 2.57M | Promotional incentives, cart reminders |

---

## 6. Logistics SLA & Operational Performance

### 6.1 Delivery Timelines & Percentiles
Fulfillment exhibits substantial dispersion:
- **Mean Delivery Time**: **12.50 days**
- **Median Delivery Time**: **10.00 days**
- **75th Percentile**: 15.72 days
- **90th Percentile**: 23.10 days
- **95th Percentile**: 29.28 days
- **99th Percentile**: 46.05 days (tail risk)

> **Key Insight**: While typical orders arrive in 10 days, 10% of customers wait more than 23 days, and 5% wait nearly a full month. Relying solely on average delivery times hides this severe tail risk.

### 6.2 Fulfillment SLA Compliance
- **On-Time Deliveries**: **89,000 orders (91.89%)**
- **Late Deliveries**: **8,000 orders (8.11%)**
- **Seasonal Drops**: Late delivery rates spiked during peak volume periods—reaching **~17%** in March 2018 and **~14%** in November 2017 (Black Friday).

### 6.3 Geographic SLA Failure Rates
Late delivery rates vary dramatically across states:
- **High-Reliability States**: `SP` (~7%), `PR` (~6%), `SC` (~7%), `MG` (~8%).
- **Severe Bottleneck States**: Alagoas (`AL`, ~24% late), Maranhão (`MA`, ~20% late), Piauí (`PI`, ~17% late), Ceará (`CE`, ~16% late), Sergipe (`SE`, ~16% late), Bahia (`BA`, ~15% late).

---

## 7. Delivery Delays and Customer Satisfaction

Customer review scores are heavily polarized across the marketplace:
- **5 Stars**: 57.78% (57,330 reviews)
- **4 Stars**: 19.29% (19,140 reviews)
- **3 Stars**: 8.24% (8,180 reviews)
- **2 Stars**: ~3.00% (2,980 reviews)
- **1 Star**: 11.51% (11,420 reviews)
- **Average Rating**: **4.09 / 5.0**

### Review Score Degradation Curve
Delivery delays are significantly associated with lower customer review scores:

```
Average Review Rating by Delivery Performance
┌────────────────────────────────────────────────────────┐
│ On-Time Delivery:   ★★★★☆ 4.29                         │
│ 1–3 Days Late:      ★★★☆☆ 3.30                         │
│ 4–7 Days Late:      ★★☆☆☆ 2.10                         │
│ 8+ Days Late:       ★☆☆☆☆ 1.70                         │
└────────────────────────────────────────────────────────┘
```

When an order arrives even 1 to 3 days late, customer rating drops by **~1 full star** (from 4.29 to 3.30). Delays beyond 4 days reliably trigger 1-star and 2-star reviews.

---

## 8. Statistical Hypothesis Testing

To verify whether the relationship between delivery delays and customer review scores was statistically meaningful rather than random variance, two rigorous tests were conducted:

### Test 1: Non-Parametric Difference in Distributions (Mann-Whitney U Test)
- **Null Hypothesis ($H_0$)**: There is no difference in the distribution of customer review scores between on-time and late deliveries.
- **Alternative Hypothesis ($H_1$)**: Orders delivered late receive statistically different review scores than on-time orders.
- **Test Result**:
  - Mann-Whitney U Statistic: **524,826,310.5**
  - $p$-value: **0.000 ($p < 0.001$)**
  - **Conclusion**: Reject $H_0$. The results provide strong evidence of a statistically significant negative association between delivery delays and review scores.

### Test 2: Monotonic Association (Spearman Rank Correlation)
- **Objective**: Quantify whether review scores decrease monotonically as delivery delays increase.
- **Test Result**:
  - Spearman's $\rho$: **-0.176**
  - $p$-value: **0.000 ($p < 0.001$)**
  - **Conclusion**: Confirms a statistically significant negative monotonic relationship. While product quality and merchant communication contribute to ratings, delivery delays are significantly associated with lower customer review scores.

---

## 9. Actionable Business Recommendations

Based on the combined sales, customer, operational, and statistical findings, the following strategies are recommended:

### 1. Optimize Logistics & Carrier Operations
- **Regional Fulfillment Hubs**: Establish partner fulfillment centers or cross-docking points in the Northeast (e.g., Bahia or Pernambuco) to reduce delivery times to Northern/Northeastern states from 20+ days to under 7 days.
- **Dynamic SLA Promises**: Update delivery date algorithms to reflect regional transit variability. Promising a realistic 14-day window and delivering in 12 days creates delight; promising 8 days and delivering in 10 creates dissatisfaction.
- **Seasonal Buffer Capacity**: Secure additional carrier capacity ahead of peak surges (March promotions and November Black Friday) to avoid recurring SLA spikes.

### 2. Build a Customer Retention Engine
- **Post-Purchase Engagement**: With 96.9% one-time purchasers, implement automated post-delivery email/WhatsApp flows tailored to the customer's purchased category (e.g., replenishment reminders for `health_beauty` after 45 days).
- **Targeted "At Risk High Value" Win-Back**: The **13,486 At-Risk High-Value customers** represent R$ 2.92M in historical spend. Prioritize personalized re-activation campaigns with compelling discounts.
- **VIP Rewards for Champions**: Offer exclusive loyalty perks, free shipping vouchers, or priority customer support to the **6,651 Champions** to protect platform advocacy.

### 3. Category Management & Assortment Focus
- **Protect Top Product Categories**: Focus seller onboarding and merchant support on the top 8 categories driving ~53% of merchandise revenue (with the top 7 driving ~49.8%). Ensure competitive pricing and broad SKU variety.
- **Audit High-Freight Friction Categories**: Investigate categories where shipping costs represent an unusually high share of cart value (e.g., heavy `furniture_decor`), introducing merchant flat-rate shipping incentives.

### 4. Merchant Governance & Operational Incentives
- **Fulfillment-Linked Search Ranking**: Weight carrier handover speed and seller cancellation rates in marketplace search ranking algorithms.
- **Early Intervention on Delayed Shipments**: Proactively notify customers and offer compensation (e.g., shipping vouchers) before orders become 4+ days late, mitigating 1-star reviews.

# Project Overview

## Introduction

This project explores the Brazilian Olist E-Commerce dataset with the goal of understanding delivery logistics, customer behavior, seller operations, and shipment performance through data analysis and machine learning.

The project combines:

- Relational data engineering
- Exploratory data analysis
- Feature engineering
- Logistics analytics
- Predictive modeling
- Business intelligence extraction

Rather than treating the dataset as isolated CSV files, the project approaches the data as an interconnected relational ecosystem, similar to a production business database.

---

# Business Problem

E-commerce systems rely heavily on providing accurate customer delivery estimates at checkout. However, current legacy systems produce static, over-conservative, or volatile estimates. This creates operational tracking blind spots due to wide variations in log-level steps (such as merchant packaging time, warehouse dispatch pipelines, and long-distance geography). This project answers a critical engineering question: Can we build a data-driven machine learning system to optimize actual fulfillment timelines and outperform the baseline production estimates?

Late deliveries negatively impact:

- Customer satisfaction
- Marketplace trust
- Seller reputation
- Operational efficiency
- Refund and complaint rates

The objective of this project is to understand the major drivers of delivery performance and build predictive systems capable of forecasting delivery behavior.

Key questions explored include:

- What factors influence delivery duration?
- Which seller or product characteristics contribute to delays?
- How do customer locations affect logistics performance?
- Can late deliveries be predicted before shipment completion?
- Which operational bottlenecks create downstream delays?

---

# Project Objectives

The primary objectives of the project are:

1. Analyze the relational structure of the Olist dataset
2. Audit and validate data quality across interconnected tables
3. Engineer meaningful delivery and logistics features
4. Explore behavioral patterns in customers, sellers, and products
5. Build an order-level machine learning dataset
6. Predict delivery duration and delivery delays
7. Extract operational and business insights from logistics data

---

# Dataset Description

## Dataset Source

The project uses the Brazilian Olist E-Commerce dataset.

The dataset contains real-world transactional data from an online marketplace, including information about:

- Orders
- Customers
- Sellers
- Products
- Payments
- Reviews
- Geolocation
- Freight and delivery logistics

  Data Source: Hosted source components sourced from the open-source Brazilian Olist E-Commerce Dataset (Kaggle).

  Scale of Data: Contains 99,441 unique orders representing real multi-regional retail activity across Brazil.

---

# Dataset Tables

| Dataset | Description |
|---|---|
| orders | Central order transaction records |
| customers | Customer demographic and geographic information |
| order_items | Product-level order details |
| products | Product metadata and dimensions |
| sellers | Seller information |
| order_payments | Payment records and installment behavior |
| order_reviews | Customer satisfaction and review scores |
| geolocation | Zip code geographic information |

---
Major Findings:

1. Systematic Buffer Strategy: The active generation layout runs on an overly safe margin configuration. Completed transactions drop off safely ahead of schedule, showcasing structural mean delivery deviation of -11.88 days ahead of target dates.
2. Broken Tracking Pipelines: Relational data audits discovered 775 transactional orders completely missing associated line items. Investigating these anomalies proved that 77.8% (603 orders) were stuck under "unavailable" states and 21.1% (164 orders) were flagged as "canceled", proving that edge-case process loops break line-item tables.
3. Business Insights: Tightening checkout timing forecasts down by matching real operational data can compress delivery estimates by 5 to 7 days. This change will directly boost cart checkout completion metrics without adding financial risk to current courier contracts.

---
# Project Workflow

The project follows a structured analytics and machine learning pipeline.

## Workflow Stages

1. Data Loading
2. Data Inspection
3. Data Quality Auditing
4. Relational Analysis
5. Exploratory Data Analysis (EDA)
6. Feature Engineering
7. Dataset Merging & Aggregation
8. Machine Learning Preparation
9. Model Training
10. Evaluation & Insights

---

# Data Loading

All datasets were imported individually and analyzed before merging.

The loading process involved:

- Reading CSV files
- Inspecting schema consistency
- Identifying primary and foreign keys
- Understanding relational dependencies

Example datasets loaded:

```python
orders = pd.read_csv('olist_orders_dataset.csv')
customers = pd.read_csv('olist_customers_dataset.csv')
order_items = pd.read_csv('olist_order_items_dataset.csv')
products = pd.read_csv('olist_products_dataset.csv')
```

# Data Quality Audit

A major focus of the project was validating data quality and relational consistency before modeling.

---

## Areas Audited

### Missing Values

The project investigated:

- Null timestamps  
- Missing product metadata  
- Missing delivery dates  
- Missing review information  

This helped distinguish between:

- Legitimate missing data  
- Operational anomalies  
- Canceled or incomplete orders  

---

### Duplicate Records

Duplicate analysis was performed across tables to ensure relational integrity and prevent inflated joins.

---

### Relational Integrity Validation

Relationships between datasets were validated using foreign key consistency checks.

Example:

```python
orders['order_id'].isin(order_items['order_id']).all()
```
This helped identify:

- Orders without items  
- Unmatched relationships  
- Incomplete transactions  

---

# Timestamp Integrity

Delivery timeline consistency was audited by validating chronological order between:

- Purchase timestamps  
- Approval timestamps  
- Carrier pickup dates  
- Customer delivery dates  
- Estimated delivery dates  

This prevented logically invalid records from entering the modeling pipeline.

---

# Relational Dataset Analysis

One of the strongest aspects of the project was approaching the datasets relationally before attempting machine learning.

The project analyzed:

- One-to-many relationships  
- Cardinality structures  
- Aggregation requirements  
- Merge expansion risks  

---

# Core Relationship Analysis

## Orders ↔ Order Items

**Relationship Type:** One-to-Many  

**Insight:**
- Most orders contain a single item  
- Some orders contain multiple products  

**Operational implication:**
- Multi-item orders may increase delivery complexity and delay probability  

---

## Orders ↔ Payments

**Relationship Type:** One-to-Many  

**Analysis included:**

- Installment behavior  
- Multiple payment records  
- Payment method distribution  

---

## Order Items ↔ Products

**Relationship Type:** Many-to-One  

**Analysis included:**

- Product dimensions  
- Product weight  
- Freight behavior  

---

## Order Items ↔ Sellers

**Relationship Type:** Many-to-One  

**Analysis included:**

- Seller operational volume  
- Seller delivery performance  
- Geographic seller distribution  

# Aggregation Workflow
```text
geolocation
    ↓
clean geolocation lookup
    ↓

customers + geo_lookup → customer_features
sellers + geo_lookup → seller_features
products cleaned       → product_features

order_items
    + seller_features
    + product_features
        ↓
order_items_enriched
        ↓
distance calculation
        ↓
aggregate by order_id
        ↓
order_level_features
        ↓
orders filtered/cleaned
    + customer_features
    + order_level_features
        ↓

FINAL ML DATASET
(1 ROW = 1 ORDER)
```

# Exploratory Data Analysis (EDA)

EDA focused on understanding delivery behavior and extracting operational insights.

---

## Delivery Analysis

The project explored:

- Delivery duration distribution  
- Early vs late deliveries  
- Average delivery time  
- Delivery performance trends  

**Key insight:**
Delivery duration varies significantly depending on product characteristics, seller region, and operational delays.

---

## Customer Analysis

Customer exploration included:

- Customer geographic concentration  
- State-level order distribution  
- Regional purchasing patterns  

**Key insight:**
Remote geographic regions showed longer delivery times on average.

---

## Seller Analysis

Seller exploration focused on:

- Seller concentration  
- Regional seller density  
- Delivery efficiency by seller  

**Key insight:**
Seller operational performance strongly influences final delivery outcomes.

---

## Product Analysis

The project explored:

- Product weight distribution  
- Product volume  
- Freight cost relationships  
- Category-level behavior  

**Key insight:**
Larger and heavier products generally produce higher freight costs and longer delivery times.

---

## Payment Analysis

Payment exploration included:

- Installment frequency  
- Payment method usage  
- Payment value distribution  

**Key insight:**
Installment-heavy purchases may correlate with higher-value orders and more complex logistics.

---

# Feature Engineering

Feature engineering transformed raw transactional data into predictive intelligence.

---

## Temporal Features

### Delivery Duration

`delivery_days`

Calculated as:
delivery_date - purchase_date


---

### Approval Delay

`approval_delay_hours`

Measured the time between:

- Purchase  
- Payment approval  

---

### Carrier Delay

`carrier_delay_days`

Measured the delay between:

- Approval  
- Carrier shipment  

---

### Delivery Accuracy

`delivery_vs_estimate`

Measured difference between:

- Actual delivery date  
- Estimated delivery date  

**Interpretation:**

- Negative values = delivered early  
- Positive values = delivered late  

---

## Product Features

Engineered product-level features included:

- Total product weight  
- Product volume  
- Average freight value  
- Item count per order  

Example:
product_volume_cm3 = length * height * width


---

## Aggregation Strategy

Since several datasets contain one-to-many relationships, aggregation was necessary before modeling.

Order-level aggregation included:

- Total freight value  
- Total product weight  
- Total item count  
- Average product dimensions  

This prevented row duplication during machine learning preparation.

---

# Dataset Merging

The project used the **orders dataset** as the central merge table because it connects most relational entities.

Merge sequence:

1. Orders  
2. Customers  
3. Order Items  
4. Products  
5. Payments  
6. Reviews  

This produced a unified analytical dataset.

---

# Machine Learning Preparation

The final dataset was transformed into an order-level modeling table.

Key preparation steps included:

- Feature aggregation  
- Null handling  
- Leakage prevention  
- Target creation  
- Train-test splitting  

---

# Target Variable

Two modeling directions were considered:

## Regression

Predict:

- `delivery_days`

## Classification

Predict:

- `is_late`

Where:
delivery_vs_estimate > 0


---

# Model Training

Initial machine learning experimentation included baseline regression models.

Potential models:

- Linear Regression  
- Random Forest  
- XGBoost  
- Gradient Boosting  

---

# Evaluation Metrics

Regression performance was evaluated using:

- MAE (Mean Absolute Error)  
- MSE (Mean Squared Error)  
- RMSE (Root Mean Squared Error)  
- R² Score  

---

# Key Insights

Several important logistics insights emerged from the project.

---

## Logistics Insights

- Multi-item orders increase delivery complexity  
- Heavy products increase freight and delivery duration  
- Approval delays propagate through the logistics pipeline  
- Seller operational efficiency strongly affects final delivery time  

---

## Customer Insights

- Geographic distance impacts delivery speed  
- Delivery delays negatively affect review scores  
- Customer satisfaction is highly sensitive to logistics reliability  

---

## Operational Insights

- Carrier pickup timing is critical  
- Delivery estimation systems may underperform in certain regions  
- Relational aggregation is essential for clean ML training  

---

# Challenges Faced

## One-to-Many Relationship Explosion

Merging transactional tables created duplicated rows that required careful aggregation.

---

## Missing Timestamp Values

Several delivery-related columns contained missing values requiring interpretation and cleaning.

---

## Leakage Prevention

Certain columns contained future information unavailable at prediction time and had to be removed from training data.

---

# Future Improvements

## Advanced Modeling

Potential upgrades:

- XGBoost  
- LightGBM  
- CatBoost  
- Deep learning architectures  

---

## Geospatial Intelligence

Possible additions:

- Distance calculations  
- Route estimation  
- Regional clustering  
- Delivery heatmaps  

---

## NLP Integration

Customer review text could be analyzed using:

- Sentiment analysis  
- Complaint classification  
- Topic extraction  

---

## Dashboard Deployment

A future production version could include:

- Streamlit dashboard  
- Power BI integration  
- Real-time prediction APIs  

---

# Conclusion

This project demonstrates the integration of:

- Data engineering  
- Relational analytics  
- Feature engineering  
- Exploratory analysis  
- Machine learning preparation  
- Logistics intelligence  

The strongest aspect of the project is the relational understanding of the datasets before modeling.

Rather than immediately training models on flat CSV files, the project first focused on understanding:

- Relational architecture  
- Cardinality  
- Aggregation requirements  
- Operational meaning of the data  

This mirrors how real-world analytical and machine learning systems are built in production environments.

The project establishes a strong foundation for scalable logistics analytics and predictive delivery intelligence systems.

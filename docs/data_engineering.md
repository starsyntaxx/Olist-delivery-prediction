#  Olist E-Commerce Data Engineering Pipeline

##  Objective

This notebook builds a complete end-to-end data engineering pipeline for the Brazilian Olist e-commerce dataset. The goal is to transform raw relational data into a clean, feature-rich, machine learning–ready dataset with one row per order.

---

#  1. Project Overview

We work with multiple relational datasets:

- Orders  
- Order Items  
- Customers  
- Sellers  
- Products  
- Geolocation  

---


Each row contains:

- Customer information  
- Seller information  
- Product characteristics  
- Geographic features  
- Logistics features  
- Aggregated order-level statistics  

---

#  2. Data Integration Strategy

##  Initial Data Flow

```
geolocation
↓
clean geolocation lookup
↓

customers + geo → customer_features
sellers + geo → seller_features
products → product_features

order_items
+ seller_features
+ product_features
+ customer_features
↓
order_items_enriched
↓
feature engineering (distance, encoding, logistics signals)
↓
aggregation by order_id
↓
order_level_features
↓
orders_clean + order_level_features
↓
```
FINAL ML DATASET

---

#  3. Geolocation Cleaning

Each zip code can map to multiple latitude/longitude values due to:

- Sampling variation  
- Multiple delivery points per region  
- Data inconsistencies  

### Solution:
We aggregate geolocation using mean coordinates per zip code prefix.

---

#  4. Customer & Seller Feature Engineering

## Merge with Geolocation

We enrich both customers and sellers using:

- Latitude  
- Longitude  
- Missing geo flags  

---

##  Customer Features

- Location (lat/lng)  
- State and city  
- Geo completeness flag  

---

##  Seller Features

- Location (lat/lng)  
- State and city  
- Geo completeness flag  

---

#  5. Product Feature Engineering

We enrich product data with:

- Weight (g)  
- Dimensions (length, height, width)  
- Description length  
- Photo count  
- Category name  

---

#  6. Order Items Enrichment

We merge all enriched datasets into `order_items_enriched`.

Each row contains:

- Customer data  
- Seller data  
- Product data  
- Order item data  

---

#  7. Geographic Feature Engineering

##  Haversine Distance (Seller → Customer)
delivery_ddistance_km

Additional features:

- Latitude difference  
- Longitude difference  

---

#  8. State & City Feature Engineering

##  Frequency Encoding
seller_state freq
customer_sstate freq

Meaning:
- High values → dense logistics regions  
- Low values → sparse regions  

---

##  Tier Encoding

- Tier 3 → SP  
- Tier 2 → RJ, MG, PR, RS, SC  
- Tier 1 → others  

---

##  Cross-State Feature
cross_state delivery

- 1 → interstate delivery  
- 0 → same state  

---

##  Logistics Gap Feature
state_logistics_gap = customer_demand - seller_supply

Captures market imbalance.

---

#  9. Order-Level Aggregation

## Aggregation Strategy

| Feature Type | Aggregation |
|--------------|------------|
| Price | sum, mean, max |
| Freight | sum, mean, max |
| Items | count |
| Products | nunique |
| Sellers | nunique |
| Weight | sum, mean |
| Dimensions | mean |
| Distance | mean, max |
| Flags | max |
| Geographic diversity | nunique |

---

## Result
order_level features

Each row = 1 order

---

#  10. Column Flattening

After aggregation:
('price', 'sum') → price_sum

This ensures ML-friendly column names.

---

#  11. Orders Cleaning

We filter only completed deliveries:

Removed:
- Orders without delivery date  
- Canceled orders  
- Unavailable orders  

---

#  12. Final Merge
orders_clean + order_level_features

Final dataset includes:

- Order metadata  
- Customer behavior  
- Seller behavior  
- Product complexity  
- Logistics signals  

---

#  13. Final Dataset
final_ml_dataset.csv

## 🎯 Final Output

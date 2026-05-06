# Delivery Time Prediction System (End-to-End ML Project)

## 📌 Project Overview
This project builds a machine learning system that predicts real delivery time for e-commerce orders using historical data from an online marketplace.

The goal is to improve delivery time estimation accuracy beyond the platform’s original estimated delivery dates by learning from real-world order behavior.

## Problem Statement
In e-commerce systems, customers are shown an estimated delivery date at checkout. However, these estimates often deviate significantly from actual delivery times due to:
- Logistics delays
- Seller processing time variability
- Geographic distance
- Operational inefficiencies

This project aims to answer:
Can we build a data-driven model that predicts delivery time more accurately than the existing system estimates?

## Dataset
This project uses the Brazilian Olist E-commerce Dataset from Kaggle. It contains multiple relational tables including:
- Orders
- Customers
- Sellers
- Order Items
- Payments
- Reviews
- Geolocation data
Each table simulates a real-world e-commerce backend system with linked relational data.

## Approach
The project follows a structured machine learning pipeline:
1. Data Understanding
- Exploration of multiple relational datasets
- Schema mapping and relationship identification
- Identification of key prediction target: delivery duration
2. Data Cleaning
- Handling missing delivery timestamps
- Parsing datetime fields
- Removing inconsistent records
3. Feature Engineering
Key features include:
- Delivery distance (customer ↔ seller)
- Order purchase time features (weekday, hour, month)
- Seller processing delay
- Payment structure
4. Modeling
Models tested:
- Linear Regression (baseline)
- Random Forest Regressor
- Gradient-based models (future improvement)
5. Evaluation
Model performance is evaluated using:
- Mean Absolute Error (MAE)
- Comparison against platform estimated delivery dates

## System Architecture
The system is designed as an end-to-end pipeline:
- Data ingestion → preprocessing → feature engineering
- Model training → serialization
- API layer for predictions (FastAPI)
- Frontend interface (future extension)

## Architecture diagram:

(Add Excalidraw diagram here)

# Tech Stack
- Python
- pandas, numpy
- scikit-learn
- matplotlib, seaborn
- FastAPI (for backend)
- Google Colab (experimentation)
- VS Code (development)
- Git & GitHub (version control)

## Project Structure
project-root/
│
├── notebooks/        # EDA and experimentation
├── src/              # Core reusable code
├── api/              # FastAPI backend
├── models/           # Saved ML models
├── reports/          # Insights and documentation
├── data/             # Raw/processed data (optional)
├── requirements.txt
└── README.md

## Key Insights (To be updated during project)
- Delivery time increases with geographic distance
- Certain sellers consistently show higher delays
- Platform estimates are often conservative or inconsistent
- Temporal patterns (weekday/month) affect logistics performance

## Model Performance
- Model	MAE
- Baseline (Mean prediction)	TBD
- Linear Regression	TBD
- Random Forest	TBD

## How to Run the Project
1. Clone repository
git clone [GitHub Repo] https://github.com/your-username/repo-name.git
cd repo-name
2. Create environment
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate
3. Install dependencies
pip install -r requirements.txt
4. Run notebooks or scripts
Start with notebooks/01_eda.ipynb

## Future Improvements
- Deploy model as REST API using FastAPI
- Build frontend dashboard for predictions
- Add real-time prediction system
- Improve model with gradient boosting (XGBoost)
- Incorporate external features (weather, holidays)

## Learning Outcomes
This project demonstrates:
- End-to-end ML system design
- Real-world dataset handling
- Feature engineering on relational data
- Model evaluation beyond accuracy
- Transition from notebooks to production-style code

## Author
Built as part of an end-to-end machine learning portfolio project focused on real-world e-commerce systems and predictive analytics.

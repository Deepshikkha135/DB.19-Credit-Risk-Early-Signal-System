# DB.19-Credit-Risk-Early-Signal-System

 A lightweight Early Warning System (EWS) to detect credit-card delinquency risk 30–45 days early using behavioral features, a rule-based scoring engine, and ML prototype.

---

## Dataset

- **Dataset Used:** [UCI Credit Card Dataset](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset) (`UCI_Credit_Card.csv`)  
- **Size:** 30,000+ customer records with billing history, payment amounts, credit limits, and repayment status.  
- **Target Variable:** `default.payment.next.month` (renamed `default`) — indicates whether a customer defaults next month.

---

## Workflow

The EWS system follows these steps:

1. **Data Input:** Load transactional and billing data for credit card customers.  
2. **Feature Engineering:** Compute behavioral features including:
   - **Utilization ratios** (`bill_amt / limit_bal` for months 4–6)  
   - **Payment ratios** (`pay_amt / bill_amt` for months 4–6)  
   - **Behavioral trends** (`pay_drop`, `util_change`)  
   - **Spend spike** (`bill_amt6 / mean(bill_amt4-6)`)  
   - **Delay trend** (`pay_0 - pay_2`)  
3. **Rule-based ERS Engine:**
   - Assigns scores based on utilization, payment ratio, delay trend, and spend spike.  
   - Categorizes customers into **Low, Medium, High risk** (`ers_flag`).  
4. **ML Model:**
   - Features: `util_6`, `pay_ratio_6`, `delay_trend`, `spike`, `pay_drop`, `util_change`.  
   - Apply **SMOTE** for class balance.  
   - Train **Logistic Regression** and **Random Forest** models.  
   - Predict probability of default and bucket into **Low, Medium, High** (`ml_bucket`).  
5. **Final Risk Decision:**
   - Combine **ERS score** and **ML bucket** into `final_risk`.  
   - If either is High, then High; if either is Medium, then Medium; else Low.  
6. **Alert Generation:**
   - Generate SMS templates for early warnings:
     - Soft reminders  
     - Personal insights  
     - Loss-aversion alerts  
     - Urgent calls  
7. **Visualization:**
   - ERS distribution charts  
   - Projected ERS trend charts  
   - Default probability histogram  
8. **Export High-risk Customers separately:**
   - Save flagged customers to `high_risk_customers.csv`.

---

## Architecture

1. Data Input Layer: Load customer transactional and billing data from CSV.

2. Feature Engineering: Compute utilization, payment ratios, behavioral trends, and spend spikes.

3. Rule-based ERS Engine: Score customers based on risk rules.

4. ML Prediction (optional): Logistic Regression / Random Forest predicts default probabilities.

5. Risk Flagging: Combine ERS score and ML bucket to determine final risk.

6. Alerts & Messaging: Generate SMS templates for high-risk customers.

Visualization: ERS distribution, projected trends, and model evaluation charts.
---

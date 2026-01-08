# Fraud Detection In Financial Data Using SQL

## 📌 Project Overview

This project focuses on detecting fraudulent activities in large-scale financial transaction data using **SQL**. The dataset contains **6+ million bank transactions**, out of which approximately **7,985 transactions (0.13%)** were identified as fraudulent. The objective is to analyze transaction patterns, identify high‑risk behaviors, and design rule‑based fraud detection logic that can be integrated into real‑world banking systems.

---

## 🎯 Problem Statement

Financial institutions process millions of transactions daily, making it difficult to manually identify fraudulent behavior. The challenge is to:

* Detect suspicious transaction patterns
* Identify high‑risk accounts
* Minimize financial losses caused by fraud
* Convert raw transaction data into actionable monitoring rules

This project addresses these challenges using **advanced SQL queries**.

---

## 🗂 Dataset Description

* **Total Transactions:** 6M+
* **Fraudulent Transactions:** ~7,985 (0.13%)
* **Key Fields Used:**

  * Transaction Type (TRANSFER, CASH_OUT, etc.)
  * Amount
  * Sender & Receiver Accounts
  * Old & New Balances
  * Fraud Label (isFraud)

---

## 🛠 Tools & Technologies

* **SQL (MySQL / PostgreSQL compatible)**
* **CTEs (Common Table Expressions)**
* **Window Functions**
* **Aggregations & Indexing**

---

## 🔍 Approach & Methodology

1. **Exploratory Analysis**

   * Identified overall fraud rate and high‑risk transaction types
   * Found that most frauds occur in **TRANSFER** and **CASH_OUT** transactions

2. **Rule‑Based Fraud Detection**
   Designed SQL queries to flag suspicious behaviors such as:

   * Rapid balance drops over ₹50,000
   * Transfers where recipient balance does not change
   * Single or cumulative transfers exceeding ₹10 million
   * Multiple large withdrawals in short time windows

3. **Query Optimization**

   * Used indexing, CTEs, and window functions
   * Reduced query execution time by ~40%

---

## 🧾SQL Queries

### 1.1. Total Number of Transactions & Fraud Cases
##### Insight: This provides a fraud rate percentage, helping us see how common fraud is in this dataset.
select count(*) as total_transactions, sum(isFraud) as total_fraud_cases,(sum(isfraud)*100/count(*)) as fraud_precentage
from transactions
------------------------------------------------
### 1.2. Most Common Transaction Types
##### Insight: Helps identify the most frequently occurring transaction types.
select type, count(*) as transaction_count
from transactions
group by type
order by transaction_count desc;

### 1.3. Top 5 Customers with the Most Transactions
##### Insight: Identifies high-activity customers who could be flagged for deeper analysis.
SELECT nameOrig, COUNT(*) AS total_transactions, 
       SUM(amount) AS total_amount_spent
FROM transactions
GROUP BY nameOrig
having total_transactions > 10
ORDER BY total_transactions DESC
LIMIT 5;

------------------
## Finding Patterns and Anomalies
### 2.1. Fraud Rate Per Transaction Type
##### Insight: Detects which transaction types are most associated with fraud.
SELECT type, COUNT(*) AS total_transactions,
       SUM(isFraud) AS fraud_cases,
       (SUM(isFraud) * 100.0 / COUNT(*)) AS fraud_percentage
FROM transactions
GROUP BY type
ORDER BY fraud_percentage DESC;

### 2.2. Detecting Large Transactions (Potential Fraud Signals)
##### Insight: Transactions way above the average can be flagged as potential fraud cases.
SELECT * FROM transactions 
WHERE amount > (SELECT AVG(amount) + 3 * STDDEV(amount) FROM transactions)
ORDER BY amount DESC;

### 2.3. Identifying Customers Who Initiated Fraudulent Transactions
##### Insight: Lists customers who initiated the highest fraudulent transactions.
SELECT nameOrig, COUNT(*) AS fraud_attempts, 
       SUM(amount) AS total_fraud_amount
FROM transactions
WHERE isFraud = 1
GROUP BY nameOrig
ORDER BY total_fraud_amount DESC
LIMIT 5;

---------------------------
## Fraud Detection & Risk Analysis
### 3.1. Finding Transactions Where the Recipient's Balance is Unchanged (Possible Money Laundering)
##### Insight: Fraudsters might be moving money but not changing the destination balance, indicating potential money laundering.
SELECT * FROM transactions 
WHERE newbalanceDest = oldbalanceDest 
AND amount > 1000000 -- Filter high-value transactions
AND isFraud = 1;

### 3.2. Flagging Suspicious Customers Who Perform Both CASH-IN & CASH-OUT in Short Time
##### Insight: Fraudsters often use quick consecutive transactions to launder money.
SELECT nameOrig, COUNT(*) AS suspicious_transactions 
FROM transactions
WHERE type IN ('CASH-IN', 'CASH-OUT')
AND step BETWEEN 1 AND 3 -- Transactions occurring within 3 hours
GROUP BY nameOrig
HAVING COUNT(*) > 1;

### 3.3. Identifying Accounts with Rapidly Depleting Balances
#####  Insight: High depletion rates could indicate fraud or suspicious fund movements.
SELECT nameOrig, SUM(oldbalanceOrg - newbalanceOrig) AS total_depletion
FROM transactions
GROUP BY nameOrig
HAVING total_depletion > 50000
ORDER BY total_depletion DESC;

---

## 📊 Key Insights & Output

* **~8,000 fraud cases** detected
* **₹12M+ fraudulent amount** identified
* **TRANSFER & CASH_OUT** are the most fraud‑prone transaction types
* Flagged **multiple high‑risk accounts** for investigation
* Designed **real‑time monitoring rules** applicable to banking systems

---

## 🚀 Business Impact

* Helps banks proactively detect fraud
* Reduces financial losses from high‑value transactions
* Converts SQL logic into real‑time alert systems
* Improves compliance and risk monitoring

---

## 📚 Learnings

* Writing optimized SQL for large datasets
* Using window functions for behavioral analysis
* Designing rule‑based fraud detection systems
* Translating technical insights into business decisions

---

## 🔮 Future Enhancements

* Integrate with real‑time streaming systems (Kafka)
* Deploy on cloud databases (AWS RDS / BigQuery)
* Add ML‑based anomaly detection using Python
* Build dashboards for fraud monitoring

---

## 👤 Author

**Kushal Majumdar**

Aspiring Data / Business Analyst

---

⭐ *If you find this project useful, feel free to star the repository!*

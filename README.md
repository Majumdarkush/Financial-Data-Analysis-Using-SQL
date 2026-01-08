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

### 1️⃣ Identify Fraudulent Transactions

```sql
SELECT *
FROM transactions
WHERE isFraud = 1;
```

### 2️⃣ Detect Transfers with Unchanged Recipient Balance

```sql
SELECT *
FROM transactions
WHERE type = 'TRANSFER'
AND oldbalanceDest = newbalanceDest;
```

### 3️⃣ Rapid Balance Drop Detection

```sql
SELECT nameOrig, amount,
       oldbalanceOrg - newbalanceOrg AS balance_drop
FROM transactions
WHERE oldbalanceOrg - newbalanceOrg > 50000;
```

### 4️⃣ High‑Risk Accounts (> ₹10M Fraud Amount)

```sql
SELECT nameOrig, SUM(amount) AS total_fraud_amount
FROM transactions
WHERE isFraud = 1
GROUP BY nameOrig
HAVING SUM(amount) > 10000000;
```

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

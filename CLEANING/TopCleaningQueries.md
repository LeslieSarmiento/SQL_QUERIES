# 🧹 My Favorite SQL Data Cleaning Functions  

---

## 📌 Project Overview

Data cleaning is one of the most critical (and time-consuming) parts of working in data. In real-world scenarios, messy datasets are the norm—not the exception. This project walks through a full SQL data cleaning workflow using a synthetic dataset of 1,000 customer records.

### 🎯 Objective
Transform a messy dataset into a clean, standardized, and deduplicated dataset using core SQL functions commonly expected in data analyst roles.

### 💼 Business Problem
Inconsistent and dirty data leads to:
- Incorrect aggregations and reporting  
- Misleading insights for stakeholders  
- Broken dashboards and downstream processes  

This project demonstrates how to systematically clean and prepare data for accurate analysis.

---

## 📊 Dataset Description

The dataset (`messy_customers`) contains 1,000 rows of customer data with common real-world issues:

- `first_name`, `last_name`: Null values and unwanted quotes  
- `email`: Missing (null) values  
- `state`: Inconsistent formats (e.g., "CA", "California", "ca")  
- `signup_date`: Mixed formats (dates, datetimes, quotes, spaces)  
- Duplicate records based on email  

---

## 🛠️ Data Cleaning Steps

---

### 1. REPLACE — Remove Special Characters from Name Columns

Removes single and double quotes from `first_name` and `last_name`, then combines them into a clean full name.

```sql
SELECT
  first_name,
  last_name,
  REPLACE(REPLACE(first_name, '''', ''), '"', '') AS first_name_clean,
  REPLACE(REPLACE(last_name, '''', ''), '"', '') AS last_name_clean,
  CONCAT(
    COALESCE(REPLACE(REPLACE(first_name, '''', ''), '"', ''), ''),
    ' ',
    COALESCE(REPLACE(REPLACE(last_name, '''', ''), '"', ''), '')
  ) AS full_name_clean
FROM messy_customers;
```

### 2. CAST — Standardize Date Format

Cleans and converts inconsistent date formats into a standardized DATE.

```sql
SELECT
  signup_date,
  REPLACE(REPLACE(signup_date, '''', ''), '"', '')::date AS date_clean
FROM messy_customers; 
```
Instead of using the traditional CAST function, which is CAST(value AS DATE), I used the ::date shorthand notation:CAST(REPLACE(REPLACE(signup_date, '''', ''), '"', '') AS DATE) AS date_clean

### 3. COALESCE — Handle Null Email Values

Replaces null emails with a default value to prevent issues in analysis.

```sql
SELECT
  email,
  COALESCE(email, 'unknown') AS email_clean
FROM messy_customers;
```

### 4. CASE WHEN — Standardize State Values

Normalizes inconsistent state entries into standardized uppercase abbreviations.

```sql
SELECT
  state,
  CASE
    WHEN LOWER(state) IN ('ca', 'california') THEN 'CA'
    WHEN LOWER(state) IN ('tx', 'texas') THEN 'TX'
    WHEN LOWER(state) IN ('fl', 'florida') THEN 'FL'
    WHEN LOWER(state) IN ('ny', 'new york') THEN 'NY'
  END AS state_code_clean
FROM messy_customers;
```

### 5. ROW_NUMBER() — Remove Duplicate Records

Step 1: Identify duplicates
```sql
SELECT
  email,
  COUNT(*) AS duplicate_count
FROM messy_customers
GROUP BY email
HAVING COUNT(*) > 1;
```
Step 2: Deduplicate using ROW_NUMBER()

```sql
SELECT
  customer_id,
  email,
  ROW_NUMBER() OVER (PARTITION BY email ORDER BY customer_id) AS row_num
FROM messy_customers
QUALIFY row_num = 1;
```

## ✅ Final Output

- Original dataset: **1,000 messy records**  
- Cleaned dataset: **11 unique, standardized records**  
- Fully deduplicated and analysis-ready  

---

## 🚀 Key Takeaways

- Clean data directly impacts the quality of insights and decisions  
 
---

## 💡 Skills Demonstrated

- Data cleaning and preprocessing  
- SQL transformations  
- Handling null values  
- Data standardization  
- Deduplication using window functions  
- Writing production-style SQL queries  

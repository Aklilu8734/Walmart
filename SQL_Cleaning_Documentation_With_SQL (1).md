
# 📄 SQL Cleaning & Feature Engineering Script

**Project:** Walmart Customer Behavior Analysis  
**Environment:** BigQuery  
**Dataset:** `portfolio-457318.walmart_customer_data.walmart_data`  
**Output Table:** `portfolio-457318.walmart_customer_data.cleaned_walmart_data`  
**File:** `01_sql_cleaning.sql`  
**Author:** [Your Name]  
**Date:** [Add Date]

---

## 🎯 Objective

To clean Walmart's customer transaction data and engineer features that enable meaningful business analysis and predictive modeling — including segmentation, seasonality, discount impact, and repeat customer behavior.

---

## 🧼 Full SQL Script with Comments

```sql
-- ==========================================================
-- Script: Data Cleaning & Feature Engineering for Walmart Data
-- Source Table: portfolio-457318.walmart_customer_data.walmart_data
-- Target Table: portfolio-457318.walmart_customer_data.cleaned_walmart_data
-- Description: Cleans raw data and creates new analytical features
-- ==========================================================

CREATE OR REPLACE TABLE portfolio-457318.walmart_customer_data.cleaned_walmart_data AS
SELECT
  -- Original Columns
  Customer_ID,
  Age,
  Gender,
  City,
  Category,
  Product_Name,
  Purchase_Date,
  Purchase_Amount,
  Payment_Method,

  -- Convert boolean columns to integers for analysis
  CASE WHEN Discount_Applied = TRUE THEN 1 ELSE 0 END AS Discount_Applied,
  Rating,
  CASE WHEN Repeat_Customer = TRUE THEN 1 ELSE 0 END AS Repeat_Customer,

  -- Feature: Age Segmentation
  CASE
    WHEN Age < 20 THEN 'Teen'
    WHEN Age BETWEEN 20 AND 35 THEN 'Young Adult'
    WHEN Age BETWEEN 36 AND 55 THEN 'Adult'
    ELSE 'Senior'
  END AS Age_Group,

  -- Feature: Extract purchase month and day of week
  EXTRACT(MONTH FROM Purchase_Date) AS Purchase_Month,
  FORMAT_DATE('%A', Purchase_Date) AS Purchase_Day,

  -- Feature: Revenue after applying 10% discount if applicable
  CASE
    WHEN Discount_Applied = TRUE THEN ROUND(Purchase_Amount * 0.9, 2)
    ELSE Purchase_Amount
  END AS Net_Revenue,

  -- Feature: High spender flag
  CASE WHEN Purchase_Amount > 300 THEN 1 ELSE 0 END AS High_Spender,

  -- Feature: Simplified rating sentiment
  CASE
    WHEN Rating >= 4 THEN 'Positive'
    WHEN Rating = 3 THEN 'Neutral'
    ELSE 'Negative'
  END AS Rating_Category

FROM
  portfolio-457318.walmart_customer_data.walmart_data

-- Data Quality Filters
WHERE
  Age IS NOT NULL AND Age BETWEEN 10 AND 100
  AND Purchase_Amount IS NOT NULL AND Purchase_Amount > 0
  AND Product_Name IS NOT NULL AND Product_Name != ''
  AND Purchase_Date IS NOT NULL;
```

---

## ✅ Outputs

The resulting table includes:
- Cleaned fields from the raw dataset
- 6 new engineered features
- Ready-to-use structure for Python analysis and Power BI reporting

---

## 📂 Save Location Suggestion in Repo

```
notebooks/
  └── SQL_Cleaning_Documentation.md
```

> Next: Use this cleaned table as input for EDA in Python (`02_eda.ipynb`)


# 📄 SQL Data Cleaning & Feature Engineering Script

**Project:** Walmart Customer Behavior Analysis  
**Environment:** BigQuery  
**Dataset:** `portfolio-457318.walmart_customer_data.walmart_data`  
**Output Table:** `portfolio-457318.walmart_customer_data.cleaned_walmart_data`  
**File:** `01_sql_cleaning.sql`  
**Author:** [Your Name]  
**Date:** [Add Date]

---

## 🎯 Objective

To clean the raw Walmart transaction data and engineer analytical features to support business questions related to customer segmentation, discount effectiveness, repeat purchasing behavior, and seasonal trends.

---

## 🧼 Cleaning Steps

- Removed invalid or null values from key fields (`Age`, `Purchase_Amount`, `Product_Name`, `Purchase_Date`)
- Restricted `Age` to a realistic range (10–100)
- Normalized boolean columns (`Discount_Applied`, `Repeat_Customer`) to integers
- Ensured only valid rows are included for further analysis

---

## 🛠️ Feature Engineering

| Feature Name       | Description |
|--------------------|-------------|
| `Age_Group`        | Buckets customers into Teen, Young Adult, Adult, and Senior |
| `Purchase_Month`   | Extracts month of transaction for trend analysis |
| `Purchase_Day`     | Extracts day of the week (Monday, Tuesday, etc.) |
| `Net_Revenue`      | Adjusts `Purchase_Amount` assuming 10% discount if applied |
| `High_Spender`     | Flags purchases over $300 |
| `Rating_Category`  | Converts numeric rating into Positive / Neutral / Negative |

---

## 📁 How to Use

Run the SQL script `01_sql_cleaning.sql` in your BigQuery SQL editor to create the cleaned and enriched dataset.

Ensure the following permissions:
- BigQuery write access to dataset `portfolio-457318.walmart_customer_data`
- Read access to the source table `walmart_data`

---

## ✅ Output Table Schema

The resulting table `cleaned_walmart_data` contains:
- All original cleaned fields
- 6 engineered fields for downstream EDA, modeling, and dashboarding

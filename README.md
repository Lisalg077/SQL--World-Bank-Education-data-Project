# SQL--World-Bank-Education-data-Project
SQL project analysing World Bank education spending data using BigQuery

 Project Overview

The dataset contains **country-level education indicators** across multiple years, such as enrollment rates and government expenditure on education.  

This project focuses on two main analyses:

1. **Government Expenditure on Education (% of GDP, 2010–2017)**
2. **Exploring Indicator Codes with Broad Coverage (2016)**

Both analyses are performed directly in **BigQuery**, and results are retrieved and displayed in Python (Jupyter notebook).


 Query 1 — Government Expenditure on Education

**Objective:**  
Identify which countries spend the largest fraction of GDP on education.

**Approach:**
- Filter rows for the indicator `SE.XPD.TOTL.GD.ZS` (Government expenditure on education as % of GDP).  
- Include data from **2010 to 2017**.  
- Calculate **average spending per country** using `AVG(value)` and rename the column `avg_ed_spending_pct`.  
- Sort results to show **highest-spending countries first**.

**SQL Query:**
```sql
SELECT country_name, 
       AVG(value) AS avg_ed_spending_pct
FROM `bigquery-public-data.world_bank_intl_education.international_education`
WHERE indicator_code = 'SE.XPD.TOTL.GD.ZS'
  AND year BETWEEN 2010 AND 2017
GROUP BY country_name
ORDER BY avg_ed_spending_pct DESC
 



---

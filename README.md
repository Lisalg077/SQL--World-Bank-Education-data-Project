# SQL-World-Bank-Education-data-Project
SQL project analysing World Bank education spending data using BigQuery

Project Overview
The dataset contains **country-level education indicators** across multiple years, such as enrollment rates and government expenditure on education.  https://www.kaggle.com/code/buddhiniw/bigquery-world-bank-education-data
- **Dataset:** `bigquery-public-data.world_bank_intl_education.international_education`  
  - Hosted publicly on **Google BigQuery**.  
  - Includes country-level education indicators across multiple years.  

- **Important Notes:**
  1. **No raw dataset is included in this repository.**  
     All queries are executed **directly on BigQuery**, so there’s no need to upload or store the data on GitHub.
     
  2. **BigQuery credentials are required to run the code.**  
     Anyone who wants to execute the SQL queries via Python or Jupyter Notebook will need their own Google Cloud account and BigQuery credentials.
Tools Used
SQL (BigQuery Standard SQL)
Python (BigQuery Client & Pandas)
Jupyter Notebook / JupyterHub
GitHub

This project focuses on two main analyses:

1. **Government Expenditure on Education (% of GDP, 2010–2017)**
2. **Exploring Indicator Codes with Broad Coverage (2016)**

Both analyses are performed directly in **BigQuery**, and results are retrieved and displayed in Python (Jupyter notebook).

Dataset: bigquery-public-data.world_bank_intl_education.international_education 
Source: https://data.worldbank.org/
BigQuery access: 

## Query 1: Government Expenditure on Education
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

safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
country_spend_pct_query_job = client.query(country_spend_pct_query, job_config=safe_config)
country_spending_results = country_spend_pct_query_job.to_dataframe()
country_spending_results.head()
```

**Interpretation:**
Highlights countries consistently investing the highest proportion of GDP in education (2010–2017).

## Query 2: Indicators with Broad Global Coverage (2016)

**Objective:**  
Determine which education indicators are **widely reported by countries** in 2016.  
This helps identify metrics suitable for **cross-country comparison** and further analysis.

**Approach:**
- Filter the dataset for **year = 2016**.  
- Group data by `indicator_code` and `indicator_name`.  
- Count the number of rows per indicator using `COUNT(1)` — each row corresponds to one country’s report.  
- Keep only indicators with **175 or more rows**, meaning reported by at least 175 countries.  
- Order the results by `num_rows` descending to see the most widely reported indicators first.

**SQL Query:**
```sql
SELECT indicator_code, 
       indicator_name, 
       COUNT(1) AS num_rows
FROM `bigquery-public-data.world_bank_intl_education.international_education`
WHERE year = 2016
GROUP BY indicator_name, indicator_code
HAVING COUNT(1) >= 175
ORDER BY num_rows DESC;

safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
code_count_query_job = client.query(code_count_query, job_config=safe_config)
code_count_results = code_count_query_job.to_dataframe()
code_count_results.head()
```

**Interpretation:**
Indicators with at least 175 rows are widely reported globally and suitable for cross-country comparisons.












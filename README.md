# Credit Risk & Loan Analytics Dashboard 

This repository contains an end-to-end data engineering and business intelligence project focused on financial risk assessment. Using a dataset of borrower characteristics and loan statuses, I developed a structured Power BI solution to identify patterns leading to loan defaults.

---

## Project Overview
The objective of this project is to provide financial stakeholders with a **Risk Assessment Tool** that identifies the correlation between borrower demographics and loan performance. 

### **Key Objectives:**
* **Identify High-Risk Segments:** Determine which income and education brackets are most prone to defaults.
* **Analyze Financial Ratios:** Evaluate how the loan-to-income ratio affects repayment.
* **Data Integrity:** Transform raw, unstructured CSV data into a professional **Star Schema**.

---

## Data Architecture (Schema Preparation)
I transitioned the flat-file source data into a **Star Schema** to optimize query performance.

### **Model Structure:**
* **Fact Table (`fact_Loans`):** Stores quantitative data including `loan_amnt`, `loan_int_rate`, `credit_score`, and `person_income` and other variables.
* **Dimension Tables:**
    * **Dim_Borrower:** Contains borrower attributes (Gender, Education).
    * **Dim_HomeOwnership:** Categorizes housing status (Rent, Own, Mortgage).
    * **Dim_LoanIntent:** Describes the purpose of the loan (e.g., Medical, Venture).
    * **Dim_Date:** A dedicated DAX calendar table supporting **Time Intelligence**.

### **Relationship Management:**
* **Cardinality:** Established **One-to-Many (1:*)** relationships.
* **Cross-Filter Direction:** Set to **'Single'** to maintain data integrity and prevent circular dependencies.

---

## Data Cleaning & Transformation (ETL)
The following transformations were performed in **Power Query** to ensure data quality:

* **Handling Outliers:** Filtered unrealistic entries in `person_age` and `person_income` to prevent skewed analysis.
* **Null Imputation:** Replaced missing values in `loan_int_rate` with the median value of the dataset.
* **Categorical Mapping:** Converted binary `loan_status` (0/1) into descriptive terms: **"Default"** and **"Non-Default"**.
* **Normalization:** Applied **Trim** and **Clean** functions to text columns to remove whitespace and prevent duplicate categories.
* **Data Typing:** Standardized numerical columns to **Fixed Decimal** and **Whole Number** formats for accurate DAX calculation.

---

## Analytical Measures (DAX)
To drive the dashboard visuals, I developed several custom measures:
* **Total Loan Amount:** `Total Loan Amount = SUM(Fact_Loans[loan_amnt])`
* **Default Rate %:** `Default Rate % = DIVIDE(CALCULATE(COUNTROWS(fact_Loans), fact_Loans[loan_status] = "Default"), COUNTROWS(fact_Loans))`
* **Average Loan Amount:** `Average Loan Amount = AVERAGE(fact_Loans[loan_amnt])`
* **Total Borrowers:** `Total Borrowers = COUNTROWS(fact_Loans)`
* **Average Interest Rate:** `Average Interest Rate = AVERAGE(fact_Loans[loan_int_rate])`
* **Approved Loan Rate:**`Approved Loan Rate % = DIVIDE(CALCULATE(COUNTROWS(fact_Loans), fact_Loans[loan_status] = "Approved"),COUNTROWS(fact_Loans))`
* **Average-to-Loan Ratio:** `Avg Loan % Income = AVERAGE(fact_Loans[loan_percent_income])`
* **High-Risk Loan Counts:** `High Risk Loans = CALCULATE(COUNTROWS(fact_Loans), fact_Loans[loan_percent_income] > 0.4)`
* **Loan Ranking By Amount:** `Loan Rank = RANKX(ALL(fact_Loans),[Total Loan Amount], ,DESC)`

---

## Challenges Encountered
* Lack of a unique borrower identifier limited the creation of a borrower dimension table
* Data inconsistencies required extensive cleaning and transformation
* Designing a meaningful star schema from a flat dataset required careful modeling decisions.

---
## Conclusion
This project demonstrates the application of advanced Power BI techniques to transform raw financial data into actionable insights. The developed dashboard enables stakeholders to explore loan performance, assess risk, and make informed decisions.

---
## 📂 Repository Contents
* `loan_data.csv`: The raw source data.
* `Credit_Risk_Analysis.pbix`: The final Power BI report file.
* `Documentation/`: PDF version of the final project report.

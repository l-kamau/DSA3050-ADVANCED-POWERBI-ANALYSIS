# Credit Risk & Loan Analytics Dashboard 🏦

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
I transitioned the flat-file source data into a **Star Schema** to optimize query performance and ensure a "Single Source of Truth."

### **Model Structure:**
* **Fact Table (`Fact_Loans`):** Stores quantitative data including `loan_amnt`, `loan_int_rate`, `credit_score`, and `person_income`.
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
* **Total Loan Volume:** `Total Loan Volume = SUM(Fact_Loans[loan_amnt])`
* **Default Rate %:** `Default Rate % = DIVIDE(CALCULATE(COUNT(Fact_Loans[loan_status]), Fact_Loans[loan_status] = "Default"), COUNT(Fact_Loans[loan_status]))`
* **Average Credit Score:** `Avg Credit Score = AVERAGE(Fact_Loans[credit_score])`

---

## Challenges Encountered
* **Many-to-Many Relationships:** Initially, linking dimensions directly to the fact table caused ambiguity errors. I resolved this by normalizing the dimension tables—removing duplicates in Power Query—to establish a clean **1:* cardinality**.
* **Schema Mismatches:** Handled missing columns in source files by using **"Choose Columns"** instead of "Remove Columns," making the ETL process more robust.

---

## 📂 Repository Contents
* `loan_data.csv`: The raw source data.
* `Credit_Risk_Analysis.pbix`: The final Power BI report file.
* `Documentation/`: PDF version of the final project report.

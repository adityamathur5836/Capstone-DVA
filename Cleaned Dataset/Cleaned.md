# Cleaned Dataset Documentation

## Project
**Loan Default Risk Analysis & Applicant Risk Segmentation**

## Dataset
Consumer Loan Records (15,000 Loans)

---

# Overview

This document summarizes all data cleaning, validation, transformation, and feature engineering steps performed to convert the raw loan dataset into a structured and analysis-ready format for KPI computation, risk segmentation, and dashboard development.

The objective was to ensure:

- Data integrity  
- Analytical accuracy  
- Reproducibility  
- Executive-ready insights  

---

# 1. Raw Data Preservation

The original dataset was preserved in a separate sheet:

**Sheet Name:** `RAW`

No direct modifications were performed on the raw dataset to ensure:

- Transparency  
- Auditability  
- Reproducibility  

All transformations were performed in the `Cleaned_Data` sheet.

---

# 2. Column Selection

The original dataset contained 33 columns.  
To align with pre-approval risk assessment logic, only 8 columns were retained:

- `loanID`
- `amount`
- `grade`
- `income`
- `debtIncRat`
- `home`
- `reason`
- `status`

These columns represent complete pre-approval financial risk dimensions.

---

# 3. Whitespace Cleaning

Removed leading and trailing spaces using:

=TRIM()


Applied to:

- grade  
- home  
- reason  
- status  

This prevented duplicate categories due to inconsistent spacing.

---

# 4. Text Standardization

Standardized categorical values using:

=UPPER()


Standardized Columns:

- grade (A–G)
- home (RENT / OWN / MORTGAGE)
- status (FULLY PAID / CHARGED OFF)

Ensured uniform grouping in pivot tables.

---

# 5. Default Flag Creation

Created derived column:

## `Default_Flag`

Logic:

=IF(status="CHARGED OFF",1,0)


- 1 → Default  
- 0 → Non-Default  

Enabled binary risk analysis.

---

# 6. Income Bracketing

Created income segmentation column:

## `Income_Bracket`

Logic:

=IF(income<40000,"LOW",
IF(income<80000,"MEDIUM","HIGH"))


Used for repayment capacity analysis.

---

# 7. Risk Score Calculation

Created a composite risk score using:

- grade  
- debtIncRat  
- income  
- home  

## `Risk_Score`

A custom scoring formula was applied to generate numeric borrower risk values.

---

# 8. Risk Category Segmentation

Converted Risk_Score into categorical risk levels.

## `Overall_Risk_Category`

Example logic:

=IF(Risk_Score>12,"HIGH RISK",
IF(Risk_Score>7,"MEDIUM RISK","LOW RISK"))


Enabled portfolio segmentation.

---

# 9. Missing Value Handling

Missing values identified using:

=COUNTBLANK()


Handling strategy:

- Numeric blanks replaced with median
- Categorical blanks replaced with mode
- Rows removed only if critical identifiers (loanID) were missing

---

# 10. Duplicate Removal

Duplicates removed based on:

- `loanID`

Method:
Data → Remove Duplicates

Ensured unique loan records.

---

# 11. Numeric Validation

Validated numeric columns:

- amount > 0
- income > 0
- debtIncRat between 0–100

Ensured realistic financial ranges.

---

# 12. Data Type Validation

| Column | Data Type |
|--------|-----------|
| loanID | Numeric |
| amount | Numeric |
| income | Numeric |
| debtIncRat | Numeric |
| grade | Text |
| home | Text |
| reason | Text |
| status | Text |
| Default_Flag | Numeric (0/1) |
| Risk_Score | Numeric |
| Overall_Risk_Category | Text |

Ensured compatibility with formulas, pivots, and charts.

---

# 13. Pivot Table Structuring

Created pivot tables to analyze:

- Default Rate by Credit Grade
- Default Rate by DTI Range
- Default Rate by Home Ownership
- Average Income by Status
- Risk Category Distribution

Primary structure:

**Rows:**
- grade / home / risk category  

**Values:**
- Average of Default_Flag  
- Count of loanID  

---

# 14. KPI Creation

## 14.1 Overall Default Rate

=COUNTIF(Default_Flag,1)/COUNTA(loanID)


## 14.2 Total Loan Exposure

=SUM(amount)


## 14.3 Average Loan Amount

=AVERAGE(amount)


## 14.4 Average DTI

=AVERAGE(debtIncRat)


## 14.5 High-Risk Borrower Count

=COUNTIF(Overall_Risk_Category,"HIGH RISK")


---

# 15. Approval Recommendation Logic

Created derived column:

## `Approval_Recommendation`

Rule-based logic:

**Approve:**
- Grade A–C AND DTI < 35%
- Grade D AND DTI < 25%

**Reject:**
- Grade E–G

**Manual Review:**
- Borderline cases

Implemented using nested IF conditions.

---

# 16. Final Dataset Structure

| loanID | amount | grade | income | debtIncRat | home | reason | status | Default_Flag | Income_Bracket | Risk_Score | Overall_Risk_Category | Approval_Recommendation |
|--------|--------|--------|--------|------------|------|--------|--------|--------------|----------------|------------|----------------------|--------------------------|

This structured dataset supports:

- Risk segmentation  
- Portfolio risk analysis  
- KPI monitoring  
- Approval decision simulation  
- Dashboard creation  

---

# 17. Data Integrity Outcome

The dataset was:

✔ Cleaned  
✔ Standardized  
✔ Validated  
✔ Segmented  
✔ Enriched with KPIs  
✔ Converted into decision-support framework  

The final cleaned dataset is fully structured for:

- Executive dashboard reporting  
- Risk-based loan approval modeling  
- Portfolio health monitoring  

---

# Conclusion

The raw loan dataset was transformed into a structured, validated, and analysis-ready risk intelligence framework.

Through systematic cleaning, feature engineering, segmentation, and KPI creation, the dataset now supports:

- Data-driven underwriting decisions  
- Early identification of high-risk borrowers  
- Portfolio risk concentration analysis  
- Financial loss reduction strategies  

This cleaning process ensures analytical reliability and executive-level reporting quality.
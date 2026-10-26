# E-commerce Data Quality Project

**README | Outcomes, Definitions, Assumptions, and Dashboard Guide**

## 1. Project Overview

This project profiles and cleans e-commerce transaction data using **Power Query** and uses **Power BI** to summarize data quality and selected business indicators. The raw query is preserved for traceability.

## 2. Dataset and Cleaning Outcomes

| Item | Outcome |
|---|---|
| Original Rows | 12,180 |
| Exact Duplicate Rows Removed | 180 |
| Clean Rows | 12,000 |
| Order_ID | Verified as unique; each order contains one SKU. |
| Quantity Reconciliation | Quantity sum recorded as 24,250 after transformations. |
| Standardization | Country and payment-method labels standardized. Missing shipping cities replaced with `Unknown`. |
| Critical Completeness Fields | `Discount_Percent` and `Shipping_City` |
| Customer Ratings | Missing ratings are treated as not provided, not automatically as a data quality failure. |

## 3. Dashboard Guide

| Section | Purpose |
|---|---|
| **Uniqueness** | Displays original and clean transactions, exact duplicates, and transactions marked for investigation. |
| **Completeness** | Shows transactions with both critical fields complete and transactions missing at least one critical field. |
| **Consistency** | Measures the share of in-scope transactions without a country, date, or payment variation flag. |
| **Timeliness** | Displays the date range, date coverage, and dates marked for investigation. |
| **Business Performance** | Presents provisional sales, returns, cancellations, unresolved transactions, and missing-discount exposure. |
| **Data Cleaning Details** | Shows original and standardized country/payment labels, date-format categories, and the missing-rating percentage. |

## 4. Key Measures

| Measure | Definition / Caution |
|---|---|
| **Uniqueness %** | Clean Transactions ÷ Original Transactions = 12,000 ÷ 12,180 ≈ **98.52%**. Reflects the duplicate-removal approach used in this project. |
| **Completeness %** | Transactions with both discount and shipping city marked `Complete Data` ÷ transactions marked `Transactions Recorded`. |
| **Consistency %** | 1 minus the share of recorded transactions with at least one `Yes` flag for country, date, or payment variation. |
| **DQ Score** | Simple average of Consistency %, Completeness %, and Uniqueness %. This is a project-defined score, not an external standard. |
| **Revenue** | `Unit_Price_USD × Quantity × (1 − Discount_Percent / 100)`. If `Discount_Percent` is missing, full price is used. See the Revenue Calculation Disclaimer below. |
| **Revenue at Risk %** | Share of recorded transactions with a missing discount. This is a transaction percentage, not a dollar-weighted measure of revenue at risk. Consider renaming it to **Transactions Missing Discount %**. |

## 5. Assumptions and Limitations

- **Post-June 2026 dates:** Transactions dated after 30 June 2026 are marked `Investigate`. The observed pattern shows activity only on days 1–6 of later months. This is a potential completeness issue, not proof of missing transactions.
- **Business KPI scope:** Business KPIs and consistency checks are intended to use only rows marked `Date_Flag = "Transactions Recorded"`.
- **Ambiguous dates:** Dates such as `10/11/2024` can have different interpretations. Parsing order can affect the result, so dates should be validated against the source context.
- **Date-format consistency:** Different date formats are not automatically inconsistent if they represent the same date.
- **Missing discounts:** Missing `Discount_Percent` values are handled by the revenue formula using full unit price, effectively assuming a 0% discount. This is a provisional assumption.
- **Missing cities and ratings:** Missing cities are replaced with `Unknown`. Missing customer ratings are treated as not provided.
- **Sales classification:** The `Sales_Status` logic is provisional because authoritative business rules for sales, returns, cancellations, and pending transactions were not provided.
- **Product prices:** Multiple prices for a product were retained because there was no evidence that they were erroneous.
- **DQ Checks:** `DQ Checks = 10` is a manually entered count, not a dynamic measure of passed checks.

## 6. Revenue Calculation Disclaimer

Revenue is calculated using `Unit_Price_USD`, `Quantity`, and the recorded `Discount_Percent`.

When `Discount_Percent` is missing, the calculation uses the full unit price, effectively assuming a 0% discount. These transactions remain included in the sales value when they meet the project's `Sales_Status` criteria.

Actual revenue may differ if a discount was applied but not recorded. The sales classification and revenue calculation are provisional because authoritative business rules have not been provided.

## 7. Project Status

The Power BI report contains two pages:

1. **Data Quality Dashboard**
2. **Data Cleaning Details**

Before treating the results as definitive business figures, review ambiguous dates, the post-June 2026 date pattern, and the provisional sales-status logic.

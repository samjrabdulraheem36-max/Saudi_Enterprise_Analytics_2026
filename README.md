# Saudi Cross-Sector Enterprise Operations BI Engine

An end-to-end Business Intelligence and Analytics Engineering project simulating an operations audit of a high-volume Saudi E-commerce & Logistics marketplace. This model integrates data workflows across Management Consulting, Logistics, and FinTech ecosystems in Riyadh and Jeddah.

## Business Scenario
A premium Saudi marketplace platform requires an executive-level performance evaluation. This project establishes a scalable data framework to track:
1. **FinTech Layer:** Processing volumes across local digital payment networks (*Mada, STC Pay, Apple Pay*) and auditing gateway fee structures.
2. **Logistics & Supply Chain Layer:** Row-level fulfillment velocity metrics across regional distribution centers.
3. **E-Commerce Layer:** Slicing cross-filtering performance parameters by merchant brand profiles and retail categories.

## Relational Data Model (Star Schema)
The database structure optimizes storage efficiency and ensures reporting integrity by connecting lookup attributes to a central transaction ledger:
* `fact_transactions` (The central FinTech transaction log)
* `dim_hubs` (Geographic operational distribution hubs in Riyadh, Jeddah, and Dammam)
* `dim_merchants` (Commercial retail registry data fields)

---

## Technical Implementation Details

### 1. SQL Extraction Layer (`consulting_extraction_query.sql`)
Multi-table query designed to generate reporting datasets for consulting stakeholders. Features multi-layered inner joins, conditional cash-margin tracking logic, and positional category ranking:

```sql
SELECT 
    f.Transaction_ID, 
    h.Region_City, 
    m.Merchant_Name, 
    f.Gross_Volume_SAR,
    CASE 
        WHEN (f.Gross_Volume_SAR - f.Gateway_Fee_SAR) > 3000 THEN 'High Yield'
        ELSE 'Standard Yield'
    END AS Net_Margin_Tier,
    ROW_NUMBER() OVER(PARTITION BY m.Category ORDER BY f.Gross_Volume_SAR DESC) AS category_sales_rank
FROM fact_transactions AS f
INNER JOIN dim_merchants AS m ON f.Merchant_ID = m.Merchant_ID
INNER JOIN dim_hubs AS h ON f.Hub_ID = h.Hub_ID
WHERE h.Region_City IN ('Riyadh', 'Jeddah')
;
```

### 2. Advanced Excel Layer (`Saudi_Marketplace_Core.xlsx`)
* Structured automated ledger systems utilizing `XLOOKUP` arrays to merge transaction sheets.
* Normalized textual anomalies and tracking time fields using `TRIM`, `IFS`, and `NETWORKDAYS` functions.
* Aggregated deep metrics into interactive pivot tables connected to regional operational slicers.

### 3. Power BI Layer (`Saudi_Marketplace_Dashboard.pbix`)
* Configured a 1:M cardinality relational data model with directional single-filtering paths in Model View.
* Formulated row-by-row iteration math via DAX modeling metrics to isolate processing velocities:
  ```dax
  Avg_Fulfillment_Delivery = 
  AVERAGEX(
      fact_transactions, 
      fact_transactions[Delivery_Date] - fact_transactions[Order_Date]
  )
  ```
* Built a fully cross-filtered dashboard layout containing regional sliver metrics, wallet channel share donut distributions, and merchant sales value charts.

**Procurement Analytics Dashboard**

A Power BI report that analyzes Purchase Orders (PO) and Purchase Requisitions (PR) to track procurement performance, spending insights, and vendor delivery efficiency.

**Project Overview**
This dashboard combines multiple procurement data sources to deliver actionable insights for:
- Monitoring total spend trends
- Identifying top vendors and items
- Evaluating delivery performance and lead time
- Highlighting late and high-value procurement risks

**Data Sources**

| Table | Description |
|-------|-------------|
| PR | Purchase Requisition information |
| PO | Purchase Order information |
| Calendar | Date table used for time intelligence |

Data was cleaned, transformed, and merged in Power Query.

**Data Model**
| From | To | Relationship | Type |
|------|----|--------------|------|
| PR[PR_ID] | PO[PR_ID] | Merge Join | INNER |
| Calendar[Date] | PO[PO Date] | Active relationship | 1-to-many |
| Calendar[Date] | PR[PR Date] | Inactive relationship | 1-to-many |

Star schema  
Date hierarchy enabled  

**DAX Calculations**
Measures
```DAX
Total Spend = SUMX('PO', 'PO'[Quantity] * 'PO'[UnitPrice])
Avg Lead Time = AVERAGE('PO'[LeadTimeDays])
Count of POs = COUNT('PO'[PO ID])
Caluclted columns: LeadTimeDays = DATEDIFF('PO'[PO Date], 'PO'[Delivery Date], DAY)
HighValue = IF('PO'[Quantity] * 'PO'[UnitPrice] > 2000, "Yes", "No")
LateFlag = IF('PO'[LeadTimeDays] > 10, "Yes", "No")
Month = FORMAT('PO'[PO Date], "MMM")
Year = YEAR('PO'[PO Date])
MonthYear = FORMAT('PO'[PO Date], "MMM YYYY")


# Inventory Intelligence Dashboard — Power BI

> **Pharmaceutical inventory management dashboard** — real-time classification of fast, slow, and non-moving products with sales trend analysis, DAX-powered KPIs, and an interactive data model built for distribution operations.

---

## Dashboard Preview

![DAX Data Model](Dax%20Model.png)

> *DAX data model powering the inventory classification and trend logic.*
> Full dashboard walkthrough available in [`Product Details Trend.pdf`](Product%20Details%20Trend.pdf)

---

## The Problem

Pharma distributors typically manage thousands of SKUs across multiple product categories. Without a clear visibility layer, operations teams face:

- **Dead stock accumulating silently** — non-moving products tying up working capital
- **No early-warning system** for slow movers before they hit expiry risk
- **Manual reporting delays** — weekly Excel consolidation taking hours with no drill-down capability

This dashboard replaces all three problems with a live, filterable Power BI report.

---

## What the Dashboard Shows

| Report Page | Purpose |
|---|---|
| **Inventory overview** | Top-level KPIs — total SKUs, stock value, movement distribution |
| **Product classification** | Fast / slow / non-moving breakdown by category and territory |
| **Sales trend analysis** | 30 / 60 / 90-day velocity trends per product line |
| **Product detail drill-down** | SKU-level performance with trend sparklines |
| **Expiry risk view** | Products approaching critical expiry windows |

---

## Key Metrics & Results

- **15% reduction** in stock dumping by surfacing non-moving products before expiry window closes
- **20% reduction** in expiry wastage through proactive batch-level classification
- **30% improvement** in reporting accuracy by consolidating multi-source data into a single Power BI model
- **Eliminated 4+ hours/week** of manual Excel triage — ops team now works from a live dashboard

---

## Tech Stack

```
Power BI Desktop
├── DAX          — custom measures, KPIs, classification logic, time intelligence
├── Power Query  — data ingestion, cleaning, and transformation (M language)
└── Data Model   — star schema with fact and dimension tables
```

---

## DAX Measures Highlights

The data model uses custom DAX measures for product classification and trend logic:

```dax
-- Stock movement classification
Stock Classification =
VAR Sales90Days =
    CALCULATE(SUM(Sales[Quantity]), DATESINPERIOD('Date'[Date], MAX('Date'[Date]), -90, DAY))
RETURN
    SWITCH(
        TRUE(),
        Sales90Days = 0,        "Non-Moving",
        Sales90Days < 10,       "Slow Moving",
        "Fast Moving"
    )

-- 30-day rolling sales trend
Rolling_30D_Sales =
CALCULATE(
    SUM(Sales[Amount]),
    DATESINPERIOD('Date'[Date], LASTDATE('Date'[Date]), -30, DAY)
)

-- Expiry risk flag
Expiry Risk Status =
VAR DaysToExpiry = DATEDIFF(TODAY(), MIN(Inventory[ExpiryDate]), DAY)
RETURN
    SWITCH(
        TRUE(),
        DaysToExpiry <= 60,  "Critical",
        DaysToExpiry <= 90,  "High",
        DaysToExpiry <= 180, "Medium",
        "Low"
    )
```

---

## Data Model Architecture

The report is built on a **star schema** for optimal DAX performance:

```
                    ┌─────────────┐
                    │  dim_Date   │
                    └──────┬──────┘
                           │
┌──────────────┐    ┌──────┴──────┐    ┌──────────────────┐
│  dim_Product │────│  fact_Sales │────│ dim_Territory    │
└──────────────┘    └──────┬──────┘    └──────────────────┘
                           │
                    ┌──────┴──────┐
                    │fact_Inventory│
                    └─────────────┘
```

See `Dax Model.png` in this repo for the full visual relationship diagram.

---

## Files in This Repo

| File | Description |
|---|---|
| `Dax Model.png` | Screenshot of the Power BI data model showing table relationships |
| `Product Details Trend.pdf` | Exported dashboard showing product-level trend analysis |
| `README.md` | This file |

> **Note:** The `.pbix` source file is not included to protect underlying business data. The PDF export and DAX model screenshot demonstrate the full scope of the work.

---

## How to Recreate This Dashboard

1. Open **Power BI Desktop**
2. Connect to your inventory data source (CSV, Excel, or SQL database)
3. Build the star schema data model using the relationships shown in `Dax Model.png`
4. Recreate the DAX measures listed above in your measures table
5. Build report pages following the layout in `Product Details Trend.pdf`

The classification thresholds (fast / slow / non-moving) and expiry buckets (0–60 / 61–90 / 91–180 days) can be adjusted in the DAX `SWITCH` statements to match your business rules.

---

## Related Projects

- [inventory-automation-system](https://github.com/henil1707/inventory-automation-system) — Python backend that powers the same classification logic programmatically
- [SQL-Sale_Distribution-Tableau](https://github.com/henil1707/SQL-Sale_Distribution-Tableau) — SQL + Tableau sales distribution analysis
- [financial-modeling-excel](https://github.com/henil1707/financial-modeling-excel) — P&L, Balance Sheet, and Cash Flow modelling

---

## Author

**Henil Jariwala** — Senior Data Analyst, Pharma Industry

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/henil-jariwala)
[![GitHub](https://img.shields.io/badge/GitHub-henil1707-black?style=flat&logo=github)](https://github.com/henil1707)
[![Tableau Public](https://img.shields.io/badge/Tableau-View_Dashboard-blue?style=flat&logo=tableau)](https://public.tableau.com/app/profile/henil3827/viz/ExploringSaledata/Dashboard1)

---

*Built with Power BI · DAX · Power Query*


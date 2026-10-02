# Blinkit Grocery Sales Dashboard

An interactive Power BI dashboard analysing sales, ratings and outlet performance for Blinkit ("India's Last Minute App") grocery data.

**File:** `blinkit_project.pbix`

---

## Overview

The report is a single 1280 × 720 page that lets you explore grocery sales by item type, fat content, outlet size, outlet location tier, outlet type and outlet establishment year. A set of KPI cards sits at the top, with slicers on the left for quick filtering.

## Key Figures (unfiltered)

| Metric | Value |
|---|---|
| Total sales | ~996,972 |
| Average sales per item | ~141.23 |
| Number of items (rows) | 7,059 |
| Average rating | ~3.92 |

Highlights from the underlying data:

- **Fat content:** Low Fat contributes ~644K vs ~353K for Regular.
- **Top item types:** Fruits and Vegetables (~147K), Snack Foods (~145K), Household (~113K).
- **Outlet location:** Tier 2 leads (~393K), then Tier 3 (~341K) and Tier 1 (~262K).
- **Outlet size:** Medium (~377K) and Small (~371K) lead; High is ~249K.
- **Outlet type:** Supermarket Type1 accounts for ~787K of sales.

## Dashboard Layout

| Area | Visual | What it shows |
|---|---|---|
| Left panel | Slicers | Outlet Location Type, Outlet Size, Item Type |
| Top centre | Multi-KPI card | Total Sales, Avg Sales, No. of Items, Avg Ratings |
| Centre | Metric selector slicer | Choose which KPI the charts emphasise (from the `metrics` table) |
| Centre | Donut chart | Total Sales by **Fat Content** |
| Centre | Clustered bar chart | Total Sales by **Item Type** |
| Centre | Clustered bar chart ("Fat By Outlet") | Sales by Outlet Location Type, split by Fat Content |
| Right | Line chart | Total Sales by **Outlet Establishment Year** |
| Right | Donut chart | Total Sales by **Outlet Size** |
| Right | Funnel chart | Sales by **Outlet Location** |
| Right | Table ("outlet type") | Per Outlet Type: Total Sales, No. of Items, Avg Sales, Avg Ratings, Item Visibility |

Cross-filtering is configured so the Item Type slicer filters the main charts, and the KPI card drives the Outlet Location and Outlet Size slicers.

## Data Model

Two tables, no explicit relationships.

### `BlinkIT-Grocery-Data` (7,059 rows)

| Column | Type | Notes |
|---|---|---|
| Item Identifier | Text | 1,555 unique items |
| Item Type | Text | 16 categories |
| Item Fat Content | Text | Cleaned to `Low Fat` / `Regular` |
| Item Weight | Decimal | |
| Item Visibility2 | Decimal | Share of display area given to the item |
| Outlet Identifier | Text | 8 outlets |
| Outlet Establishment Year | Whole number | |
| Outlet Size | Text | Small / Medium / High |
| Outlet Location Type | Text | Tier 1 / 2 / 3 |
| Outlet Type | Text | Grocery Store, Supermarket Type1, Supermarket Type2 |
| Sales | Decimal | |
| Rating | Whole number | 1–5 |

### `metrics` (calculated table)

A field-parameter table that lets users switch between the four measures below (Total Sales, No. of Items, Avg sales, Avg ratings).

### DAX Measures

```dax
Total Sales  = SUM('BlinkIT-Grocery-Data'[Sales])
Avg sales    = AVERAGE('BlinkIT-Grocery-Data'[Sales])
No. of Items = COUNTROWS('BlinkIT-Grocery-Data')
Avg ratings  = AVERAGE('BlinkIT-Grocery-Data'[Rating])
```

## Data Preparation (Power Query)

Source: an Excel workbook, `BlinkIT-Grocery-Data.xlsx` (sheet `BlinkIT-Grocery-Data`). Steps applied:

1. Promote the first row to headers.
2. Set column data types.
3. Remove blank rows.
4. Standardise **Item Fat Content**: `LF` → `Low Fat`, `low fat` → `Low Fat`, `reg` → `Regular`.
5. Remove rows where Item Fat Content is null.

> **Note:** The source path is hard-coded to a local machine (`C:\Users\ACER\OneDrive\Desktop\BlinkIT-Grocery-Data.xlsx`). See *Getting Started* for how to repoint it.

## Getting Started

**Requirements:** Power BI Desktop (a recent version; the report uses the newer PBIR report format) and the source Excel file.

1. Place `BlinkIT-Grocery-Data.xlsx` somewhere accessible.
2. Open `blinkit_project.pbix` in Power BI Desktop.
3. If you see a data source error, go to **Home → Transform data → Data source settings → Change Source…** and point it to your copy of the Excel file.
4. Click **Refresh** to reload the data.

## Project Structure

```
blinkit_project.pbix
├── DataModel        # tables, measures, Power Query
└── Report           # 1 page, custom theme, KPI background image
```

## Tech Stack

- Power BI Desktop
- DAX
- Power Query (M)
- Excel (data source)

## Possible Improvements

- Add a date or time dimension to analyse trends beyond establishment year.
- Create a separate outlet dimension table and relate it to the fact table.
- Add measures for sales share (% of total) and rating by item type.
- Parameterise the file path so the project works on any machine.


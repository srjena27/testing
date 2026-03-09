# z/OS Mainframe Performance Dashboard

An interactive, single-file HTML dashboard for z/OS Mainframe system performance monitoring. Open `index.html` directly in any modern browser — no server or build step required.

## How to Use

1. Clone or download this repository.
2. Open `index.html` in a browser (Chrome, Firefox, Edge, Safari).
3. Use the **Filter Panel** to narrow data by System, Environment, Application Type, Shift, Alert Level, and Date Range.
4. Click any **column header** in the data table to sort ascending/descending.
5. Type in the **Search box** above the table to filter rows by any text value.
6. Click **Reset Filters** to return to the full dataset.

> The only external dependency is **Chart.js** loaded from the jsdelivr CDN. An internet connection is required for the charts to render; all data is embedded directly in the file.

## Dashboard Sections

### Header
Displays the dashboard title ("z/OS Mainframe Performance Dashboard"), subtitle ("Real-time System Monitoring — March 2026"), and a last-updated timestamp.

### KPI Summary Cards
Six color-coded metric cards at the top:
| Card | Description |
|------|-------------|
| ⚡ Average CPU % | Mean CPU utilization across filtered records (green <50, yellow 50–75, red >75) |
| 📊 Peak CPU MSU | Highest single MSU reading in filtered data |
| 💾 Average Memory % | Mean memory utilization (same color thresholds as CPU) |
| 📋 Total Records | Count of rows matching current filters |
| 🚨 Critical Alerts | Number of Critical-level records (shown in red when >0) |
| 🎯 Avg WLM Goal % | Mean WLM service-class goal attainment (green >95, yellow 90–95, red <90) |

### Filter Panel
- **System Name** — ZOS1, ZOS2, ZOS3, ZOS4, or All
- **Environment** — Production, Test, UAT, or All
- **Application Type** — CICS, DB2, IMS, MQ Series, Batch, or All
- **Shift** — Night, Morning, Afternoon, Evening, or All
- **Alert Level** — Normal, Warning, Critical, or All
- **Date Range** — From / To date pickers (2026-03-01 to 2026-03-06)
- **Reset Filters** button

All charts and the KPI cards update instantly when any filter changes.

### Charts

| # | Chart | Type | Description |
|---|-------|------|-------------|
| 1 | CPU Usage Over Time | Line | CPU% per system over time; dashed threshold lines at 50% (yellow) and 80% (red) |
| 2 | CPU vs Memory | Bubble | Each bubble = one record; X=CPU%, Y=Memory%, bubble size ∝ DASD I/O rate; colored by Environment |
| 3 | Environment Distribution | Doughnut | Record count split by Production / Test / UAT |
| 4 | Alert Level Distribution | Doughnut | Record count split by Normal / Warning / Critical |
| 5 | Avg CPU by Application Type | Horizontal Bar | Mean CPU% per app type (CICS, DB2, IMS, MQ Series, Batch) |
| 6 | Shift-wise Resource Usage | Grouped Bar | Avg CPU%, Memory%, and Channel Busy% by work shift |
| 7 | CPU Heatmap | Canvas (custom) | Day (rows) × Hour (columns) grid; cell color intensity = avg CPU% (green→yellow→red) |
| 8 | MSU by LPAR | Bar | Total CPU MSU per LPAR; horizontal reference line at 400 MSU |

### Data Table
Displays all filtered records with columns: Row, Date, Time, System, LPAR, Environment, Application, App Type, CPU%, MSU, Memory%, DASD I/O, Alert.
- Click any **column header** to sort.
- **Search box** filters rows by any field.
- **Alert Level** and **CPU%** cells are color-coded.
- Row count shown above the table ("Showing X of 100 records").

### Footer
`z/OS Performance Dashboard v1.0 | Data Period: March 1-6, 2026 | Powered by Chart.js`

## Data Dictionary

| Column | Description | Unit/Values |
|--------|-------------|-------------|
| Row | Sequential record number | 1–100 |
| Date | Observation date | YYYY-MM-DD (2026-03-01 to 2026-03-06) |
| Day | Day of week | Sunday–Friday |
| Time | Observation time | HH:MM |
| Hour | Hour of day | 0–23 |
| Shift | Work shift | Night / Morning / Afternoon / Evening |
| System_Name | z/OS system identifier | ZOS1–ZOS4 |
| LPAR_Name | Logical partition name | LPROD1, LPROD2, LTEST1, LUAT1 |
| Sysplex_Name | Sysplex cluster | SYSPLEX1, SYSPLEX2 |
| Environment | Deployment environment | Production / Test / UAT |
| Application_Name | Application instance | e.g. CICS_Banking, DB2_CoreBank |
| Application_Type | Middleware/technology | CICS / DB2 / IMS / MQ Series / Batch |
| CPU_Usage_Pct | CPU utilization | % (0–100) |
| CPU_MSU | CPU millions of service units | MSU |
| Memory_Usage_Pct | Memory utilization | % (0–100) |
| DASD_IO_Rate | Direct-access storage I/O rate | I/Os per second |
| Channel_Busy_Pct | Channel busy percentage | % (0–100) |
| Batch_Jobs_Running | Concurrent batch jobs | count |
| Online_Transactions_Per_Sec | Online transaction rate | TPS |
| WLM_Service_Class | WLM service class | HIGHPROD / MEDPROD / BATCHHI / TESTCLS / UATCLS |
| WLM_Goal_Pct | WLM goal attainment | % (0–100) |
| Alert_Level | Performance alert level | Normal / Warning / Critical |

The raw CSV file is also available at `data/zOS_System_Performance_Data.csv`.

## Files

```
.
├── index.html                          # Self-contained dashboard (open in browser)
├── data/
│   └── zOS_System_Performance_Data.csv # Raw CSV data (100 rows)
└── README.md                           # This file
```

## Technologies

| Technology | Purpose |
|------------|---------|
| HTML5 | Page structure |
| CSS3 (Grid, Flexbox, Custom Properties) | Layout and dark theme |
| JavaScript (ES6+) | Data processing and interactivity |
| [Chart.js](https://www.chartjs.org/) (CDN) | All charts except the heatmap |
| HTML5 Canvas API | Custom CPU heatmap (Chart 7) |
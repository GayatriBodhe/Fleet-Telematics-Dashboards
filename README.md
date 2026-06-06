# Fleet-Telematics-Dashboards

# Logistics Core Performance & Financial Optimization Analytics Platform

[![Power BI](https://img.shields.io/badge/BI-Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Excel](https://img.shields.io/badge/Data-Excel%20%26%20Power%20Query-217346?style=flat&logo=microsoftexcel&logoColor=white)](https://microsoft.com/excel)
[![DAX](https://img.shields.io/badge/Logic-DAX-blue?style=flat)](https://learn.microsoft.com/en-us/dax/)

An enterprise-grade business intelligence solution that transforms raw, multi-tenant fleet telematics logs into high-impact operational insights and financial cost-control metrics. This repository showcases advanced relational data modeling, data quality assurance workflows, and vector-optimized DAX architecture designed to resolve supply chain constraints and eliminate operational waste.

---

## 🎬 Platform Demonstration & Visual Assets

### 📱 Full Walkthrough
<!-- PLACE YOUR SCREEN RECORDING HERE -->
https://github.com/user-attachments/assets/b8a1efd6-99a3-488f-8c8a-7d83704222c4

### 📊 Dashboard Page 1: Operations & SLA Fulfillment
*Focuses on delivery pipeline health, fulfillment exceptions, and transit bottlenecks.*

| Primary Interface Overview | Regional Congestion Mapping |
| :---: | :---: |
|<img width="1304" height="734" alt="image" src="https://github.com/user-attachments/assets/f7155b40-9018-4a5f-b312-62543c5a5569" /> | <img width="459" height="605" alt="image" src="https://github.com/user-attachments/assets/1fbffdc2-73da-4333-9a07-f804c5b69927" />
| **Main Metrics & Alerts Breakdown** <br> Highlights 1K Total Trips, 461 Delays, and the 53.90% Compliance metric. | **Geographical Bottleneck Map Plotter** <br> Dynamic bubble sizing scaled by route delay count attributes. |


### 💰 Dashboard Page 2: Cost & Safety Optimization
*Focuses on behavior-driven fuel waste tracking, risk classification, and driver efficiency rankings[cite: 5].*

| Primary Cost Interface | Fleet Asset Efficiency Profile |
| :---: | :---: |
| <img width="806" height="432" alt="image" src="https://github.com/user-attachments/assets/72825cd2-fda3-49e1-9b07-12cac1cdc534" />|<img width="388" height="309" alt="image" src="https://github.com/user-attachments/assets/a272107e-515e-413f-81e1-9ecc71ebce20" /> |
| **Driver Safety & Efficiency Ledger** <br> Tabular matrix tracking 30,451 total idle minutes with color-coded formatting[cite: 5]. | **Fuel Consumption Profiler** <br> Coordinate scatter plot isolating fuel usage against total distance[cite: 5]. |

### 🚚 Fleet Vehicle Class Drill-Downs
*Custom consumption slopes split dynamically by vehicle capacity constraints[cite: 5].*

| Class 1: Delivery Vans | Class 2: Light Commercial Vehicles (LCVs) | Class 3: Heavy Trucks |
| :---: | :---: | :---: |
| <img src="https://github.com/user-attachments/assets/30cce442-a64e-49e8-b0f6-c65074742e63" width="100%" alt="Delivery Vans Profile"> | <img src="https://github.com/user-attachments/assets/a1504f3d-c9b1-439e-ad82-7c634ffb6746" width="100%" alt="LCVs Cluster Mapping"> | <img src="https://github.com/user-attachments/assets/96e4cf88-40d0-4bed-8ed7-caa81e1e34e7" width="100%" alt="Heavy Trucks Analysis"> |
| Baseline profile tracking high-efficiency urban delivery assets. | Cluster mapping medium-capacity freight distribution routing[cite: 5]. | Asset analysis profiling heavy multi-axle freight hauling. |
---

## 🧠 Data Architecture & Star Schema Design

To guarantee instantaneous report filtering, cross-functional dashboard slicing, and scalable query processing speeds, the telemetry data was refactored into a relational **Star Schema Data Model**[cite: 5]:

*   **Fact Table:** `Fact_Fleet_Telemetry` (Stores 1,000+ logged trip entries, actual runtime durations, exact fuel consumed, and alert mapping indexes)[cite: 5].
*   **Dimension Tables:**
    *   `Dim_Drivers`: Driver profile logs, unique employee keys, and baseline safety behaviors[cite: 5].
    *   `Dim_Routes`: Route identifiers, source-to-destination coordinates, and scheduled target delivery windows (Scheduled TAT)[cite: 5].
    *   `Dim_Vehicles`: Fleet asset parameters split into three categories (Vans, LCVs, Heavy Trucks)[cite: 5].

---
## 📐 Core DAX Implementations (Metrics Engine)

The following formulas showcase the custom business logic and optimized vector-based DAX expressions engineered to build the data model's real-time analytical layer:

### 1. Global Volume & Delivery SLA Tracking
*Monitors fleet fulfillment rates and calculates deviations against contractually agreed turnaround times (Scheduled TAT).*

```dax
Total Trips = COUNTROWS('VTU_Logs')
```
```dax
Delayed Trips Count = 
SUMX(
    'VTU_Logs',
    IF('VTU_Logs'[Actual_Travel_Time_Hrs] > 'VTU_Logs'[Scheduled_TAT_Hrs], 1, 0)
)
```
```dax
SLA Compliance % = 
DIVIDE(
    [Total Trips] - [Delayed Trips Count],
    [Total Trips],
    0
)
```

### 2. Operational Asset Anomaly & Safety Systems
*Translates raw sensor telemetry patterns and geofencing violations into dynamic exception logs and behavioral safety ratings.*

```dax
Critical Alert Rate % = 
DIVIDE(
    CALCULATE(
        [Total Trips], 
        NOT(ISBLANK('VTU_Logs'[Alert_Type])) && 'VTU_Logs'[Alert_Type] <> "None"
    ),
    [Total Trips],
    0
)
```
```dax
Driver Safety Score = 
AVERAGEX(
    'VTU_Logs',
    MAX(
        0,
        100 - ('VTU_Logs'[Over_Speeding_Count] * 5) - IF('VTU_Logs'[Geofence_Breach] = "Yes", 10, 0)
    )
)
```

### 3. Fleet Financial Extractions & Resource Waste Metrics
*Exposes financial bottom-line leakage by calculating fuel wastage costs and operational variance profiles against established corporate baselines.*
```dax
Fleet Fuel Efficiency (KM/L) = 
DIVIDE(
    SUM('VTU_Logs'[Distance_KM]),
    SUM('VTU_Logs'[Fuel_Consumed_Liters]),
    0
)
```
```dax
Fuel Efficiency Variance = 
[Fleet Fuel Efficiency (KM/L)] - 5.5
```
```dax
Fuel Wasted Idling (Liters) = 
SUMX(
    'VTU_Logs',
    'VTU_Logs'[Idle_Time_Mins] * (2.0 / 60)
)
```
```dax
Idling Penalty Cost = 
SUMX(
    'VTU_Logs',
    IF(
        'VTU_Logs'[Idle_Time_Mins] > 15,
        ('VTU_Logs'[Idle_Time_Mins] - 15) * 10,
        0
    )
)
```
```dax
Route Distance Variance % = 
VAR AvgRouteDistance = 
    CALCULATE(
        AVERAGE('VTU_Logs'[Distance_KM]), 
        ALLEXCEPT('VTU_Logs', 'VTU_Logs'[Route_ID])
    )
RETURN
    DIVIDE(
        SUM('VTU_Logs'[Distance_KM]) - ( [Total Trips] * AvgRouteDistance ),
        [Total Trips] * AvgRouteDistance,
        0
    )
```
### 4. Dynamic Context & Filter Reporting
*Enables automated visual context cards that intelligently adjust descriptive titles when slicing data fields across vehicle models.*
```dax
Selected Fleet Context = 
SELECTEDVALUE(
    'VTU_Logs'[Vehicle_Type], 
    "All Commercial Vehicles"
) & " Performance Profile"
```

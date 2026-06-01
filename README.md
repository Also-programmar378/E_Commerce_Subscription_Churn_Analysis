# E-Commerce Subscription Retention & Churn Analysis (Power BI)

![Dashboard UI Mode](https://img.shields.io/badge/UI_Design-Dark_Mode-0F172A?style=for-the-badge)
![Power BI](https://img.shields.io/badge/Power_BI-Desktop_&_Service-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Data_Analysis_Expressions-005A9C?style=for-the-badge)
## Project Overview:
This repository contains an enterprise-grade, 3-page Power BI dashboard designed to address a critical operational crisis: customer subscription churn and revenue leakage. 

Analyzing a transactional dataset representing **$2.05M in total pipeline value**, this interactive tracking application moves beyond static reporting to deliver predictive behavioral insights. It equips executive, product, and fulfillment teams with the exact metrics needed to diagnose why customers cancel memberships and where revenue is being lost.


## Live Dashboard Previews
*Tip: To view the full data interaction model, please download the project file linked below or explore the page walkthroughs.*
### 1. Executive Summary Page
Focuses on macro-level financials, overall health KPIs, and high-level customer breakdown.
<img width="888" height="498" alt="Screenshot 2026-05-25 134901" src="https://github.com/user-attachments/assets/93529c37-cebf-4d96-be83-b68b2ef437e9" />
### 2. Customer Behavioral Risk Analysis
Moves into customer habits, tracking early-warning risk patterns before an account officially churns.
<img width="895" height="499" alt="Screenshot 2026-05-25 135238" src="https://github.com/user-attachments/assets/4dd80504-284f-42a3-ae0d-0072539379c3" />
### 3. Product & Operational Performance
A granular breakdown helping logistics and inventory teams align actual purchases with customer preferences.
<img width="899" height="506" alt="Screenshot 2026-05-25 135320" src="https://github.com/user-attachments/assets/f9ee7a3a-db87-4cea-90a4-4dc3322aa216" />

## 🛠️ Data Architecture & Star Schema
To ensure maximum processing efficiency, quick visual loading times, and clean DAX calculations, the data was re-architected from a flat file into an optimized **Star Schema** model.
**Fact Table:** `Fact_Orders` (Tracks raw transactions, quantities, prices, and line items).
**Dimension Tables:** * `Dim_Customers` (Stores customer profiles, metrics, and `subscription_status`).
* `Dim_Calendar` (Dedicated time-intelligence lookup table).
relationships are mapped via stuctured **Single-Direction Filters (1:Many)**, allowing selections on customer attributes to automatically propagate down to financial transactions without creating performance-heavy bidirectional loops.
## Advanced DAX Measures Developed:
A collection of custom, highly dynamic metrics were developed to bypass hardcoded limitations and handle interactive filtering:

* **Total Revenue Value:** Evaluates overall historical gross pipeline value across all customer segments.
* **Revenue Lost:** Isolates the financial footprint of terminated subscriptions by filtering transaction histories dynamically when a customer hits a "cancelled" state.
Here are the dax codes:
  ```dax
    Revenue Lost = 
    CALCULATE(
        SUM('Fact_Orders'[Purchase_Amount]), 
        'Dim_Customers'[subscription_status] = "cancelled"
    )
    ```
* **Dynamic Subscription Share (%):** Uses advanced filter negation (`ALLSELECTED`) to calculate real-time percentage breakdowns of active, paused, and cancelled customer bases across changing demographic views.
    ```dax
    Subscription % = 
    DIVIDE(
        [Total Customers], 
        CALCULATE([Total Customers], ALLSELECTED('Dim_Customers'[subscription_status]))
    )
    ```
## 💡 Key Business Insights Discovered:


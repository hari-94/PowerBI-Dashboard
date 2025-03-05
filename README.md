# 📊 Power BI Dashboard: Spending vs Savings Analysis Using Snowflake

![Country Vs sales](https://github.com/user-attachments/assets/036c7095-aa93-419b-9138-3a0a65ad3991)


## 🌍 Overview
This Power BI project analyzes **global spending vs savings trends**, using **Snowflake's sample data** with a **mart layer implementation** and **DirectQuery mode** for real-time insights.

🔹 **Business Use Case:**  
Understanding **financial behavior** across different nations by comparing **total spending (O_TOTALPRICE) vs account balance (C_ACCTBAL)**.

---

## 📈 Key Insights from the Dashboard
- **Spending vs Savings Analysis**  
  - Countries like **Kenya (103.62%) and Egypt (102.13%)** save more than they spend.  
  - **Argentina (99.91%) and Vietnam (99.87%)** show higher spending than savings.  
  - Derived **%GT Nation Ratio** to measure financial sustainability.

- **Top 5 Countries by Sales Over Time**  
  - A **Sankey Chart** visualizes country-wise market trends across years.
  - A **Waterfall Chart** highlights **yearly sales growth** from **1992 to 1998**.

---

## 🏛️ Data Modeling & Snowflake Integration
- **Data Source:** Snowflake's sample data (`SNOWFLAKE_SAMPLE_DATA.TPCH_SF1`)
- **Mart Layer Optimization:** Extracted **only required data** to improve efficiency.
- **DirectQuery Mode:** Ensured **real-time data updates** in Power BI.

### 🔗 **Data Model Design (Star Schema)**
| Table Type  | Tables Used |
|-------------|------------|
| **Fact Tables** | `ORDERS`, `LINEITEM` (Sales Transactions) |
| **Dimension Tables** | `CUSTOMER`, `NATION`, `REGION`, `PART`, `SUPPLIER` |
| **Optimized Relationships** | 1:M between `ORDERS` → `LINEITEM`, 1:M between `NATION` → `CUSTOMER` |

✅ **Avoided Many-to-Many (M:M) joins**  
✅ **Implemented efficient filters for better performance**  

---

## ⚡ Key DAX Calculations

```DAX
Nation_Ratio = DIVIDE(SUM(ORDERS[C_ACCTBAL]), SUM(ORDERS[O_TOTALPRICE]), 0)

Year = YEAR(ORDERS[O_ORDERDATE])

Total_Account_Balance = SUM(ORDERS[C_ACCTBAL])

Spending_Savings_Ratio = DIVIDE(SUM(ORDERS[C_ACCTBAL]), SUM(ORDERS[O_TOTALPRICE]), 0) * 100

Orders_Per_Year = COUNT(ORDERS[O_ORDERKEY])
Cumulative_Sales = 
CALCULATE(
    SUM(ORDERS[O_TOTALPRICE]),
    FILTER(
        ALL(ORDERS[O_ORDERDATE]),
        ORDERS[O_ORDERDATE] <= MAX(ORDERS[O_ORDERDATE])
    )
)

Customer_Contribution = 
DIVIDE(
    SUM(ORDERS[O_TOTALPRICE]),
    CALCULATE(SUM(ORDERS[O_TOTALPRICE]), ALL(ORDERS[C_CUSTKEY]))
) * 100


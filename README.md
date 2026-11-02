# Chicken Plus – Q1 2026 Sales Analysis (SQL + Power BI)

Sales analysis for **Chicken Plus**, my own fried-chicken restaurant in Vietnam. It sells in-store (dine-in, takeaway) and through delivery apps (Be, Shopee, Grab, Xanh SM).
The project answers one main question: **where does the restaurant make its money, and what should it change?**

**Tools:** SQL (Oracle) · Power BI (Power Query, DAX, data modelling) · Excel
**Data:** 7,987 orders · 17,689 order lines · 110 menu items · 1 Jan – 31 Mar 2026 (simulated)

## About the data

The dataset is **simulated**, but it is modelled on real sales from my own restaurant, so it keeps the patterns of the real business while protecting real customer and financial data.

What comes from the real restaurant:

* The menu, prices, food costs and product structure
* The sales channels and the 25% delivery-app fee
* The vouchers and how their cost is shared between the shop and the apps
* Real operating patterns: lunch and dinner peaks, the Tết closure (14–23 Feb) and the March app promotion

Order-level records (times, baskets, payment methods) were generated to follow these patterns. The business questions and decisions are real ones I face as an owner.

\---

## Business questions

1. How are sales and earnings trending, and what happened during Tết?
2. Which channels make the most money after app fees, discounts and food costs?
3. Which products sell best, and which are bought together?
4. Did the vouchers and the March app promotion pay off?
5. When do orders come in, and how should staff be planned?

\---

## Key insights

|Area|Insight|
|-|-|
|Overall|1.56B ₫ gross sales, 937M ₫ earnings (60.1% margin), 195K ₫ average order|
|Channels|In-store keeps \~72 ₫ of every 100 ₫ sold; delivery apps keep only \~52 ₫ because of the 25% app fee|
|Products|Coke or fries appear in all 7 top product pairs → a main + Coke/fries combo is an easy bundle|
|Vouchers|Voucher orders are 28K ₫ bigger but earn \~9K ₫ less per order (110K vs 119K ₫)|
|March promotion|App earnings rose **+50% vs January** while in-store stayed the same , so the promotion likely paid off even after discounts|
|Order timing|12:00 and 18:00 alone bring 38.6% of all orders; 14:00–17:00 is almost empty|

> Only one promotion month is available, so the March result is a strong signal, not proof.

\---

## Report pages

### 1\. Overview

Daily sales trend (with the Tết closure marked), monthly performance, sales by channel and earnings from every 100K ₫ sold.
!\[Overview](images/01\_overview.png)

### 2\. Channels \& products

From sales to earnings by channel, top products by revenue, and products ordered together.
!\[Channels and products](images/02\_channels\_products.png)

### 3\. Promotions

Voucher KPIs, a voucher performance table and a before/during comparison of the March app promotion, using in-store as the comparison group.
!\[Promotions](images/03\_promotions.png)

### 4\. Order patterns

Orders by weekday × hour (heatmap), orders by hour of day, daypart split and payment methods.
!\[Order patterns](images/04\_order\_patterns.png)

### Demo

!\[Demo](ChickenPlusAnalysis\_Demo.gif)

\---

## SQL analysis

ChickenPlus\_Analysis.sql answers 10 questions with Oracle SQL, using window functions (`DENSE\\\_RANK`, `LAG`, `FIRST\\\_VALUE`, `SUM() OVER`), self-joins for market-basket analysis and nested subqueries.

|#|Question|Techniques|
|-|-|-|
|Q1–Q2|Revenue by month and channel|`GROUP BY`, `TO\\\_CHAR`|
|Q3|Top 3 food products per channel per month|`DENSE\\\_RANK() OVER (PARTITION BY …)`|
|Q4|Channel share of monthly revenue|`SUM(SUM()) OVER`|
|Q5|Channel with the highest AOV each month|ranking in a subquery|
|Q6|March promotion impact vs Feb and Jan|`LAG`, `FIRST\\\_VALUE`|
|Q7|Top 3 peak hours per channel|`COUNT(\\\*)` + `DENSE\\\_RANK`|
|Q8|Products bought together|self-join on `order\\\_id`|
|Q9|Discounts and platform fees per app|filtered aggregation|
|Q10|Profit margin per channel by month|derived ratio|

```

## How to open

* **Power BI:** download `powerbi/ChickenPlusv02.pbix` and open it in Power BI
* **Quick view:** open `report.pdf`
* **SQL:** run the queries in Oracle


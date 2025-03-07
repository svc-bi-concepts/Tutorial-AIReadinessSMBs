# Data Visualization in Snowflake

## Introduction

Once our data has been **ingested, transformed, and validated**, the next step is **visualizing it** to enable better business insights.  
**Snowflake Dashboards** provide a **built-in, out-of-the-box visualization tool**, allowing users to explore data without third-party BI tools.  

By the end of this section, you will:

- Have a **fully built dashboard in Snowflake**  
- Use **interactive charts** for business analysis  


---

## 1. Creating a New Dashboard in Snowflake

To start visualizing your data:

1. **Go to the Snowflake UI**, navigate to the top left and **+Create > Dashboard**. Give it a name like `Sales Performance`. **Create Dashboard**.

   ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz1.png)

2. Click on **"New Tile** and select **"Create New Dashboard"**.

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz2.png)

    and select **"From SQL Worksheet"**.

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz3.png)

3. **Set the Context** (`ANALYTICS.DM`).

   ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz4.png)

Proceed to the next step to create a tile (chart) for the dashboard.

---

## 2. Creating a Tile (Chart) for the Dashboard

1. Enter your SQL query in the **SQL Worksheet** and rename the sheet at the top in the middle.

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz5.png)

```sql title="KPI Total Revenue"
SELECT
    SUM(ORDER_TOTAL) AS total_revenue
FROM orders;
```
 or if you want to be more explicit:

```sql title="KPI Total Revenue"
SELECT 
    SUM(ORDER_TOTAL) AS TOTAL_REVENUE
FROM ANALYTICS.DM.ORDERS;
```


??? example "Additional Examples"

    Please, adapt `FROM ANALYTICS.DM.<Table>` to your schema.

    **KPIs**
    ```sql title="Total Customers"
    SELECT 
        COUNT(DISTINCT CUSTOMER_ID) AS TOTAL_CUSTOMERS
    FROM ANALYTICS.DM.ORDERS;
    ```

    ```sql title="Total Orders"
    SELECT 
        COUNT(*) AS TOTAL_ORDERS
    FROM ANALYTICS.DM.ORDERS;
    ```

    ```sql title="Average Order Value (AOV)"
    SELECT 
        SUM(ORDER_TOTAL) / NULLIF(COUNT(*), 0) AS AVG_ORDER_VALUE
    FROM ANALYTICS.DM.ORDERS;
    ```

    **Line Charts**
    ```sql title="Revenue Trend (Daily)"
    SELECT 
        DATE_TRUNC('DAY', ORDERED_AT) AS ORDER_DATE, 
        SUM(ORDER_TOTAL) AS DAILY_REVENUE
    FROM ANALYTICS.DM.ORDERS
    GROUP BY ORDER_DATE
    ORDER BY ORDER_DATE;
    ```

    ```sql title="Orders Trend (Daily)"
    SELECT 
        DATE_TRUNC('DAY', ORDERED_AT) AS ORDER_DATE, 
        COUNT(*) AS DAILY_ORDERS
    FROM ANALYTICS.DM.ORDERS
    GROUP BY ORDER_DATE
    ORDER BY ORDER_DATE;
    ```


    **Bar Charts**

    ```sql title="Revenue by Product Category (Food vs. Drink)"
    SELECT 
        CASE 
            WHEN IS_FOOD_ITEM = 1 THEN 'Food' 
            WHEN IS_DRINK_ITEM = 1 THEN 'Drink' 
            ELSE 'Other' 
        END AS CATEGORY,
        SUM(PRODUCT_PRICE) AS TOTAL_REVENUE
    FROM ANALYTICS.DM.ORDER_ITEMS
    GROUP BY CATEGORY
    ORDER BY TOTAL_REVENUE DESC;
    ```

    ```sql title="Most Popular Products (Top 10)"
    SELECT 
        PRODUCT_ID, 
        COUNT(*) AS TOTAL_SALES, 
        SUM(PRODUCT_PRICE) AS TOTAL_REVENUE
    FROM ANALYTICS.DM.ORDER_ITEMS
    GROUP BY PRODUCT_ID
    ORDER BY TOTAL_SALES DESC
    LIMIT 10;
    ```

    ```sql title="Gross Profit by Order (Top 10)"
    SELECT 
        ORDER_ID, 
        SUM(PRODUCT_PRICE - SUPPLY_COST) AS GROSS_PROFIT
    FROM ANALYTICS.DM.ORDER_ITEMS
    GROUP BY ORDER_ID
    ORDER BY GROSS_PROFIT DESC
    LIMIT 10;
    ```

    ```sql title="Profit Margin Products (Top 10)"
    SELECT 
        PRODUCT_ID,
        SUM(PRODUCT_PRICE - SUPPLY_COST) * 100.0 / SUM(PRODUCT_PRICE) AS PROFIT_MARGIN
    FROM ANALYTICS.DM.ORDER_ITEMS
    WHERE :daterange = ORDERED_AT
    GROUP BY PRODUCT_ID
    ORDER BY PROFIT_MARGIN desc
    LIMIT 10;
    ```

2. Click **Run** to preview the results.

    Run the query by pressing on **Win** `CTRL + Enter`, **Mac** `CMD + Enter` or by clicking the **Run** button in the top right.

---

## 3. Choosing a Chart Type

1. **Click on the "Chart" tab** to visualize the data.
2. **Select a Chart type**:
   - **Line Chart** – Show trends over time.
   - **Bar Chart** – Compare total orders by status.
   - **Scatter Plot** – Analyze the relationship between order total and quantity.
   - **Heat Grid** – Visualize order distribution.
   - **Scorecard** – Display a single KPI value.

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz6.png)

3. **Add the Tile to your Dashboard** by clicking on **"Return to <dashboard_name>"**.

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz7.png)

Repeat this process by clicking on `+` in the top left to add more tiles to your dashboard.

---

## 4. Enhancing the Dashboard with Filters

1. **Select a Tile** and **Edit Query**.

    If you select the filter icon in the top left, 

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz8.png)

    you can see which filters are available by default, 

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz9.png)

    along with the option to add your custom filters.



2. **Add a Filter** to the query.

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz10.png)


    ```sql title="Total Revenue" linenums="1" hl_lines="4"
    SELECT
        SUM(ORDER_TOTAL) AS total_revenue
    FROM ANALYTICS.DM.ORDERS
    WHERE :daterange = ordered_at;
    ```

??? example "Additional Examples with filter"

    Add a date field available in the table to the query.

    **KPIs**
    ```sql title="Total Revenue"
    SELECT 
        SUM(ORDER_TOTAL) AS TOTAL_REVENUE
    FROM ANALYTICS.DM.ORDERS
    WHERE :daterange = ORDERED_AT;
    ```
    ```sql title="Total Customers"
    SELECT 
        COUNT(DISTINCT CUSTOMER_ID) AS TOTAL_CUSTOMERS
    FROM ANALYTICS.DM.ORDERS;
    ```
    ```sql title="Total Orders"
    SELECT 
        COUNT(*) AS TOTAL_ORDERS
    FROM ANALYTICS.DM.ORDERS
    WHERE :daterange = ORDERED_AT;
    ```

    ```sql title="Average Order Value (AOV)"
    SELECT 
        SUM(ORDER_TOTAL) / NULLIF(COUNT(*), 0) AS AVG_ORDER_VALUE
    FROM ANALYTICS.DM.ORDERS
    WHERE :daterange = ORDERED_AT;
    ```

    **Line Charts**
    ```sql title="Revenue Trend (Daily)"
    SELECT 
        DATE_TRUNC('DAY', ORDERED_AT) AS ORDER_DATE, 
        SUM(ORDER_TOTAL) AS DAILY_REVENUE
    FROM ANALYTICS.DM.ORDERS
    WHERE :daterange = ORDERED_AT
    GROUP BY ORDER_DATE
    ORDER BY ORDER_DATE;
    ```

    ```sql title="Orders Trend (Daily)"
    SELECT 
        DATE_TRUNC('DAY', ORDERED_AT) AS ORDER_DATE, 
        COUNT(*) AS DAILY_ORDERS
    FROM ANALYTICS.DM.ORDERS
    WHERE :daterange = ORDERED_AT
    GROUP BY ORDER_DATE
    ORDER BY ORDER_DATE;
    ```


    **Bar Charts**

    ```sql title="Revenue by Product Category (Food vs. Drink)"
    SELECT 
        CASE 
            WHEN IS_FOOD_ITEM = 1 THEN 'Food' 
            WHEN IS_DRINK_ITEM = 1 THEN 'Drink' 
            ELSE 'Other' 
        END AS CATEGORY,
        SUM(PRODUCT_PRICE) AS TOTAL_REVENUE
    FROM ANALYTICS.DM.ORDER_ITEMS
    WHERE :daterange = ORDERED_AT
    GROUP BY CATEGORY
    ORDER BY TOTAL_REVENUE DESC;
    ```

    ```sql title="Most Popular Products (Top 10)"
    SELECT 
        PRODUCT_ID, 
        COUNT(*) AS TOTAL_SALES, 
        SUM(PRODUCT_PRICE) AS TOTAL_REVENUE
    FROM ANALYTICS.DM.ORDER_ITEMS
    WHERE :daterange = ORDERED_AT
    GROUP BY PRODUCT_ID
    ORDER BY TOTAL_SALES DESC
    LIMIT 10;
    ```

    ```sql title="Gross Profit by Order (Top 10)"
    SELECT 
        ORDER_ID, 
        SUM(PRODUCT_PRICE - SUPPLY_COST) AS GROSS_PROFIT
    FROM ANALYTICS.DM.ORDER_ITEMS
    WHERE :daterange = ORDERED_AT
    GROUP BY ORDER_ID
    ORDER BY GROSS_PROFIT DESC;
    ```

    ```sql title="Profit Margin Products (Top 10)"
    SELECT 
        PRODUCT_ID,
        SUM(PRODUCT_PRICE - SUPPLY_COST) * 100.0 / SUM(PRODUCT_PRICE) AS PROFIT_MARGIN
    FROM ANALYTICS.DM.ORDER_ITEMS
    WHERE :daterange = ORDERED_AT
    GROUP BY PRODUCT_ID
    ORDER BY PROFIT_MARGIN desc
    LIMIT 10;
    ```


2. **Return to your Dashboard** and repeat that process for other tiles to add filters.

    !!! note "Dashboard Filters"
        Filters can be applied to multiple tiles, allowing users to interactively explore data. You can control the filter at the top left.
        
        ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz11.png)

3. **Customize the layout** by dragging and resizing tiles for better clarity. Congratulations on building your first Snowflake dashboard!

    ![Create Dashboard](./assets/screenshots/SnowViz/SnowViz12.png)

✅ **Your dashboard is now live and interactive!**  
Users can **filter, explore and share insights** directly within Snowflake.



---

## 🎉 This is the end of the Hands-On Lab. 🎉


🔗 **For more References, go to:** [Further Resources](resources.md)



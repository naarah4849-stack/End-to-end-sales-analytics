# End-to-End Sales Data Pipeline & Analytics Dashboard

A complete data project featuring advanced data cleaning using **Power BI/Python (Pandas)** and professional interactive reporting using **Power BI** with complex time-intelligence modeling. 

## 🛠️ Tech Stack & Architecture
1. **Data Engineering (Python/Pandas):** Handled heavy data anomalies, structural whitespace trimming, data type coercion, and standardizing text/boolean records.
2. **Data cleaning/Data Modeling & Visualization (Power BI):** Replaced values, designed schema relationships, built a dedicated calendar dimension, and engineered advanced interactive visual components.

---

## 📊 Dashboard Preview
<img width="1473" height="831" alt="GIF sales finished 2026" src="https://github.com/user-attachments/assets/e4b078c5-dde2-4887-b074-9a77b75d89a5" />

---

## 🧼 Python Data Cleaning Phase

The initial dataset was heavily corrupted with missing data, structural bugs, and inconsistent patterns. The following pipeline was built in Jupyter Notebook to clean the data:

* **Duplicate Control:** Trimmed hidden string spaces and removed specific duplicated order records (`order_id`) while preserving original occurrences.
* **Text Formatting:** Unified text records and column fields using strict title-case patterns (e.g., standardizing text inputs like "east" to "East").
* **Boolean Standardization:** Fixed inconsistent logical responses in the `is_priority` column, mapping values directly to standard patterns.
* **Empty Value Handlers:** Developed regular expressions (`r'^\s*$'`) to find empty rows or space-only cells, converting them to structured `NaN` (null) markers.
* **Numeric Scale Corrections:** Removed invalid currency flags (`%`), adjusted financial decimal scales, erased negative pricing anomalies in `unit_price`, and forced typed boundaries.
* **Pipeline Export:** Automated the data validation steps and exported the final clean state directly to a `.csv` target file.

---

## 🧮 DAX Measures Engineered

A robust set of 20 advanced DAX measures was developed to support deep time-intelligence analysis and key performance indicator (KPI) tracking across five business dimensions:

<details>
<summary><b>📐 Click here to expand and view all 20 DAX Formulations</b></summary>

### 🪙 1. Gross Sales Performance
* **Total Gross Sales**
```sql
Total_Gross_Sales = SUM(synthetic_sales_cleaned[gross_sales])
```
* **Gross Sales YTD**
```sql
gross_salesYTD = TOTALYTD([Total_Gross_Sales], 'dcalendar'[Date])
```
* **Gross Sales Last Year YTD**
```sql
gross_sales LY YTD = 
CALCULATE(
    [gross_salesYTD], 
    SAMEPERIODLASTYEAR('dcalendar'[Date])
)
```
* **Gross Sales Year-over-Year (YoY) %**
```sql
gross_sales YOY = DIVIDE([gross_salesYTD] - [gross_sales LY YTD], [gross_sales LY YTD])
```

### 💸 2. Net Sales Performance
* **Total Net Sales**
```sql
Total_Gross_net_Sales = SUM(synthetic_sales_cleaned[net_sales])
```
* **Net Sales YTD**
```sql
net_salesYTD = TOTALYTD([Total_Gross_net_Sales], 'dcalendar'[Date])
```
* **Net Sales Last Year YTD**
```sql
net_sales LY YTD = 
CALCULATE(
    [Total_Gross_net_Sales], 
    SAMEPERIODLASTYEAR('dcalendar'[Date])
)
```
* **Net Sales Year-over-Year (YoY) %**
```sql
net_sales YOY = DIVIDE([net_salesYTD] - [net_sales LY YTD], [net_sales LY YTD])
```

### 📈 3. Profit Analytics
* **Total Profit**
```sql
total_profit = SUM(synthetic_sales_cleaned[profit])
```
* **Total Profit YTD**
```sql
total_profit_YTD = TOTALYTD([total_profit], 'dcalendar'[Date])
```
* **Total Profit Last Year YTD**
```sql
total_profit_LY YTD = 
CALCULATE(
    [total_profit_YTD], 
    SAMEPERIODLASTYEAR('dcalendar'[Date])
)
```
* **Profit Year-over-Year (YoY) %**
```sql
total_profit YOY = DIVIDE([total_profit_YTD] - [total_profit_LY YTD], [total_profit_LY YTD])
```

### 📦 4. Volume & Quantity Distribution
* **Total Quantity**
```sql
total_quantity = SUM(synthetic_sales_cleaned[quantity])
```
* **Total Quantity YTD**
```sql
total_quantity_YTD = TOTALYTD([total_quantity], dcalendar[Date])
```
* **Total Quantity Last Year YTD**
```sql
total_quantity_LY YTD = 
CALCULATE(
    [total_quantity_YTD], 
    SAMEPERIODLASTYEAR('dcalendar'[Date])
)
```
* **Quantity Year-over-Year (YoY) %**
```sql
total_quantity YOY = DIVIDE([total_quantity_YTD] - [total_quantity_LY YTD], [total_quantity_LY YTD])
```

### 🛒 5. Order Tracking
* **Total Orders**
```sql
total_orders = COUNT(synthetic_sales_cleaned[order_id])
```
* **Total Orders YTD**
```sql
total_orders_YTD = TOTALYTD([total_orders], dcalendar[Date])
```
* **Total Orders Last Year YTD**
```sql
total_orders_LY YTD = 
CALCULATE(
    [total_orders_YTD], 
    SAMEPERIODLASTYEAR('dcalendar'[Date])
)
```
* **Orders Year-over-Year (YoY) %**
```sql
total_orders YOY = DIVIDE([total_orders_YTD] - [total_orders_LY YTD], [total_orders_LY YTD])
```

</details>

---

## 📈 Power BI Insights & Report View
- **Total Sales by Month:** Line chart displaying monthly sales trends to identify seasonality.
- **Top 5 Products by Sales:** Horizontal bar chart highlighting the highest grossing inventory items.
- **Geographic Breakdown:** Interactive matrix analyzing commercial performance across different global markets.


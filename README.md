# 🛒 Superstore Business Insights Dashboard

This project analyzes the **Superstore Sales Dataset** to uncover key business insights using **PostgreSQL** and **Python (Pandas + Matplotlib)**.  
It helps visualize **sales trends, profits, and performance metrics** across categories, regions, and time.

---

## ⚙️ Installation and Setup

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/<your-username>/superstore-analysis.git
cd superstore-analysis
```

### 2️⃣ Create and Activate a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate     # For Mac/Linux
venv\Scripts\activate        # For Windows
```

### 3️⃣ Install Required Libraries

```bash
pip install -r requirements.txt
```

If you're using Jupyter Notebook:

```bash
pip install jupyterlab psycopg2-binary pandas matplotlib python-dotenv
```

---

## 🌍 Environment Variables

Create a `.env` file in your project root with the following variable:

```bash
NEON_DB_URL=postgresql+psycopg2://<username>:<password>@<host>/<database>
```

This connection string points to your **NeonDB PostgreSQL instance**, enabling direct access to your Superstore dataset.

---

## 🧩 Project Structure

```
📦 superstore-analysis/
 ┣ 📜 superstore.ipynb         # Jupyter Notebook for analysis
 ┣ 📜 requirements.txt         # Python dependencies
 ┣ 📜 .env                     # Database credentials (not committed)
 ┣ 📜 README.md                # Project documentation
 ┗ 📊 outputs/                 # Folder for generated visualizations
```

---

## 💡 Business Insights and SQL Analysis

### 1️⃣ Total Sales and Profit

```sql
SELECT SUM(sales)::int AS total_sales, SUM(profit)::int AS total_profit
FROM superstore;
```

**Insight:**  
Gives an overview of the **overall business revenue** and **profit margins**, providing a quick summary of company performance.

🖼️ _Screenshot Placeholder:_  
_Add a summary card or table image here showing total sales and profit._

---

### 2️⃣ Regional Performance Analysis

```sql
SELECT
    region,
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM superstore
GROUP BY region;
```

**Insight:**  
Compares **sales and profit performance** across regions, identifying **high-performing markets** and **areas needing improvement**.

🖼️ _Screenshot Placeholder:_  
_Add a map or grouped bar chart comparing regions._

### 3️⃣ Top 5 Most Selling Products

```sql
SELECT product_id, product_name, SUM(sales) as total_sales
FROM superstore
GROUP BY product_id, product_name
ORDER BY SUM(sales) DESC
LIMIT 5
```

**Insight:**  
Reveals which top 5 **product** generate the most **revenue and profit**, aiding strategic marketing and assortment planning.

🖼️ _Screenshot Placeholder:_  
_Add a pie or bar chart comparing category-wise profits._

---

### 4️⃣ Category-Wise Profitability

```sql
SELECT
    category,
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM superstore
GROUP BY category
ORDER BY total_profit DESC;
```

**Insight:**  
Analyzes **profitability across product categories**, identifying which categories contribute most to **overall revenue and profit**.  
Helps management decide where to **prioritize investment**, **expand product lines**, or **introduce discounts** for underperforming categories.

---

### 5️⃣ Year-on-Year (YoY) Growth Percentage

```sql
WITH base AS (
    SELECT
        EXTRACT(YEAR FROM order_date::date) AS year,
        SUM(profit) AS total_profit
    FROM superstore
    GROUP BY EXTRACT(YEAR FROM order_date::date)
)
SELECT
    year1 AS year,
    CASE
        WHEN sum2 IS NOT NULL THEN (sum1 - sum2) * 100.0 / sum2
        ELSE NULL
    END AS yoy_growth
FROM (
    SELECT
        b1.year AS year1,
        b1.total_profit AS sum1,
        b2.year AS year2,
        b2.total_profit AS sum2
    FROM base b1
    LEFT JOIN base b2 ON b1.year = b2.year + 1
) AS T;
```

**Insight:**  
Calculates **year-on-year profit growth percentages**, revealing how the business has **performed compared to the previous year**.  
This helps evaluate **long-term profitability trends** and measure **sustained business growth** over time.

🖼️ _Screenshot Placeholder:_  
_Add a line or bar chart comparing yearly profits and growth percentage._

---

### 6️⃣ Monthly Profit Trends

```sql
SELECT
    EXTRACT(MONTH FROM order_date::date) AS month,
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM superstore
GROUP BY month
ORDER BY month;
```

**Insight:**  
Identifies **monthly sales and profit variations**, helping businesses recognize **seasonal trends** and plan inventory or promotions accordingly.

---

### 7️⃣ Percentage Break-Up of Sales by Customer Segment

```sql
SELECT
    segment,
    SUM(sales) AS total_sales
FROM superstore
GROUP BY segment
ORDER BY SUM(sales) DESC;
```

**Insight:**  
Shows the **sales contribution percentage by customer segment** (e.g., Consumer, Corporate, Home Office).  
This helps identify which **customer group drives the highest revenue** and where to **focus marketing or retention strategies**.

🖼️ _Screenshot Placeholder:_  
_Add a pie chart showing sales share across customer segments._

---

## 📊 Visualization Samples (Python)

Example: Monthly Profit Trends Visualization

```python
import pandas as pd
import matplotlib.pyplot as plt
import psycopg2
from sqlalchemy import create_engine
from dotenv import load_dotenv
import os

load_dotenv()
db_url = os.getenv("NEON_DB_URL")
engine = create_engine(db_url)

query = """
SELECT
    EXTRACT(MONTH FROM order_date::date) AS month,
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM superstore
GROUP BY month
ORDER BY month;
"""
df = pd.read_sql(query, engine)

plt.figure(figsize=(10,6))
plt.plot(df['month'], df['total_profit'], marker='o')
plt.title("Monthly Profit Trends")
plt.xlabel("Month")
plt.ylabel("Total Profit")
plt.grid(True)
plt.show()
```

🖼️ _Screenshot Placeholder:_  
_Add your generated visualization screenshot here._

---

## 🧠 Key Insights Summary

- **Sales & Profit Overview:** Immediate understanding of overall performance.
- **Monthly Trends:** Detects seasonal high and low periods.
- **Category-Wise Analysis:** Helps in strategic product focus.
- **Sub-Category Analysis:** Optimizes marketing and promotions.
- **Regional Insights:** Supports better geographic decision-making.

---

## 📈 Future Improvements

- Add **interactive dashboards** using Streamlit or Plotly.
- Include **forecasting models** for profit prediction.
- Automate **ETL pipeline** for daily updates.

---

## 🏁 Conclusion

This project provides a clear, data-driven overview of the **Superstore business performance**.  
By integrating **SQL, Python, and visual analytics**, it empowers stakeholders to make **informed, strategic decisions**.

---

🧩 **Author:** Sutirtha Dey
📧 **Contact:** deysutirtha1997@gmail.com
🌐 **GitHub:** [https://github.com/sutirthadey](https://github.com/sutirthadey)

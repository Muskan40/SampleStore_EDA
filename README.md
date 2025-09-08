
# SampleStore_EDA

**Exploratory Data Analysis (EDA) & Interactive Dashboard** for the Sample Superstore dataset.  
This project performs end-to-end EDA, data cleaning, visualization and interactive reporting (Streamlit) to surface business insights for sales, profit, and product performance.

**Live demo:** https://samplestoreeda-fsqqnvuycl76jehxcryaqo.streamlit.app/

---

## Table of Contents
- [Project Overview](#project-overview)   
- [Dataset](#dataset)  
- [Installation](#installation)  
- [Run the app / Reproduce analysis](#run-the-app--reproduce-analysis)  
- [Detailed EDA steps performed](#detailed-eda-steps-performed)  
- [Visualizations & Dashboard features](#visualizations--dashboard-features)  
- [Key findings & business insights](#key-findings--business-insights)  
- [Files description](#files-description)  
- [Dependencies](#dependencies)  


---

## Project Overview

This repository provides a self-contained EDA project built around the Sample Superstore dataset. The goals are to:
- Clean and prepare sales data for analysis.
- Produce descriptive and diagnostic analytics (sales, profit, discount, quantity, product performance).
- Provide an interactive Streamlit dashboard to explore the data and surface actionable business insights (top products, underperforming segments, region-level analysis, shipping performance).
- Produce deliverables suitable for business stakeholders (charts, summary tables, and recommended actions).

---

## Dataset
The project uses the canonical **Sample Superstore** dataset (`Sample - Superstore.csv`) — a transactional dataset with order lines, customer & product attributes, shipping information, and monetary values (Sales, Profit, Discount, Quantity). 

> Tip: typical useful columns in this dataset are: `Order ID`, `Order Date`, `Ship Date`, `Customer ID`, `Customer Name`, `Segment`, `Country`, `City`, `State`, `Postal Code`, `Region`, `Product ID`, `Category`, `Sub-Category`, `Product Name`, `Sales`, `Quantity`, `Discount`, `Profit`.

---

## Installation

1. Clone the repository:
```bash
git clone https://github.com/Muskan40/SampleStore_EDA.git
cd SampleStore_EDA
````

2. Create and activate a virtual environment (recommended):

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

> If you run into version conflicts, consider pinning versions in `requirements.txt` and re-installing.

---

## Run the app / Reproduce analysis

**To launch the Streamlit dashboard (if `main.py` is a Streamlit app):**

```bash
streamlit run main.py
```

Open `http://localhost:8501` in your browser.

**To reproduce from a Jupyter notebook** (if provided), open the notebook and run cells in order:

* Data ingestion → cleaning → feature engineering → visualization → export.

---

## Detailed EDA steps performed

Below are the **step-by-step EDA & data-prep tasks** you should see in the project. (If your `main.py` differs, adapt as needed.)

1. **Data ingestion**

   * Load CSV: `df = pd.read_csv("Sample - Superstore.csv", parse_dates=['Order Date','Ship Date'])`
   * Quick check: `df.shape`, `df.head()`, `df.info()`

2. **Data cleaning**

   * Check and handle duplicates: `df.duplicated().sum()` → drop duplicates if any.
   * Missing values: `df.isna().sum()` → decide: drop / impute / flag.
   * Data types: ensure numeric columns (Sales, Profit, Discount, Quantity) are numeric; dates parsed.

3. **Feature engineering**

   * Create `Order_Month`, `Order_Year`, `Order_MonthYear` for time-series aggregation.
   * Create `Profit_Ratio = Profit / Sales` (handle zero sales).
   * Create `Shipping_Delay = (Ship Date - Order Date).days`.
   * Create `Revenue_Category` or `Profitability_Flag` (profit / loss buckets).

4. **Exploratory analysis**

   * Univariate: distributions and summary stats for numeric variables (Sales, Profit, Discount).

     ```python
     df[['Sales','Profit','Discount','Quantity']].describe()
     ```
   * Categorical counts: top `Category`, top `Sub-Category`, top `Region` by orders.
   * Grouped aggregations:

     ```python
     sales_by_region = df.groupby('Region')['Sales'].sum().sort_values(ascending=False)
     profit_by_subcat = df.groupby('Sub-Category')['Profit'].sum().sort_values()
     ```
   * Trend analysis: monthly sales and profit trend (time series) and YoY growth.

5. **Bivariate & diagnostic analysis**

   * Correlation (Sales vs Profit, Discount vs Profit).
   * Scatter plots (Sales vs Profit) with `Quantity` or `Discount` encoded by color/size.
   * Pivot tables: `pd.pivot_table(df, values='Sales', index='Region', columns='Category', aggfunc='sum')`

6. **Outliers & anomalies**

   * Identify extreme profit/loss transactions and examine product IDs / customers.
   * Seasonal spikes/dips detection.

7. **Summary tables**

   * Rank top 10 products by sales and by profit.
   * Compute customer-level metrics: total spend, average order value, churn candidates (if applicable).

---

## Visualizations & Dashboard features

The interactive dashboard (Streamlit) is expected to include:

* KPI cards: Total Sales, Total Profit, Total Orders, Average Discount, Average Delivery Time.
* Time series charts: Monthly Sales & Profit trend (line charts).
* Bar charts: Sales by Region, Profit by Category / Sub-Category.
* Heatmap / Pivot table: Region vs Category sales.
* Scatter / bubble chart: Discount vs Profit to detect discounting impact.
* Treemap: Product category revenue breakdown.
* Top-N leaderboards: top customers, products, cities.
* Filter controls: date range, region, category, top-N selector.

Example Plotly or Seaborn snippets:

```python
import plotly.express as px
fig = px.line(monthly_df, x='Order_MonthYear', y='Sales', title='Monthly Sales Trend')
st.plotly_chart(fig)
```

---

## Key findings & business insights (PLACEHOLDERS — replace with your results)

> Replace the items below with numbers from your analysis to make the README specific and business-ready.

* **Total Sales:** `<<REPLACE WITH TOTAL SALES>>`
* **Total Profit:** `<<REPLACE WITH TOTAL PROFIT>>`
* **Most profitable category:** `<<REPLACE: e.g., Technology / Furniture / Office Supplies>>`
* **Top-selling product(s):** `<<REPLACE WITH PRODUCT NAMES>>`
* **Least profitable segments (negative profit):** `<<REPLACE WITH SUB-CATEGORY/REGION>>`
* **Impact of discounts on profit:** Observed that high discounts correlate with `<<REPLACE with observed relation e.g., lower profit margins>>`.
* **Regional insights:** `<<E.g., West region generates X% of sales but Y% of profit>>`.
* **Operational insight:** Shipping method `<<XYZ>>` has higher delivery delays and lower profit margin → recommend reviewing shipping agreements.

---

## Files description

* `main.py` — Main app script (Streamlit) that loads the CSV, runs EDA, and builds interactive visuals. ([GitHub][2])
* `Sample - Superstore.csv` — Raw dataset used for all analysis. ([GitHub][3])
* `requirements.txt` — Python packages required to run the app/analysis. ([GitHub][4])

---

## Dependencies

Install from `requirements.txt`:

```bash
pip install -r requirements.txt
```

Typical packages used:

* pandas, numpy, matplotlib, seaborn, plotly, streamlit, scikit-learn , openpyxl

---

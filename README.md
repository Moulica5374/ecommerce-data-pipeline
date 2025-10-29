---

## Tech Stack

- **Data Processing:** Apache Spark (PySpark), Python, Pandas
- **Cloud Storage:** AWS S3 (Bronze/Silver/Gold layers)
- **Data Format:** Parquet (columnar, compressed)
- **Query Engine:** AWS Athena
- **Visualization:** Plotly (interactive charts)
- **Development:** Google Colab, Jupyter Notebooks
- **Version Control:** Git, GitHub

---

## Key Business Insights

### Revenue Analysis
- **Total Revenue:** $9.75M across 12 months
- **Top Market:** United Kingdom (92% of total revenue - $9.03M)
- **Growth Rate:** 83% increase from Dec 2010 ($824K) to Nov 2011 ($1.51M)

### Product Performance
- **Best Seller:** DOTCOM POSTAGE ($206K revenue)
- **Top 10 Products:** Contribute $1.2M in revenue
- **Popular Categories:** Home decor, gift items, party supplies

### Customer Segments
- **Registered Customers:** 406,789 transactions (75.4%)
- **Guest Purchases:** 132,603 transactions (24.6%, $1.73M revenue)
- **Returns:** 9,288 transactions properly tracked

---

## Visualizations

### Sales by Country
Top performing countries by revenue:

![Sales by Country](images/sales_by_country.png)

### Top Products
Best-selling products analysis:

![Top Products](images/top_products.png)

### Monthly Revenue Trend
Revenue growth over time showing seasonal patterns:

![Monthly Revenue](images/monthly_revenue.png)

---

## Data Pipeline Process

### Bronze Layer (Raw Data)
- Source: Kaggle E-commerce dataset
- Format: CSV (541,909 rows, 8 columns)
- Storage: s3://bucket/raw/data.csv

### Silver Layer (Data Cleaning)

**Data Quality Issues Identified:**
- 135,080 NULL CustomerIDs (24.9%)
- 10,624 negative quantities
- 2,517 bad prices (≤ $0)
- 1,454 NULL descriptions

**Transformations Applied:**
- Removed 2,517 bad records (0.46%)
- Created customer_type: "Guest" vs "Registered"
- Added is_return flag for returns tracking
- Filled NULL descriptions with "Unknown Product"
- Created TotalPrice: Quantity × UnitPrice
- Parsed dates: Extracted Year, Month, Day, Hour

**Result:** 539,392 clean records in Parquet format

### Gold Layer (Business Aggregations)

Created analytical tables:
- **Sales by Country:** Revenue, order count, unique customers per country
- **Top Products:** Best sellers by revenue and quantity
- **Monthly Revenue:** Time-series analysis of sales trends

---

## Data Engineering Decisions

### Why Keep Guest Purchases?
Initially considered dropping NULL CustomerIDs, but analysis revealed:
- 132K transactions = 24.6% of data
- $1.73M revenue = 17.8% of total revenue
- **Decision:** Segment as "Guest" customers rather than delete

### Why Remove Internal Adjustments?
Identified 1,336 records with:
- Negative quantities without "C" prefix (not customer returns)
- Descriptions like "damages", "faulty", "check"
- No customer information
- **Decision:** These are operational adjustments, not sales transactions

### Why Use Parquet Format?
- 70-80% compression vs CSV
- Columnar storage for fast queries
- Native support in Spark, Athena, and modern data tools

---

## How to Run

### Prerequisites
- Python 3.8+
- Apache Spark 3.5+
- AWS CLI configured
- AWS S3 bucket access

### Setup
```bash

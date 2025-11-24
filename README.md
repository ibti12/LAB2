 LAB 2 — E-Commerce Dataset Exploration (DataFrames)

![Python](https://img.shields.io/badge/python-3.10-blue)
![Apache Spark](https://img.shields.io/badge/spark-3.4-orange)
![Docker](https://img.shields.io/badge/docker-yes-brightgreen)

## Overview

This project focuses on **data exploration and basic analytics** using Spark DataFrames in a simulated e-commerce environment.  

**Goals:**

- Generate synthetic CSV datasets for customers, products, and orders.
- Load data into Spark and perform data quality checks.
- Compute descriptive statistics and answer business questions.
- Export a summary report.

---

## Project Structure

```

data-engineering-course/
├── spark-data/
│   └── ecommerce/
│       ├── customers.csv
│       ├── products.csv
│       ├── orders.csv
│       └── summary/       # Summary report output
├── generate_data.py       # CSV generation script
├── lab2_explore_data.py   # Spark exploration script
└── lab2_questions.py      # Business questions (optional)

````

---

## 1. Dataset Generation

- **Script:** `generate_data.py`
- **Purpose:** Generates synthetic e-commerce data.
- **Datasets:**
  - `customers.csv` — 1000 customers with fields like `customerNumber, customerName, contactFirstName, contactLastName, phone, addressLine1, city, state, country, creditLimit, customerSegment`
  - `products.csv` — 100 products with fields like `productCode, productName, productCategory, quantityInStock, buyPrice, MSRP`
  - `orders.csv` — 5000 orders with fields like `orderNumber, orderDate, requiredDate, status, customerNumber, totalAmount, paymentMethod`

**Run the generator:**

```bash
python generate_data.py
````

Verify the CSV files are created under `spark-data/ecommerce/`.

---

## 2. Data Exploration with Spark

* **Script:** `lab2_explore_data.py`
* **SparkSession configuration:**

```python
from pyspark.sql import SparkSession

spark = (
    SparkSession.builder
    .appName("Day1-DataExploration")
    .master("spark://localhost:7077")
    .config("spark.driver.memory", "2g")
    .getOrCreate()
)
```

**Tasks:**

1. Load CSV files with `header=True`, `inferSchema=True`.
2. Inspect schemas with `.printSchema()`.
3. Compute basic stats: row counts, `.show(5)` preview.
4. Data quality checks: nulls and duplicate IDs.
5. Exploratory analysis:

   * Customers by segment
   * Top 10 countries
   * Orders by status & payment method
   * Products by category
6. Numerical analysis: order amounts, credit limits, product prices.
7. Export summary report (`summary.csv`) with:

   * Total Customers
   * Total Products
   * Total Orders
   * Total Revenue
   * Average Order Value

---

## 3. Business Questions

1. Country with highest total credit limit
2. Most common order status
3. Product category with most stock
4. Percentage of Enterprise customers
5. Distribution of orders by month

*All answers use Spark DataFrame operations.*

---

## 4. Data Quality Notes

* Nulls: `state` missing in some customers.
* Duplicates: 0 in customers and orders.
* Skewed distributions: majority of orders use credit card, SMB segment dominates.
* Surprising patterns: Electronics most stocked category; USA highest credit total.

---

## 5. How to Run

```bash
# Generate CSV datasets
python generate_data.py

# Explore datasets with Spark
python lab2_explore_data.py

# Run business questions (optional)
python lab2_questions.py
```

* Outputs appear in console.
* Summary CSV in `spark-data/ecommerce/summary/`.

---

## 6. Environment Notes

* Ensure Docker Spark containers can access CSV files via **bind mounts** or **symlinks**.
* Use `header=True` and `inferSchema=True` when loading CSVs.
* Use `.coalesce(1)` to write single CSV file.

---

## 7. Deliverables

* CSV files: `customers.csv`, `products.csv`, `orders.csv`
* Console output of `lab2_explore_data.py`
* Scripts answering business questions
* Short data quality notes

---

## 8. References

* [Apache Spark — CSV Data Source](https://spark.apache.org/docs/latest/sql-data-sources-csv.html)
* Docker + Spark tutorials for volume mounting

```

---


# Retail Data Engineering & Analytics Project

## Overview

This project demonstrates how to design, build and analyse a **big-data pipeline** for e-commerce using **Amazon Web Services (AWS)** and **PySpark**.  
Using the *Online Retail* dataset from the **UCI Machine Learning Repository**, I ingested transaction data from **S3**, processed it with **PySpark on an EC2 instance**, generated aggregated metrics and visualisations, and persisted both the raw and processed data back to **S3**.  
I also explored **machine-learning modelling with Amazon SageMaker** and created **interactive dashboards in Power BI**.

The project was completed as part of a **Big Data Applications** course and showcases my skills in **data engineering, cloud infrastructure, SQL, and analytics**.  
I have intentionally documented the entire workflow – from infrastructure setup through to data ingestion, processing, storage and reporting – to provide potential employers with a clear view of how I tackle real-world data problems.

**Figure 1 – Architectural overview of the data pipeline:**  
Raw data is ingested from Amazon S3 into a PySpark environment running on EC2, processed and aggregated, then written back to S3. Amazon SageMaker consumes the processed data to build ML models, while Power BI connects via Athena or data exports for dashboards and reports.

---

## Project Objectives

- Build a scalable data pipeline using **AWS services (S3, EC2, SageMaker)** and **PySpark**.  
- Perform **data cleaning, transformation, and aggregation** on a transactional dataset.  
- Compute metrics such as **total revenue by country**, **monthly spending trends**, **customer transaction values**, and **average basket size**.  
- Persist processed data back to **S3** in **partitioned CSV** files.  
- Analyse the data using **Spark SQL** for exploratory queries.  
- Develop a **binary classification model** in **SageMaker** to predict high-value purchases.  
- Create **interactive Power BI dashboards** to illustrate geographic and temporal sales trends.

---

## Dataset

The analysis is based on the **Online Retail dataset** from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/online+retail).  
It contains **541,909 purchase records** from *1 December 2010 – 9 December 2011* for a UK-based online gift retailer.

**Fields:**
- **Transaction identifiers:** `InvoiceNo`, `InvoiceDate`, `Quantity`, `UnitPrice`  
- **Product details:** `StockCode`, `Description`  
- **Customer info:** `CustomerID`, `Country`

Because the data is multivariate, sequential, and time-series in nature, it supports aggregation, customer segmentation, and forecasting.

---

## Implementation

### 1. Infrastructure & Environment Setup
- **AWS S3 storage:** Separate buckets for raw and processed data with structured hierarchy; access via AWS CLI.  
- **EC2 compute:** Configured Linux instance, installed PySpark, validated S3 connectivity.  
- **Tools:** Jupyter Lab + VS Code (SSH) + AWS CLI for interactive development and data transfer.

### 2. Data Ingestion & Initial Processing
- Downloaded the raw `Online_Retail.xlsx` file from S3.  
- Converted Excel → CSV with **Pandas**, ingested into **Spark DataFrame**.  
- Validated schema, confirmed **541,909 records**.  
- Dropped nulls in `CustomerID` and `Description`.

### 3. Transformations & Calculations
- **Date/time:** Converted `InvoiceDate` → timestamp; derived `InvoiceYearMonth`.  
- **Revenue:** Added `TotalPrice = Quantity × UnitPrice`.  
- **Aggregations:** Grouped revenue, quantity, and average transaction value by **customer**, **country**, and **month**.

### 4. Feature Engineering & ML Modelling
- Derived `HighValue_Purchase` indicator for large transactions.  
- Calculated segmentation metrics (avg. transaction value per customer).  
- Built and trained **binary classifier** in **SageMaker** achieving **99.994 % balanced accuracy** with **0.101 s inference latency**; `Quantity` and `UnitPrice` identified as top predictors.

### 5. Data Storage & Output
- Wrote cleaned/enriched data to S3 in partitioned **CSV** directories:  
  - `processed/`, `totalRevenueByCountry/`, `totalQuantityCountry/`, `customerTransactionValue/`,  
    `averageTransactionPerCustomer/`, `monthly_spending_trends/`
- Each directory includes a `_SUCCESS` marker and one or more part files.

### 6. Analytics & Reporting
- Registered DataFrame as temporary SQL view and ran queries such as:
  - Total revenue by country  
  - Monthly spending trends  
  - Top customers by revenue  
- Exported processed data to **Power BI** for dashboards with:
  - Geographic revenue distribution  
  - Product quantity breakdown  
  - Interactive time sliders & filters  

---

## Folder Structure

├── Architecture.svg                 # High-level pipeline diagram  
├── Report - With Screenshot.pdf     # Detailed report with screenshots and visualisations  
├── Retail_visualization.pdf         # Sample Power BI dashboard  
├── Video Presentation.mp4           # Recorded project presentation  
└── Data_and_Code/  
  ├── rawdata/                     # Source data downloaded from S3  
  │   └── Online_Retail.csv  
  ├── processed/                   # Cleaned dataset written by PySpark  
  ├── totalRevenueByCountry/       # Revenue aggregated by country  
  ├── totalQuantityCountry/        # Quantity aggregated by country  
  ├── customerTransactionValue/    # Total revenue per customer  
  ├── averageTransactionPerCustomer/ # Mean revenue per customer  
  ├── monthly_spending_trends/     # Revenue grouped by InvoiceYearMonth  
  └── Project.ipynb                # Jupyter notebook containing the entire pipeline

## Results & Insights

- Successfully processed **541 909 transactions** and saved structured outputs to **Amazon S3**.  
- **United Kingdom** dominated sales, with clear **seasonal peaks around Christmas 2010**.  
- **Quantity** and **UnitPrice** emerged as the strongest predictors for **high-value purchases**.  
- **Power BI dashboards** provided rich, interactive insights into **regional** and **temporal sales trends**.

---

## Future Work

- **Real-time streaming:** Integrate **AWS Kinesis** for continuous data ingestion and processing.  
- **API integration:** Expose aggregated metrics through **REST API endpoints** for system interoperability.  
- **Forecasting:** Implement **time-series models** to predict future revenue and demand trends.  
- **Advanced ML:** Extend the pipeline to include **customer segmentation** and **recommendation systems**.

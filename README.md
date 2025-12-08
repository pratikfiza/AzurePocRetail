# AzurePocRetail
Azure Retail Analytics – End-to-End Data Engineering Pipeline

This project demonstrates a complete Azure Data Engineering solution for ingesting, transforming, and analyzing retail datasets using Azure Data Factory (ADF), Azure Databricks, Azure Storage (ADLS Gen2), and a Lakehouse medallion architecture (Bronze → Silver → Gold).

🚀 Architecture Overview

The solution is built using a modular pipeline approach:

1. Data Sources

Data is ingested from 4 sources:

Azure SQL Database

Customer

Transaction

Store

HTTP API

Product dataset

🧊 Bronze Layer (Raw Storage)

All ingested datasets are stored in the Bronze container inside ADLS Gen2:

/bronze/customer
/bronze/transaction
/bronze/store
/bronze/product


Data is stored as-is for traceability and audit purposes.

⚙️ Extraction Pipeline (ADF)

Built in Azure Data Factory

Pulls data from Azure SQL DB & HTTP API

Uses Copy Data Activities to land them into the Bronze layer

SAS Token is used to mount the ADLS container into Databricks

🔄 TransformationLoad Pipeline (ADF)

This pipeline is responsible for executing Databricks transformations.

Key Features

Uses ADF → Databricks Notebook activity

Authentication via Databricks Access Token

Executes the transformation notebook which:

Reads Bronze data

Performs cleaning & standardization

Generates Silver and Gold layers

💎 Silver Layer (Cleaned & Refined)

In Databricks, notebooks perform basic transformations:

Implemented Logic

Null removal

Dedupe operations

Column standardization

Data type corrections

Basic filtering

Output

Cleaned datasets stored in:

/silver/customer
/silver/transaction
/silver/store
/silver/product

🥇 Gold Layer (Business Ready)

The Silver datasets are joined to create a wide unified retail dataset.

Logic Implemented

Customer + Store + Product + Transaction join

Derived columns for KPI calculations

Aggregated business tables

KPIs created

Monthly Sales Amount

Total Orders per Month

Top Performing Stores

Best-Selling Products

Revenue per Customer Segment

Gold output paths:

/gold/sales_monthly
/gold/store_performance
/gold/product_performance
/gold/customer_revenue

🔀 ExecutePipeline (ADF Orchestration)

A master pipeline executes both:

ExtractionPipeline

TransformationLoad

This ensures end-to-end automation with dependencies maintained.

📒 Databricks Notebook Highlights
Bronze → Silver

Load raw data

Basic cleaning, filtering, type casting

Remove nulls & duplicates

Apply schema enforcement

Silver → Gold

Join all datasets into one retail fact table

Aggregate for business KPIs

Write to Gold container as Delta tables

🏗️ Tech Stack
Service	Purpose
Azure Data Factory	Orchestration & data ingestion
Azure Databricks	Data transformation using PySpark
ADLS Gen2	Storage for raw/cleaned/business data
Azure SQL Database	Source system
HTTP API	External product dataset
Delta Lake	For ACID-compliant Lakehouse storage
📁 Project Structure (Repo)
AzurePocRetail/
│
├── notebooks/
│   ├── bronze_to_silver.py
│   ├── silver_to_gold.py
│
├── adf/
│   ├── ExtractionPipeline.json
│   ├── TransformationLoad.json
│   ├── ExecutePipeline.json
│
├── README.md

🧪 How to Run
Prerequisites

Azure subscription

Databricks workspace

ADF instance

ADLS Gen2 storage account

SAS token (for mounting)

Steps

Run ExtractionPipeline
→ loads raw data to Bronze.

Run TransformationLoad
→ Databricks handles Silver & Gold.

Run ExecutePipeline
→ orchestrates both pipelines.

📊 Output

After execution, you will have:

Clean retail datasets (Silver)

Analytical KPI tables (Gold)

End-to-end automated pipeline

Delta tables ready for Power BI using direct lake import

🤝 Contributing

Pull requests are welcome.
Please open an issue for significant changes before submitting PRs.

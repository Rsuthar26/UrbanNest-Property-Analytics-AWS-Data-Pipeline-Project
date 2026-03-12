# 🏠 UrbanNest Property Analytics — AWS Data Pipeline

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Pipeline-Complete-brightgreen)

---

## Scenario Project

UrbanNest Property Analytics Ltd is a London-based property startup looking to understand housing price trends across the city. The analytics team needed a fast, scalable way to query a raw housing dataset using SQL — without the overhead of managing traditional database infrastructure.
As the Data Engineer on the team, I designed and built a fully serverless pipeline on AWS — taking the data from a raw CSV file all the way through to a clean, queryable table that analysts can query with SQL instantly.

---

## Dataset

**File:** `house_prices.csv` — 506 rows

<table>
  <tr>
    <th>Column</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>rooms</code></td>
    <td>Average rooms per property</td>
  </tr>
  <tr>
    <td><code>distance</code></td>
    <td>Distance from employment hubs</td>
  </tr>
  <tr>
    <td><code>value</code></td>
    <td>Median property value</td>
  </tr>
</table>

---

## Pipeline

```
house_prices.csv
       │
       ▼
Amazon S3 (raw)          → raw data
       │
       ▼
AWS Glue Crawler         → Detects schema automatically
       │
       ▼
Glue Data Catalog        → Registers table: urban_nest_db.raw_raw_data
       │
       ▼
AWS Glue ETL Job         → Removes nulls, converts CSV → Parquet
       │
       ▼
Amazon S3 (processed)    → Processed data
       │
       ▼
Amazon Athena            → SQL queries on clean data
```

---

## AWS Services Used

| Service | What it does |
|---------|-------------|
| Amazon S3 | Stores raw and processed data |
| AWS Glue Crawler | Auto-detects schema from the CSV |
| AWS Glue Data Catalog | Registers the table for querying |
| AWS Glue ETL | Cleans data and converts to Parquet |
| Amazon Athena | Runs SQL queries directly on S3 |

---


## Tasks

### Task 1 — Upload Dataset to AWS S3
The first step was setting up cloud storage for the raw dataset. I created an S3 bucket and uploaded `house_prices.csv` into a structured folder — this becomes the entry point of the pipeline.

- Created a bucket: `urban-nest-data-lake-rs`
- Uploaded the dataset: `house_prices.csv`
- Structured the data lake: `s3://urban-nest-data-lake-rs/raw-data/house_prices.csv`


---

### Task 2 — Create AWS Glue Crawler
Instead of manually defining the dataset structure, I set up a Glue Crawler to scan the CSV and detect the schema automatically — saving time and removing the risk of human error.

- Navigated to: AWS Glue → Crawlers
- Created crawler named: `urban-nest-house-crawler`
- Set data source: S3 → `urban-nest-data-lake-rs/raw-data/`
- Set output database: `urban_nest_db`


---

### Task 3 — Run the Crawler
I ran the crawler against the S3 bucket. It scanned the CSV, detected the three columns and their data types, and registered the table in the Glue Data Catalog — making it ready to query.

- Detected the dataset schema automatically
- Created table `raw_raw_data` in the AWS Glue Data Catalog
- Made the dataset queryable via Athena

---

### Task 4 — Build ETL Job in AWS Glue Studio
Using Glue Studio's visual editor, I built a three-node ETL pipeline that reads the raw data, drops any null rows, and writes the cleaned output to S3 in Parquet format — optimised for fast and cost-efficient querying.

- Read data from the Glue Data Catalog
- Removed null values
- Saved cleaned data to: `s3://urban-nest-data-lake-rs/processed/`
- Output format: Parquet

---

### Task 5 — Query Using Amazon Athena
With the pipeline complete, I used Athena to run SQL queries directly on the processed data in S3 — no database server needed. This is what the analytics team uses to generate insights from the housing data.

- Connected Athena to `urban_nest_db`
- Ran SQL queries directly on the processed data in S3

---


## Key Learning Outcomes

- Built a structured S3 data lake with raw and processed layers
- Used AWS Glue Crawler to auto-detect dataset schema without writing code
- Registered dataset metadata in the Glue Data Catalog for querying
- Built a serverless ETL pipeline using Glue Visual ETL
- Converted CSV data into Parquet format for faster, cheaper querying
- Diagnosed and resolved an IAM permission error on the Glue service role
- Queried cloud data using Amazon Athena SQL with no infrastructure setup
- Documented the full pipeline architecture end to end

---

## Author

**Name:** Ripal Suthar

# Automated Order Report Generation using AWS S3 and Lambda

## Project Overview

This project automates the generation of a **city-wise revenue summary report** whenever a CSV file containing order data is uploaded to an Amazon S3 bucket.

An AWS Lambda function is triggered automatically, processes the file using **pandas**, calculates revenue per city, and stores the summary report back in S3.

---

## Architecture

S3 (incoming) → Lambda → S3 (reports)

1. Operations team uploads `orders.csv` to S3
2. S3 event triggers Lambda function
3. Lambda processes the file
4. Revenue per city is calculated
5. Summary report is stored in S3

---

## Technologies Used

- AWS S3
- AWS Lambda
- AWS Lambda Layer (Pandas)
- AWS CloudWatch
- Python
- Pandas

---

## S3 Bucket Structure

```
order-report-bucket
│
├── incoming/
│   └── orders.csv
│
└── reports/
    └── city_revenue_summary_<date>.csv
```

---

## Lambda Function Workflow

1. Detect CSV upload in `incoming/`
2. Read CSV from S3
3. Calculate revenue (`quantity × price`)
4. Group by city
5. Sort by highest revenue
6. Save summary file to `reports/`

---

## Example Output

```
city,total_revenue
Mumbai,135000
Delhi,45000
Bangalore,33000
```

---

## How to Run

### Step 1 — Create S3 Bucket
Create a bucket and two folders:

```
incoming/
reports/
```

### Step 2 — Create Lambda Layer

Install pandas locally:

```
pip install pandas -t python/
```

Zip the folder:

```
zip -r pandas_layer.zip python
```

Upload as a Lambda Layer.

---

### Step 3 — Create Lambda Function

Runtime: Python 3.10

Attach the pandas layer.

---

### Step 4 — Add S3 Trigger

Configure S3 trigger:

- Event type: Object Created
- Prefix: `incoming/`
- Suffix: `.csv`

---

### Step 5 — Upload File

Upload `orders.csv` to:

```
incoming/orders.csv
```

Lambda will automatically generate the report.

---

## Verification

Check:

- `reports/` folder for output file
- CloudWatch logs for Lambda execution

---

## Screenshots

Screenshots included for:

- S3 bucket structure
- Lambda trigger configuration
- Generated report file
- CloudWatch logs

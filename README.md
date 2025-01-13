# Amazon Ads Analytics Automation

## Overview
This project was developed for a small e-commerce firm to automate the process of retrieving, cleaning, and transforming Amazon Ads data. The goal was to help the firm measure the performance of their ads effectively using automated workflows. The solution integrates API requests for Amazon Ads reports, data cleaning, and transformation workflows in Google BigQuery, executed on a daily basis.

## Features
- **Automated API Requests**: Fetch Amazon Ads performance reports daily using the Amazon Ads API.
- **Data Cleaning and Transformation**: Clean and transform the retrieved data into a usable format for analysis using BigQuery.
- **Scheduling and Automation**: Automate the entire workflow using Python scripts and orchestration tools like Cron or Cloud Scheduler.

## Project Architecture
1. **Amazon Ads API Integration**:
   - Scheduled API requests to retrieve ad performance reports.
   - Saved data as JSON or CSV files for further processing.

2. **Data Cleaning and Transformation**:
   - Automated cleaning of raw API data.
   - Transformation into structured tables in BigQuery.

3. **Daily Automation**:
   - Python scripts scheduled to run daily using cron jobs or Google Cloud Scheduler.

4. **Visualization and Reporting**:
   - The cleaned data was made available for BI tools such as Looker Studio or Tableau.

---

## Installation

### Prerequisites
1. Python 3.7+
2. Google Cloud SDK and BigQuery API enabled
3. Amazon Ads API credentials
4. Required Python libraries:
   ```bash
   pip install requests google-cloud-bigquery pandas
   ```

### Setup
1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd amazon-ads-analytics-automation
   ```

2. Add your credentials:
   - **Amazon Ads API**: Place your `access_token` and `refresh_token` in a secure `.env` file.
   - **Google Cloud BigQuery**: Set up your `GOOGLE_APPLICATION_CREDENTIALS` environment variable pointing to your service account key.

3. Configure your BigQuery Dataset and Table in the provided configuration file (`config.json`).

---

## Usage

### 1. Fetching Amazon Ads Reports
Run the script to fetch daily reports:
```bash
python fetch_amazon_ads.py
```

### 2. Cleaning and Transforming Data
Run the data processing script:
```bash
python process_data_bigquery.py
```

### 3. Automating Daily Workflow
Use a scheduler like cron to automate these scripts:
```bash
0 2 * * * python fetch_amazon_ads.py
0 3 * * * python process_data_bigquery.py
```

---

## Code

### fetch_amazon_ads.py
```python
import requests
import os
import json
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

ACCESS_TOKEN = os.getenv("AMAZON_ADS_ACCESS_TOKEN")
REFRESH_TOKEN = os.getenv("AMAZON_ADS_REFRESH_TOKEN")
API_URL = "https://advertising-api.amazon.com/v2/reports"

def fetch_ads_report():
    headers = {
        "Authorization": f"Bearer {ACCESS_TOKEN}",
        "Content-Type": "application/json",
    }
    payload = {
        "reportDate": "yesterday",
        "metrics": ["impressions", "clicks", "spend", "sales"]
    }
    response = requests.post(API_URL, headers=headers, json=payload)
    if response.status_code == 200:
        with open("ads_report.json", "w") as f:
            json.dump(response.json(), f)
        print("Report fetched and saved successfully.")
    else:
        print("Failed to fetch report:", response.text)

if __name__ == "__main__":
    fetch_ads_report()
```

### process_data_bigquery.py
```python
import pandas as pd
from google.cloud import bigquery
import json

# Load BigQuery client
client = bigquery.Client()

# Configure BigQuery Dataset and Table
DATASET_ID = "your_dataset"
TABLE_ID = "ads_performance"

# Load and clean data
def load_and_clean_data(file_path):
    with open(file_path, "r") as f:
        raw_data = json.load(f)
    df = pd.DataFrame(raw_data["data"])
    df["report_date"] = pd.to_datetime(raw_data["reportDate"])
    return df

# Upload to BigQuery
def upload_to_bigquery(df):
    table_ref = client.dataset(DATASET_ID).table(TABLE_ID)
    job = client.load_table_from_dataframe(df, table_ref)
    job.result()  # Wait for job to complete
    print("Data uploaded to BigQuery successfully.")

if __name__ == "__main__":
    cleaned_data = load_and_clean_data("ads_report.json")
    upload_to_bigquery(cleaned_data)
```

---

## Next Steps
1. **Enhancements**: Implement retry mechanisms for API requests and error handling for BigQuery operations.
2. **Visualization**: Set up dashboards in a BI tool like Looker Studio for tracking ad performance trends.

---

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---

## Contact
For questions or support, please contact:
**[Your Name]** - [your.email@example.com]


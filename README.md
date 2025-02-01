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


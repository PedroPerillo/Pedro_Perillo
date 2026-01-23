Lab 2.4 — Reproducible ETL with Logging (Run Instructions)

This document describes how to reproduce the ETL pipeline using the Week 1 restaurant dataset (menu_items.csv + order_details.csv).

1. Repository Structure

The repository should contain:

data/ → input CSVs
logs/ → log files generated at runtime
etl_output/ → ETL metric outputs (CSV)
lab_2_4_repro_logging.ipynb → main notebook
requirements.txt → captured environment
data_hashes.json → SHA-256 hashes for inputs
RUN.md → run instructions (this file)
README.md → project overview
config.yaml → optional notebook configuration

2. Prerequisites

Environment requirements:

• Python 3.9+ or Databricks cluster
• menu_items.csv and order_details.csv must exist in data/

If running locally:

pip install -r requirements.txt

If running in Databricks, dependency installation may not be required.

3. How to Run the ETL Notebook

Open the notebook:
lab_2_4_repro_logging.ipynb

Run all cells top-to-bottom. The notebook performs:
• Logging setup (console + file)
• Reproducibility seeds (PYTHONHASHSEED, random, numpy)
• Environment capture (pip freeze > requirements.txt)
• Data hashing (SHA-256 → data_hashes.json)
• CSV load and cleaning
• Join on menu_item_id = item_id
• Tidy table creation (order, datetime, item, category, price, quantity, revenue)
• Metric computation
• Output saving
• Assert tests for verification

4. Outputs Generated

After a successful run, the following artifacts are produced:

A. Logs

Written to:
logs/run_YYYYMMDD_HHMM.log

Contains:
• timestamps
• log level
• config values
• runtime information
• test results

B. ETL Artifacts

Written to:
etl_output/metrics_YYYYMMDD_HHMM.csv

Contains:
• top_items_by_quantity
• revenue_by_category
• busiest_hour_of_day

C. Hashes

data_hashes.json
Contains SHA-256 values for reproducibility validation.

Example:
{
"data/menu_items.csv": "<sha256>",
"data/order_details.csv": "<sha256>"
}

5. Reproducibility Guarantees

This pipeline is reproducible due to:

• Fixed seeds:
PYTHONHASHSEED, random.seed(0), numpy.random.seed(0)

• Environment capture:
requirements.txt

• Input hashing:
data_hashes.json

• File-based logging with timestamped run logs

• Assert tests verifying data integrity

6. Validation Tests

The final assert block confirms:

• tidy_df is non-empty
• Expected tidy_df columns exist:
order_id, order_datetime, item_name, category, price, quantity, revenue
• metrics_df is non-empty
• All three metrics are present:
top_items_by_quantity, revenue_by_category, busiest_hour_of_day

Example log output:
INFO | Running assert tests
INFO | All assert tests passed ✔

7. Optional Spark Extension (If Implemented)

If the Spark extension is enabled, Spark loads the same CSVs and recomputes one or more metrics for comparison. Comparison logs are recorded in the runtime log.

8. Troubleshooting

If new CSVs are added, place them in data/ and rerun the notebook.

If package differences arise, regenerate requirements.txt using:
pip freeze > requirements.txt
# Fetch Job Logs and Status

This project provides a Google Colab notebook to batch fetch **logs** and **status** for multiple job IDs from the `acadcodegen-production.up.railway.app` API.  
It is designed to help debug failed jobs by saving their responses into structured files.

---

## Features

- Queries both endpoints for each Job ID:
  - `https://acadcodegen-production.up.railway.app/api/job/<JOB_ID>/logs`
  - `https://acadcodegen-production.up.railway.app/api/job/<JOB_ID>/status`
- Saves results into an `outputs/` folder:
  - `<JOB_ID>_logs.json` — Logs in JSON wrapper
  - `<JOB_ID>_logs.txt` — Raw text (if logs are plain text)
  - `<JOB_ID>_status.json` — Status in JSON wrapper
  - `<JOB_ID>_status.txt` — Raw text (if status is plain text)
- Creates a `summary.csv` with:
  - Job ID
  - HTTP response codes
  - Extracted job status (if available)
  - References to the saved files
- Optional: Zips the `outputs/` folder for easy download
- Built-in retries and backoff for network resiliency

---

## Usage

You have two options to use the notebook:

### 1. Open the shared Colab link directly

[Open in Google Colab](https://colab.research.google.com/drive/1xaZWYERGoR6zpLEntt72KhIUYRuiyTDl#scrollTo=qWjVuS3Dju5q)

### 2. Use the provided Colab notebook file

1. Upload the `Fetch_Job_Logs_and_Status.ipynb` notebook to Colab.  
2. Paste your Job IDs into the `job_ids` list (pre-filled with examples).  
3. Run all cells.  
4. Colab will:  
   - Call the API endpoints for each job  
   - Save logs and status files under `outputs/`  
   - Generate `summary.csv`  
   - Optionally, zip the entire output folder  

---

## Example Job IDs

From your failed jobs list:

```text
ai_pipeline_d9de2314-d984-45b6-b130-2bc0d0886fa2  # BadgeRegistry
ai_pipeline_b8c58f70-7f3d-4e4b-96c1-cad82372af3b  # ContentRegistry
ai_pipeline_0d8a9752-b54b-4fd8-a6a6-e607b3c21206  # WhitelistRegistry
ai_pipeline_9d3e8c13-7fde-4d5d-ae19-846fcf66a905  # SimpleLottery
```

---

## Output Files

- `outputs/<JOB_ID>_logs.json`
- `outputs/<JOB_ID>_logs.txt` *(if logs returned as text)*
- `outputs/<JOB_ID>_status.json`
- `outputs/<JOB_ID>_status.txt` *(if status returned as text)*
- `outputs/summary.csv`
- `job_results.zip` *(optional zipped archive)*

---

## Troubleshooting

- **401/403 Unauthorized**: Add authentication headers in the `make_session()` function.  
- **Text logs instead of JSON**: Use the `*_logs.txt` or `*_status.txt` files.  
- **Timeouts / flaky network**: Increase `total_retries` and `timeout` values in `make_session()`.  
- **Different API base URL**: Update the `BASE` constant at the top of the notebook.  

---

## License

MIT License — feel free to adapt this notebook to your own pipelines.

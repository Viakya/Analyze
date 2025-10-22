# Overview

This project fixes a data processing script and sets up CI to:

- Lint with ruff (results printed in CI logs)
- Run `python execute.py > result.json`
- Publish `result.json` via GitHub Pages
- Convert and commit `data.csv` from the provided `data.xlsx`

Key fixes and deliverables:

- execute.py updated to work on Python 3.11+ and Pandas 2.3
- Corrected the typo and logic around revenue computation and rolling averages
- `.github/workflows/ci.yml` builds on pushes, runs ruff, executes the script, and deploys `result.json` via Pages
- `data.csv` is generated from `data.xlsx` by CI and committed back if needed
- `result.json` is not committed to the repo and is produced only in CI

# Setup

Prerequisites:

- Python 3.11+
- Pandas 2.3.x
- `data.xlsx` present at the repo root (from the provided attachment)

Recommended local install:

- Create a virtual environment and install dependencies:
  - python -m venv .venv
  - source .venv/bin/activate (Linux/Mac) or .venv\Scripts\activate (Windows)
  - pip install "pandas==2.3.0" openpyxl ruff

Files to add/replace:

- execute.py (use the version from this repo)
- .github/workflows/ci.yml
- Optionally a .gitignore that ignores result.json

# Usage

Local run (generates JSON to stdout):

- python execute.py > result.json

What the script outputs:

- row_count: total rows in the dataset
- regions_count: count of unique regions
- top_n_products_by_revenue: top 3 products by total revenue
- rolling_7d_revenue_by_region: last value of 7-day moving average of daily revenue per region

CI behavior on push:

- Installs Python 3.11, Pandas 2.3, openpyxl, and ruff
- Converts data.xlsx to data.csv and commits data.csv if it changes
- Runs ruff and prints findings in the workflow logs
- Runs python execute.py > result.json
- Publishes result.json to GitHub Pages at:
  - https://<owner>.github.io/<repo>/result.json

Do not commit result.json:

- It’s generated in CI and published; it should not be in the repo.
- You can add result.json to .gitignore to avoid accidental commits.

Notes:

- The script prefers reading data.xlsx (engine=openpyxl) and falls back to data.csv if reading Excel fails.
- The CI also ensures data.csv always matches the Excel attachment for traceability.
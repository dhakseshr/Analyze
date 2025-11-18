# CI-ready pipeline: execute.py, data.csv, and GitHub Pages publishing

## Overview
This project provides:
- A corrected execute.py compatible with Python 3.11+ and Pandas 2.3 that reads data.csv and prints a JSON summary to stdout.
- A committed data.csv (converted from the original Excel dataset) so CI does not need Excel dependencies.
- A GitHub Actions workflow that:
  - Runs Ruff and shows lint results in CI logs.
  - Executes python execute.py > result.json.
  - Publishes result.json to GitHub Pages.
- result.json is not committed; it is produced by CI and published via Pages.

What execute.py does:
- Loads data.csv.
- Ensures a revenue column exists (computes from units × price if necessary).
- Aggregates total revenue, revenue by product, monthly revenue, and top product.
- Outputs a single JSON object to stdout.

Bug fix note:
- The script no longer uses any misspelled fields and is robust to missing revenue; it avoids the earlier logic issue by normalizing columns and computing revenue when needed.

## Setup
1. Requirements:
   - Python 3.11+
   - Git
   - A GitHub repository with Actions enabled (default)
2. Optional local tools:
   - Ruff: pip install ruff
   - Pandas 2.3: pip install "pandas>=2.3,<2.4"

## Files to add
Add the following files to your repository:

- execute.py (place at repo root)
- data.csv (place at repo root)
- .github/workflows/ci.yml (GitHub Actions workflow)

The HTML app (index.html) in this repository contains exact file contents and buttons to copy/download each file.

## Usage
Local run:
1. Install dependencies:
   - python -m pip install -U pip
   - pip install "pandas>=2.3,<2.4"
2. Generate JSON:
   - python execute.py > result.json
3. Inspect result:
   - cat result.json

CI run (on push):
- The included workflow will:
  1. Set up Python 3.11.
  2. pip install ruff and pandas.
  3. Run ruff check . and print results to the log.
  4. Run python execute.py > result.json (not committed).
  5. Upload result.json as a GitHub Pages artifact and deploy.

GitHub Pages:
- After the workflow completes, your JSON will be available at:
  - https://YOUR_USER.github.io/YOUR_REPO/result.json

Verification checklist:
- execute.py, data.csv, and .github/workflows/ci.yml exist in your repo.
- result.json is NOT committed to the repo.
- CI logs show Ruff output and the execution of execute.py.
- result.json is published and accessible via GitHub Pages.

Notes:
- If your original dataset was an Excel file (data.xlsx), it has been converted and committed as data.csv to simplify CI and avoid Excel reader dependencies.
- The workflow purposefully uploads only result.json to Pages; no additional files are required to satisfy publishing requirements.
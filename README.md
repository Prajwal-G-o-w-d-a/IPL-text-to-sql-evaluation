# IPL Text-to-SQL Evaluation

A Python project for evaluating text-to-SQL models through OpenRouter. Models generate SQLite queries from natural-language questions about IPL match data, and the project compares query results against a golden dataset.

## Features

- Sends prompts to the models configured in `model_openrouter_slug.py` through OpenRouter.
- Executes generated SQL against a local SQLite database.
- Compares results with the expected answers and writes detailed scores to `eval_results.csv`.
- Includes scripts for building the database and inspecting its schema.

## Requirements

- Python
- An OpenRouter API key to run model evaluations

Python's standard library provides SQLite support. Third-party Python packages are listed in `requirements.txt`.

## Setup

Run these commands in the project directory using PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
```

Open `.env` and replace the placeholder with your OpenRouter API key:

```text
OPENROUTER_API_KEY=your_actual_api_key
```

Keep `.env` private; it is excluded from Git.

## Prepare the database

The generated SQLite database is not included in Git. Build it from the CSV files in `data/`:

```powershell
python db.py
```

This creates `ipl_2021_2024.db` in the project directory. The evaluation scripts expect this database, `schema.sql`, and `golden_dataset.csv` to be available in the same directory.

## Run an evaluation

After configuring `.env` and preparing the database, run:

```powershell
python main.py
```

The script runs the models listed in `model_openrouter_slug.py`, prints their execution-accuracy scores, and saves detailed results to `eval_results.csv`. Model availability and API usage costs depend on OpenRouter.

## Other scripts

- `first_test.py` sends a single test question to a model through OpenRouter.
- `schema_extractor.py` prints and saves the database schema; it can also create a renamed database when configured to do so.
- `make_golden_dataset.py` generates `golden_hard.csv` from the questions in `golden_dataset_generator.py`.
- `evaluator.py` contains the result-comparison logic used by `main.py`.

## Project files

- `data/` — source match and delivery CSV files.
- `db.py` — builds the local SQLite database from the source CSVs.
- `schema.sql` — database schema supplied to models in the prompt.
- `golden_dataset.csv` — evaluation questions and expected results.
- `model_openrouter_slug.py` — model names and OpenRouter IDs used by the evaluation.
- `requirements.txt` — Python package dependencies.


# Netflix-Streaming-Analysis (DuckDB Data Warehouse + EDA)

A Netflix-style analytics project built on a small **data-warehouse style schema** (users, watch history, movies, reviews, recommendations, and search logs).  
The notebooks load CSV tables into **DuckDB**, perform **data cleaning + quality checks**, create a **merged analytical view**, and generate EDA insights like **watch-time by plan/age**, **top genres**, **genre quality (watch-hours + ratings)**, and **recommendation performance (CTR + click→watch conversion)**.

## Notebooks
- `netflix_streaming_cleaned.ipynb` — full cleaned workflow + EDA + merged insights
- `netflix_streaming_cleaned_styled.ipynb` — lighter “styled” version with cleaner presentation visuals

## Data (required)
Place these CSV files in a local folder (recommended: `data/`):

- `users.csv`
- `watch_history.csv`
- `movies.csv`
- `reviews.csv`
- `recommendation_logs.csv`
- `search_logs.csv`

> The notebooks currently contain an **absolute path** (`DATA_DIR = "/Users/.../netflix"`).  
> Before pushing to GitHub, change it to a relative path like:
```python
DATA_DIR = "data"
```

### Typical join keys
- `user_id` links: `users` ↔ `watch_history` ↔ `reviews` ↔ `recommendation_logs` ↔ `search_logs`
- `movie_id` links: `movies` ↔ `watch_history` ↔ `reviews` ↔ `recommendation_logs`

## What the notebooks do
### 1) Raw checks
- Missing values + duplicate checks on raw tables

### 2) Cleaning steps (DuckDB SQL)
- De-duplication (keep latest/most relevant records)
- Missing-value handling / standardisation
- Type fixes (dates, numeric fields)
- Post-clean quality checks

### 3) EDA (clean tables)
- **Users**: demographics & subscription plans
- **Watch history**: engagement (watch duration, completion/progress)
- **Movies**: catalogue overview (genres, years, ratings)
- **Reviews**: ratings + text/sentiment columns (if present)
- **Recommendations**: CTR and conversion (click → watch)
- **Search logs**: search behaviour + “search vs non-search” users

### 4) Merged analytical view
A merged sessions-style view (users + movies + watch history) to answer:
- Average watch duration & progress by plan  
- Top genres by total hours watched  
- Engagement by age band  
- Genre-level “quality” (hours watched + avg rating)  
- Recommendation CTR by type  
- Click → watch conversion  
- Search vs non-search user behaviour  

## How to run
### 1) Create an environment
```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -U pip
```

### 2) Install dependencies
```bash
pip install -U duckdb pandas numpy matplotlib jupyter
```

### 3) Run
```bash
jupyter notebook
```
Open either notebook and run cells top-to-bottom.

## Suggested repo structure
```text
.
├── netflix_streaming_cleaned.ipynb
├── netflix_streaming_cleaned_styled.ipynb
├── README.md
└── data/                    # keep local; add to .gitignore if needed
    ├── users.csv
    ├── watch_history.csv
    ├── movies.csv
    ├── reviews.csv
    ├── recommendation_logs.csv
    └── search_logs.csv
```

## Notes
- DuckDB runs fully locally and is great for SQL-style analytics over CSVs.
- If your dataset is from Kaggle or another provider, **follow their license/terms** and avoid committing large raw data files.

## License
Add a license (e.g., MIT) if you want others to reuse this work.
```

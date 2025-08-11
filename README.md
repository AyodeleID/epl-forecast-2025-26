# epl-forecast-2025-26
Machine learning model predicting EPL 2025_26 Season outcomes.
This project trains a Dixon–Coles Poisson model on the last three Premier League seasons and predicts probabilities for every 2025/26 fixture (Home/Draw/Away + most-likely score).
- Training data: football-data.co.uk Premier League results (E0) for 2022/23, 2023/24, 2024/25.
- Fixtures with dates: football-data.org (PL 2025 season).
- Output: predictions_epl_2025_26.csv (openable in Excel).

What you get
- Full-season predictions for all scheduled fixtures (typically 380 matches).
- For each match: Date, HomeTeam, AwayTeam, P(Home), P(Draw), P(Away), MostLikelyScore, Tip.

How it works (high level)
	1.	Download results for the last 3 seasons from football-data.co.uk.
	2.	Fit Dixon–Coles (time-decayed likelihood + low-score adjustment).
	3.	Fetch 2025/26 fixtures (with dates) from football-data.org.
	4.	Predict outcome probabilities and the most likely scoreline for each fixture.

Requirements
	•	Python 3.9+
	•	Packages:

 pip install -r requirements.txt

 requirements.txt:
- pandas
- numpy
- scipy
- requests

- Quick start (two ways)

A) Jupyter Notebook (recommended for exploration)
	1.	Open the notebook in this repo:
ML Model Predicting EPL 25 and 26 Season Weekly Outcome...ipynb
	2.	In a cell near the top, set your API key one of two ways:
Option 1 (env magic):
%env FOOTBALL_DATA_API_KEY=YOUR_KEY_HERE
API_KEY = "YOUR_KEY_HERE"
3.	Run the notebook top-to-bottom. It will create predictions_epl_2025_26.csv.

B) Python script (hands-off run)
	1.	Save the script file from this repo: epl_2025_26_run.py.
	2.	Set your key and run:
 
macOS/Linux:
export FOOTBALL_DATA_API_KEY=YOUR_KEY_HERE
python epl_2025_26_run.py

Windows PowerShell:
$env:FOOTBALL_DATA_API_KEY="YOUR_KEY_HERE"
python epl_2025_26_run.py

This will produce predictions_epl_2025_26.csv in the repo folder.


Output columns
- HomeTeam, AwayTeam — harmonized team names
- P(Home), P(Draw), P(Away) — predicted probabilities
-	MostLikelyScore — most probable exact scoreline (0–6 goals grid)
-	Tip — simple pick: H / D / A (max of the three probabilities)

Verify it’s the full season

Run this quick check (in Python or a notebook cell):

import pandas as pd
df = pd.read_csv("predictions_epl_2025_26.csv")
len(df), df["HomeTeam"].nunique(), df["AwayTeam"].nunique()
You should see something like (380, 20, 20) for a standard 20-team, 38-matchweek season.


gitignore
# Jupyter & Python
.ipynb_checkpoints/
__pycache__/
*.pyc
.venv/
env/
# OS cruft
.DS_Store
# Secrets
*.env

Notes & assumptions
- Promoted teams (not seen in last 3 EPL seasons) are initialized at league-average strength so every match gets a prediction out of the box.
- The model uses a score grid up to 6 goals. You can enlarge it if you want, but defaults are fast and stable.
- To reproduce exactly: keep the same three training seasons and PL 2025 fixtures.

  Troubleshooting
- 401 Unauthorized when fetching fixtures
Ensure the football-data.org key is set (env or notebook variable).
-	ModuleNotFoundError
Run pip install -r requirements.txt in the same Python environment.
-	Row count not ~380
Fixture lists can update; re-run to refresh predictions.

FAQ
Is this prediction for all clubs for the whole season?
Yes — it covers every scheduled 2025/26 Premier League fixture (typically 380 matches). Check with the snippet in Verify full season.





Credits
	•	Dixon & Coles (1997) methodology for football score modelling.
	•	football-data.co.uk (match results CSVs).
	•	football-data.org (fixtures API).



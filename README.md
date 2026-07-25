# EPL Over 2.5 Goals Prediction Model

A machine learning pipeline that predicts the probability of over 2.5 goals in English Premier League matches, and flags value bets where the model's predicted probability meaningfully exceeds the bookmaker's implied (vig-free) probability.

## How It Works

- **Model:** XGBoost classifier trained on a 43-feature set derived from team form, scoring rates, and expected-goals (xG) data across ~3,000 EPL matches.
- **Leakage prevention:** All rolling/form-based features are computed with `shift(1)` so that no feature for a given match can see data from that match or later. An automated `pytest` suite in `tests/` checks this directly rather than relying on manual review.
- **Validation:** Walk-forward cross-validation across 10+ seasons evaluates calibration realistically (training only on past seasons, testing on the next), instead of a single random train/test split that would overstate performance.
- **Staking strategy:** Predicted probabilities feed a Kelly criterion staking model, used to test whether the model's edge over vig-free market odds is statistically meaningful rather than noise.

## Repository Structure

```
.
├── config/leagues/     # League-specific configuration
├── core/               # Core pipeline logic
├── data/               # Raw and processed match/xG data
├── database/           # PostgreSQL schema and connection setup
├── features/           # Feature engineering (rolling stats, shift(1) enforcement)
├── ingestion/          # Data ingestion from football-data.co.uk, Understat, and odds APIs
├── models/             # Trained XGBoost model artifacts
├── predictions/        # Generated weekly predictions
├── scripts/            # Utility/one-off scripts
├── simulation/         # Backtesting and staking simulation (Kelly criterion)
├── tests/              # pytest suite, including automated leakage checks
├── predict.py          # Entry point: generate predictions for upcoming matches
├── tracker.py           # Tracks prediction outcomes over time
├── testing.py          # Model evaluation / walk-forward validation
├── refresh.sh          # Refreshes data and retrains/reruns the pipeline
└── requirements.txt
```

## Tech Stack

- **Language:** Python
- **Modeling:** XGBoost, scikit-learn
- **Data:** pandas, NumPy
- **Database:** PostgreSQL via SQLAlchemy + psycopg2
- **Data sources:** football-data.co.uk (results/odds), Understat (`understatapi`) for xG data
- **Testing:** pytest
- **CLI output:** rich

## Setup

```bash
pip install -r requirements.txt
cp .env.example .env   # fill in DB credentials and odds API key
python predict.py
```

## Testing

```bash
pytest
```

The suite includes automated checks that rolling features do not leak future information into the training set, in addition to standard unit tests for feature computation and prediction logic.

## Roadmap

The current pipeline is a research/CLI tool. Planned productization work (not yet implemented) includes:

- FastAPI backend exposing predictions via API
- React + Tailwind frontend dashboard (bet tracking, bankroll visualization)
- User authentication (Clerk) and subscription payments (Stripe)
- Scheduled ingestion/prediction jobs via a task queue (Celery or APScheduler)
- Deployment to a DigitalOcean or Hetzner VM

This section describes direction, not current functionality — see **Repository Structure** above for what's actually implemented today.
# SmartBet
A machine learning pipeline that predicts over 2.5 goals in English Premier League matches and identifies value bets where the model's probability meaningfully exceeds the bookmaker's implied probability.

# File Structure

smartbet/
├── api/                # FastAPI Backend
│   ├── main.py         # Entry point
│   ├── routes/         # Auth, Predictions, Payments
│   └── database.py     # SQLAlchemy/Postgres setup
├── frontend/           # React + Tailwind
│   ├── src/
│   │   ├── components/ # BetCard, BankrollChart
│   │   └── pages/      # Dashboard, Landing
├── ml_engine/          # Your existing model logic
│   ├── models/         # Saved XGBoost .json files
│   ├── ingestion/      # xG and Odds scrapers
│   └── predict.py      # Logic to generate weekly picks
└── docker-compose.yml  # To run Postgres and API locally


💻 Core Technology Stack
  -   Frontend: React + Tailwind CSS 
  -   Backend: Python + FastAPI
  -   Database: PostgreSQL
  -   Authentication: Clerk
  -   Payments: Stripe
  -   Task Queue: Celery / APScheduler

📊 Data Sources & Pipeline
  -   football-data.co.uk: Provides match results and closing 
  -   odds.Understat: Provides expected goals (xG) data.
  -   The Odds API: Provides live odds.
Automation: Scheduled via task queues and ingestion scripts inside the ml_engine/ingestion/ folder.

🤖 Machine Learning Stack
  -   Language: Python.
  -   Model: XGBoost Classifier (hyperparameters optimized to prevent overfitting on small datasets).
  -   Key Libraries:pandas SQLAlchemy psycopg2 scikit-learn rich

☁️ Infrastructure & Deployment
  -   Deployment Target: DigitalOcean Droplet or Hetzner VM.
  -   Orchestration / Automation: GitHub Actions.

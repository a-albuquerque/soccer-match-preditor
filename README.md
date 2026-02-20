# Soccer Match Result Predictor

A machine learning project for predicting the outcome of soccer matches — win, draw, or loss — based on pre-match statistics and historical team data.

---

## Background

This started as a weekend experiment that got a bit out of hand. I wanted to see how well a model could predict match results using publicly available stats, without relying on live odds or bookmaker data. Turns out there's a lot more signal in the numbers than I expected.

The model treats each match as a three-class classification problem: home win, away win, or draw.

---

## Features Used

Below are the attributes currently fed into the model, grouped by category. Some have more predictive weight than others — that's part of what the project explores.

### Team Form & Performance
- **Recent form** — points earned in the last 5 matches (separate for home and away games)
- **Goals scored / conceded** per game (rolling 5-match average)
- **Win/draw/loss streak** going into the match
- **xG (expected goals)** — both for and against, if available
- **Shot accuracy** — shots on target as a percentage of total shots

### Head-to-Head History
- Win/draw/loss record between the two teams in the last 5–10 meetings
- Average goals in H2H matches
- Home advantage factor based on H2H results at the same venue

### Squad & Lineup
- **Key player availability** — whether top scorers or key defenders are injured or suspended
- **Squad depth rating** — based on overall quality of available substitutes
- **Number of players on international duty** (fatigue factor, especially in mid-season windows)

### Fixture Context
- **Days since last match** — rest time for both teams
- **Match importance** — league match vs. cup, relegation six-pointer, title decider, etc.
- **Travel distance** for the away team
- **Altitude difference** between home city and away team's home city

### League & Season Context
- **Current league position** of both teams
- **Points gap** to the top four / relegation zone (motivation proxy)
- **Stage of the season** — early, mid, run-in
- **Home/away record** for the current season separately

### Managerial & Tactical
- **Manager tenure** — new managers tend to get a short-term bounce
- **Tactical matchup rating** — attacking vs. defensive style index (experimental)

### Odds-Derived (optional)
- **Implied probabilities from betting markets** — used as a feature, not a target
- **Line movement** — how odds shifted in the 48h before kickoff

> **Note:** Not all features are available for every league or dataset. The model handles missing values via imputation, but sparsely covered leagues will naturally underperform.

---

## Model

Currently experimenting with a few approaches:

- **Baseline:** Logistic Regression (good interpretability benchmark)
- **Main model:** Gradient Boosted Trees (XGBoost / LightGBM)
- **Experimental:** A simple feedforward neural net for comparison

Evaluation is done using log loss and accuracy on a held-out test set. Because draws are hard to predict and relatively rare, class weights are adjusted during training.

---

## Project Structure

```
soccer-predictor/
│
├── data/
│   ├── raw/              # Raw match data, not committed to repo
│   └── processed/        # Cleaned and feature-engineered datasets
│
├── notebooks/
│   ├── eda.ipynb         # Exploratory data analysis
│   └── model_dev.ipynb   # Model training and evaluation
│
├── src/
│   ├── features.py       # Feature engineering pipeline
│   ├── train.py          # Model training script
│   ├── predict.py        # Run predictions on new fixtures
│   └── utils.py          # Shared helpers
│
├── models/               # Saved model artifacts
├── requirements.txt
└── README.md
```

---

## Getting Started

```bash
git clone https://github.com/yourusername/soccer-predictor.git
cd soccer-predictor
pip install -r requirements.txt
```

To train the model on your own data:

```bash
python src/train.py --data data/processed/matches.csv --output models/
```

To predict upcoming fixtures:

```bash
python src/predict.py --fixtures data/fixtures.csv --model models/xgb_model.pkl
```

---

## Data Sources


---

## Results



---

## Roadmap



---

## Contributing

This is a personal project, but I'm happy to take ideas or PRs. If you've worked on something similar or have thoughts on features I'm missing, open an issue or reach out.

---

## License

MIT

# Smartphone Addiction Prediction (Kaggle Playground Series S6E8)
**Status: ongoing / work in progress**

Binary classification project for the Kaggle Playground Series competition
[*Predicting Smartphone Addiction*](https://www.kaggle.com/competitions/playground-series-s6e8/data)
(Season 6, Episode 8). The goal is to predict whether a person is addicted
to their smartphone (`addicted_label`) from behavioral and demographic
features.

## Data

Place the competition files in `smartphone_addiction_data/` (gitignored):

- `train.csv` — 691,369 rows, labeled
- `test.csv` — 296,302 rows, unlabeled
- `sample_submission.csv` — expected submission format (`id`, `addicted_label`)

### Features

| Column | Description |
|---|---|
| `age` | Age of respondent |
| `daily_screen_time_hours` | Total daily screen time |
| `social_media_hours` | Hours spent on social media |
| `gaming_hours` | Hours spent gaming |
| `work_study_hours` | Hours spent on work/study |
| `sleep_hours` | Hours of sleep |
| `notifications_per_day` | Number of notifications received per day |
| `app_opens_per_day` | Number of app opens per day |
| `weekend_screen_time` | Screen time on weekends |
| `gender` | Gender |
| `stress_level` | Self-reported stress level (Low/Medium/High) |
| `academic_work_impact` | Whether phone use impacts academic/work performance (Yes/No) |
| `addicted_label` | **Target** — 1 if addicted, 0 otherwise (train only) |

Several numeric columns contain missing values that need handling.

## Project structure

- `example.ipynb` — Kaggle-provided starter notebook (gitignored)
- `notebook.ipynb` — working analysis / modeling notebook
- `smartphone_addiction_data/` — raw competition data (gitignored, not tracked)

## TODO

- [ ] Exploratory data analysis
- [ ] Handle missing values
- [ ] Feature engineering
- [ ] Baseline model
- [ ] Model tuning / submission
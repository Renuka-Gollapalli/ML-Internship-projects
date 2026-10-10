# ML Internship Projects

Three machine learning projects built during my ML internship. Each one covers the full flow: exploring the data, cleaning it, building a model, and checking how well it really works.

| # | Project | Task | Data | Model |
|---|---------|------|------|-------|
| 1 | [Bike Sharing Demand Prediction](01-bike-sharing-demand-prediction) | Regression | Seoul hourly bike rentals and weather | Random Forest Regressor |
| 2 | [Diabetes Risk Prediction](02-diabetes-risk-prediction) | Binary classification | Symptom questionnaire, 520 patients | Random Forest Classifier |
| 3 | [Human Activity Recognition](03-human-activity-recognition) | Multi-class classification | Wearable accelerometers, 2.2M readings | Random Forest on window features |

## What I paid attention to

- **No data leakage.** Bike data is split by whole days, diabetes data is de-duplicated before splitting, and activity data is split by person. Random row splits can give prettier scores that do not hold up on new data. In the diabetes project, 269 of 520 records were duplicates, and removing them moved accuracy from 98% to about 92% on a held-out split.
- **Metrics that fit the problem.** Macro F1 for the imbalanced activity data, recall-focused scoring for the diabetes screening task.
- **Honest comparison.** Each notebook shows the baseline model next to the tuned one, even when tuning gives little or no gain.

## How to run

1. Clone the repo and install the packages:
   ```
   pip install -r requirements.txt
   ```
2. The Bike and Diabetes CSVs are already included. For Human Activity, download the 18 files from the link in that project's folder and place them in its `data/` folder (they are too large for GitHub).
3. Open the notebook with `jupyter notebook` and run all cells.

## Datasets and credits

All three datasets are from the UCI Machine Learning Repository under CC BY 4.0.

- [Seoul Bike Sharing Demand](https://doi.org/10.24432/C5F62R)
- [Early Stage Diabetes Risk Prediction](https://doi.org/10.24432/C5VG8H)
- [HAR70+](https://doi.org/10.24432/C5CW3D)

## Disclaimer

The diabetes project is a learning exercise on a small questionnaire dataset. It is not a medical tool and must not be used to diagnose anyone.

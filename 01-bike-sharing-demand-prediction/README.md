# Bike Sharing Demand Prediction

Predict how many bikes will be rented in a given hour in Seoul, using weather and calendar information.

## Data

- [Seoul Bike Sharing Demand](https://archive.ics.uci.edu/dataset/560/seoul+bike+sharing+demand), UCI, CC BY 4.0
- 8,760 hourly rows, 14 columns (temperature, humidity, wind, rainfall, season, holiday and more)
- Download `SeoulBikeData.csv` and place it in the `data/` folder

## Approach

1. **Explore:** missing values, duplicates, skew, busiest hours, hourly pattern by season and holiday, effect of temperature and rain
2. **Prepare:** extract month and day of week from the date, one-hot encode season and holiday
3. **Model:** Random Forest Regressor, then a grid search over `n_estimators`, `max_depth` and `min_samples_split`
4. **Evaluate:** R², RMSE, MAE and feature importance

## Results

Tested on 20% of days the model never saw:

- **R² = 0.926**, RMSE = 174 bikes per hour, MAE = 111 bikes per hour
- Grid search gave R² = 0.926, the same as the baseline, so the default Random Forest settings were already good enough
- Top features: temperature, hour of day, solar radiation, humidity

## Decisions worth knowing

- **Non-functioning hours are removed.** On those hours the system was closed, so rentals are always 0. Keeping them makes the model look better by predicting something trivial.
- **Train and test are split by whole days.** With a random row split, 11 AM can be in train and 12 noon of the same day in test, which can inflate the score. On this dataset the effect turned out small (R² was about 0.93 with a random split too), but the day-wise split is the safer way to measure it.
- **Year and day-of-month are not used as features.** Combined with month they let the model recognise a specific date instead of learning a pattern.
- **Tuning uses grouped cross-validation** (by day) for the same reason.

## Run it

```
pip install -r ../requirements.txt
jupyter notebook Bike_Sharing_Demand_Prediction.ipynb
```

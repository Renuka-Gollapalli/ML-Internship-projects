# Human Activity Recognition from Wearable Sensors

Recognise what a person is doing (walking, shuffling, stairs up, stairs down, standing, sitting, lying) from two body-worn accelerometers.

## Data

- [HAR70+](https://archive.ics.uci.edu/dataset/780/har70), UCI, CC BY 4.0
- 18 older adults (age 70 to 95), accelerometers on the lower back and right thigh, 50 Hz, 2,259,597 readings
- Download the 18 files `501.csv` to `518.csv` and place them in the `data/` folder
- The files are about 150 MB in total, so they are not stored in this repo

## Approach

1. **Explore:** class imbalance, readings per subject, average and spread of each axis per activity, waveforms of walking vs sitting, acceleration magnitude
2. **Windowing:** cut each person's signal into 2-second windows with 50% overlap (100 readings per window)
3. **Features:** mean, standard deviation, max and min of each of the 6 axes, giving 24 features per window
4. **Model:** Random Forest with balanced class weights
5. **Evaluate:** 4 subjects held out for testing, plus 6-fold cross-validation grouped by subject
6. **Tune:** randomized search scored on macro F1

## Results

- 45,167 windows from 18 people
- 6-fold cross-validation grouped by subject: **accuracy 91.2%, macro F1 0.63**
- Held-out test on 4 subjects: accuracy 89.8%, macro F1 0.57. Tuning raised macro F1 to 0.59 with no gain in accuracy.
- Strong: lying (recall 0.98), standing (0.94), sitting (0.93), walking (0.92)
- Weak: shuffling (recall 0.30), stairs down (0.17), stairs up (0.07). Most stairs windows are predicted as walking.
- An earlier version tested on a single person showed 97% accuracy. Across all 18 people the honest figure is about 91%.

## Decisions worth knowing

- **Split by person, not by row.** Neighbouring windows overlap by half, so a random split would put near-copies of the same data in train and test. Holding out whole people also answers the real question: will it work on someone new?
- **Macro F1 next to accuracy.** About 48% of the data is walking, so accuracy alone looks good even if rare activities fail. Stairs make up well under 1% of readings.
- **Grouped cross-validation for tuning**, using subjects as groups, for the same leakage reason.
- **Rare classes are the weak spot.** Shuffling and stairs are hard with simple summary features. Ideas to try next: frequency-domain features, longer windows, and sequence models.

## Run it

```
pip install -r ../requirements.txt
jupyter notebook Human_Activity_Recognition.ipynb
```

Cross-validation and tuning take a few minutes.

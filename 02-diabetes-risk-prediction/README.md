# Early Stage Diabetes Risk Prediction

Predict whether a person is likely to have diabetes from age, gender and 14 yes/no symptoms.

## Data

- [Early Stage Diabetes Risk Prediction](https://archive.ics.uci.edu/dataset/529/early+stage+diabetes+risk+prediction+dataset), UCI, CC BY 4.0
- 520 patients from Sylhet Diabetes Hospital, Bangladesh, collected through questionnaires
- Download `diabetes_data_upload.csv` and place it in the `data/` folder

## Approach

1. **Explore:** class balance, value counts of every symptom, correlation of each symptom with the diagnosis, age distribution, number of symptoms per patient
2. **Clean:** check for exact duplicate records and remove them
3. **Prepare:** convert Yes/No and Male/Female to 1/0, stratified 80/20 split
4. **Model:** Random Forest Classifier, evaluated with a classification report, confusion matrix, 5-fold cross-validation and feature importance
5. **Tune:** grid search over trees, depth and class weights, scored with F2 so that missing a diabetic patient costs more than a false alarm

## Results

- **269 of the 520 records are exact duplicates**, leaving 251 unique patients (173 diabetic, 78 healthy)
- Held-out test split (51 patients): accuracy 92.2%, recall 0.89 for diabetic patients
- 5-fold cross-validation: accuracy 0.936 +/- 0.019, recall 0.954 +/- 0.029, F1 0.954 +/- 0.013
- Tuned for recall (F2): recall on the test split rises to 0.94, but accuracy drops to 86% because more healthy people are flagged
- An earlier version that kept the duplicates showed 98% accuracy. That number was inflated because the same patients appeared in train and test.

## Decisions worth knowing

- **Duplicates are removed before the split.** If the same record is in train and test, the model is tested on something it has already seen.
- **Cross-validation is reported next to the single test split.** After de-duplication the test set is small, so one split alone can mislead.
- **F2 instead of plain recall for tuning.** Optimising only recall rewards a model that labels everyone as diabetic.

## Limits

This is a small, questionnaire-based dataset from one hospital. Results from here do not carry over to other populations, and the model is not a medical tool.

## Run it

```
pip install -r ../requirements.txt
jupyter notebook Diabetes_Prediction.ipynb
```

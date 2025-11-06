# Model inventory

All figures below come from the files in this repository. No accuracy metric is
recorded because no evaluation run is checked in.

## Crop recommendation

| Item | Value |
| --- | --- |
| Artifact | `model.pkl` |
| Scaler | `minmaxscaler.pkl` |
| Algorithm | Random Forest (scikit-learn) |
| Features | N, P, K, temperature, humidity, pH, rainfall |
| Classes | 22 |
| Training data | `Crop_recommendation.csv` (2,200 rows) |

Class order is defined by `CROP_DICT` in `app.py`.

## Leaf disease

| Item | Value |
| --- | --- |
| Artifact | `models/plant_disease_model.tflite` |
| Input | 224×224 RGB, values divided by 255 |
| Classes | 39 |
| Metadata | `disease_data.json` |

Each entry in `disease_data.json` holds the display name and treatment notes for
one class.

## Produce identification

| Item | Value |
| --- | --- |
| Artifact | `models/fruit_vegetable_classifier_quantized.tflite` |
| Input | 224×224 RGB, values divided by 255 |
| Classes | 41 |
| Labels | `labels.txt` |
| Nutrition data | `fruit_data.json` |

`labels.txt` order must match the model output order. `app.py` keeps a small
alias table for spelling differences between labels and nutrition entries.

## Freshness

| Item | Value |
| --- | --- |
| Artifact | `models/fruit_vegetable_healty_classifier.tflite` |
| Input | 224×224 RGB, values divided by 255 |
| Classes | 28 |
| Labels | `class_names.txt` |

Class names use the `<produce>__<status>` form, which the health page splits into
a produce name and a status.

# AgrowAssist Web

A browser front end for the AgrowAssist models, so the project can be used
without installing the Android app.

## What is in the repository

- `app.py` — Flask application that loads the trained models and serves the pages
- `templates/` — Jinja pages for the home screen, the four tools, analytics, and the class reference
- `static/` — stylesheet, client script, and crop imagery
- `models/` — TFLite classifiers
- `model.pkl`, `minmaxscaler.pkl` — crop recommender and its scaler
- `Crop Classification With Recommendation System.ipynb` — training notebook
- `Crop_recommendation.csv` — training data

## Setup

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate

pip install -r requirements.txt
python app.py
```

Then open <http://127.0.0.1:5000>.

The first start loads every model into memory, so it takes a few seconds.

## Routes

| Route | Purpose |
| --- | --- |
| `/` | Landing page |
| `/recommendation` | Crop recommendation from seven soil and climate readings |
| `/disease` | Leaf disease diagnosis |
| `/identify` | Produce identification |
| `/health` | Freshness check |
| `/analytics` | Embedded Power BI crop dashboard |
| `/information` | Supported crops, diseases, produce, and health classes |

## Models

| Model | File | Classes |
| --- | --- | --- |
| Crop recommendation | `model.pkl` | 22 |
| Leaf disease | `models/plant_disease_model.tflite` | 39 |
| Produce identification | `models/fruit_vegetable_classifier_quantized.tflite` | 41 |
| Freshness | `models/fruit_vegetable_healty_classifier.tflite` | 28 |

The crop model expects the same MinMax scaling used during training. The vision
models take a 224×224 RGB image normalised to `[0, 1]`.

## Notes

- Large binaries are tracked with Git LFS.
- Uploaded images are written to `static/uploads/`, which is ignored by Git.
- The project has not been benchmarked on a held-out set in this repository.

## License

No license file has been added. All rights reserved by the author.

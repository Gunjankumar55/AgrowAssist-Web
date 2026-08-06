# AgrowAssist

AI-assisted crop and produce tools, packaged as a Flask website so the models
can be used without installing the Android app.

Live demo: <https://gkc55-agrowassist.hf.space>  
Android app: <https://github.com/Gunjankumar55/Agroassist----Smart-Agricultural-Assistance-System-App>

## What it does

| Module | Input | Output | Classes |
| --- | --- | --- | --- |
| Crop recommendation | 7 soil and climate readings | Best-fit crop | 22 |
| Disease detection | Leaf photo | Diagnosis and treatment notes | 39 |
| Produce identification | Photo | Name and nutrition data | 41 |
| Freshness check | Photo | Fresh or spoiled | 28 |
| Analytics | — | Embedded Power BI dashboard | — |
| AgroBot | Natural-language question | Agriculture-scoped answer | — |

## How it works

### Crop recommendation

```
N, P, K, temperature, humidity, pH, rainfall
        |
   MinMaxScaler
        |
 RandomForestClassifier
        |
   predicted crop
```

The model was trained on MinMax-scaled features, so inference applies the same
scaler. Skipping it would silently shift every prediction.

### Vision models

All three image models share one pipeline:

```
upload -> RGB -> resize 224x224 -> divide by 255 -> TFLite -> argmax + confidence
```

Produce identification and freshness checks apply a 50% confidence floor. Below
it the page reports that no confident prediction was possible instead of showing
a misleading label.

### AgroBot

Gemini with an agriculture-only system instruction. Chat history is kept in
`localStorage` and the last ten turns are sent as context. If
`GEMINI_API_KEY` is unset the endpoint returns 503 and the widget stays closed.

## Stack

- Backend: Flask, Gunicorn, Python 3.9
- ML: scikit-learn Random Forest, three TensorFlow Lite CNNs
- AI: Google Gemini
- Frontend: HTML, CSS, vanilla JS, Bootstrap 5
- Models and images: Git LFS

## Run locally

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate

pip install -r requirements.txt
python app.py
```

Open <http://127.0.0.1:5000>. The first request is slow because all four models
are loaded at start-up.

## Run with Docker

```bash
docker build -t agrowassist .
docker run -p 7860:7860 -e GEMINI_API_KEY=your_key agrowassist
```

The container runs Gunicorn on port 7860 with a single worker, which is what the
Hugging Face Space uses.

## Configuration

| Variable | Purpose |
| --- | --- |
| `GEMINI_API_KEY` | Enables AgroBot; without it the chat endpoint returns 503 |

## Repository layout

```
app.py                  Flask routes, model loading, prediction helpers
templates/              Jinja pages
static/css/style.css    Design system and page styles
static/js/main.js       Small UI helpers
static/js/chatbot.js    AgroBot widget
models/                 TFLite classifiers
docs/models.md          Model and class inventory
```

## Tests

There is no automated test suite. The routes were checked manually against
sample images and the crop form against the training notebook's scaler.

## Limitations

- Trained on a single dataset per task; no held-out evaluation is included.
- The crop model covers 22 classes and the vision models cover 39, 41, and 28.
- Uploads are stored under `static/uploads/` and are not cleaned automatically.
- AgroBot depends on an external API and is unavailable without a key.

## Author

Gunjankumar Choudhari — [GitHub](https://github.com/Gunjankumar55)

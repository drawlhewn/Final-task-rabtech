Real-Time ML Inference REST API & Capstone

A production-style machine-learning inference service built with **FastAPI**, **scikit-learn**, **Pydantic**, **pytest**, and **Docker**.

The project packages a trained Logistic Regression "champion" model for the Iris classification problem and exposes it through a JSON REST API.

## Architecture

```text
JSON Client
    |
    v
POST /predict
    |
    v
Pydantic Request Validation
    |
    v
FastAPI Service
    |
    v
Persisted scikit-learn Pipeline
(StandardScaler + LogisticRegression)
    |
    +----> Predicted class
    |
    +----> Prediction probabilities
```

## Project structure

```text
real-time-ml-inference-capstone/
├── app/
│   ├── __init__.py
│   └── main.py
├── model/
│   └── iris_classifier.joblib
├── tests/
│   └── test_api.py
├── Dockerfile
├── .dockerignore
├── .gitignore
├── requirements.txt
├── train.py
└── README.md
```

## 1. Run locally

Python 3.13 is used for the pinned environment.

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Linux/macOS:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the API:

```bash
uvicorn app.main:app --reload
```

Open:

```text
http://127.0.0.1:8000/docs
```

FastAPI automatically provides interactive Swagger documentation.

## 2. API endpoints

### GET /

Basic service information.

### GET /health

Returns model/service health.

Example response:

```json
{
  "status": "healthy",
  "model_loaded": true
}
```

### POST /predict

Accepts a JSON payload:

```json
{
  "sepal_length": 5.1,
  "sepal_width": 3.5,
  "petal_length": 1.4,
  "petal_width": 0.2
}
```

Example response:

```json
{
  "predicted_class": "0",
  "prediction_probabilities": {
    "0": 0.981,
    "1": 0.019,
    "2": 0.0
  }
}
```

The probability values depend on the model and input.

## 3. Run tests

```bash
python -m pytest -q
```

The test suite validates:

- root endpoint
- health endpoint
- prediction response schema
- probability output
- invalid input handling
- HTTP response codes

## 4. Re-train the champion model

The included training script recreates the persisted model:

```bash
python train.py
```

The script uses a stratified train/test split, StandardScaler and LogisticRegression, then saves the trained pipeline to:

```text
model/iris_classifier.joblib
```

## 5. Docker

Build:

```bash
docker build -t real-time-ml-api .
```

Run:

```bash
docker run --rm -p 8000:8000 real-time-ml-api
```

Then open:

```text
http://127.0.0.1:8000/docs
```

## 6. Example curl request

```bash
curl -X POST "http://127.0.0.1:8000/predict" \
  -H "Content-Type: application/json" \
  -d "{\"sepal_length\":5.1,\"sepal_width\":3.5,\"petal_length\":1.4,\"petal_width\":0.2}"
```

## 7. Production considerations

For a real deployment, the following should be added as the project evolves:

- authentication/API keys
- HTTPS/TLS at the deployment layer
- structured logging and monitoring
- rate limiting
- model/version metadata
- CI/CD pipeline
- model drift monitoring
- secure secret management
- a non-pickle model format where appropriate

## Task checklist

- [x] FastAPI `/predict` JSON endpoint
- [x] Prediction probabilities in response
- [x] Persisted trained model
- [x] Dockerfile with pinned dependencies
- [x] Unit tests for schema and response codes
- [x] End-to-end architecture documentation
- [x] Ready for a public GitHub repository# Final-task-rabtech
Real-Time ML Inference REST API &amp; Capstone

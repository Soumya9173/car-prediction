# Car Price Prediction

This project predicts the selling price of a used car from a small set of vehicle features. It is built around a FastAPI backend for prediction logic and a Streamlit frontend for the user interface.

## Live Demo

- Streamlit app: https://car-prediction-mfs5ss3yc2jcxrglwngmnm.streamlit.app/
- FastAPI backend: https://car-prediction-wm0q.onrender.com

The Streamlit app sends prediction requests to the FastAPI backend.

## Project Overview

The project has two main parts:

- FastAPI backend that loads a trained Random Forest model and exposes a `/predict` endpoint.
- Streamlit app that collects user input and calls the backend API to display the predicted price.

## Tech Stack

- FastAPI
- Uvicorn
- Streamlit
- scikit-learn
- pandas
- numpy
- joblib

## Repository Structure

- `main.py` - FastAPI application and prediction endpoint
- `model.py` - Model loading, preprocessing, and inference helpers
- `schema.py` - Pydantic request and response models
- `train.py` - Model training script
- `streamlit_app.py` - Streamlit frontend
- `cardekho_data (1).csv` - Training dataset
- `random_forest_model.pkl` - Saved trained model
- `feature_columns.pkl` - Saved training feature order

## How It Works

1. The training script reads the dataset and prepares the feature matrix.
2. A Random Forest Regressor is trained on the car features.
3. The model and the one-hot encoded training columns are saved as artifacts.
4. On startup, the FastAPI app loads those artifacts.
5. The `/predict` endpoint receives a JSON payload, preprocesses it, and returns the predicted selling price.
6. The Streamlit app collects user inputs and displays the result from the API.

## API Endpoint

### `GET /`

Health check route.

Example response:

```json
{
  "success": true,
  "message": "this is test route"
}
```

### `POST /predict`

Accepts the following fields:

- `Car_Name`
- `Year`
- `Present_Price`
- `Kms_Driven`
- `Fuel_Type`
- `Seller_Type`
- `Transmission`
- `Owner`

Example request:

```json
{
  "Car_Name": "swift",
  "Year": 2014,
  "Present_Price": 5.59,
  "Kms_Driven": 40000,
  "Fuel_Type": "Petrol",
  "Seller_Type": "Dealer",
  "Transmission": "Manual",
  "Owner": 0
}
```

Example response:

```json
{
  "prediction_price": 3.42
}
```

You can also explore the API interactively at `/docs` when the FastAPI server is running.

## Local Setup

### 1. Create and activate a virtual environment

On Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the FastAPI backend

```bash
uvicorn main:app --reload
```

The API will be available at `http://127.0.0.1:8000`.

### 4. Run the Streamlit app

```bash
streamlit run streamlit_app.py
```

## Training the Model

If you want to retrain the model from scratch, run:

```bash
python train.py
```

This generates the model and feature column artifacts used by the backend.

## Deployment Notes

### FastAPI on Render

The backend is deployed on Render and serves the prediction API. Make sure the service starts with something like:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

### Streamlit on Streamlit Community Cloud

The frontend is deployed on Streamlit and points to the Render API endpoint. If you deploy it yourself, update `API_URL` in `streamlit_app.py` to your backend URL.

## Important Files

- `main.py` handles the FastAPI routes.
- `model.py` keeps preprocessing and inference consistent with training.
- `schema.py` validates incoming requests.
- `streamlit_app.py` provides the UI and calls the backend.

## Notes

- The backend uses CORS so the Streamlit frontend can call it from a browser.
- The request values must match the expected enum values in `schema.py`.
- The saved feature columns are required so inference uses the same encoding as training.

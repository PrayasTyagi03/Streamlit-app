# Streamlit-app
# 🤖 Watsonx AutoAI Streamlit App

This project demonstrates how to deploy an IBM Watsonx AutoAI model and consume it using a Streamlit app. The model is trained using Watsonx Studio and deployed via an online endpoint. The Streamlit frontend collects user input, sends it to the model, and displays real-time predictions.

## Features
- AutoAI model trained on IBM Watsonx Studio
- REST API-based deployment
- Streamlit frontend for predictions

## Setup
1. Clone the repository
2. Add your API Key and Deployment URL to Streamlit Secrets
3. Run the app with `streamlit run streamlit_app.py`

## Secrets
Configure secrets like this in `.streamlit/secrets.toml`:

```toml
DEPLOYMENT_URL = "https://au-syd.ml.cloud.ibm.com/ml/v4/deployments/2be32b0b-25bc-440f-9dff-dcfec35506ed/predictions?version=2021-05-01"
```
import streamlit as st
import requests

# Load API credentials from Streamlit secrets
API_KEY = st.secrets["API_KEY"]
DEPLOYMENT_URL = st.secrets["DEPLOYMENT_URL"]

def get_token(api_key):
    response = requests.post(
        "https://iam.cloud.ibm.com/identity/token",
        data={"apikey": api_key, "grant_type": "urn:ibm:params:oauth:grant-type:apikey"},
        headers={"Content-Type": "application/x-www-form-urlencoded"}
    )
    return response.json()["access_token"]

def predict(input_data):
    token = get_token(API_KEY)
    headers = {
        "Authorization": f"Bearer {token}",
        "Content-Type": "application/json"
    }
    payload = {
        "input_data": [{
            "fields": ["feature1", "feature2"],  # Replace these with actual model features
            "values": [input_data]
        }]
    }
    response = requests.post(DEPLOYMENT_URL, headers=headers, json=payload)
    response.raise_for_status()
    return response.json()["predictions"][0]["values"][0][0]

# Streamlit App UI
st.title("Watsonx AutoAI Predictor")

# Replace these inputs with your model's real input features
f1 = st.number_input("Enter value for Feature 1", value=0.0)
f2 = st.number_input("Enter value for Feature 2", value=0.0)

if st.button("Predict"):
    try:
        result = predict([f1, f2])
        st.success(f"Prediction: {result}")
    except Exception as e:
        st.error(f"Error during prediction: {e}")


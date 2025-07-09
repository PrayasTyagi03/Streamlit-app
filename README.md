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
API_KEY = "AMbaW8SPIr5ymevcTnUeAA20wN1uSSr4utMSh6Qi4sBq"
DEPLOYMENT_URL = "https://au-syd.ml.cloud.ibm.com/ml/v4/deployments/2be32b0b-25bc-440f-9dff-dcfec35506ed/predictions?version=2021-05-01"
```

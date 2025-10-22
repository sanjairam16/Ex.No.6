# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Date: 21/10/25
# Register no.25009774
# Aim: Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools

#AI Tools Required: CHATGPT , MICROSOFT COPILOT 

# Explanation:
Experiment the persona pattern as a programmer for any specific applications related with your interesting area. 
Generate the outoput using more than one AI tool and based on the code generation analyse and discussing that. 
# PROCEDURE
  ** DESCRIPTION:
    CHAT GPT AND COPILOT IS ASKED TO WRITE API KEYS FOR A WEATHER FOR IMAGINARY WEBSITE CALLED CLIMATEPREDICTOR 
    THE AI TOOLS USE FAST API LIBRARY OF PYTHON DO WORK 
# CHAT GPT PROMPTS :
-> You are backend programmer who codes API Keys using FASTAPI in python your website name is climatepredictor and you are creating several endpoints of your own show the code and corresponding outputs 
# ChatGPT's code :
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.security.api_key import APIKeyHeader
from typing import Dict
import random

app = FastAPI(title="ClimatePredictor API", version="1.0")

# === Simulated API Key ===
API_KEY = "12345-CLIMATE-SECRET"
api_key_header = APIKeyHeader(name="X-API-Key", auto_error=False)

# === Dependency to verify API Key ===
async def verify_api_key(api_key: str = Depends(api_key_header)):
    if api_key != API_KEY:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid or missing API Key",
        )
    return api_key

# === Root Endpoint ===
@app.get("/", tags=["Root"])
async def home():
    return {"message": "Welcome to ClimatePredictor API!"}

# === 1. Predict temperature endpoint ===
@app.get("/predict/temperature", tags=["Prediction"])
async def predict_temperature(city: str, api_key: str = Depends(verify_api_key)):
    base_temp = random.uniform(20.0, 35.0)
    variation = random.uniform(-2, 2)
    predicted = round(base_temp + variation, 2)
    return {
        "city": city,
        "predicted_temperature": f"{predicted}°C",
        "confidence": f"{random.randint(85, 98)}%"
    }

# === 2. Predict rainfall endpoint ===
@app.get("/predict/rainfall", tags=["Prediction"])
async def predict_rainfall(region: str, api_key: str = Depends(verify_api_key)):
    rainfall_mm = round(random.uniform(50, 200), 1)
    status = "Heavy" if rainfall_mm > 150 else "Moderate" if rainfall_mm > 100 else "Light"
    return {
        "region": region,
        "expected_rainfall": f"{rainfall_mm} mm",
        "intensity": status
    }

# === 3. Climate summary endpoint ===
@app.get("/summary/{country}", tags=["Analytics"])
async def climate_summary(country: str, api_key: str = Depends(verify_api_key)):
    data = {
        "India": {"avg_temp": 29.5, "avg_rainfall": 120, "trend": "Warming"},
        "USA": {"avg_temp": 23.8, "avg_rainfall": 90, "trend": "Stable"},
        "Brazil": {"avg_temp": 27.1, "avg_rainfall": 180, "trend": "Humidifying"},
    }
    summary = data.get(country, {"avg_temp": "N/A", "avg_rainfall": "N/A", "trend": "Unknown"})
    return {
        "country": country,
        "average_temperature": f"{summary['avg_temp']}°C",
        "average_rainfall": f"{summary['avg_rainfall']} mm",
        "climate_trend": summary["trend"]
    }

# === 4. Health check endpoint ===
@app.get("/health", tags=["System"])
async def health_check():
    return {"status": "API is running smoothly."}
# code output examples 
GET:
http://127.0.0.1:8000/predict/temperature?city=Delhi

Header:
X-API-Key: 12345-CLIMATE-SECRET

Output:

{
  "city": "Delhi",
  "predicted_temperature": "33.47°C",
  "confidence": "91%"
}
# Copilot prompt :
You are backend programmer who codes API Keys using FASTAPI in python your website name is climatepredictor and you are creating several endpoints of your own show the code and corresponding outputs 
# Copilot OutPut:
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_home_no_auth_needed():
    res = client.get("/")
    assert res.status_code == 200
    assert "Welcome to" in res.json()["message"]

def test_current_requires_api_key():
    res = client.get("/climate/current?city=Chennai")
    assert res.status_code == 401

def test_current_with_api_key():
    headers = {"X-API-Key": "mysecretapikey123"}
    res = client.get("/climate/current?city=Chennai", headers=headers)
    # Will fail unless .env is set to mysecretapikey123; example expectation:
    assert res.status_code in (200, 401)

# Conclusion:
-> Both ChatGPT and Copilot gave similar output responses .
-> CHATGPT instant produced the desired output readily available to its user shows it's training on directly satsifing cilent requirement 
-> Copilot enquired about virtual enviroment that is to be used and focused on developer side assiantance as it is usally does in it's Microsoft ecosystem
-> Both the AI models responsed perfectly and reached the same goal, the path both focus varies on basis of their respective model training puspose and proccess.


# Result: The corresponding Prompt is executed successfully.

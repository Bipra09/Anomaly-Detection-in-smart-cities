# Anomaly-Detection-in-smart-cities
# Ollama Jupyter Notebook Setup

This Jupyter Notebook provides a step-by-step guide to setting up and running Ollama, a tool for managing and interacting with large language models (LLMs), in a Jupyter environment. The project demonstrates how to use Ollama for **real-time anomaly detection** in data streams.

## Features

- Installs Ollama and its dependencies.
- Runs Ollama as a background service.
- Downloads and manages LLM models (e.g., `llama3.2`).
- Demonstrates **real-time anomaly detection** using Ollama's inference capabilities.

## Prerequisites

- Ubuntu-based system (tested on Ubuntu 22.04).
- `curl` and `sudo` permissions.
- NVIDIA GPU (recommended for optimal performance).

## Usage

1. **Install Dependencies**:  
   The notebook installs `pciutils` and Ollama automatically.

2. **Run Ollama Service**:  
   The notebook starts Ollama in the background using a Python subprocess.

3. **Download Models**:  
   Use `ollama pull <model_name>` to download models (e.g., `llama3.2`). Replace `<model_name>` with the desired model from the [Ollama library](https://ollama.com/library).

4. **Anomaly Detection**:  
   The notebook includes an example of real-time anomaly detection by analyzing data streams with Ollama's LLM inference.

## Example

```python
!ollama pull llama3.2  # Downloads the llama3.2 model

# Example: Real-time anomaly detection
import requests

def detect_anomalies(data):
    response = requests.post(
        "http://127.0.0.1:11434/api/generate",
        json={
            "model": "llama3.2",
            "prompt": f"Analyze this data for anomalies: {data}. Respond with 'Normal' or 'Anomaly'."
        }
    )
    return response.json()["response"]

# Simulate data stream
data_stream = [10, 12, 11, 15, 100]  # 100 is an anomaly
for data in data_stream:
    result = detect_anomalies(data)
    print(f"Data: {data} | Status: {result}")

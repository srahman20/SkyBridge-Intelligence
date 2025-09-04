# AI Services Configuration

This document outlines how AI capabilities were integrated into the SkyBridge Intelligence project using Azure OpenAI and other AI-related services.

---

## Objectives

- Enable natural language processing via OpenAI models
- Secure access via Azure Identity
- Expose AI inference via a Flask-based API
- Log all AI interactions for audit and analysis

---

## Azure OpenAI Setup

### 1. Create Azure OpenAI Resource

- Go to [Azure Portal](https://portal.azure.com)
- Search **"Azure OpenAI"** in the marketplace
- Click **Create** → Fill in:
  - **Resource Name**: `skybridge-openai`
  - **Region**: East US (or any supported)
  - **Pricing Tier**: Standard (default)

> Note: You must apply for access to Azure OpenAI before use. Approval may take time.

---

### 2. Deploy GPT Model

- Navigate to your `skybridge-openai` resource
- Go to **Deployments**
- Click **Create** and choose:
  - **Model**: `gpt-35-turbo`
  - **Deployment Name**: `skybridge-gpt`
  - Accept default parameters

---

## API Access

After deployment:

- Navigate to `skybridge-openai` > **Keys and Endpoint**
- Save:
  - **Endpoint**: `https://skybridge-openai.openai.azure.com/`
  - **Key 1**

 ---

 ## Backend Integration

### Python Flask App

Install dependencies:

```bash
pip install flask openai azure-identity
```

## Sample app.py
```
from flask import Flask, request, jsonify
import openai
import os

app = Flask(__name__)
openai.api_type = "azure"
openai.api_base = "https://skybridge-openai.openai.azure.com/"
openai.api_version = "2023-05-15"
openai.api_key = os.getenv("AZURE_OPENAI_KEY")

@app.route("/ask", methods=["POST"])
def ask():
    question = request.json.get("question", "")
    response = openai.ChatCompletion.create(
        engine="skybridge-gpt",
        messages=[{"role": "user", "content": question}]
    )
    answer = response['choices'][0]['message']['content']
    return jsonify({"answer": answer})

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

Replace AZURE_OPENAI_KEY with a secure environment variable.

## Security Considerations

API key should never be hardcoded

Use environment variables or Azure Key Vault

Add rate limiting and request logging to protect the service

## Logs & Monitoring

Use Flask logging to write logs to file

Optionally push logs to Azure Log Analytics via custom script or agent

### Track:

  IP Address

  Prompt sent

  Timestamp

  Response from AI

## Example Request

```
Introduce caching layer (e.g., Redis) for frequent queries
curl -X POST http://<your-server-ip>/ask \
  -H "Content-Type: application/json" \
  -d '{"question": "What is the purpose of network subnets?"}'
```
## Future Enhancements

Add role-based access control (RBAC)

Add prompt templates for internal use cases (e.g. SOP generator, policy drafting)

Introduce caching layer (e.g., Redis) for frequent queries


# Azure AI Integration – SkyBridge Intelligence

## Overview

This document outlines how Azure AI services were conceptually integrated into the SkyBridge Intelligence lab to simulate document analysis, translation, and sentiment tagging. These services represent real-world use cases where intelligent automation augments traditional IT workflows.

---

## Services Used

| Service              | Purpose                                           |
|----------------------|---------------------------------------------------|
| **Form Recognizer / OCR** | Extracts text from uploaded documents (e.g., PDFs, scanned forms) |
| **Text Analytics**    | Analyzes sentiment, key phrases, and language     |
| **Translator**        | Automatically translates documents into English   |

---

## Architecture Diagram (Simplified)

       Windows 11 Client VM
               |
       Uploads File (PDF)
               |
     -----------------------
     |     File Share      |
     | Windows Server 2022 |
     -----------------------
               |
    Script / Scheduled Logic App
               |
      Calls Azure AI Services
               |
    Receives OCR / Translation / Tags
               |
    Logs Result / Stores Metadata
    

---

## Sample Workflow

1. User places a document (e.g., a customer feedback form) in a monitored folder on the **Windows Server** file share.
2. A script (Python or PowerShell) or Logic App picks up the document and:
   - Sends it to **Form Recognizer** for text extraction
   - Passes the extracted text to **Text Analytics** for language detection and sentiment
   - Sends it to **Translator API** if not in English
3. Results are returned and stored in:
   - A log file
   - A local CSV
   - Or optionally, uploaded to Azure Blob or stored in a database

---

## Sample Python Script (Conceptual)

```python
import requests

ocr_url = "https://<region>.api.cognitive.microsoft.com/formrecognizer/documentModels/prebuilt-read:analyze?api-version=2023-07-31"
translator_url = "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=en"

headers = {
    "Ocp-Apim-Subscription-Key": "<your_key>",
    "Content-Type": "application/pdf"
}

with open("input.pdf", "rb") as f:
    response = requests.post(ocr_url, headers=headers, data=f)
```

## Azure Setup Summary

| **Resource Type**        | **Name**                | **Notes**                                |
|--------------------------|--------------------------|-------------------------------------------|
| Cognitive Services       | `skybridge-ai-service`   | Multi-service resource                    |
| Resource Group           | `AI-Networking-Lab`      | All resources grouped here                |
| Pricing Tier             | Free (F0) or Standard (S0)| Free tier allows limited usage           |
| Region                   | East US / West Europe    | Match region with other services          |
| Authentication           | Subscription key or AAD  | Key management via portal    

# Key Learnings

 Azure AI APIs are stateless and easy to integrate via REST or SDKs

 OCR, translation, and sentiment workflows can run locally or in the cloud

 This modular AI layer is ideal for future use in ITSM, knowledge management, or ticket tagging

# Ideas for Expansion

 Automate with Azure Logic Apps instead of local scripts

 Store all output in Azure Blob Storage or a central Log Analytics Workspace

 Trigger alerts for negative sentiment or keyword detection (e.g., “urgent,” “outage”)

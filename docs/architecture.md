# System Architecture – SkyBridge Intelligence

## Objective

The SkyBridge Intelligence lab emulates a modern enterprise IT environment that integrates:

- **On-premise infrastructure** (Windows Server via UTM VM)
- **Cloud services** (Azure Active Directory, Intune, Virtual Networks, AI APIs)
- **Networking components** (Hybrid identity, secure access policies, endpoint control)
- **AI automation** (Azure Cognitive Services like OCR, Translator, and Sentiment Analysis)

This hybrid model reflects the shift toward intelligent, cloud-augmented IT environments used by organizations of all sizes.

---

## High-Level Topology

            +----------------------------+
            |        Azure Cloud         |
            |                            |
            |  +----------------------+  |
            |  | Azure AD / Intune    |  |
            |  +----------------------+  |
            |  | Azure AI Services    |  |
            |  +----------------------+  |
            |  | Azure Virtual Network|  |
            |  +----------+-----------+  |
            |             |              |
            +-------------|--------------+
                          |
     VPN or Hybrid        |
  (simulated connection) 
  
                          |
           +--------------+--------------+
           |      UTM Host on macOS       |
           |                              |
           |  +------------------------+  |
           |  | Windows Server 2022    |  |
           |  | (AD, File Share,       |  |
           |  |  Azure AD Connect)     |  |
           |  +------------------------+  |
           |  +------------------------+  |
           |  | Windows 11 Client VM   |  |
           |  | (Hybrid Domain Join)   |  |
           |  +------------------------+  |
           +------------------------------+

---

## Components and Roles

| Component               | Role                                                                 |
|------------------------|----------------------------------------------------------------------|
| **Windows Server 2022**| Acts as a local domain controller, file server, and runs AD Connect. |
| **Windows 11 VM**       | Simulates an end-user machine joined to hybrid Azure AD.            |
| **Azure Active Directory** | Provides identity management, SSO, MFA, conditional access.         |
| **Azure Intune**        | Deploys policies, apps, and compliance checks to devices.           |
| **Azure Virtual Network** | Simulates enterprise network segmentation and firewalling.         |
| **Azure AI Services**   | Adds intelligent capabilities like OCR, language detection, etc.     |

---

## User and Device Flow

1. User logs into a **Windows 11** machine joined to Azure AD.
2. Device is **provisioned via Intune** and receives policies.
3. User accesses a **shared folder** on the on-prem **Windows Server**.
4. Files in the folder are picked up by a **Python script or Logic App** to:
   - Extract text (OCR)
   - Detect language or sentiment
   - Store metadata or forward results to a simulated database/log
5. Results are logged for internal use and future expansion (Power BI, ticketing system, etc.)

---

## Key Integrations

- **Azure AD Connect**: Syncs on-prem users to Azure AD
- **Intune Auto-enrollment**: Applies compliance and security policies to Windows 11 VM
- **Azure AI Services (REST API)**:
  - *Form Recognizer / Computer Vision* – Extracts text from scanned documents
  - *Translator API* – Translates non-English submissions
  - *Text Analytics* – Tags sentiment for feedback or support tickets

---

## Optional Future Add-ons

- **Azure Sentinel** for SIEM/log analysis
- **Logic Apps** for automated alerting or workflows
- **Azure Bastion** for secure browser-based VM access
- **Power BI** for document and ticket trend reporting

---

## Goals Achieved

Demonstrated hybrid cloud infrastructure setup  
Simulated enterprise-grade identity and access management  
Integrated AI automation for operational intelligence  
Created portable and reproducible environment using UTM VMs


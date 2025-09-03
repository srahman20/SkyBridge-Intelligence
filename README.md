# SkyBridge Intelligence

## Overview

**SkyBridge Intelligence** is a hybrid infrastructure project designed to simulate a modern IT environment that combines on-prem servers, Azure cloud services, networking, and AI integration. The lab replicates real-world enterprise scenarios where organizations leverage cloud and local resources for efficient and intelligent operations.

This documentation outlines how to set up and simulate the components of the SkyBridge Intelligence lab using virtual machines and Azure services — with a strong focus on IT automation, centralized device management, and AI-driven insights.

---

## Components

- **Windows Server 2022** (on UTM): Configured as an on-prem file server and hybrid domain controller using Azure AD Connect.
- **Azure Active Directory (AAD)**: Manages cloud identities, authentication, and secure access.
- **Azure Virtual Network (VNet)**: Simulates secure, segmented network environments.
- **Azure AI Services**: Used to add OCR, translation, or sentiment analysis capabilities to internal document and ticketing flows.
- **Windows 11 VM**: Used to test hybrid domain join, access policies, and endpoint management.
- **Intune Endpoint Management**: Demonstrates remote device provisioning and policy enforcement.

---

## Key Features

- Hybrid identity with **Azure AD Connect** synchronizing on-prem AD users to Azure.
- AI automation using **Azure AI Services** like OCR, Translator, or Sentiment Analysis.
- Secure networking using **Azure Virtual Networks** and subnet segmentation.
- Windows 11 **client onboarding** to Azure AD and Intune for device policy testing.
- Simulated **document processing pipeline** using AI to extract and tag file contents.
- Fully virtualized and testable from a macOS machine using **UTM**.

---

## Documentation

All supporting documentation is available in the `docs/` directory:

- `architecture.md` – High-level system design and component overview
- `azure-ai.md` – AI integration using Azure Cognitive Services
- `networking.md` – VNet and firewall configuration in Azure
- `server-setup.md` – Windows Server installation, file share, and AD Connect
- `virtualization.md` – UTM-based VM setup and host resource configuration
- `troubleshooting.md` – Common issues and how to fix them

---

## Lessons Learned

- Azure AI services are easy to integrate via REST API or SDKs and enhance IT workflows.
- Hybrid domain joins require careful time sync and AD Connect configuration.
- Intune greatly simplifies remote device policy deployment and compliance enforcement.
- UTM allows ARM Mac users to build complex infrastructure labs without physical servers.

---

## Status

Project documented and planned   
Can be extended in the future with additional Azure services (e.g., Sentinel, Logic Apps)

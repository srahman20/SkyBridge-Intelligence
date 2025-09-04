# Troubleshooting – SkyBridge Intelligence

This document captures known issues, workarounds, and tips encountered while setting up the SkyBridge Intelligence lab. It includes both real and anticipated problems to better simulate realistic IT workflows.

---

## Common Issues and Fixes

### 1. Azure OpenAI Resource Not Available

**Symptom:** Azure OpenAI resource is not listed in the Marketplace.

**Fix:**
- You must first apply for access to Azure OpenAI via [https://aka.ms/oai/access](https://aka.ms/oai/access).
- Approval may take a few days.
- Once approved, the resource becomes visible in your region.

---

### 2. Windows Server VM Cannot Access Internet (UTM)

**Symptom:** VM fails to download packages or resolve DNS.

**Fix:**
- Make sure the UTM network is set to **"Shared (NAT)"**
- Check that `VirtIO` drivers were installed for the network interface during Windows setup.
- Try restarting the networking service or rebooting the VM.

---

### 3. Azure AD Connect Fails to Sync Users

**Symptom:** User accounts from on-premise AD are not appearing in Azure AD.

**Fix:**
- Confirm correct credentials were entered during AD Connect setup.
- Ensure the domain suffix (e.g. `@yourdomain.local`) is verified in Azure AD.
- Run `Azure AD Connect Troubleshooter` or reconfigure sync options.

---

### 4. API Key Rejected by Azure AI Service

**Symptom:** `403 Forbidden` or `Invalid Key` error when calling Azure AI APIs.

**Fix:**
- Confirm you are using **Azure OpenAI key**, not OpenAI.com key.
- Check `api_base`, `api_version`, and `engine` in the request.
- Make sure API key is valid and not expired.
- Regenerate key from Azure Portal if needed.

---

### 5. Flask App Not Reachable from Browser

**Symptom:** Web app runs but is inaccessible from the browser.

**Fix:**
- Make sure the Flask app is running with `host="0.0.0.0"` and `port=5000`.
- Allow port 5000 through NSG or firewall settings.
- Use `curl` from the VM itself to verify service is running:  
  `curl http://127.0.0.1:5000/`

---

## Emulated Issues for Resume Depth

To simulate realistic IT experience, the following scenarios are documented as “expected challenges”:

### 6. Latency Between On-Prem and Azure

**Problem:** Perceived delay between local Windows Server and Azure API endpoints.

**Explanation:** Demonstrates how network latency can affect AI-driven automation.

**Mitigation:**
- Simulate delay with `time.sleep()` in test scripts.
- Use local caching to batch requests.

---

### 7. Cognitive Service Rate Limits

**Problem:** AI services return rate-limiting headers or temporary failures.

**Explanation:** Azure enforces throughput limits on Free (F0) tiers.

**Mitigation:**
- Implement retry logic with exponential backoff.
- Upgrade to Standard tier if testing at scale.

---

### 8. AD Connect Sync Time Lag

**Problem:** New users take time to appear in Azure AD.

**Explanation:** Azure AD Connect syncs on a schedule (every 30 mins by default).

**Mitigation:**
- Force sync using PowerShell:
```powershell
Start-ADSyncSyncCycle -PolicyType Delta
```
## Lessons Learned

Network driver setup is critical for VM internet access

Azure API permissions can silently fail if improperly configured

Debugging REST APIs often requires reviewing full request/response logs

Including simulated troubleshooting adds depth to project documentation

## Tools Used for Troubleshooting

Azure Resource Logs

Postman / curl for API testing

```ipconfig```, ```nslookup```, ```netstat``` in Windows Server

systemctl status nginx, journalctl in Ubuntu

UTM snapshot rollback for testing configurations safely

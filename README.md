# 🚨 Automated SOC Alert Triage & Response Pipeline

This project is a **Security Orchestration, Automation, and Response (SOAR)** pipeline designed to automate the initial triage and enrichment of security alerts from **Splunk**.  
It uses **n8n** to connect multiple threat intelligence and IT service management tools, reducing **Mean Time to Respond (MTTR)** from minutes to seconds.

---

## 🚀 Key Features

- **Automated Triage:** Instantly processes alerts from Splunk 24/7 without human intervention.  
- **Threat Enrichment:** Automatically gathers context from **VirusTotal** and **AbuseIPDB** on IPs, hashes, and domains.  
- **AI-Powered Analysis:** Leverages the **Gemini API** to summarize complex data, assess risk, and recommend a priority level.  
- **Seamless Integration:** Connects SIEM, Threat Intel, AI, and ITSM platforms into a single, unified workflow.  
- **Actionable Notifications:** Delivers high-signal, low-noise alerts in **Slack** with a direct link to the auto-generated **Jira** ticket.  
- **Drastic MTTR Reduction:** Frees up SOC analysts from repetitive data-gathering tasks to focus on active incident response.  

---

## 🛠️ Technology Stack & Architecture

| Component | Technology | Purpose |
|------------|-------------|----------|
| **SIEM** | Splunk | Alert generation (e.g., "Potential Malware") & initial webhook trigger. |
| **Orchestration** | n8n | Core automation engine. Receives webhooks and orchestrates API calls. |
| **Threat Intel (IoC)** | VirusTotal | Enriches file hashes, domains, and IPs to find known malicious indicators. |
| **Threat Intel (IP)** | AbuseIPDB | Checks IP reputation and gathers data on previous malicious activity. |
| **AI Analysis** | Gemini API | Summarizes collected data, determines priority, and suggests next steps. |
| **Team Communication** | Slack | Pushes real-time, human-readable summaries to the SOC channel. |
| **Incident Management** | Jira | Creates incident tickets pre-populated with enriched data and analysis. |

---

## 🔄 Workflow Diagram & Logic

The entire automation is orchestrated by **n8n**, which acts as the “glue” between all services.


### Step-by-Step Flow

1. **Alert (Splunk):**  
   A correlation search (e.g., “Malicious File Hash Detected on Host”) triggers an alert.

2. **Ingest (n8n):**  
   Splunk sends its full JSON payload to the **n8n Webhook node**.

3. **Enrich (n8n):**  
   Parallel enrichment tasks are executed:
   - **VirusTotal Node:** Sends file hash (or IP/domain) to VirusTotal API.  
   - **AbuseIPDB Node:** Sends source IP address to AbuseIPDB API.

4. **Analyze (n8n):**  
   Consolidates original alert + enrichment results → sends prompt to **Gemini API**:  
   > “Act as a Tier-2 SOC Analyst. Summarize findings and assign priority (Low, Medium, High, Critical).”

5. **Act (n8n):**  
   Final automated actions:
   - **Jira Node:** Creates new incident ticket with AI summary & raw data attached.  
   - **Slack Node:** Sends formatted alert message to `#soc-alerts` with summary & Jira link.

---

## 🖼️ Project Showcase

> 

1. **Splunk Alert Trigger**  
<img width="2458" height="1282" alt="Screenshot 2025-10-09 122028" src="https://github.com/user-attachments/assets/e31fe3cc-e181-4cab-89e8-92a4290845a7" />

2. **Slack Notification**
   <img width="1753" height="587" alt="Screenshot 2025-10-09 122216" src="https://github.com/user-attachments/assets/e2ef7170-fe04-4def-9db2-e4f43ee46131" />
  
   *Final AI-summarized message delivered to the SOC team for immediate awareness.*

4. **Jira Incident Ticket**
<img width="2466" height="1072" alt="Screenshot 2025-10-09 122129" src="https://github.com/user-attachments/assets/1e1e3f0a-1a69-40ef-8e1c-42045374521d" />
 
   *Auto-generated ticket serving as the single source of truth for the incident.*

---

## 🔧 Configuration & Setup

### Requirements

- **Splunk Instance** with correlation searches and webhook alerting enabled  
- **n8n Instance** (cloud-hosted or self-hosted, e.g., Docker)  
- **API Keys & Credentials:**
  - VirusTotal (free API key)
  - AbuseIPDB (free API key)
  - Gemini API (Google AI Studio key)
  - Jira (Cloud URL, user email, API token)
  - Slack (incoming webhook URL)

### Setup Steps

1. Import your exported **n8n workflow JSON** into your n8n instance.  
2. Configure the credentials for each service node (API keys, URLs, etc.).  
3. Set up Splunk to send webhook alerts to your n8n endpoint.  
4. Test end-to-end by triggering a Splunk alert.

---

## 📈 Future Enhancements

- **Automated Remediation:**  
  For *Critical* alerts, automatically block IPs via firewall APIs (e.g., Palo Alto, Fortinet).  
- **Analyst Feedback Loop:**  
  Listen for Jira updates. If tagged as “False Positive,” auto-update Splunk lookup tables.  
- **Additional Intel:**  
  Integrate **Shodan.io** or other intel sources for device-level context enrichment.

---



---

## 📁 Repository Structure


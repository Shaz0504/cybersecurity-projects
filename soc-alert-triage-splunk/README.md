# SOC Alert Triage Using Splunk SIEM

This project simulates a Security Operations Center (SOC) workflow using Splunk to detect brute-force login attempts, suspicious POST behavior, and account misuse across web and endpoint logs. The investigation uses log correlation, MITRE ATT&CK mapping, and IOC documentation to prioritize and respond to alerts.

---

## Objectives

- Ingest and normalize Apache and Windows logs within Splunk  
- Develop detection rules for brute-force activity, account lockouts, and abnormal POST requests  
- Create dashboards to visualize request behavior, IP geolocation, and frequency patterns  
- Map alerts to MITRE ATT&CK techniques (Initial Access, Credential Access)  
- Analyze and document indicators of compromise and remediation steps

---

## Tools and Techniques Used

- Splunk SIEM and CIM (Common Information Model)  
- Apache Web Server logs and Windows Event Logs  
- CyberChef for URL decoding and IOC inspection  
- VirusTotal for enrichment of malicious IPs and links

---

## Deliverables

- [Download Full Case Study Report (PDF)](./Splunk_SIEM_Case_Study_Shaza.pdf)  
- [Download Presentation Slides (PPT)](./Splunk_Project_Presentation.pptx)

---

## Skills Demonstrated

- Log normalization and detection engineering using Splunk  
- SOC alert triage based on severity and IOC classification  
- Visualization development for SOC dashboards  
- MITRE ATT&CK alignment and incident documentation

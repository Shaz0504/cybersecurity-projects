
# SOC Alert Triage Using Splunk

This project simulates a real-world Security Operations Center (SOC) workflow using Splunk SIEM. It focuses on alert detection, triage, and investigation involving both Apache web logs and Windows event data. The objective is to demonstrate core competencies in log analysis, detection engineering, and threat response using visualizations and rule-based detections.

## Tools and Technologies

- Splunk Enterprise (SIEM)
- Apache Web Server Logs
- Windows Event Logs (Security, System)
- MITRE ATT&CK Framework
- CyberChef, VirusTotal

## Project Objectives

- Ingest and normalize Apache and Windows logs into Splunk
- Create detection rules for brute-force login attempts, account lockouts, and suspicious POST activity
- Build dashboards to visualize log activity by country, IP, method, and response code
- Investigate anomalies and map findings to MITRE ATT&CK
- Simulate Tier 1 alert triage and documentation workflow

## Detection Logic and Findings

- Brute-force login attempts identified using high counts of failed logins (Event ID 4625) within a short time window
- Lockouts and privilege abuse detected via Event ID 4740 and account anomalies
- POST request volume spikes and rare referrer domains indicated potential automated probing
- Geo-based anomaly detection flagged excessive traffic from non-standard regions

## Visual Overview

This Splunk dashboard visualizes Apache HTTP traffic. It highlights request volume, anomalous POST behavior, unusual geographic access, and referrer domains—providing clear situational awareness for SOC analysts.

![Apache Dashboard](./Apache%20Dashboard.png)

## Outcomes

- Built correlation rules and detection dashboards within Splunk
- Triaged brute-force login attempts and abnormal POST activity
- Mapped attack patterns to MITRE ATT&CK techniques (T1110, T1499)
- Recommended actions such as MFA enforcement, IP filtering, and POST throttling

## MITRE ATT&CK Mapping

- T1110 – Brute Force
- T1190 – Exploit Public-Facing Application
- T1078 – Valid Accounts
- T1040 – Network Sniffing
- T1499 – Endpoint Denial of Service

## Summary

This project demonstrates a full SOC Tier 1 triage workflow from alert detection to investigation and reporting. It highlights practical experience using Splunk to hunt for threats, respond to alerts, and deliver findings using visual and analytical evidence.

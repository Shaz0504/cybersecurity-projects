
# SOC Alert Triage Simulation – Splunk Project

This project simulates a real-world Security Operations Center (SOC) alert triage and investigation process using Splunk. Conducted as part of a 3-day blue team challenge during a cybersecurity bootcamp, it replicates the tasks of a Tier I/II SOC analyst responding to simulated insider and external threats targeting both Windows and Apache environments.

## Objectives

- Develop detection logic and triage workflows using Splunk
- Investigate simulated brute-force, account lockouts, and DDoS attacks using log correlation
- Create dashboards, alerts, and reports for security monitoring
- Align findings with MITRE ATT&CK techniques
- Document investigation outcomes and propose mitigation strategies

## Tools and Data Sources

- Splunk Core Platform
- Apache Web Server Logs (access_combined)
- Windows Security Logs (4624, 4625, 4740, 1102)
- SPL queries
- Custom dashboards and reports

## Visual Overview

![Apache Dashboard](./Apache%20Dashboard.png)
### Apache Log Insights

### Referrer Domain Analysis

### Referrer Domain Anomaly During Attack

### HTTP Method Volume (Baseline)

### HTTP Method Volume (Attack)

### Response Code Spike Comparison

## Detection Logic (SPL Examples)

```spl
# Brute-force login attempts
index=wineventlog EventCode=4625
| stats count by Account_Name, src_ip
| where count > 15

# Excessive successful logins
index=wineventlog EventCode=4624
| stats count by Account_Name, src_ip
| where count > 25

# POST request anomaly
index=apache_logs method=POST
| stats count by src_ip
| where count > 10

# Referrer domain anomaly
index=apache_logs 
| top 10 referer_domain
```

## Findings

- **Windows Environment**:
  - 1,811 lockouts for `user_a`, 2,128 password resets (`user_k`)
  - 329 high severity alerts (6.9%) and 4435 informational events (93%)
  - Signature patterns revealed account takeovers and privilege escalations

- **Apache Server**:
  - POST volume spiked from 1.06% to 29.4% of traffic
  - High volume of referrer domains from semi-unknown sources like `tuxradar.com`
  - HTTP 404 errors increased 7x during attack period

## MITRE ATT&CK Mapping

| Technique ID | Name                        | Context                    |
|--------------|-----------------------------|-----------------------------|
| T1110        | Brute Force                 | Login failure spikes        |
| T1078        | Valid Accounts              | Suspicious logons           |
| T1499        | Endpoint Denial of Service  | POST flood from Ukraine     |
| T1566        | Phishing/Recon via domains  | Referrer domains like `tuxradar.com` |

## Recommendations

- Enforce MFA and password lockout thresholds
- Geo-block non-relevant foreign access
- Rate-limit HTTP POST endpoints
- Tune Splunk alerts to baseline deviation
- Forward alert events to incident response via ServiceNow or email integration

## Skills Demonstrated

- Splunk SPL detection engineering
- Dashboard and alert tuning
- PCAP log correlation and anomaly detection
- Report creation and executive-level documentation
- Incident simulation under MITRE framework

## Project Context

This capstone project reflects a hands-on simulation of SOC workflows using Splunk. From log parsing to triage and escalation, it mirrors real-world detection, documentation, and decision-making processes. Findings are backed by threshold analysis and dashboard-driven evidence, demonstrating both detection logic and operational maturity.


# SOC Alert Triage Simulation – Splunk Project

This project simulates a real-world Security Operations Center (SOC) alert triage and investigation process using Splunk. Conducted as part of a 3-day blue team challenge during a cybersecurity bootcamp, it replicates the tasks of a Tier I/II SOC analyst responding to simulated insider and external threats targeting both Windows and Apache environments.

## Objectives

- Develop detection logic and triage workflows using Splunk
- Investigate simulated brute-force, account lockouts, and DDoS attacks using log correlation
- Create dashboards, alerts, and reports for security monitoring
- Align findings with MITRE ATT&CK techniques
- Document investigation outcomes and propose future mitigation strategies

## Tools and Data Sources

- Splunk Core Platform
- Apache Web Server Logs (access_combined)
- Windows Security Logs (e.g., 4625, 4624, 4740)
- Splunk Add-ons for Apache and CSV
- Custom SPL queries
- Simulated threat actor activity (e.g., JobeCorp targeting VSI)

## Detection Examples

```spl
# Brute-force login attempts (Windows)
index=wineventlog EventCode=4625
| stats count by Account_Name, src_ip
| where count > 15

# Spike in successful logins (Windows)
index=wineventlog EventCode=4624
| stats count by Account_Name, src_ip
| where count > 25

# Excessive POST requests (Apache - potential DDoS)
index=apache_logs method=POST
| stats count by src_ip
| where count > 10

# Non-US IP surge (Apache)
index=apache_logs NOT src_ip="US*"
| stats count by src_ip
| where count > 200
```

## Key Alerts Created

| Alert Name               | Description                                              | Threshold Used |
|--------------------------|----------------------------------------------------------|----------------|
| Failed Login Spike       | Excessive login failures vs. baseline of 6–10/hr         | >15            |
| Successful Login Spike   | Rapid login success exceeding 14–21/hr baseline          | >25            |
| POST Request Surge       | POST requests >7/hr observed in baseline                 | >10            |
| Non-US IP Volume Spike   | Baseline avg 100 (max 120) → Alert at 200                | >200           |

## Findings

- **Windows Server**
  - 1,811 account lockouts for `user_a` in 3 hours
  - 2,128 password reset attempts for `user_k`
  - 432 successful logins by `user_j` in 3 hours
  - Alert thresholds of 15 (failed) and 25 (successful) were exceeded and triggered

- **Apache Web Server**
  - POST requests surged from 1.06% to 29.4% of all traffic
  - 877 POST requests occurred within 0.001 seconds at 4:05:59 PM
  - 800+ non-US IPs (mostly from Ukraine) accessed the site in a narrow time window
  - 404 error rate jumped from 2.1% to 15%, suggesting probing or enumeration
  - Referrer domain of interest: `tuxradar.com`

## Mitigation Recommendations

- Implement Multi-Factor Authentication (MFA) for all privileged and service accounts
- Enforce account lockout policies after multiple failed attempts
- Deploy an IDS/IPS for real-time traffic inspection and blocking
- Limit login access to trusted geographic regions (e.g., US-only)
- Apply rate limiting and CAPTCHA on login and form submission endpoints
- Enable Splunk alert forwarding to incident response channels
- Baseline expected POST and login volumes and alert on outliers

## Skills Demonstrated

- SOC-style alert triage and incident simulation in Splunk
- Detection rule engineering using SPL (Search Processing Language)
- Correlating network and endpoint logs to identify attack chains
- Creation of executive dashboards and automated alerts
- Threat classification, MITRE mapping (T1110 – Brute Force, T1499 – DoS)
- Clear documentation of findings and proposed mitigations

## Project Context

This capstone project was completed during a blue team simulation in which our team acted as SOC analysts for a fictional company (VSI). We were tasked with detecting and responding to simulated attacks by a competitor (JobeCorp) aiming to disrupt operations. Using Windows and Apache logs, we engineered detections, tuned alerts against baselines, and documented spikes in malicious activity. This project emphasizes real-world response workflows, escalation logic, and the importance of using analytics to detect stealthy attacks across different log sources.

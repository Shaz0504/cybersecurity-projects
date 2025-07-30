
# SOC Alert Triage Simulation – Splunk Project

This project simulates a real-world Security Operations Center (SOC) alert triage and investigation process using Splunk. Conducted as part of a 3-day blue team challenge during a cybersecurity bootcamp, it replicates the tasks of a Tier I/II SOC analyst responding to live threats against a simulated enterprise environment.

## Objectives

- Develop detection logic and triage workflows using Splunk
- Investigate simulated brute-force and DDoS attacks using log correlation
- Create dashboards, alerts, and reports for security monitoring
- Align findings with MITRE ATT&CK techniques
- Document investigation outcomes and response recommendations

## Tools and Data Sources

- Splunk Core Platform
- Apache Web Server Logs
- Windows Event Logs (e.g., 4625, 4624, 4740)
- Custom SPL queries
- Simulated attacker behavior and compromised accounts

## Detection Examples

```spl
# Brute-force login attempts
index=wineventlog EventCode=4625
| stats count by Account_Name, src_ip
| where count > 5

# HTTP POST flood detection (potential DDoS)
index=apache_logs method=POST
| stats count by src_ip
| where count > 100

# Account lockout tracking
index=wineventlog EventCode=4740
| stats count by user, src_ip
```

## Key Alerts Created

| Alert Name           | Description                                  | Log Source            |
|----------------------|----------------------------------------------|------------------------|
| Brute Force Attempt  | Multiple failed logins within 1-minute window| Windows Security Logs  |
| HTTP POST Flood      | Excessive POST requests from single IP       | Apache Access Logs     |
| Account Lockout Spike| Multiple account lockouts in short span      | Windows Event Logs     |

## Findings

- Brute-force login attempts originated from a known threat IP range
- Apache logs indicated high-frequency POST requests targeting `/login` endpoint
- Security events revealed multiple account lockouts tied to the same time window as brute-force alerts
- Elevated failed login activity from a service account outside of business hours

## Mitigation Recommendations

- Block abusive IPs via perimeter firewall or automated SIEM response
- Enforce stricter account lockout and password retry policies
- Enable MFA for administrator and service accounts
- Establish baseline login activity alerts with time-of-day thresholds
- Implement Splunk alert forwarding to incident response team channels

## Skills Demonstrated

- SOC-style alert triage and log correlation in Splunk
- Detection rule creation using SPL (Search Processing Language)
- Dashboard/report creation to support SOC visibility
- Threat classification and response prioritization
- MITRE ATT&CK technique mapping (e.g., T1110, T1499)

## Project Context

This simulation was conducted during a capstone blue team challenge and represents a realistic application of SOC tools and triage processes under pressure. It reinforces the importance of structured alert handling, accurate detection tuning, and clear documentation in a security operations setting.


# Network Security: Firewall and IDS Configuration – Module 11 Project

## Lab Objectives

This hands-on project simulates real-world tasks of a junior security analyst responsible for defending corporate infrastructure. The primary objectives of this lab are:

- Configure and segment network zones using firewalld in alignment with PCI-DSS requirements
- Analyze and write Snort intrusion detection rules to detect common threats
- Understand layered security concepts such as Defense-in-Depth and the Cyber Kill Chain
- Triaging alerts and identifying threats using Security Onion
- Translate technical findings into clear and actionable security recommendations

This project simulates a hands-on network security engagement using firewall management, Snort rule analysis, and Security Onion. It was developed as part of a cybersecurity bootcamp and is structured to reflect the responsibilities of a junior SOC or security administrator.

---

## Tools and Technologies

- firewalld and UFW (Linux firewalls)
- Snort (IDS rule analysis)
- Security Onion (packet capture and alert triage)
- OSI Model reference
- PCI-DSS compliance considerations
- Cyber Kill Chain methodology

---

## Part 1: Review Questions Summary

### Security Control Types

- **Physical Controls**: Walls, bollards, fences, guard dogs, cameras, lighting  
- **Administrative Controls**: Security awareness programs, BYOD policies, ethical hiring  
- **Technical Controls**: Encryption, firewalls, fingerprint readers, IDS, endpoint security

### IDS vs. IPS

- **IDS**: Monitors and alerts, out-of-band  
- **IPS**: In-line system that actively blocks malicious traffic

### IOA vs IOC

- **IOA (Indicator of Attack)**: Behavior-based, helps detect attacks in progress  
- **IOC (Indicator of Compromise)**: Evidence that a system has been compromised

### Cyber Kill Chain (with examples)

1. **Reconnaissance** – Scanning public IPs or OSINT
2. **Weaponization** – Crafting malware or payloads
3. **Delivery** – Phishing emails, malicious links
4. **Exploitation** – Executing payload using vulnerability
5. **Installation** – Dropping malware onto system
6. **Command & Control** – Beaconing to attacker server
7. **Actions on Objectives** – Data theft, exfiltration, persistence

---

## Part 2: Drop Zone Firewall Lab Summary

### Configuration Performed

- Verified firewalld service and removed UFW
- Created custom firewall zones:
  - **public**: ETH0 (HTTP, HTTPS, POP3, SMTP)
  - **web**: ETH1 – 201.45.34.126 (HTTP)
  - **sales**: ETH2 – 201.45.15.48 (HTTPS)
  - **mail**: ETH3 – 201.45.105.12 (SMTP, POP3)
- Blocked blacklisted attacker IPs:
  - `10.208.56.23`
  - `135.95.103.76`
  - `76.34.169.118`
- Used `--permanent` rules and `firewall-cmd --reload`
- Added rich rule to block `138.138.0.3`
- Disabled ICMP echo to reduce ping-based scanning

### Compliance Note

Configuration aligns with **PCI-DSS requirement 1**: “Install and maintain a firewall configuration to protect cardholder data.”

---

## Part 3: IDS, IPS, and Defense-in-Depth

### IDS Connectivity Types

- **Port Mirroring**: Connected to a SPAN port on switch  
- **Network TAP**: Passive physical split

### IPS Connectivity

- **In-line Deployment**: Directly in path of traffic; capable of dropping malicious packets

### IDS Types

- **Signature-Based IDS**: Detects known patterns (can't detect zero-days)  
- **Anomaly-Based IDS**: Detects deviations from baselines

### Defense-in-Depth Scenarios

| Scenario | Layer |
|----------|-------|
| Tailgating through a door | Physical |
| Zero-day bypasses antivirus | Application |
| Unauthorized DB access | Data |
| OS vulnerability exploit | Host |
| DDoS attack | Network |
| Misclassification of data | Policy |
| Firewall fingerprinting | Perimeter |

### Additional Concepts

- **Data-at-rest protection**: Disk encryption (e.g., BitLocker)  
- **Data-in-transit protection**: TLS/SSL  
- **Tracking stolen device**: LoJack / Endpoint detection agents  
- **Boot protection**: BIOS password, Secure Boot, disk encryption

---

## Snort Rule Analysis

### Rule 1: VNC Scan Detection

**Header**:
```
alert tcp $EXTERNAL_NET any -> $HOME_NET 5800:5820
```
Detects port scan for VNC (5800-5820); flags indicate SYN scan.  
**Kill Chain**: Reconnaissance  
**Type of Attack**: Port scanning / attempted recon

### Rule 2: PE EXE Download Detection

**Header**:
```
alert tcp $EXTERNAL_NET $HTTP_PORTS -> $HOME_NET any
```
Detects EXE/DLL Windows binaries in HTTP responses  
**Kill Chain**: Delivery  
**Type of Attack**: Malware delivery (e.g., drive-by download)

### Rule 3: Custom Rule

```snort
alert tcp $EXTERNAL_NET any -> $HOME_NET 4444 (msg:"Inbound traffic to port 4444 detected"; sid:1000001; rev:1;)
```
Detects unsolicited traffic to port 4444, often associated with backdoors or remote access tools

---

## Part 4: Optional – Spam Attack Analysis (Security Onion)

### Alert: ET TROJAN JS/Nemucod.M.gen

- **Source**: 188.124.9.56:80  
- **Destination**: 192.168.3.35:1035  
- **Motivation**: Download and execute malicious payload

### Kill Chain Mapping

| TTP             | Findings |
|-----------------|----------|
| Reconnaissance  | Victim targeted via spam email campaign |
| Weaponization   | JS/Nemucod malware |
| Delivery        | HTTP download from attacker-controlled IP |
| Exploitation    | JavaScript executed on client browser |
| Installation    | EXE payload dropped on host |
| Command & Control | Callback to attacker IP |
| Actions on Objectives | Potential data exfiltration or ransomware staging |

### Mitigation

- Block known malicious IPs and domains
- Implement email filtering and attachment scanning
- Enable EDR on endpoints for behavior-based detection
- Isolate infected hosts and notify IR team

---

## Outcome

This project demonstrates foundational SOC and network defense capabilities:

- Configured zone-based firewalld segmentation aligned with PCI-DSS compliance
- Wrote and analyzed Snort rules to detect port scans and malware downloads
- Differentiated IDS vs IPS functionality and deployment models
- Mapped multi-stage attacks to the Cyber Kill Chain
- Applied layered security principles (Defense-in-Depth) to real-world scenarios
- Triaged a malicious event using Security Onion and proposed mitigation strategies
- Documented findings and recommendations clearly for incident response


# Network Vulnerability Assessment – Wireshark-Based Investigation

This project simulates a network vulnerability assessment conducted within an internal corporate office environment. It demonstrates the process of identifying vulnerable hosts, enumerating open ports, investigating DNS manipulation, and analyzing network packet captures using standard blue team tools.

This project was completed as part of a cybersecurity bootcamp and reflects Tier I SOC analyst workflows involving ping sweeps, SYN scans, remote access validation, DNS resolution, and PCAP inspection using Wireshark.

---

## Tools and Technologies

- Ping (ICMP) – Host discovery  
- Nmap – SYN port scanning  
- SSH – Remote access testing  
- nslookup – DNS inspection  
- Wireshark – Packet capture analysis (HTTP, ARP)  
- OSI Layers Analyzed – Layers 2 through 7  

---

## Assessment Workflow

### Phase 1: Host Discovery

Ping sweep of target subnet revealed responsive hosts:
- 15.199.95.91 – Responded
- 161.35.96.20 – Responded
- 15.199.94.91 – No response (likely ICMP disabled)

Findings at OSI Layer 3 (Network).

---

### Phase 2: Port Scanning

SYN scans conducted using Nmap:
```
nmap -sS 15.199.95.91
nmap -sS 161.35.96.20
```

Common open ports discovered:
- 22/tcp – SSH
- 80/tcp – HTTP
- 443/tcp – HTTPS

Findings at OSI Layer 4 (Transport).

---

### Phase 3: Remote Access and DNS Investigation

SSH access was attempted with default credentials:
```
ssh jimi@15.199.95.91
password: hendrix
```

Upon login, `/etc/hosts` file revealed DNS redirection for `rollingstone.com`. The domain resolved to a suspicious IP address. An `nslookup` confirmed that the IP was not associated with the legitimate domain, indicating tampering or DNS spoofing.

Findings at OSI Layers 3 and 7.

---

### Phase 4: Wireshark PCAP Analysis

Packet capture retrieved from the compromised host revealed:

- ARP Spoofing activity: multiple inconsistent MAC addresses mapped to the same IP
- HTTP GET requests to unknown or suspicious domains

Findings at OSI Layers 2 (Data Link), 3 (Network), and 7 (Application).

---

## Key Findings

- Live hosts responded to ICMP pings, enabling basic network reconnaissance
- Multiple services (SSH, HTTP) exposed to external access
- Host file was altered to redirect a known domain to a malicious IP
- PCAP analysis confirmed ARP poisoning and outbound communication to malicious domains

---

## Recommendations

- Block ICMP replies to reduce host enumeration surface
- Restrict external access to SSH and web services via firewall and allowlists
- Monitor host file integrity and enforce centralized DNS
- Enable dynamic ARP inspection and port security on switching infrastructure
- Replace password-based SSH authentication with key-based access control

---

## Outcome

This project demonstrates a structured SOC investigation from initial host discovery through to packet-level analysis and reporting. It showcases core skills in vulnerability detection, packet analysis, threat documentation, and remediation planning for blue team and security operations roles.

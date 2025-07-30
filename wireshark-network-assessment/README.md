
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

## Target Environment Overview

The assessment was conducted against designated infrastructure associated with a simulated Hollywood office. Below is the server inventory used during scanning and investigation:

| Server Name              | IP Address     | OS           | Service Role     |
|--------------------------|----------------|--------------|------------------|
| HW-Web-01                | 15.199.95.91   | Ubuntu 18.04 | Web Server       |
| HW-Mail-01               | 15.199.94.91   | Windows SVR  | Mail Server      |
| HW-DNS-01                | 161.35.96.20   | Ubuntu 20.04 | DNS/AD Server    |
| HW-Web-Server-2          | 203.0.113.32   | Linux        | Web Server       |

These IPs were probed using ping, Nmap, and SSH as part of the multi-phase assessment described below.

---

## Assessment Workflow

### Phase 1: Host Discovery

Ping sweep was conducted across all IPs listed in the internal server inventory. Example:
```
ping 203.0.113.32
```
Responsive IPs included 203.0.113.32 and 15.199.95.91. Some servers did not reply, suggesting ICMP is filtered or disabled.

OSI Layer: Network (Layer 3)

---

### Phase 2: Port Scanning

SYN scans were conducted using:
```
nmap -sS 203.0.113.32
```

Open port discovered:
- 80/tcp – HTTP

SYN scanning is a half-open scan that does not complete the full TCP handshake, making it fast and stealthy.

OSI Layer: Transport (Layer 4)

---

### Phase 3: Remote Access and DNS Investigation

Upon gaining access to the target system:
- Viewed `/etc/hosts` and identified redirection for `rollingstone.com` to IP:
```
98.137.234.8
```

An `nslookup` confirmed it was not tied to the legitimate domain, indicating malicious redirection.

OSI Layers: Application (7), Network (3)

---

### Phase 4: Wireshark PCAP Analysis

Packet capture analysis revealed:
- **ARP Spoofing**: Conflicting MAC addresses tied to internal hosts
- **HTTP Activity**: Traffic to port 80
- MAC address tied to attacker device (from packet 5 in the pcap): [REDACTED — placeholder, since not confirmed in quiz]

OSI Layers: Data Link (2), Network (3), Application (7)

---

## Key Findings

- Several servers responded to ICMP echo requests
- Open HTTP port accessible publicly
- `/etc/hosts` tampering used for DNS spoofing
- PCAP confirmed ARP poisoning and suspicious HTTP traffic

---

## Recommendations

- Disable ICMP echo replies on production systems
- Restrict HTTP access to internal-only users or authenticated zones
- Monitor and protect `/etc/hosts` integrity
- Deploy dynamic ARP inspection and switch-level protections
- Monitor open ports regularly for unauthorized access

---

## Outcome

This project replicates a Tier I SOC analysis cycle, including discovery, exploitation analysis, and response planning. It showcases investigation techniques applicable to endpoint monitoring, traffic analysis, and blue team workflows.

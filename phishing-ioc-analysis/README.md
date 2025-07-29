# Phishing Simulation and IOC Detection

This project simulates a complete phishing lifecycle, combining red team tactics (email delivery and payload simulation) with blue team response (email analysis and detection scripting). The campaign involved sending phishing emails to classmates and analyzing click-through rates, followed by building a custom Bash script to flag suspicious messages.

---

## Objectives

- Design and deliver simulated phishing campaigns
- Track user interaction metrics to assess awareness
- Develop a detection script to analyze emails and extract IOCs

---

## Tools and Techniques Used

- GoPhish and SendGrid for campaign delivery  
- Carrd.co for landing pages  
- DigitalOcean Linux VM for hosting  
- Bash and Fetchmail for email ingestion and parsing  
- Gmail, Outlook, Yahoo for cross-platform testing  
- OSINT for target enumeration and custom domains

---

## Campaign Results

| Campaign                 | Sent | Opened | Clicked | Click Rate |
|--------------------------|------|--------|---------|------------|
| Splunk Email Campaign    | 31   | 21     | 6       | 29%        |
| Cybersecurity Summit     | 31   | 20     | 6       | 30%        |

---

## Detection Script Logic

The custom Bash script flags potential phishing emails by scanning:
- Untrusted sender domains (e.g., public email services)
- Generic greetings and urgency phrases
- Suspicious links or file attachments

Output is color-coded and designed to mimic alert-level visibility for SOC analysts.

---

## Report

[Download Full Project Report (PDF)](./phishing-report.pdf)

---

## Demo Video

[Watch Phishing Campaign Demo (Google Drive)](https://drive.google.com/file/d/1VLDhIcAzyM1ZtZtXig-ejgr2s0WbbDsn/view)

---

## Skills Demonstra

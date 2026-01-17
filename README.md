<div align="center">

<img src="https://via.placeholder.com/1200x260/f8fafc/111827?text=SnortSheet" width="100%" alt="SnortSheet"/>

<h1>SnortSheet</h1>

<p>
A Serverless SIEM & Agentic SOC Framework<br/>
Snort → Python Middleware → Google Sheets → AI Automation
</p>

</div>

---

## License Status

<table>
<tr>
<td width="8%" align="center">🔒</td>
<td>
<strong>Proprietary Software</strong><br/>
This project is <strong>not open source</strong>. All rights are reserved by the author.
</td>
</tr>
</table>

---

## Usage Permissions

<table>
<thead>
<tr>
<th align="left">Action</th>
<th align="center">Status</th>
</tr>
</thead>
<tbody>
<tr><td>View source code</td><td align="center">✅ Allowed</td></tr>
<tr><td>Copy / reuse</td><td align="center">❌ Not allowed</td></tr>
<tr><td>Modify / fork</td><td align="center">❌ Not allowed</td></tr>
<tr><td>Commercial or non-commercial use</td><td align="center">❌ Not allowed</td></tr>
<tr><td>Reverse engineering</td><td align="center">❌ Not allowed</td></tr>
</tbody>
</table>

---

## About SnortSheet

SnortSheet is a **proprietary security middleware** that connects **Snort IDS**
to **cloud-native monitoring and automation workflows** using Google Sheets as
a lightweight SOC dashboard.

It removes the need for ELK stacks, databases, and commercial SIEM platforms,
focusing instead on **lean, practical security engineering**.

---

## Core Capabilities

<table>
<tr>
<td width="50%">

- Snort CSV alert ingestion  
- Intelligent deduplication  
- Rate-limited forwarding  
- HTML email alerts  

</td>
<td width="50%">

- Google Sheets SOC dashboard  
- Mobile-friendly visibility  
- Agentic AI compatibility  
- Automation-ready data  

</td>
</tr>
</table>

---

## Architecture Overview

Attacker
↓
Snort IDS
↓
alert.csv
↓
Python Bridge
(deduplication & throttling)
↓
Google Apps Script
↓
Google Sheets (SOC) + Email Alerts


---

## Security Design Principles

- One-way outbound data flow only  
- No inbound ports required  
- TLS-encrypted transport  
- No Google credentials stored locally  
- Webhook-based backend isolation  
- Strict payload validation  

---

## Installation Guide

### Prerequisites

<table>
<tr><td><strong>OS</strong></td><td>Ubuntu 20.04 / 22.04, Debian, Kali</td></tr>
<tr><td><strong>Python</strong></td><td>3.8 or newer</td></tr>
<tr><td><strong>Network</strong></td><td>Active internet connection</td></tr>
</table>

---

### Phase 1: Google Sheets Backend (Apps Script)

1. Create a new Google Sheet  
2. Go to **Extensions → Apps Script**  
3. Delete default code  
4. Paste contents of `code.gs`  
5. Click **Deploy → New deployment**  
6. Select **Web app**
   - Execute as: **Me**
   - Access: **Anyone**
7. Authorize permissions  
8. Copy the **Web App URL**

---

### Phase 2: Install & Configure Snort

```bash
sudo apt update
sudo apt install snort -y
Edit configuration:

sudo nano /etc/snort/snort.conf
Enable CSV alerts:

output alert_csv: /var/log/snort/alert.csv timestamp,msg,proto,src,srcport,dst,dstport
Validate configuration:

sudo snort -T -c /etc/snort/snort.conf
Phase 3: Python Bridge Setup
git clone https://github.com/geeksahil/snortsheet.git
cd snortsheet
pip3 install -r requirements.txt
Edit snort_bridge.py:

WEBHOOK_URL = "PASTE_YOUR_GOOGLE_APPS_SCRIPT_URL"
Running the System
Start Python Bridge
sudo python3 snort_bridge.py
Expected:

[READY] Watching log file...
Start Snort
ip addr
sudo snort -c /etc/snort/snort.conf -i wlo1
Testing Scenarios
ICMP Test
ping <SENSOR_IP>
DNS Test
nslookup google.com 8.8.8.8
HTTP Test
curl http://example.com
Advanced Configuration
<table> <tr><td><strong>EMAIL_COOLDOWN</strong></td><td>Alert frequency (default: 60s)</td></tr> <tr><td><strong>LOG_FILE</strong></td><td>Custom Snort log path</td></tr> <tr><td><strong>Rate limiting</strong></td><td>Do not reduce below 0.5s</td></tr> </table>
Troubleshooting
Wrong interface

ip addr
No alerts

tail -f /var/log/snort/alert.csv
No emails

Check spam

Redeploy Apps Script

Ensure access is “Anyone”

Repository Structure
snortsheet/
├── snort_bridge.py
├── code.gs
├── requirements.txt
├── README.md
└── LICENSE
Licensing & Permissions
Any use of this software requires explicit written permission.

📧 Contact: dev.sahilthakur@gmail.com

Unauthorized usage constitutes copyright infringement.

Author
Sahil Thakur
Security Researcher & Developer

License
<details> <summary>View full license</summary>
Copyright (c) 2026 Sahil Thakur.
All Rights Reserved.

This software is proprietary.
Unauthorized copying, modification, distribution,
reverse engineering, or use is strictly prohibited.

THE SOFTWARE IS PROVIDED "AS IS".
</details>
<div align="center"> <sub>Security is a process — not a product.</sub> </div> ```

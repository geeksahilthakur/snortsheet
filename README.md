<div align="center">

<img src="https://via.placeholder.com/900x220/0f172a/ffffff?text=SnortSheet" width="100%" alt="SnortSheet Banner"/>

# 🛡️ SnortSheet  
### A Serverless SIEM & Agentic SOC Framework

**Snort → Python Middleware → Google Sheets → AI / Automation**

</div>

---

## ⚠️ LICENSE NOTICE (READ FIRST)

**This project is NOT open source.**

All rights are reserved by the author.  
You are **NOT permitted** to use, copy, modify, distribute, or deploy this software **without explicit written permission** from the author.

Viewing the source code does **not** grant usage rights.

See the [`LICENSE`](./LICENSE) file for full legal terms.

---

## 🧐 About SnortSheet

SnortSheet is a **proprietary security middleware framework** that bridges **Snort IDS** with **cloud-native workflows** using Google Sheets as a real-time SOC dashboard.

It eliminates the need for:
- ELK Stack
- Splunk
- Databases
- Dedicated SOC servers

Instead, it provides:
- Real-time alert ingestion
- Intelligent alert throttling
- Cloud-hosted visualization
- AI / Agentic SOC readiness

This project is designed for **research, controlled deployments, and commercial evolution**, not unrestricted public reuse.

---

## 🎯 Problem Statement

Traditional IDS monitoring fails small teams because:

- Logs are locked inside servers
- Visualization requires heavy infrastructure
- SIEMs are expensive and complex
- Alert noise overwhelms analysts

SnortSheet solves this by **moving intelligence, not infrastructure**.

---

## 💡 Core Capabilities

- **Snort CSV ingestion**
- **Python-based smart deduplication**
- **Rate-limited alert forwarding**
- **Google Sheets as live SOC**
- **HTML email alerting**
- **Agentic AI compatibility (n8n, LLMs)**

---

## 🧠 Architecture Overview

[ Attacker ]
│
▼
[ Snort IDS ]
│
▼
alert.csv
│
▼
[ SnortSheet Python Bridge ]
│ (deduplication + throttling)
▼
[ Google Apps Script Webhook ]
│
├──► Google Sheets (SOC Dashboard)
└──► Email Alerts


---

## 🔒 Security Design Principles

- One-way outbound data flow only
- No inbound firewall ports
- HTTPS (TLS-encrypted transport)
- No Google credentials stored locally
- Webhook-based backend isolation
- Strict payload validation

The sensor node remains isolated and hardened.

---

## 🛠️ Repository Structure

snortsheet/
├── snort_bridge.py # Python middleware (core logic)
├── code.gs # Google Apps Script backend
├── requirements.txt # Python dependencies
├── README.md # Documentation
└── LICENSE # Proprietary license


---

## 🚧 Access & Usage

This repository is published for:
- Technical review
- Architecture evaluation
- Controlled demonstrations

### ❌ You MAY NOT:
- Deploy this in production
- Redistribute the code
- Fork or repackage
- Sell or monetize
- Use in academic or commercial products

### ✅ You MAY:
- Read the code
- Request permission for use

---

## 📬 Permission & Commercial Licensing

For:
- Commercial usage
- Research deployments
- Educational workshops
- Custom integrations
- Licensing discussions

Contact the author directly.

Unauthorized usage will be treated as **copyright infringement**.

---

## 🗺️ Roadmap (Author-Controlled)

- Dockerized deployment
- Secure webhook authentication
- Multi-sensor aggregation
- Slack / Discord alerting
- GeoIP intelligence enrichment
- Autonomous response agents

---

## 👨‍💻 Author

**Sahil Thakur**  
(Security Researcher & Developer)  

Project Codename: **SnortSheet**

Mission:  
> Build lean, intelligent security systems without enterprise bloat.

---

## 📄 License

© 2026 Sahil Thakur  
**All Rights Reserved**

See the `LICENSE` file for full terms.

---

<div align="center">
<sub><i>Security is a process — not a dashboard.</i></sub>
</div>

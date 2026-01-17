<div align="center">
<!-- PLACEHOLDER FOR LOGO -->
<img src="https://www.google.com/search?q=https://via.placeholder.com/800x200/1e1e2e/ffffff%3Ftext%3DSnortSheet" alt="SnortSheet Logo" width="100%" />
🛡️ SnortSheet
The "Serverless" SIEM & Agentic SOC Framework
Turn Google Sheets into a Real-Time Security Dashboard. An Open-Source Python Middleware bridging Snort IDS and Agentic AI (n8n, LLMs).
View Demo • Report Bug • Request Feature
</div>
📖 Table of Contents
🧐 About the Project
💎 Why SnortSheet? (Feasibility & Value)
🚀 The Agentic SOC Vision
⚙️ Technical Architecture
🔒 Security Considerations
🛠️ Installation Guide
Prerequisites
Phase 1: Google Cloud Backend
Phase 2: Sensor Configuration
Phase 3: The Bridge
🏃‍♂️ Usage & Testing Scenarios
🔧 Advanced Configuration
❓ Troubleshooting & FAQ
🗺️ Roadmap
👨‍💻 Developer
📄 License
🧐 About the Project
SnortSheet serves as a vital bridge in the modern cybersecurity stack, acting as lightweight, open-source middleware. It seamlessly connects the industry-standard deep packet inspection capabilities of Snort IDS with the ubiquitous accessibility and collaborative power of Google Sheets. By marrying a battle-tested security engine with a cloud-native spreadsheet, it democratizes network monitoring and incident response.
In the traditional cybersecurity landscape, effectively managing and visualizing Snort logs is a significant hurdle. It typically necessitates deploying and maintaining resource-intensive infrastructure like MySQL databases, the ELK Stack (Elasticsearch, Logstash, Kibana), or expensive proprietary solutions like Splunk. These setups often require dedicated hardware, complex configuration, and significant maintenance overhead, which can be a barrier for students, hobbyists, and small teams trying to learn security concepts.
SnortSheet radically simplifies this architecture. It bypasses the need for local databases entirely by piping alert data directly to the cloud via a custom, secure Python bridge. Key features include:
Intelligent Anti-Flood Logic: A custom algorithm that groups repetitive alerts (deduplication) to prevent API throttling and inbox spam, ensuring you only get notified when it matters.
Real-time Email Alerts: Delivers actionable intelligence immediately to your device with rich HTML formatting, including source IPs and threat classifications.
Live Dashboard: Leverages Google Sheets' native charting tools for instant visualization of attack vectors, protocol distribution, and threat timelines.
More than just a logging tool, SnortSheet represents the foundational layer for a democratized Agentic SOC (Security Operations Center). It enables users to construct a sophisticated security monitoring environment without incurring enterprise software costs. By moving data to the cloud, it opens the door for integration with automation platforms (like n8n) and Large Language Models, allowing for automated threat analysis and response workflows that were previously accessible only to large organizations.
📂 Repository Structure
snortsheet/
├── snort_bridge.py    # The Python Connector (Smart Logic & Anti-Flood)
├── code.gs            # Google Apps Script (The Backend Receiver)
├── README.md          # Documentation
└── requirements.txt   # Dependencies


💎 Why SnortSheet? (Feasibility & Value)
Why choose SnortSheet over traditional SIEM solutions?
1. Zero-Cost Infrastructure (The "Serverless" Advantage)
Enterprise SIEMs like Splunk can cost thousands of dollars annually, and even open-source alternatives like ELK require minimum RAM/CPU specs that rule out low-end hardware. SnortSheet leverages Google Sheets as a database, which is free, requires no maintenance, and boasts 99.9% uptime. You can run the sensor on a $35 Raspberry Pi Zero and the backend on Google's free tier. This makes advanced security monitoring feasible for home labs, small businesses, and educational workshops where budget is a primary constraint.
2. Immediate Accessibility & Mobility
Traditional IDSs lock logs inside a server (/var/log/snort). To view them, you need SSH access, a VPN, or a complex web interface. With SnortSheet, your security logs are available instantly via the Google Sheets Mobile App. You can monitor your network's health from a coffee shop, verify alerts, and even share read-only access with team members instantly. This accessibility ensures faster reaction times to critical incidents.
3. Intelligent "Anti-Flood" Throttling
Raw Snort output is noisy. A single port scan can generate 1,000 lines of logs in seconds. Sending all 1,000 alerts to an API would crash your script and ban your IP. SnortSheet's Python Bridge includes a smart deduplication algorithm that:
Identifies unique attack vectors using a composite key (Attacker IP + Threat Message).
Enforces a strict Cooldown Timer (default: 60s) to prevent notification fatigue.
Aggregates rapid-fire alerts into a single notification while ensuring every single log is still preserved in the database for forensic analysis.
4. Agentic AI Readiness
The future of SOC is AI. By getting your data into a structured, cloud-accessible format (CSV/JSON via Sheets API), you eliminate the hardest part of AI integration: Data Engineering. SnortSheet makes your network logs instantly consumable by Agents (AutoGPT, n8n, LangChain) for automated threat hunting. This prepares your infrastructure for the next generation of cybersecurity automation.
🚀 The Agentic SOC Vision: Integrate with n8n & LLMs
SnortSheet is designed to be the "Trigger" in an autonomous security workflow. Once data hits Google Sheets, it triggers an ecosystem of automation.
Use Case 1: AI-Powered False Positive Analysis
The Problem: Snort is sensitive; it often flags legitimate traffic as malicious (False Positives).
The Agentic Solution: Connect n8n to your Google Sheet. When a new row is added, n8n grabs the payload, sends it to OpenAI GPT-4 or Google Gemini with a prompt: "Analyze this packet header and rule message. Is this likely a false positive for a home network?"
The Result: The AI writes its analysis back into the sheet in a new column, saving the analyst hours of investigation.
Use Case 2: Automated Active Defense
The Problem: An attacker is brute-forcing your SSH port at 3 AM.
The Agentic Solution: A script monitors the Google Sheet. If it sees >20 alerts from the same Source IP within 1 minute with the message "SSH Brute Force", it automatically triggers a webhook to your firewall (e.g., via Ansible or Uncomplicated Firewall) to drop all traffic from that IP.
The Result: The threat is neutralized before you even wake up.
Use Case 3: Threat Intelligence Enrichment
The Problem: You see an alert from IP 45.33.x.x but don't know who it is.
The Agentic Solution: Google Apps Script can automatically fetch data from AbuseIPDB or VirusTotal whenever a new IP is logged.
The Result: Your dashboard automatically populates with the ISP name, country of origin, and "Abuse Confidence Score" of the attacker.
⚙️ Technical Architecture
The system follows a unidirectional data flow pattern designed for resilience and speed.
graph LR
    subgraph "Local Network (The Sensor)"
        A[Attacker] -- Packet --> B[Snort IDS]
        B -- Writes (Append) --> C[alert.csv]
        C -- Tails File --> D{Python Bridge}
    end

    subgraph "The Middleware"
        D -- "Smart Logic (Debounce/Throttle)" --> D
        D -- HTTP POST (JSON) --> E[Google Apps Script]
    end

    subgraph "The Cloud (SOC)"
        E -- Appends Row --> F((Google Sheets))
        E -- SMTP Alert --> G[Admin Email]
        F -- Data Feed --> H[Visual Dashboard]
    end


The Sensor: Snort analyzes traffic and appends alerts to a local CSV file.
The Watchdog: snort_bridge.py monitors this file using file seeking (similar to tail -f).
The Brain: The Python script applies logic to determine if this is a "New" alert or "Spam," then formats a JSON payload.
The Cloud: Google Apps Script receives the POST request, handles concurrency via LockService, parses the data, writes to the sheet, and dispatches HTML emails.
🔒 Security Considerations
Since this project involves sending security logs over the internet, we have implemented several safeguards:
One-Way Traffic: The Python Bridge pushes data out. It does not open any inbound ports on your firewall. Your internal network remains closed and secure.
HTTPS Encryption: All data sent to Google Sheets is encrypted in transit via TLS 1.2/1.3.
Source Obfuscation: The Google Apps Script is deployed as a Web App. While the URL is public, the script logic is hidden.
No Sensitive Credentials: The Python script does not require your Google Password. It communicates solely via the Webhook URL.
Input Validation: The backend script strictly validates incoming JSON payloads to prevent injection attacks or malformed data corruption.
🛠️ Installation Guide
📋 Prerequisites
OS: Ubuntu 20.04/22.04 LTS (Recommended), Debian, or Kali Linux.
Runtime: Python 3.8 or newer.
Network: An active internet connection for the sensor.
Phase 1: Google Cloud Backend (Apps Script)
Create a new Google Sheet.
Navigate to Extensions > Apps Script.
Clean Slate: Delete any default code in Code.gs.
Import Core Logic: Copy the content of code.gs from this repository and paste it into the editor.
Deploy:
Click the blue Deploy button > New deployment.
Type: Select "Web app".
Configuration:
Description: "SnortSheet Receiver v1"
Execute as: "Me" (your email).
Who has access: "Anyone" (This is required so your Python script can POST without OAuth complexity).
Authorize: Grant the script permission to access your Sheets and Gmail.
Save the URL: Copy the generated Web App URL (https://script.google.com/...). You will need this later.
Phase 2: Sensor Configuration (Snort)
Install Snort:
sudo apt update && sudo apt install snort -y


Configure the Output Plugin. Open the config file:
sudo nano /etc/snort/snort.conf


Locate "Step https://www.google.com/search?q=%236: Configure output plugins". Add the following line to enable Safe Mode CSV output. This format is critical for the Python bridge to parse correctly:
output alert_csv: /var/log/snort/alert.csv timestamp,msg,proto,src,srcport,dst,dstport


Validate configuration:
sudo snort -T -c /etc/snort/snort.conf


Phase 3: The Bridge (Python Middleware)
Clone the repository:
git clone [https://github.com/geeksahil/snortsheet.git](https://github.com/geeksahil/snortsheet.git)
cd snortsheet


Install dependencies:
pip3 install requests


Configure the Bridge:
Open snort_bridge.py and locate the WEBHOOK_URL variable.
# CONFIGURATION
WEBHOOK_URL = 'PASTE_YOUR_COPIED_GOOGLE_APPS_SCRIPT_URL_HERE'


Save the file.
🏃‍♂️ Usage & Testing Scenarios
1. Launching the System
Order matters. Start the bridge before the sensor.
Window 1: Start the Bridge
sudo python3 snort_bridge.py


You should see: [READY] Watching log file...
Window 2: Start Snort
Replace wlo1 with your actual interface (find it using ip addr).
sudo snort -c /etc/snort/snort.conf -i wlo1


2. Validating Detection (Simulated Attacks)
Test A: The Connectivity Check (ICMP)
From a different machine, ping your Snort sensor.
ping <SENSOR_IP_ADDRESS>


Expected Result: Terminal shows [EMAIL SENT]. Dashboard shows "ICMP Test".
Test B: The "Google" Detection (DNS/TCP)
If you are using the custom rules provided in this repo, run:
nslookup google.com 8.8.8.8


Expected Result: Terminal shows [EMAIL SENT]. Dashboard shows "DNS Traffic Detected".
Test C: Web Traffic (HTTP)
curl [http://example.com](http://example.com)


Expected Result: Dashboard shows "HTTP Web Traffic Detected".
🔧 Advanced Configuration
The snort_bridge.py file contains several tunable parameters for power users:
EMAIL_COOLDOWN: Default is 60 seconds. Decrease this to 5 for debugging or increase to 300 for high-traffic environments to reduce noise.
LOG_FILE: If you installed Snort in a custom location, update the path here.
Rate Limiting: The script sleeps for 1.0 seconds between uploads to respect Google's API limits. Do not decrease this below 0.5 seconds or Google may block your requests.
❓ Troubleshooting & FAQ
Q: I see "No such device exists" when starting Snort.
A: You are likely using the wrong network interface. Run ip addr to list devices. Common names are eth0, ens33, wlo1 (Wi-Fi). Update the -i flag in your command.
Q: The Python script is running, but no alerts appear.
A:
Check if Snort is writing to the file: tail -f /var/log/snort/alert.csv.
If the file is empty, check permissions: ls -l /var/log/snort/alert.csv. The file should be readable by root.
Ensure you are generating traffic that matches your Snort rules (/etc/snort/rules/local.rules).
Q: I get "Email Sent" in the terminal, but no email arrives.
A:
Check your Spam folder.
Verify that you deployed the Apps Script as "New Version". If you updated the code but didn't click "New Version" during deployment, the changes won't take effect.
Verify the Apps Script permission setting is "Anyone".
🗺️ Roadmap
We are constantly improving SnortSheet. Here is what's coming next:
[ ] Docker Support: A containerized version of the Python Bridge.
[ ] Slack/Discord Integration: Native webhooks for chat alerts.
[ ] GeoIP Visualization: Map plotting of attacker origins directly in Sheets.
[ ] One-Click Installer: A bash script to set up everything automatically.
👨‍💻 Developer
Sahil Thakur (aka geeksahil)
Role: Lead Developer & Security Researcher.
Mission: To simplify cybersecurity tooling and enable "Agentic" workflows for everyone.
Connect: GitHub | Website
📄 License
Distributed under the MIT License. See LICENSE for more information.
<div align="center">
<sub>Built with ❤️ using Python, Google Cloud Platform, and Snort.</sub>



<sub><i>"Security is not a product, but a process." - Bruce Schneier</i></sub>
</div>

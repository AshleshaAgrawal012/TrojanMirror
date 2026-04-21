Trojan Behavior Analysis Sandbox (Educational Simulation)
A GUI-driven, web-based Trojan malware simulation sandbox designed to demonstrate the complete lifecycle of a Banking Trojan attack in a fully controlled and safe environment.
This project replicates how a Trojan infiltrates a system via social engineering, performs data exfiltration, establishes Command & Control (C2) communication, and executes ransomware-like file encryption, while providing real-time monitoring and behavioral analysis.

Project Objective
To build an educational cybersecurity sandbox that:
Simulates real-world Trojan attack behavior
Demonstrates attack lifecycle stages
Provides live monitoring via dashboard
Generates behavioral analysis reports with IOCs
Ensures 100% safety via sandbox isolation

System Architecture
User (Victim UI - Shopping Website)
        ↓
Flask Backend (Trojan Logic Engine)
        ↓
Server-Sent Events (SSE Stream)
        ↓
Real-Time Dashboard (Analyst View)

+ core/          → Sensitive system data
+ victim_data/   → Files targeted for encryption
+ stolen_data/   → Exfiltrated data logs

Tech Stack
Backend: Python, Flask
Frontend: HTML, Tailwind CSS
Real-Time Communication: Server-Sent Events (SSE)
Monitoring: Event streaming + logging
File Handling: Python OS & I/O modules

Key Features
🛒 1. Social Engineering Entry Point
Myntra-like e-commerce replica
User logs in and performs checkout
Trojan is triggered via “Download Invoice”
🐴 2. Trojan Simulation (Banking Trojan)
🔐 Data Exfiltration
Reads sensitive files from core/
Captures live user credentials & card details
Stores data in stolen_data/ (simulated C2)
🌐 C2 Communication (Simulated)
Mimics connection to attacker server (192.168.0.99)
Logs outbound “network packets”
Demonstrates attacker-controlled workflow
⌨️ Keylogging (Simulated)
Captures user input (login + payment)
Stores logs for analysis
🔒 Ransomware Behavior
Targets files in victim_data/
Modifies content → makes it unreadable
Renames files → .encrypted
Simulates data loss / denial of access
📊 3. Real-Time Monitoring Dashboard

Accessible at:

http://localhost:5000/dashboard

Displays:

🔁 Live Event Logs
🌐 Network Activity (C2 simulation)
📂 File Access & Encryption
📦 Exfiltrated Data

Powered by Server-Sent Events (SSE) — no page refresh required.

📄 4. Behavioral Analysis Report

Accessible at:

http://localhost:5000/report

Includes:

Attack Summary
Indicators of Compromise (IOCs)
File Modifications
Network Activity
Data Exfiltration Logs

📌 Exportable via Print → Save as PDF


Attack Lifecycle Simulation
1. User logs into shopping website
2. User enters payment details
3. User clicks "Download Invoice"

        ↓ (Trojan Triggered)

4. Data Access
   → Reads sensitive files (core/)

5. Credential Capture
   → Extracts session + payment data

6. Exfiltration
   → Writes data to stolen_data/

7. C2 Communication
   → Simulated attacker interaction

8. Impact Phase
   → Encrypts files in victim_data/

9. Monitoring
   → All actions streamed to dashboard (SSE)

10. Reporting
   → Final behavioral report generated

Project Structure 
Trojan-Sandbox-GUI/
├── backend/
│   ├── app.py
│   ├── trojan_sim.py
│   ├── event_stream.py
│   └── assets/
│
├── frontend/
│   ├── templates/
│   │   ├── shop/
│   │   └── dashboard/
│   └── static/
│
├── core/
├── victim_data/
├── stolen_data/
├── behavior_report.json
└── README.md



How to Run
1. Clone the Repository
git clone https://github.com/your-username/trojan-sandbox.git
cd trojan-sandbox
2. Create Virtual Environment (Recommended)
python -m venv venv
venv\Scripts\activate   # Windows
source venv/bin/activate  # Mac/Linux
3. Install Dependencies
pip install flask
4. Run the Application
python backend/app.py
5. Open in Browser
Interface	URL
🛒 Victim UI	http://localhost:5000/

📊 Dashboard	http://localhost:5000/dashboard

📄 Report	http://localhost:5000/report
🧪 Demo Credentials

You can use:

Username: garv
Password: test@123

OR enter any custom credentials (captured dynamically).

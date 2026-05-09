**TrojanMirror — Banking Trojan Behavior Simulation Sandbox**

An interactive, web-based educational sandbox that simulates the complete lifecycle of a Banking Trojan attack in a fully controlled and safe environment — built for cybersecurity awareness and academic demonstration.

---
**Group Members**

- Ashlesha Agrawal
- Garv Gupta
- Aashi Soni
---

**Project Aim**

The goal of TrojanMirror is to demonstrate how a real-world Banking Trojan operates — from the moment a victim is socially engineered into downloading a malicious file, all the way through credential theft, C2 communication, and ransomware-style file encryption.

This project is entirely sandboxed and safe — no actual malware is involved. All behavior is simulated using Python in a controlled local environment, making it ideal for:

- Cybersecurity education and awareness
- Academic viva / presentations
- Understanding Indicators of Compromise (IOCs)
- Visualizing attacker TTPs (Tactics, Techniques and Procedures)
---

**What We Built**

**1. Victim-Facing Shopping Website**

- A realistic e-commerce storefront (Myntra-inspired)
- Users can register, log in, browse products, and checkout
- Payment details (card number, expiry, CVV) are entered during checkout
- Trojan is silently triggered when the user clicks "Download Invoice"

**2. Trojan Simulation Engine**

A multi-phase attack simulation running in a background thread:

| Phase | What Happens |
|-------|-------------|
| Execution | Payload "executes" via the invoice download |
| Credential Capture | Harvests login credentials and card details from the session |
| Data Access | Reads sensitive files from core/ (bank data, session tokens) |
| Exfiltration | Dumps all stolen data into stolen_data/ |
| C2 Communication | Simulates outbound connection to attacker server (192.168.0.99:443) |
| Impact / Encryption | Encrypts all files in victim_data/ with .encrypted extension |

**3. Real-Time Analyst Dashboard**

- Live event stream powered by Server-Sent Events (SSE) — no page refresh
- Shows: file access events, network packets, exfiltrated credentials, encryption activity
- Accessible at http://localhost:5000/dashboard

**4. Behavioral Analysis Report**

- Auto-generated after each simulation run
- Includes attack summary, IOCs, encrypted file list, network activity, and stolen data artifacts
- Exportable as PDF via browser print
- Accessible at http://localhost:5000/report

**5. Auto Sandbox Reset**

- victim_data/ is automatically restored to plaintext every time the app starts or shuts down
- This ensures every simulation run starts with a clean, encryptable slate — no manual cleanup needed

---

**Project Architecture and Pipeline**

```
[ Victim ]
    |
    |  1. Opens shopping site -> Logs in -> Checkout -> Clicks "Download Invoice"
    v
[ Flask Backend — app.py ]
    |
    |  2. Trojan simulation triggered in background thread
    v
[ trojan_sim.py — Attack Engine ]
    |
    |-- Phase 1: Execution       -> Invoice payload "runs"
    |-- Phase 2: Credential Capture -> Session data harvested
    |-- Phase 3: Data Access     -> Reads core/bank_data.txt, session_data.txt
    |-- Phase 4: Exfiltration    -> Writes stolen_data/dump_<timestamp>.log
    |-- Phase 5: C2 Network      -> Simulated CONNECT/SEND/RECEIVE to 192.168.0.99:443
    `-- Phase 6: Impact          -> Encrypts victim_data/*.txt -> *.txt.encrypted
    |
    |  3. Each action broadcast in real time via SSE
    v
[ Server-Sent Events — event_stream.py ]
    |
    v
[ Analyst Dashboard — /dashboard ]
    |
    `-- Live logs, network events, file activity, exfiltrated data
    |
    v
[ Behavior Report — /report ]
    `-- Full IOC report generated as JSON -> rendered as HTML
```

**Directory Map**

```
TrojanMirror/
|
|-- backend/
|   |-- app.py              <- Flask routes, session handling, sandbox reset hooks
|   |-- trojan_sim.py       <- Full attack simulation engine (6 phases)
|   |-- event_stream.py     <- SSE broadcaster for real-time dashboard
|   `-- assets/
|       `-- Invoice_Order_45821.pdf   <- Dummy payload file served to victim
|
|-- frontend/
|   |-- templates/
|   |   |-- shop/           <- Victim-facing pages (index, login, checkout, success)
|   |   `-- dashboard/      <- Analyst pages (monitor, report)
|   `-- static/             <- CSS, JS, images
|
|-- core/
|   |-- bank_data.txt       <- Simulated sensitive banking data (read by trojan)
|   `-- session_data.txt    <- Simulated session tokens (read by trojan)
|
|-- victim_data/            <- Files targeted for encryption (auto-reset on each run)
|-- stolen_data/            <- Exfiltration dump logs (gitignored)
|-- behavior_report.json    <- Generated IOC report (gitignored)
|-- .gitignore
`-- README.md
```

---

**How to Run Locally**

Prerequisites: Python 3.10+, pip

**Step 1 — Clone the Repository**

```bash
git clone https://github.com/AshleshaAgrawal012/TrojanMirror.git
cd TrojanMirror
```

**Step 2 — Create a Virtual Environment**

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Mac / Linux
python -m venv venv
source venv/bin/activate
```

**Step 3 — Install Dependencies**
```bash
pip install flask
```

**Step 4 — Run the App**
```bash
python backend/app.py
```

**Step 5 — Open in Browser**
| Interface | URL |
|-----------|-----|
| Victim Shopping Site | http://localhost:5000/ |
| Analyst Dashboard | http://localhost:5000/dashboard |
| Behavior Report | http://localhost:5000/report |

---

**Demo Credentials**
Use any of the following pre-configured accounts:
| Username | Password |
|----------|----------|
| ashlesha | test@123 |
| garv | test@123 |
| aashi | test@123 |

You can also enter any custom username and password — the trojan will capture whatever you type.

---
**Disclaimer**
This project is built purely for educational purposes. It contains no real malware, no actual network connections to external servers, and no genuinely harmful code. All attack behavior is simulated within a local sandbox. Do not attempt to replicate any of these techniques outside of a controlled environment.

---
**Tech Stack**
| Layer | Technology |
|-------|-----------|
| Backend | Python, Flask |
| Frontend | HTML, CSS, JavaScript |
| Real-Time Events | Server-Sent Events (SSE) |
| Simulation Engine | Python threading, OS/IO modules |
| Encryption Sim | Base64 encoding with mock header |

# SentinelTrace (Sysmon Threat Analyzer)

Offline SOC-grade forensic analysis and threat-hunting platform for Sysmon XML telemetry.

SentinelTrace is designed for **post-incident investigation and analyst-driven triage**, not real-time alerting.  
It converts raw Sysmon XML logs into **correlated behavioral signals, attack campaigns, and confidence-based verdicts**.

> **Primary question SentinelTrace answers:**  
> _“Does this endpoint show evidence of malicious activity, and how confident are we?”_

---

## Table of Contents

1. Overview  
2. Design Philosophy  
3. What SentinelTrace Is (and Is Not)  
4. Core Capabilities  
5. Architecture Overview  
6. Installation  
7. Usage  
8. Output Artifacts  
9. Dashboard & Analyst Workflow  
10. MITRE ATT&CK Interpretation  
11. Limitations  
12. Intended Audience  
13. License  

---

## 1. Overview

SOC analysts are frequently overwhelmed by high-volume endpoint telemetry where:
- Individual events lack context
- Single detections generate excessive noise
- True attack activity is buried in normal system behavior

SentinelTrace addresses this problem by **correlating behavior over time** instead of treating each event as an alert.

It is built for:
- Offline forensic analysis
- Threat hunting
- Incident validation
- Analyst training and portfolio demonstration

---

## 2. Design Philosophy

SentinelTrace is built on the following SOC principles:

- **Signals are not alerts**  
  Individual events are treated as weak indicators, not conclusions.

- **Correlation > Volume**  
  Confidence increases only when behavior clusters across time, processes, and kill-chain stages.

- **Explainability over automation**  
  Analysts must understand *why* something is suspicious.

- **Honest scope**  
  If something cannot be observed from Sysmon telemetry, SentinelTrace does not pretend otherwise.

---

## 3. What SentinelTrace Is (and Is Not)

### SentinelTrace IS:
- An offline forensic analysis tool
- A SOC investigation and triage aid
- A behavioral correlation engine
- A producer of audit-ready artifacts

### SentinelTrace IS NOT:
- A real-time EDR or XDR
- A prevention or response tool
- A replacement for SIEM
- An automated “malware detector”

This distinction is intentional.

---

## 4. Core Capabilities

### 4.1 Behavioral Burst Detection

- Groups rapid sequences of related activity into **bursts**
- Based on:
  - Process execution
  - Network connections
  - File and registry activity
- Reduces noise from single isolated events

---

### 4.2 Campaign Correlation

- Links multiple bursts across time into **attack campaigns**
- Tracks:
  - Process lineage
  - Host context
  - Kill-chain progression
- Enables incident-level reasoning instead of event-level analysis

---

### 4.3 Confidence Scoring

- Assigns a confidence score (0–100) based on:
  - Detection density
  - Severity escalation
  - Kill-chain advancement
- Prevents binary “infected / clean” conclusions

---

### 4.4 MITRE ATT&CK Mapping

- Maps observed behavior to MITRE ATT&CK tactics and techniques
- Used for **context and classification**, not automatic verdicts
- Explicitly avoids equating MITRE presence with malicious intent

---

### 4.5 Audit-Ready Artifacts

Every analysis run produces structured JSON outputs suitable for:
- SIEM ingestion
- Incident documentation
- DFIR case files
- Analyst review and handoff

---

## 5. Architecture Overview

Sysmon XML Logs
│
▼
[ XML Parsing & Normalization ]
│
▼
[ Detection Signals ]
│
▼
[ Behavioral Bursts ]
│
▼
[ Campaign Correlation ]
│
▼
[ Confidence Scoring ]
│
▼
[ Verdict + JSON Artifacts ]

yaml
Copy code

Key architectural properties:
- CLI-first execution
- Run-level isolation
- Deterministic analysis
- UI is optional and non-authoritative

---

## 6. Installation

### Requirements

| Dependency | Version |
|----------|---------|
| Python   | 3.10+   |
| Logs     | Sysmon XML |

### Setup

```bash
git clone https://github.com/lokilokith/sentineltrace.git
cd sentineltrace

python -m venv venv

# Windows
.\venv\Scripts\activate

# Linux / macOS
source venv/bin/activate

pip install -r requirements.txt
7. Usage
7.1 Interactive CLI Menu
bash
Copy code
python cli.py
Provides access to all analysis modes.

7.2 Strict Audit Mode (Headless)
bash
Copy code
python cli.py analyze data/suspect_log.xml --out reports/
Use cases:

Batch processing

Automated sandboxes

Controlled forensic pipelines

7.3 Dashboard Mode
bash
Copy code
python cli.py --open
Access via browser:

cpp
Copy code
http://127.0.0.1:5000/
The dashboard is investigative, not authoritative.

8. Output Artifacts
Each run creates a dedicated directory:

pgsql
Copy code
reports/
└── run_<uuid>/
    ├── summary.json
    ├── detections.json
    ├── bursts.json
    ├── campaigns.json
    ├── mitre.json
    └── metadata.json
Example: summary.json
json
Copy code
{
  "run_id": "3d73582ea008465a901196fca75727f5",
  "total_events": 60984,
  "detections_count": 30664,
  "attack_confidence": {
    "score": 85,
    "level": "High",
    "highest_kill_chain": "Actions on Objectives"
  },
  "verdict": "Critical threat patterns detected."
}
9. Dashboard & Analyst Workflow
The dashboard is designed to mirror real SOC investigation flow:

Triage suspicious campaigns

Review timelines and process trees

Drill into MITRE techniques

Escalate, mark benign, or flag false positives

Add analyst notes

The UI intentionally suppresses execution-only noise and exposes why something is not alerting.

10. MITRE ATT&CK Interpretation
SentinelTrace treats MITRE ATT&CK as:

A behavioral taxonomy

A classification aid

A reporting framework

Not as proof of compromise.

The MITRE drill-down view:

Considers frequency, severity, and context

Explicitly warns against single-event conclusions

Requires analyst judgment

11. Limitations
Known and documented limitations:

Offline analysis only

Single-host focus

No response or containment

XML parsing is non-streaming

Detection logic is heuristic and lab-tuned

No allowlisting or FP feedback loop (yet)

These constraints are intentional and transparent.

12. Intended Audience
SOC Tier-1 and Tier-2 Analysts

DFIR practitioners

Blue team engineers

Security students building SOC portfolios

This project prioritizes SOC reasoning over production deployment.


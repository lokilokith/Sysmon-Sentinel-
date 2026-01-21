# Sysmon Threat Analyzer

**A professional SOC tool for offline detection of advanced threats in Sysmon XML logs.**

## 🎯 What Problem It Solves
SOC teams often face "alert fatigue" when dealing with massive log files. This tool helps analysts **triage large Sysmon XML logs** and **identify suspicious behavior patterns** (like bursts of activity) without relying solely on static signatures.

It is designed to confirm: *"Is this machine compromised?"*

## 🚀 Key Features
*   **Behavioral Analysis**: Detects attack bursts (Process, Network, File) rather than just single events.
*   **Correlation Engine**: Groups disparate events into cohesive "Campaigns".
*   **MITRE ATT&CK Mapping**: Automatically maps activity to kill-chain stages (e.g., T1059 Command and Scripting Interpreter).
*   **Audit-Ready**: Generates structured JSON reports for SIEM integration.
# Sysmon Threat Analyzer

**Offline SOC-grade threat detection and forensic investigation tool for Sysmon XML logs**

Sysmon Threat Analyzer is a post-incident analysis utility designed for Security Operations Center (SOC) and DFIR use cases.  
It processes exported Sysmon XML logs to identify suspicious behavior, correlate related events, and produce evidence-driven conclusions.

> **Primary Objective:**  
> Determine whether an endpoint shows indicators of malicious activity and assess the confidence level of compromise.

---

## Table of Contents

1. Overview  
2. Problem Statement  
3. Scope Definition  
4. Core Capabilities  
5. Architecture  
6. Installation  
7. Usage  
8. Output Interpretation  
9. Detection Philosophy  
10. MITRE ATT&CK Coverage  
11. Limitations  
12. Intended Audience  
13. License  

---

## 1. Overview

Sysmon Threat Analyzer focuses on **offline forensic triage** rather than real-time monitoring.  
It is designed to support SOC analysts during investigations where Sysmon logs have already been collected from endpoints.

The tool emphasizes:
- Detection explainability
- Event correlation
- Analyst trust
- Audit-ready output

---

## 2. Problem Statement

SOC teams commonly face the following challenges:

| Challenge | Description |
|---------|------------|
| Log Volume | Sysmon generates extremely high event volumes |
| Alert Fatigue | Individual events lack context when viewed in isolation |
| Manual Analysis | Reviewing raw XML logs is slow and error-prone |
| Time Pressure | Incidents require rapid and defensible conclusions |

Traditional signature-based tools often fail to provide behavioral context.

**Sysmon Threat Analyzer addresses this gap** by correlating events and identifying suspicious activity patterns rather than isolated signals.

---

## 3. Scope Definition

### In Scope

| Capability | Supported |
|----------|-----------|
| Offline log analysis | Yes |
| Behavioral detection | Yes |
| Event correlation | Yes |
| MITRE ATT&CK mapping | Yes |
| JSON incident reporting | Yes |

### Out of Scope

| Capability | Supported |
|----------|-----------|
| Real-time monitoring | No |
| Host containment | No |
| Automated remediation | No |
| Cross-host correlation | No |
| Cloud / Identity logs | No |

This scope is intentional and documented.

---

## 4. Core Capabilities

### 4.1 Behavioral Detection

- Detects bursts of related activity across time
- Focuses on execution, file, registry, and network behavior
- Avoids single-event alerting where context is insufficient

---

### 4.2 Event Correlation

- Groups related Sysmon events into logical activity chains
- Preserves parent-child process relationships
- Enables timeline-based investigation

---

### 4.3 MITRE ATT&CK Mapping

- Maps detections to ATT&CK tactics and techniques
- Improves attacker intent understanding
- Supports SOC reporting and classification

---

### 4.4 Audit-Ready Reporting

- Generates structured JSON output
- Designed for SIEM ingestion and case documentation
- Suitable for DFIR evidence retention

---

## 5. Architecture

### High-Level Flow

Sysmon XML Logs
│
▼
[ XML Parsing ]
│
▼
[ Detection Engine ]
│
▼
[ Correlation Engine ]
│
▼
[ Scoring & Verdict ]
│
▼
[ JSON Incident Report ]

yaml
Copy code

### Component Responsibilities

| Component | Responsibility |
|---------|----------------|
| Parser | Efficient XML ingestion and normalization |
| Detection Engine | Behavioral and rule-based analysis |
| Correlation Engine | Event grouping and timeline construction |
| Scoring Engine | Confidence calculation |
| Report Generator | Structured output generation |

---

## 6. Installation

### Requirements

| Dependency | Version |
|----------|---------|
| Python | 3.9+ |
| Sysmon Logs | XML format |

### Setup

```bash
git clone https://github.com/lokilokith/sysmon-threat-analyzer.git
cd sysmon-threat-analyzer
pip install -r requirements.txt
7. Usage
Analyze a Sysmon XML Log
powershell
Copy code
python cli.py analyze data/sample_sysmon.xml --out reports/
Execution Workflow
Load Sysmon XML file

Parse and normalize events

Apply detection logic

Correlate related activity

Score attack confidence

Generate JSON report

8. Output Interpretation
Console Output
Field	Description
Run ID	Unique analysis identifier
Verdict	Final logic-driven conclusion
Attack Confidence	Low / Medium / High
Highest Kill Chain Stage	Most advanced MITRE stage observed

JSON Report Structure
reports/run_<id>.json

Field	Description
run_id	Analysis session ID
detections_count	Total detection hits
attack_confidence.score	Numerical score
attack_confidence.level	Confidence classification
verdict	Final assessment
mitre_techniques	Mapped ATT&CK techniques
timeline	Chronological event sequence

9. Detection Philosophy
This project avoids opaque or probabilistic black-box detection.

Detection logic is designed to be:

Principle	Description
Deterministic	Same input produces same output
Explainable	Every detection can be justified
Tunable	Thresholds can be adjusted
Analyst-Readable	No hidden heuristics

This aligns with real SOC investigation requirements.

10. MITRE ATT&CK Coverage (Example)
Tactic	Technique ID	Technique Name
Execution	T1059	Command and Scripting Interpreter
Persistence	T1547	Boot or Logon Autostart Execution
Credential Access	T1003	OS Credential Dumping
Defense Evasion	T1070	Indicator Removal

Coverage expands as detection rules are added.

11. Limitations
Limitation	Impact
Offline-only	No real-time alerts
Single-host focus	No lateral movement correlation
Lab-trained logic	Requires production tuning
No response actions	Detection only

These constraints are explicitly documented and intentional.

12. Intended Audience
Role	Relevance
SOC Tier 1 Analysts	Triage and investigation
SOC Tier 2 Analysts	Incident validation
DFIR Practitioners	Post-compromise analysis
Blue Team Learners	Detection engineering practice


## 📦 How to Run

The tool is accessed via the `cli.py` wrapper.

### Analyze a File
Process a raw Sysmon XML log and generate a professional report.

```powershell
python cli.py analyze data/sample_sysmon.xml --out reports/
```

## 📊 Understanding the Output

### Console Summary
*   **Run ID**: Unique identifier for the analysis session.
*   **Verdict**: The tool's final assessment (e.g., "Attack patterns detected").
*   **Attack Confidence**:
    *   **Low**: Likely background noise.
    *   **Medium/High**: Strong indicators of compromise.
*   **Highest Killchain**: The most advanced stage of the attack observed (e.g., "Actions on Objectives").

### JSON Report (`reports/run_<id>.json`)
A machine-readable file containing:
*   `detections_count`: Total number of rule hits.
*   `attack_confidence`: Object containing score and level.
*   `verdict`: The final logic-driven conclusion.

## ⚠️ Limitations
*   **Batch Analysis Only**: This is an offline forensic tool, not a real-time EDR agent.
*   **No Live Response**: It analyzes logs but does not block processes or isolate hosts.
*   **Lab-Trained Baselines**: Anomaly detection logic is tuned on lab data; production environments may require whitelist tuning.
*   **Not an EDR**: It complements EDRs by providing detailed post-incident analysis.

## 🛠️ Architecture
*   **Ingestion**: Streaming XML parser (efficient for large files).
*   **Engine**: Hybrid logic (Behavioral + YARA).
*   **Persistence**: SQLite-backed state management.

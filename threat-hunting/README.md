# Threat Hunting

Threat hunting in the Enterprise SOC Lab focuses on proactively searching for suspicious behavior that may not have triggered an existing detection.

The objective is to move beyond alert-driven monitoring and investigate hypotheses using Windows Security telemetry, Sysmon, Windows Firewall logs, and Splunk SPL.

---

## Hunting Philosophy

The hunting process follows:

```text
Hypothesis
    ↓
Identify Relevant Telemetry
    ↓
Construct SPL Search
    ↓
Analyze Behavioral Patterns
    ↓
Investigate Supporting Evidence
    ↓
Determine Malicious / Benign Activity
    ↓
Document Findings
    ↓
Create or Improve Detection

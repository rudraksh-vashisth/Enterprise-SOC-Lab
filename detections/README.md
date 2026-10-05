# Detection Engineering

This directory contains the detection engineering portfolio developed for the Enterprise SOC Lab.

The detections are designed around Windows Security Event Logs, Sysmon telemetry, Windows Firewall telemetry, and controlled adversarial activity generated inside the isolated laboratory environment.

The objective is not simply to create SPL searches that return results. Each detection follows a repeatable engineering process:

> Telemetry → Detection Logic → Validation → ATT&CK Mapping → Alerting → Tuning → Documentation

---

## Detection Engineering Objectives

The detection layer is designed to:

- Identify suspicious authentication activity
- Detect suspicious PowerShell behavior
- Detect credential-access activity
- Detect account and group discovery
- Detect suspicious service creation
- Detect suspicious scheduled task creation
- Identify suspicious network connections
- Support controlled network reconnaissance detection
- Map applicable detections to MITRE ATT&CK
- Validate detections through controlled adversarial activity
- Tune thresholds and reduce unsupported false positives
- Provide analyst-oriented security alerts

---

## Detection Portfolio

| ID | Detection | Primary Telemetry | ATT&CK | Status |
|---|---|---|---|---|
| DET-001 | Possible Password Guessing | Windows Security 4771 | T1110.001 | 🟡 Audit |
| DET-002 | Possible Password Spraying | Windows Security 4771 | Legacy / Superseded | ⚪ Disabled |
| DET-003 | Suspicious PowerShell Indicators | Windows / Sysmon | T1059.001 | 🟢 Validated |
| DET-004 | Suspicious PowerShell Parent Process | Sysmon | T1059.001 | 🟢 Validated |
| DET-005 | PowerShell Network Connection | Sysmon Event 3 | T1059.001 | 🟢 Validated |
| DET-006 | Encoded PowerShell | Sysmon Event 1 | T1059.001 | 🟢 Validated |
| DET-007 | PowerShell Ingress Tool Transfer | Sysmon Event 1 | T1105 | 🟢 Validated |
| DET-008 | PowerShell Account & Group Discovery | Sysmon Event 1 | T1087 / T1069 | 🟢 Validated |
| DET-009 | Kerberos Password Guessing | Windows Security 4771 | T1110.001 | 🟢 Validated |
| DET-010 | Kerberos Password Spraying | Windows Security 4771 | T1110.003 | 🟢 Validated |
| DET-011 | Windows Account Lockout | Windows Security 4740 | Operational Monitoring | 🟢 Validated |
| DET-012 | Suspicious Service Creation | Windows System 7045 | T1543.003 | 🟢 Validated |
| DET-013 | Suspicious Scheduled Task Creation | Windows Security 4698 | T1053.005 | 🟢 Validated |
| DET-014 | Suspicious Remote Administrative Logon | Windows Security 4624 | Operational Monitoring | 🟢 Validated |
| DET-015 | Suspicious Outbound Connection | Sysmon Event 3 | Operational Monitoring | 🟢 Validated |

---

## Detection Lifecycle

Each detection is developed and validated through the following lifecycle:

```text
                    ┌─────────────────────┐
                    │ Adversarial Activity│
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Security Telemetry │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    SPL Detection    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Detection Validation│
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ ATT&CK Classification│
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Alert Engineering │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Analyst Investigation│
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Tuning / Improvement│
                    └─────────────────────┘

# SOC Dashboard Ecosystem

The Enterprise SOC Lab uses Splunk Dashboard Studio to provide different operational views across the security operations lifecycle.

The dashboards are designed for different audiences and workflows rather than functioning as duplicate visualizations of the same telemetry.

## Dashboard Architecture

The dashboard ecosystem is organized into the following capability areas:

| Dashboard | Primary Purpose | Audience |
|---|---|---|
| Executive SOC Dashboard | Management-level security visibility | SOC Manager / Security Leadership |
| SOC Overview — Enterprise Security Operations Center | Analyst-facing operational visibility | SOC Analysts |
| Detection Engineering Dashboard | Detection lifecycle and engineering visibility | Detection Engineers |
| Incident Response & Threat Hunting | Investigation and hunting workflows | SOC Analysts / Incident Responders |
| Threat Intelligence & IOC Dashboard | IOC investigation and enrichment | Threat Intelligence / SOC Analysts |
| SOAR & Incident Automation | Automation and response workflow visibility | SOC Analysts / Incident Responders |
| MITRE ATT&CK Detection Coverage | ATT&CK-based detection coverage | Detection Engineers / Purple Team |
| Purple Team & MITRE ATT&CK Validation Dashboard | Detection validation against controlled adversarial activity | Purple Team / Detection Engineering |

---

## 1. Executive SOC Dashboard

### Purpose

Provides management-level visibility into the security posture and operational state of the laboratory.

### Focus Areas

- Security posture
- Detection activity
- Security trends
- Operational metrics
- Management-oriented security information

The dashboard is designed to provide a high-level view rather than expose detailed analyst investigation data.

---

## 2. SOC Overview — Enterprise Security Operations Center

### Purpose

Provides analyst-facing operational visibility into security activity across the monitored environment.

### Focus Areas

- Security event volume
- High-severity detection activity
- Affected hosts
- Security events over time
- Detection activity
- Detection severity distribution
- Authentication attack activity
- PowerShell activity
- Network reconnaissance
- Source IP activity
- Recent security detections

This dashboard acts as the primary operational overview for the laboratory.

---

## 3. Detection Engineering Dashboard

### Purpose

Provides visibility into the behavior and operational performance of the detection portfolio.

### Focus Areas

- Detection activity
- Detection behavior
- Severity
- Engineering metrics
- Detection-oriented operational analysis

The dashboard supports the detection engineering lifecycle from detection development through validation and tuning.

---

## 4. Incident Response & Threat Hunting

### Purpose

Supports analyst investigation and threat-hunting workflows.

### Focus Areas

- Threat-hunting activity
- Suspicious behavior investigation
- Incident-oriented analysis
- Investigation context
- Security-event review

The dashboard provides an operational foundation for developing repeatable hunting hypotheses and incident-response workflows.

---

## 5. Threat Intelligence & IOC Dashboard

### Purpose

Provides a dedicated view for threat intelligence and indicator investigation.

### Focus Areas

- IOC-oriented analysis
- Indicator investigation
- Threat intelligence context
- Security-event enrichment
- Analyst investigation workflows

The dashboard provides the foundation for future external intelligence integrations and automated enrichment.

---

## 6. SOAR & Incident Automation

### Purpose

Visualizes post-detection automation and response workflows.

### Focus Areas

- Playbook execution
- Automation queue
- Analyst approval
- Incident workflow
- Escalation
- Mean Time to Respond (MTTR)
- Automated resolution
- Response activity

### Advanced SOAR

The next maturity stage is to connect these concepts into an integrated workflow:

```text
Detection
    ↓
IOC Extraction
    ↓
Threat Intelligence Enrichment
    ↓
Risk Scoring
    ↓
Analyst Approval
    ↓
Containment / Response
    ↓
Evidence Collection
    ↓
Case Update
    ↓
Resolution

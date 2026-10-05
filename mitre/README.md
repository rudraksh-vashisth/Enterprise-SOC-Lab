# MITRE ATT&CK Integration

The Enterprise SOC Lab uses MITRE ATT&CK to describe adversary behaviors represented by the detection portfolio.

ATT&CK mappings are applied only where the available telemetry and detection logic provide sufficient evidence for the associated technique.

The project intentionally avoids assigning unsupported ATT&CK techniques simply to increase the apparent coverage of the detection portfolio.

## Detection Mapping

| Detection | Detection Area | ATT&CK Technique | Tactic | Mapping Status |
|---|---|---|---|---|
| DET-001 | Password Guessing | T1110.001 — Password Guessing | Credential Access | Mapped |
| DET-002 | Password Spraying | — | — | Legacy / Superseded |
| DET-003 | Suspicious PowerShell | T1059.001 — PowerShell | Execution | Mapped |
| DET-004 | PowerShell Parent Process | T1059.001 — PowerShell | Execution | Mapped |
| DET-005 | PowerShell Network Activity | T1059.001 — PowerShell | Execution | Mapped |
| DET-006 | Encoded PowerShell | T1059.001 — PowerShell | Execution | Mapped |
| DET-007 | Ingress Tool Transfer | T1105 — Ingress Tool Transfer | Command and Control | Mapped |
| DET-008 | Account / Group Discovery | T1087 / T1069 sub-techniques | Discovery | Mapped |
| DET-009 | Kerberos Password Guessing | T1110.001 — Password Guessing | Credential Access | Mapped |
| DET-010 | Kerberos Password Spraying | T1110.003 — Password Spraying | Credential Access | Mapped |
| DET-011 | Windows Account Lockout | — | — | Operational Monitoring |
| DET-012 | Windows Service Creation | T1543.003 — Windows Service | Persistence | Mapped |
| DET-013 | Scheduled Task Creation | T1053.005 — Scheduled Task | Persistence | Mapped |
| DET-014 | Suspicious Remote Administrative Logon | — | — | Operational Monitoring |
| DET-015 | Suspicious Outbound Connection | — | — | Operational Monitoring |

## Mapping Principles

### 1. Evidence before mapping

A detection should only be mapped to an ATT&CK technique when its available telemetry provides sufficient evidence for that behavior.

### 2. Do not force mappings

Some detections monitor suspicious behavior without providing enough context to identify a specific ATT&CK technique.

These detections remain classified as operational monitoring.

### 3. Detection is not the same as coverage

A technique appearing in the ATT&CK mapping does not automatically mean that the organization has comprehensive coverage for that technique.

The project distinguishes between:

- Technique mapping
- Detection implementation
- Detection validation
- Detection coverage

## Key Techniques

### T1110.001 — Password Guessing

Used for detections identifying repeated authentication failures against an account.

Primary detection examples:

- DET-001
- DET-009

### T1110.003 — Password Spraying

Used for detection of authentication failures distributed across multiple accounts.

Primary detection:

- DET-010

### T1059.001 — PowerShell

Used across multiple PowerShell-focused detections.

Primary detection examples:

- DET-003
- DET-004
- DET-005
- DET-006

### T1105 — Ingress Tool Transfer

Used to identify PowerShell-based activity associated with downloading or transferring tools.

Primary detection:

- DET-007

### T1087 / T1069 Sub-techniques

Used for account and group discovery activity.

Primary detection:

- DET-008

### T1543.003 — Windows Service

Used to identify suspicious Windows service creation.

Primary detection:

- DET-012

### T1053.005 — Scheduled Task

Used to identify suspicious scheduled task creation.

Primary detection:

- DET-013

## Operationally Unmapped Detections

Some detections intentionally remain outside a specific ATT&CK technique mapping.

Examples include:

- Windows account lockout monitoring
- Network logon monitoring
- Suspicious outbound connection monitoring

This is intentional.

The goal is to maintain analytically defensible mappings rather than artificially increasing ATT&CK coverage.

## ATT&CK and Purple Team Validation

The project uses two related but distinct concepts:

**MITRE ATT&CK Detection Coverage**

> What adversarial techniques does the detection portfolio address?

**Purple Team Validation**

> Have those detections actually been tested against controlled adversarial behavior?

The Purple Team workflow is:

```text
Adversary Simulation
        ↓
Observable Telemetry
        ↓
Detection
        ↓
Validation
        ↓
ATT&CK Mapping
        ↓
Detection Tuning
        ↓
Repeat

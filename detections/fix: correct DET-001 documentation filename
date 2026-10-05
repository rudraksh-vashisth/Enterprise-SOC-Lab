# DET-001 — Possible Password Guessing

## Objective

Detect repeated failed Kerberos authentication attempts
against the same account within a short time window.

## Telemetry

- Windows Security Event ID: 4771
- Failure Code: 0x18
- Source IP
- Target account

## Detection Logic

- 5-minute correlation window
- Minimum 4 failed attempts
- Loopback source excluded
- IPv4-mapped IPv6 addresses normalized

## MITRE ATT&CK

- Technique: T1110.001
- Name: Password Guessing
- Tactic: Credential Access

## Validation

Controlled test performed against:

`dettest@soclab.local`

Result:

- 5 failed authentication events observed
- Event 4771 generated
- Splunk ingestion confirmed
- Detection triggered
- Severity: High

## Result

PASS

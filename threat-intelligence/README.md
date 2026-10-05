# Threat Intelligence & IOC Investigation

The Enterprise SOC Lab includes a Threat Intelligence and IOC investigation capability designed to support indicator-focused security analysis.

The current implementation provides a dashboard and investigation workflow for examining indicators observed within the laboratory telemetry.

The capability is intentionally separated into two maturity levels:

1. Current IOC investigation and enrichment workflow
2. Future external threat-intelligence integration and automated enrichment

---

## Objective

The objective of the Threat Intelligence capability is to answer questions such as:

- What indicator was observed?
- Where was it observed?
- Which host generated it?
- Which user or process was associated with it?
- When was it observed?
- How frequently did it occur?
- Is the indicator associated with suspicious behavior?
- Can additional context improve the investigation?

The objective is not to treat every IP address, hostname, or process as malicious.

Indicators must be investigated in behavioral context.

---

# IOC Types

The lab can investigate several indicator categories.

## IP Addresses

Examples include:

- Source IP
- Destination IP
- Remote connection address
- Authentication source address

Example:

```text
192.168.56.103

**📌 Project Overview**
The **Enterprise SOC Lab** is a controlled and isolated cybersecurity environment designed to simulate the major functions of a modern Security Operations Center.
The project connects the complete security operations lifecycle:
Adversarial Activity
        ↓
Security Telemetry
        ↓
SIEM Ingestion
        ↓
Detection Engineering
        ↓
Alerting
        ↓
Investigation
        ↓
MITRE ATT&CK Mapping
        ↓
Threat Hunting / Incident Response / Threat Intelligence
        ↓
SOAR & Automation
        ↓
Purple Team Validation
The objective is not simply to build Splunk dashboards. The objective is to understand how attacker behavior becomes observable security evidence and how a SOC can detect, investigate, validate, enrich, and respond to that activity.

**🎯 Project Objectives**
- Build an isolated enterprise-style Windows security environment.
- Configure Active Directory and centralized identity infrastructure.
- Collect Windows Security Event Logs, Sysmon telemetry, and Windows Firewall logs.
- Forward endpoint telemetry into Splunk Enterprise.
- Develop and tune SPL-based security detections.
- Generate controlled adversarial activity for detection validation.
- Map applicable detections to MITRE ATT&CK.
- Configure scheduled security alerts.
- Build analyst, management, threat-hunting, incident-response, threat-intelligence, SOAR, and Purple Team dashboards.
- Validate detections through controlled adversarial activity.
- Develop a foundation for advanced SOC automation.
- Establish measurable experiments that can support cybersecurity research.
  
**🏗️ Lab Architecture**
                         ┌──────────────────────────────┐
                         │       WINDOWS HOST           │
                         │                              │
                         │     Splunk Enterprise        │
                         │       192.168.56.1           │
                         └──────────────┬───────────────┘
                                        │
                              Host-Only Network
                              192.168.56.0/24
                                        │
              ┌─────────────────────────┼─────────────────────────┐
              │                         │                         │
              ▼                         ▼                         ▼
     ┌────────────────┐       ┌────────────────┐       ┌────────────────┐
     │      DC01      │       │   Windows 11   │       │     Kali       │
     │ Windows Server │       │    Endpoint    │       │    Attacker    │
     │     2022       │       │                │       │                │
     │ 192.168.56.101 │       │ 192.168.56.102 │       │ 192.168.56.103 │
     │                │       │                │       │                │
     │ AD DS          │       │ Sysmon         │       │ Controlled     │
     │ DNS            │       │ Splunk UF      │       │ Attack /       │
     │ Authentication │       │ Firewall Logs  │       │ Validation     │
     └────────────────┘       └────────────────┘       └────────────────┘
     
**Lab Components**
Component	Hostname	IP Address	Role
Windows Host	Host	192.168.56.1	Splunk Enterprise
Windows Server 2022	DC01	192.168.56.101	AD DS / DNS / Authentication
Windows 11	win11-client	192.168.56.102	Monitored endpoint
Kali Linux	Kali	192.168.56.103	Controlled attacker / test source


Domain: soclab.local

**🔄 Telemetry Pipeline**
                  Controlled Attack Activity
                            │
                            ▼
                  Windows Environment
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
        Windows Events     Sysmon      Firewall Logs
             │              │              │
             └──────────────┼──────────────┘
                            ▼
                 Splunk Universal Forwarder
                            │
                            ▼
                    Splunk Enterprise
                            │
                            ▼
                   SPL Detection Rules
                            │
                            ▼
                         Alerts
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
       Investigation     Dashboards     ATT&CK Mapping
       
**🛡️ Telemetry Sources**

**Windows Security Events**

Key events used in the lab include:
Event ID	Purpose
4624	Successful logon
4698	Scheduled task creation
4740	Account lockout
4771	Kerberos authentication failure
5156	Windows Filtering Platform connection
7045	Service creation


**Sysmon**
Sysmon provides enhanced endpoint visibility.
Important telemetry includes:
- Process creation — Event ID 1
- Network connection — Event ID 3
- Command-line information
- Parent process information
- Process GUIDs
- User context
  
**Windows Firewall**
Firewall logging provides:
- Source IP
- Destination IP
- Source port
- Destination port
- Protocol
- Allowed connections
- Dropped connections
  
**🔎 Detection Engineering**
The project currently contains 15 detection IDs, with DET-002 retained as a disabled legacy/duplicate detection.
ID	Detection	               Primary Evidence	ATT&CK / Classification
DET-001	Possible Password Guessing	Kerberos 4771	T1110.001 — Password Guessing
DET-002	Possible Password Spraying	Kerberos 4771	Legacy / superseded by DET-010
DET-003	Suspicious PowerShell Indicators	Windows / Sysmon	T1059.001 — PowerShell
DET-004	Suspicious PowerShell Parent Process	Sysmon	T1059.001 — PowerShell
DET-005	PowerShell Network Connection	Sysmon 3	PowerShell context
DET-006	Encoded PowerShell	Sysmon 1	T1059.001 — PowerShell
DET-007	PowerShell Ingress Tool Transfer	Sysmon 1	T1105 — Ingress Tool Transfer
DET-008	PowerShell Account & Group Discovery	Sysmon 1	T1087 / T1069 sub-techniques
DET-009	Kerberos Password Guessing	DC01 4771	T1110.001 — Password Guessing
DET-010	Kerberos Password Spraying	DC01 4771	T1110.003 — Password Spraying
DET-011	Windows Account Lockout	DC01 4740	Operational monitoring
DET-012	Suspicious Service Creation	Windows 7045	T1543.003 — Windows Service
DET-013	Suspicious Scheduled Task Creation	Windows 4698	T1053.005 — Scheduled Task
DET-014	Suspicious Remote Administrative Logon	Windows 4624	Operational network-logon monitoring
DET-015	Suspicious Outbound Connection	Sysmon 3	Operational suspicious-connection monitoring


ATT&CK mappings are applied only where the available telemetry supports the technique. Some detections are intentionally treated as operational monitoring rather than being forced into an unsupported ATT&CK technique.

**🧪 Detection Validation**

A major focus of the project is validation rather than simply writing SPL.
The validation workflow is:
1. Generate controlled activity
          ↓
2. Confirm Windows / Sysmon telemetry
          ↓
3. Confirm ingestion into Splunk
          ↓
4. Execute detection SPL
          ↓
5. Validate expected detection result
          ↓
6. Verify severity and ATT&CK mapping
          ↓
7. Tune thresholds / false positives
          ↓
8. Document the result
Validated areas include:
- Encoded PowerShell
- PowerShell network activity
- PowerShell tool transfer
- Kerberos password guessing
- Kerberos password spraying
- Windows account lockout
- Suspicious service creation
- Suspicious scheduled task creation
- Network reconnaissance / connection activity
  
**📊 SOC Dashboard Ecosystem**
The project contains multiple custom Dashboard Studio views representing different SOC functions.

**1. Executive SOC Dashboard**
**Purpose**: management-level security visibility.
Designed to provide a high-level view of:
- Security posture
- Detection activity
- Security trends
- Operational metrics
- Management-oriented SOC information
  
**2. SOC Overview — Enterprise Security Operations Center**
**Purpose**: analyst-facing operational visibility.

Includes areas such as:
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
  
**3. Detection Engineering Dashboard**
**Purpose**: detection lifecycle and engineering visibility

Focuses on:
- Detection activity
- Detection behavior
- Severity
- Engineering metrics
- Detection-oriented operational analysis
  
**4. Incident Response & Threat Hunting**

**Purpose**: support analyst investigation and hunting workflows.

Provides a dedicated operational view for:
- Threat-hunting activity
- Suspicious behavior investigation
- Incident-oriented analysis
- Investigation context
- Security-event review
The dashboard/workflow has been built; the project will continue expanding the underlying hunting hypotheses, case procedures, evidence handling, and repeatable response workflows.

**5. Threat Intelligence & IOC Dashboard**

**Purpose**: threat intelligence and indicator investigation.

Designed around:
- IOC-oriented analysis
- Indicator investigation
- Threat intelligence context
- Security-event enrichment
- Analyst investigation workflows
The dashboard layer is built; deeper external intelligence integrations and automated enrichment remain part of the next maturity stage.

**6. SOAR & Incident Automation**

**Purpose: visualize post-detection automation and response workflows.**

**Includes concepts such as:**
- Playbook execution
- Automation queue
- Analyst approval
- Incident workflow
- Escalation
- MTTR
- Automated resolution
- Response activity
  
**Advanced SOAR — Remaining**

The next maturity stage is to move toward integrated workflows such as:
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

**7. MITRE ATT&CK Detection Coverage**

**Purpose:** visualize detection coverage against MITRE ATT&CK techniques.

**Focuses on**:
- ATT&CK techniques
- Tactic-level visibility
- Detection coverage
- Technique-to-detection relationships
- Coverage gaps
  
**8. Purple Team & MITRE ATT&CK Validation Dashboard**

**Purpose**: validate whether detections actually work against controlled adversarial behavior.

**Includes**:
- Techniques tested
- Detection validation matrix
- Atomic test activity
- ATT&CK tactic/technique views
- Detection outcomes
- Purple Team validation
  
The key distinction is:
MITRE Coverage
      =
"What can we detect?"

Purple Team Validation
      =
"Have we actually tested that detection?"

**🟣 Purple Team Methodology**

The project follows a continuous Purple Team feedback loop:

Adversary Simulation
        │
        ▼
Observable Telemetry
        │
        ▼
Detection
        │
        ▼
Validation
        │
        ▼
ATT&CK Mapping
        │
        ▼
Detection Tuning
        │
        └──────────────► Repeat
        
The goal is to identify detection gaps, validate existing detections, tune false positives, and improve detection coverage.

**🧭 SOC Capability Model**

The project is organized around the following operational capability layers:
                    ┌─────────────────────┐
                    │      RESEARCH       │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │  PURPLE TEAM /      │
                    │  ATT&CK VALIDATION  │
                    └──────────┬──────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
        ▼                      ▼                      ▼
 THREAT HUNTING         INCIDENT RESPONSE       THREAT INTEL
        │                      │                      │
        └──────────────────────┼──────────────────────┘
                               ▼
                     SOAR / AUTOMATION
                               │
                               ▼
                    DETECTION ENGINEERING
                               │
                               ▼
                         SPLUNK SIEM
                               │
                               ▼
                    TELEMETRY COLLECTION
                               │
                               ▼
                     ENTERPRISE LAB
                     
**📈 Current Project Status**
Capability	Status
Enterprise Lab Infrastructure	✅ Complete
Windows Server / DC01	✅ Complete
Active Directory / DNS	✅ Complete
Windows 11 Endpoint	✅ Complete
Kali Attacker	✅ Complete
Sysmon Telemetry	✅ Complete
Windows Security Auditing	✅ Complete
Windows Firewall Telemetry	✅ Complete
Splunk Enterprise	🟢 Operational
Universal Forwarder	🟢 Operational
Detection Engineering	🟢 Built
Alert Engineering	✅ Complete
MITRE ATT&CK Mapping	🟢 Built
MITRE Detection Coverage	🟢 Built
Executive SOC Dashboard	🟢 Built
SOC Overview Dashboard	🟢 Built
Detection Engineering Dashboard	🟢 Built
Threat Hunting Dashboard / Workflow	🟢 Built / Continuing
Incident Response Dashboard / Workflow	🟢 Built / Continuing
Threat Intelligence & IOC Dashboard	🟢 Built / Continuing
SOAR & Incident Automation	🟢 Built
Purple Team Validation	🟢 Substantially Complete
Dashboard UI/UX	🟡 Refinement Ongoing
Enterprise SPL Audit	🟡 Current
Advanced SOAR	⏳ Remaining
Documentation	🟡 In Progress
Research Paper	⏳ Planned


**Status terminology**
- Complete: foundational implementation is finished.
- Built: the capability/dashboard has been implemented.
- Built / Continuing: the interface and initial workflow exist, while operational depth is still being expanded.
- Substantially Complete: the core capability is implemented and validated, with additional maturity work remaining.
- Current: actively being audited or improved.
- Remaining: planned next-stage engineering.
- Planned: future research/development work.
🗺️ Project Roadmap
Phase 1 — Enterprise Lab Foundation
- [x] Enterprise lab infrastructure
- [x] Active Directory
- [x] Windows endpoint
- [x] Kali attacker
- [x] Splunk SIEM
- [x] Sysmon
- [x] Windows Security auditing
- [x] Firewall telemetry
Status: COMPLETE
Phase 2 — Detection Engineering
- [x] Detection portfolio
- [x] SPL development
- [x] Alert configuration
- [x] MITRE ATT&CK mapping
- [x] Controlled detection validation
- [ ] Final Enterprise SPL audit
- [ ] Detection analytics refinement
Status: SUBSTANTIALLY COMPLETE
Phase 3 — SOC Visibility & Operational Dashboards
- [x] Executive SOC Dashboard
- [x] SOC Overview Dashboard
- [x] Detection Engineering Dashboard
- [x] Incident Response & Threat Hunting Dashboard
- [x] Threat Intelligence & IOC Dashboard
- [x] SOAR & Incident Automation Dashboard
- [x] MITRE ATT&CK Detection Coverage Dashboard
- [x] Purple Team & MITRE ATT&CK Validation Dashboard
- [ ] Enterprise UI/UX refinement
- [ ] Dashboard metric audit
Status: BUILT / REFINEMENT ONGOING
Phase 4 — SOC Operations
- [x] Threat-hunting dashboard/workflow foundation
- [x] Incident-response dashboard/workflow foundation
- [ ] Expand structured hunting hypotheses
- [ ] Formalize investigation procedures
- [ ] Expand incident case management
- [ ] Evidence collection workflow
- [ ] Analyst playbooks
- [ ] Repeatable end-to-end incident scenarios
Status: BUILT / MATURITY IN PROGRESS
Phase 5 — Threat Intelligence & Advanced Automation
- [x] Threat Intelligence & IOC dashboard
- [x] SOAR & Incident Automation dashboard
- [ ] External threat-intelligence enrichment
- [ ] IOC enrichment workflow
- [ ] Automated enrichment
- [ ] Advanced SOAR workflows
- [ ] Analyst approval automation
- [ ] Automated containment experiments
- [ ] Detection → enrichment → response pipeline
Status: BUILT / ADVANCED AUTOMATION REMAINING
Phase 6 — Purple Team & Detection Validation
- [x] Purple Team dashboard
- [x] MITRE ATT&CK validation matrix
- [x] Controlled attack simulations
- [x] Detection validation
- [x] ATT&CK mapping
- [ ] Expand technique coverage
- [ ] Increase repeatable adversary simulations
- [ ] Measure detection performance
- [ ] Regression testing
Status: SUBSTANTIALLY COMPLETE
Phase 7 — Research
- [ ] Define research question
- [ ] Literature review
- [ ] Review related work
- [ ] Formalize experimental methodology
- [ ] Design controlled experiments
- [ ] Collect measurements
- [ ] Analyze detection performance
- [ ] Write research paper
- [ ] Final paper / publication preparation
Status: PLANNED
**🔬 Research Direction**
A potential research direction based on the lab is:
A Repeatable Framework for Validating SIEM Detection Rules Using Adversary Simulation and MITRE ATT&CK Mapping

**Research Question**
Can a small enterprise SOC lab provide repeatable detection validation by combining:
- Windows telemetry
- Sysmon
- Splunk
- MITRE ATT&CK
- Controlled adversary simulation
- Detection engineering
- Purple Team validation
  
**Experimental Model**
Attack
  ↓
Telemetry
  ↓
Detection
  ↓
Alert
  ↓
Validation
  ↓
Measurement
Potential Measurements
- Detection success rate
- Time to detection
- Event volume
- False-positive behavior
- Detection threshold
- Repeatability
- Analyst effort
- Detection coverage
  
The research will be based on controlled experiments and measurable results rather than dashboard screenshots alone.
📚 Learning Objectives
The project is also being used as a practical learning environment for:
SIEM
- SIEM architecture
- Log ingestion
- Event analysis
- Detection correlation
- Alerting
Splunk / SPL
- Search
- Fields
- stats
- eval
- rex
- where
- bin
- case
- Time windows
- Alert creation
- Dashboard development
SOC Operations
- Alert triage
- Detection validation
- False-positive analysis
- Threat hunting
- Incident investigation
- Incident response
- Threat intelligence
- Security automation
MITRE ATT&CK
- Technique identification
- Detection mapping
- Purple Team validation
- Detection engineering
- Coverage analysis
  
**📁 Repository Structure**
Enterprise-SOC-Lab/
│
├── README.md
│
├── architecture/
│   ├── network-diagram.png
│   ├── soc-architecture.png
│   └── architecture.md
│
├── detections/
│   ├── DET-001-password-guessing.md
│   ├── DET-002-password-spraying-legacy.md
│   ├── DET-003-suspicious-powershell.md
│   ├── ...
│   └── DET-015-suspicious-outbound-connection.md
│
├── splunk/
│   ├── detections/
│   ├── alerts/
│   ├── dashboards/
│   └── queries/
│
├── mitre/
│   └── attack-mapping.md
│
├── attacks/
│   ├── password-guessing.md
│   ├── password-spraying.md
│   ├── powershell.md
│   ├── scheduled-task.md
│   ├── service-creation.md
│   └── network-recon.md
│
├── threat-hunting/
│   ├── README.md
│   └── hunts/
│
├── incident-response/
│   ├── README.md
│   ├── playbooks/
│   └── cases/
│
├── threat-intelligence/
│   ├── README.md
│   ├── ioc-analysis/
│   └── enrichment/
│
├── soar/
│   ├── README.md
│   ├── playbooks/
│   └── automation/
│
├── purple-team/
│   ├── detection-validation.md
│   └── attack-scenarios/
│
├── dashboards/
│   ├── executive-soc.png
│   ├── soc-overview.png
│   ├── detection-engineering.png
│   ├── incident-response-threat-hunting.png
│   ├── threat-intelligence-ioc.png
│   ├── soar-incident-automation.png
│   ├── mitre-coverage.png
│   └── purple-team-validation.png
│
├── screenshots/
│   ├── detections/
│   ├── alerts/
│   ├── investigations/
│   └── dashboards/
│
├── documentation/
│   ├── project-report.pdf
│   └── management-presentation.pdf
│
├── research/
│   ├── research-question.md
│   ├── literature-review.md
│   └── experiments/
│
├── .gitignore
└── LICENSE

**🔐 Security & Ethics**
This project is intended for authorized laboratory use only.
All adversarial activity is performed inside a controlled environment configured for security testing.
Do not use attack simulations, exploitation techniques, credentials, or security-testing procedures against systems without explicit authorization.
The repository must never contain:
- Passwords
- API keys
- Authentication tokens
- Private SSH keys
- .env files containing secrets
- Splunk credentials
- Database credentials
- VM disk images
- .vdi / .vmdk files
- License keys
- Sensitive personal information
Configuration examples should be sanitized before publication.

**🤖 Development & AI Assistance**
AI assistance was used during parts of the project development process.
The project work includes designing the lab architecture, configuring telemetry, generating controlled adversarial activity, developing and validating detection logic, mapping applicable detections to MITRE ATT&CK, and building SOC dashboards.
AI-generated guidance is treated as a development aid rather than a substitute for validation. Detection logic and configurations are tested against the lab environment before being treated as part of the project.
The long-term objective is to independently understand and reproduce the major engineering decisions, SPL queries, detection assumptions, investigation workflows, and security concepts represented in this repository.

**🚀 Long-Term Goal**
The long-term objective is to evolve this project from a learning laboratory into a mature SOC engineering and research environment covering:
SIEM → Detection Engineering → SOC Operations → Threat Hunting → Incident Response → Threat Intelligence → SOAR → Purple Teaming → Security Research

**👤 Author
Rudraksh Vashisth**
B.Tech — Computer Science & Engineering
Cybersecurity | SOC | Detection Engineering | Blue Team

**Project philosophy:**
Don't just detect the attack. Understand the telemetry, validate the detection, investigate the behavior, measure the result, and improve the SOC.

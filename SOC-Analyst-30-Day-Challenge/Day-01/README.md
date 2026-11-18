Day 01 — SOC Fundamentals & Lab Environment Preparation

Project: Windows Endpoint Threat Detection and Investigation using Wazuh SIEM

Challenge: 30-Day SOC Analyst Practical Challenge

Day: 01/30

Status: Learning and Lab Planning Completed; System Verification Pending

---

1. Learning Objectives

The main objective of Day 1 is to understand Security Operations Center (SOC) fundamentals, SIEM architecture and the components required to build a practical home SOC laboratory.

By the end of this session, I aim to:

- Understand the purpose and responsibilities of a SOC.
- Understand the responsibilities of a Tier 1 SOC Analyst.
- Learn SIEM fundamentals and its working process.
- Understand the Wazuh security monitoring platform.
- Identify the role of endpoint monitoring and log collection.
- Understand basic threat detection and incident triage.
- Review available hardware and software resources.
- Plan the architecture of a home SOC laboratory using VMware Workstation.

2. What is a Security Operations Center (SOC)?

A Security Operations Center (SOC) is a centralized security function responsible for continuously monitoring, detecting, analysing and responding to cybersecurity threats.

A SOC helps organizations protect their systems, networks, applications and sensitive information against cyberattacks.

Main Responsibilities of a SOC

1. Security Monitoring — Continuously monitor network traffic, endpoints and security events.
2. Threat Detection — Identify suspicious activities and potential cyber threats.
3. Alert Investigation — Analyse security alerts and determine whether they are legitimate threats.
4. Incident Response — Take appropriate action when a security incident is identified.
5. Threat Intelligence — Use Indicators of Compromise (IOCs) to identify malicious infrastructure and activities.
6. Reporting — Document investigations, findings and incident response activities.

SOC Team Structure

Tier 1 — SOC Analyst

- Monitor SIEM dashboards.
- Review incoming security alerts.
- Perform initial alert triage.
- Check suspicious IP addresses, usernames and timestamps.
- Review endpoint and authentication logs.
- Escalate suspicious or confirmed incidents to Tier 2.

Tier 2 — SOC Analyst

- Perform deeper security investigations.
- Correlate events from multiple sources.
- Investigate malware and suspicious processes.
- Support incident containment and response.

Tier 3 — Senior Analyst / Threat Hunter

- Perform advanced threat hunting.
- Investigate complex attacks.
- Develop detection strategies.
- Analyse advanced persistent threats (APTs).
- Improve security monitoring and detection capabilities.

3. Understanding SIEM

SIEM stands for Security Information and Event Management.

A SIEM platform collects security logs from different sources, analyses events, identifies suspicious patterns and generates security alerts.

It helps SOC analysts monitor security activities from a centralized platform.

Main Functions of SIEM

1. Log Collection

Collect logs from:

- Windows endpoints.
- Linux servers.
- Firewalls.
- Routers.
- Applications.
- Authentication systems.

2. Log Normalization

Convert different log formats into structured information so they can be analysed consistently.

3. Event Analysis

Analyse incoming events using detection rules and security logic.

4. Event Correlation

Identify relationships between multiple events to detect suspicious patterns.

5. Alert Generation

Generate alerts when suspicious activities match detection conditions.

6. Investigation

SOC analysts investigate alerts to determine whether the activity is legitimate, suspicious or malicious.

Real-World Example: Brute-Force Login Attempt

Suppose an account experiences 20 failed login attempts within a short period.

A SIEM may identify the repeated authentication failures and generate an alert.

A Tier 1 SOC analyst investigates:

- Username involved.
- Source IP address.
- Event timestamps.
- Number of failed login attempts.
- Whether a successful login followed.
- Whether the source IP is known or suspicious.
- Whether the activity is legitimate or malicious.

MITRE ATT&CK Mapping:

- Tactic: Credential Access.
- Technique: T1110 — Brute Force.

Note: The detection depends on available telemetry and configured detection rules.

4. Understanding Security Logs

A security log is a record of activities performed on an operating system, application or network device.

Logs help security analysts understand what happened, when it happened and which account or system was involved.

Important Windows Security Event IDs

Event ID| Description
4624| Successful logon
4625| Failed logon
4634| Account logged off
4648| Logon attempted using explicit credentials
4672| Special privileges assigned to a new logon
4688| New process created
4720| User account created
4722| User account enabled
4726| User account deleted
4732| Member added to a security-enabled local group

These events are useful during authentication monitoring and endpoint investigations.

Note: Some events require specific Windows audit policies to be enabled.

5. Introduction to Wazuh

Wazuh is an open-source security platform used for security monitoring, threat detection, log analysis, vulnerability detection and endpoint security.

For this project, Wazuh will be our primary SIEM and endpoint monitoring platform.

Main Wazuh Components

1. Wazuh Agent

The Wazuh Agent is installed on monitored endpoints.

Responsibilities:

- Collect system and security information.
- Monitor configured log sources.
- Collect relevant security events.
- Send collected data to the Wazuh Server.

2. Wazuh Server

The Wazuh Server is responsible for:

- Receiving data from agents.
- Analysing incoming events.
- Applying detection rules.
- Generating security alerts.
- Managing connected agents.

3. Wazuh Indexer

The Wazuh Indexer stores and indexes security event data.

It allows analysts to search and analyse indexed security information.

4. Wazuh Dashboard

The Wazuh Dashboard provides a graphical interface for:

- Monitoring security alerts.
- Viewing endpoint information.
- Searching security events.
- Investigating suspicious activities.
- Reviewing security monitoring results.

Wazuh Data Flow

1. The Wazuh Agent collects endpoint information.
2. The agent sends collected data to the Wazuh Server.
3. The server analyses incoming events.
4. Detection rules identify potentially suspicious activities.
5. Security alerts are indexed.
6. Analysts investigate alerts through the Dashboard.

6. Understanding Endpoint Monitoring

An endpoint is a device connected to a network, such as:

- Desktop computers.
- Laptops.
- Servers.
- Workstations.

Endpoint monitoring helps security teams observe activities occurring on these devices.

Examples of activities monitored:

- Successful and failed logins.
- New process creation.
- User account changes.
- Privilege assignments.
- Suspicious system activity.
- File integrity changes.

For this project, Windows 11 will initially act as our monitored endpoint.

7. Lab Environment — Hardware & Software

Host Machine Specifications

Component| Configuration
Host Operating System| Windows 11
Processor| AMD Ryzen 5 7000 Series
RAM| 16 GB
Storage| 512 GB SSD
Virtualization Support| Available
Hypervisor| VMware Workstation

Existing Software Environment

Software| Purpose
Windows 11| Main host operating system
VMware Workstation| Virtual machine management
Kali Linux| Cybersecurity learning environment
Ubuntu WSL2| Linux command-line environment
Docker| Containerization and application deployment

Planned SOC Lab Software

Software| Purpose
Ubuntu Server| Host Wazuh infrastructure
Wazuh Agent| Endpoint monitoring
Wazuh Server| Security event analysis
Wazuh Indexer| Security event storage and indexing
Wazuh Dashboard| Security monitoring and investigation
GitHub| Project documentation and version control

8. Virtualization Environment — VMware Workstation

Virtualization allows multiple operating systems to run as virtual machines on a single physical computer.

We will use VMware Workstation as the primary hypervisor for this project.

What is a Hypervisor?

A hypervisor is software that creates and manages virtual machines.

It allocates physical resources such as CPU, RAM, storage and network connectivity to virtual machines.

Planned Virtual Machine Configuration

Resource| Planned Allocation
Virtual Machine| Ubuntu Server
RAM| 8 GB
CPU| 4 virtual cores
Storage| 60 GB
Network Adapter| NAT
Purpose| Wazuh SIEM infrastructure

These are initial planning values, not confirmed deployed settings.

Monitored Endpoint

Initially, the Windows 11 host will act as the monitored endpoint.

The Wazuh Agent will be installed on Windows to collect security events.

A separate Windows VM may be introduced later if additional isolation and testing capabilities are needed.

9. Planned SOC Lab Architecture

             PHYSICAL COMPUTER
                    |
               Windows 11
                HOST OS
                    |
             VMware Workstation
                    |
             Ubuntu Server VM
                    |
             WAZUH INFRASTRUCTURE
                    |
          +---------+---------+
          |         |         |
       Wazuh     Wazuh     Wazuh
       Server   Indexer   Dashboard
          |
       Analysis
          |
       Alerts
          |
       Investigation


       MONITORED ENDPOINT
              |
       Windows 11 Host
              |
         Wazuh Agent
              |
       Security Events
              |
         Wazuh Server

Network Architecture

Initial planned network configuration:

- Hypervisor: VMware Workstation.
- Ubuntu Server VM: VMware NAT networking.
- Windows Endpoint: Windows 11 host.
- Communication: Wazuh Agent to Wazuh Server.

Later, we will evaluate Host-only networking for isolated lab exercises.

10. Practical Activities — System Information

The following commands will help verify system resources before deploying the Wazuh infrastructure.

A. Windows PowerShell Commands

Check System Information

systeminfo

Purpose:

- Display Windows operating system information.
- Identify processor details.
- Review installed memory.
- Check system configuration.

Check Available Memory

Get-CimInstance Win32_OperatingSystem |
Select-Object TotalVisibleMemorySize, FreePhysicalMemory

Purpose:

- Identify total visible physical memory.
- Identify currently available physical memory.

Check Processor Information

Get-CimInstance Win32_Processor |
Select-Object Name, NumberOfCores, NumberOfLogicalProcessors

Purpose:

- Display processor name.
- Identify physical CPU cores.
- Identify logical processors.

B. Linux Commands

Check Available RAM

free -h

Purpose: Display total, used and available memory.

Check CPU Cores

nproc

Purpose: Display the number of available processing units.

Check Disk Space

df -h

Purpose: Display filesystem storage usage.

Check Network Interfaces

ip addr

Purpose: Display available network interfaces and IP addresses.

These commands are for system verification. Their outputs must be recorded after actual execution.

11. Important Cybersecurity Terminology

Term| Meaning
SOC| Security Operations Center
SIEM| Security Information and Event Management
Endpoint| Device connected to a network
Log| Recorded system or application event
Alert| Notification of potentially suspicious activity
IOC| Indicator of Compromise
Event Correlation| Connecting related security events
Incident Triage| Initial assessment of a security alert
Threat Intelligence| Information about potential or existing threats
MITRE ATT&CK| Framework describing adversary tactics and techniques
Hypervisor| Software that manages virtual machines
Wazuh Agent| Endpoint monitoring component
Wazuh Server| Central security analysis component

12. MITRE ATT&CK Introduction

MITRE ATT&CK is a knowledge base that describes the tactics, techniques and procedures used by adversaries.

It helps security analysts understand and classify attacker behaviour.

Example:

Technique ID| Technique| Purpose
T1110| Brute Force| Repeatedly attempting authentication
T1078| Valid Accounts| Using legitimate account credentials
T1059| Command and Scripting Interpreter| Executing commands through scripting environments
T1053| Scheduled Task/Job| Using scheduled tasks or jobs for execution or persistence

SOC analysts use this framework to map observed behaviour to known adversary techniques.

13. Day 1 Practical Observations

System verification: Pending actual command execution.

VMware Workstation: Confirmed available.

Ubuntu Server deployment: Not yet performed.

Wazuh installation: Not yet performed.

Windows Agent integration: Not yet performed.

Network connectivity testing: Pending.

Important: Planned activities must not be recorded as completed practical work.

14. Day 1 Learning Outcomes

After completing the learning portion of Day 1, I understand:

- The purpose and responsibilities of a Security Operations Center.
- The role of a Tier 1 SOC Analyst.
- SIEM fundamentals and the log analysis process.
- The main components of Wazuh.
- How endpoint monitoring works.
- The importance of Windows Security Event Logs.
- Basic MITRE ATT&CK concepts.
- The role of virtualization in building a cybersecurity lab.
- The proposed architecture of my home SOC environment.

15. Day 1 Completion Checklist

Theory

- [x] Understand SOC fundamentals.
- [x] Understand SOC Tier 1 responsibilities.
- [x] Understand SIEM architecture.
- [x] Study Wazuh components.
- [x] Learn basic Windows Security Event IDs.
- [x] Understand endpoint monitoring.
- [x] Introduction to MITRE ATT&CK.

Lab Preparation

- [x] Review hardware specifications.
- [x] Confirm VMware Workstation availability.
- [x] Plan Ubuntu Server VM configuration.
- [x] Plan Windows endpoint monitoring.
- [ ] Execute Windows resource-check commands.
- [ ] Execute Linux resource-check commands.
- [ ] Record actual system outputs.
- [ ] Verify VMware networking.

Deployment

- [ ] Ubuntu Server VM creation.
- [ ] Ubuntu Server installation.
- [ ] Wazuh deployment.
- [ ] Windows Wazuh Agent installation.
- [ ] Endpoint connectivity verification.

16. Key Takeaways

Day 1 established the theoretical foundation and proposed infrastructure for a practical SOC laboratory.

I learned that a SOC relies on continuous monitoring, security event analysis, threat detection and incident response.

I also understood how Wazuh can be used to monitor endpoints and analyse security events.

My next objective is to deploy Ubuntu Server using VMware Workstation and prepare the infrastructure for Wazuh installation.

17. References & Learning Resources

1. Wazuh Official Documentation:
   https://documentation.wazuh.com/

2. MITRE ATT&CK Framework:
   https://attack.mitre.org/

3. Microsoft Windows Documentation:
   https://learn.microsoft.com/windows/

4. VMware Documentation:
   https://docs.vmware.com/

5. Microsoft Windows Security Auditing:
   https://learn.microsoft.com/windows/security/threat-protection/auditing/

---

Day 2 — Next Session

Topic: Ubuntu Server VM Creation and Initial Configuration.

Planned activities:

1. Verify VMware Workstation configuration.
2. Download Ubuntu Server ISO.
3. Create a new Ubuntu Server VM.
4. Allocate RAM, CPU and storage.
5. Configure VMware NAT networking.
6. Install Ubuntu Server.
7. Verify network connectivity.
8. Update Ubuntu packages.
9. Prepare the system for Wazuh deployment.

Learning Workflow:

Learn → Practise → Investigate → Document → Commit.

Project Objective: Build a practical SOC home lab and demonstrate real-world security monitoring and investigation skills.
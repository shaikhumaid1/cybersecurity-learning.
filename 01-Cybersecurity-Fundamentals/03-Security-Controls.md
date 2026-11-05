Security Controls in Cybersecurity

1. Introduction

Security controls are safeguards implemented by organizations to protect systems, networks, applications and sensitive information from security threats.

Their purpose is to reduce risk and protect the CIA Triad:

- Confidentiality
- Integrity
- Availability

2. Types of Security Controls

Security controls are commonly classified by their function.

2.1 Preventive Controls

Definition: Controls designed to prevent security incidents before they occur.

Examples:

- Firewalls
- Multi-Factor Authentication (MFA)
- Strong password policies
- Access control
- Network segmentation

SOC Example: A firewall blocks unauthorized inbound connections before they reach an internal server.

2.2 Detective Controls

Definition: Controls designed to identify and alert security teams when suspicious activity occurs.

Examples:

- Intrusion Detection Systems (IDS)
- Security Information and Event Management (SIEM)
- Log monitoring
- File Integrity Monitoring
- Endpoint Detection and Response (EDR)

SOC Example: A SIEM generates an alert after detecting multiple failed login attempts from the same IP address.

2.3 Corrective Controls

Definition: Controls designed to reduce the impact of an incident and restore systems to a secure state.

Examples:

- Removing malware
- Patching vulnerable software
- Restoring systems from backups
- Resetting compromised credentials

SOC Example: After identifying a compromised account, the security team disables the account and resets its credentials.

3. Other Security Control Categories

Deterrent Controls

Discourage attackers from attempting unauthorized activities.

Examples:

- Warning banners
- Security policies
- Visible security cameras

Compensating Controls

Alternative safeguards used when the primary security control cannot be implemented.

Example:

If an old system cannot support MFA, an organization may restrict its network access and closely monitor its activity as additional safeguards.

Recovery Controls

Help restore operations after a security incident or disruption.

Examples:

- Disaster recovery plans
- Backup restoration
- Business continuity procedures

4. Comparison Table

Control| Main Purpose| Example
Preventive| Stop incidents| Firewall
Detective| Identify incidents| SIEM
Corrective| Fix incidents| Malware removal
Deterrent| Discourage attacks| Warning banner
Compensating| Provide alternative protection| Restricted network access
Recovery| Restore operations| Backup restoration

5. SOC Analyst Perspective

Scenario:

A SIEM detects 30 failed login attempts against an employee account within two minutes.

Possible security controls:

1. Detective: SIEM identifies suspicious login activity.
2. Preventive: Account lockout policy limits further attempts.
3. Corrective: Security team resets credentials if compromise is confirmed.
4. Recovery: Restore affected systems if the incident caused disruption.

A Tier 1 SOC Analyst should review the alert, investigate authentication logs, identify the source IP, check for successful logins and escalate according to the organization's procedures.

6. Key Takeaways

- Preventive controls reduce the likelihood of incidents.
- Detective controls identify suspicious activities.
- Corrective controls address security problems.
- Deterrent controls discourage unwanted behavior.
- Compensating controls provide alternative protection.
- Recovery controls restore normal operations.

7. Revision Questions

1. What are security controls?
2. Differentiate preventive and detective controls.
3. Give three examples of corrective controls.
4. How does a SIEM support detective controls?
5. What is a compensating control?
6. Explain how security controls work together during a brute-force attack.

---

Topic: Cybersecurity Fundamentals
Status: Theory documented
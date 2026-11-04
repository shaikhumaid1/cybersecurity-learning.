CIA Triad – Core Principles of Cybersecurity

1. Introduction

The CIA Triad is a fundamental information security model consisting of three principles:

- Confidentiality
- Integrity
- Availability

It helps organizations protect information systems and maintain data security.

2. Confidentiality

Definition: Ensures that information is accessible only to authorized individuals, systems or processes.

Objective: Prevent unauthorized disclosure of sensitive information.

Examples:

- Password-protecting user accounts.
- Encrypting sensitive files.
- Applying role-based access control (RBAC).
- Using multi-factor authentication (MFA).

Real-world scenario:

An attacker steals customer records from a company's database and exposes personal information.

Security impact: Confidentiality is violated because sensitive information is disclosed without authorization.

3. Integrity

Definition: Ensures that information remains accurate, complete and protected against unauthorized modification or destruction.

Objective: Prevent unauthorized changes to data.

Examples:

- Using cryptographic hashes such as SHA-256.
- Implementing file integrity monitoring.
- Maintaining audit logs.
- Applying appropriate access permissions.

Real-world scenario:

An attacker modifies customer bank balances without authorization.

Security impact: Integrity is violated because financial records have been changed without permission.

4. Availability

Definition: Ensures that information, services and systems are accessible to authorized users when required.

Objective: Maintain reliable access to systems and data.

Examples:

- Regular data backups.
- Redundant servers.
- Disaster recovery planning.
- DDoS protection.

Real-world scenario:

An attacker launches a Distributed Denial-of-Service (DDoS) attack against an online banking service.

Security impact: Availability is violated because legitimate customers cannot access the service.

5. CIA Triad Comparison

Principle| Protects Against| Example
Confidentiality| Unauthorized disclosure| Data theft
Integrity| Unauthorized modification| Balance manipulation
Availability| Service disruption| DDoS attack

6. SOC Analyst Perspective

A SOC Analyst uses the CIA Triad to understand the potential impact of security alerts.

Security Alert| Possible Impact
Unauthorized database access| Confidentiality
Unexpected file modification| Integrity
Server unavailable| Availability
Ransomware encrypting files| Integrity and Availability

Investigation example:

Alert: Multiple failed login attempts followed by a successful login from an unfamiliar IP address.

A Tier 1 SOC Analyst should:

1. Review authentication logs.
2. Identify the source IP address.
3. Check timestamps and affected accounts.
4. Determine whether unauthorized access occurred.
5. Look for data access or modification.
6. Escalate confirmed suspicious activity according to the incident response process.

7. Key Takeaways

- Confidentiality protects information from unauthorized disclosure.
- Integrity protects information from unauthorized modification.
- Availability ensures reliable access to systems and information.
- Security controls help organizations maintain all three principles.

8. Revision Questions

1. What are the three principles of the CIA Triad?
2. Which principle is violated when an attacker modifies financial records?
3. How does encryption support confidentiality?
4. How can backups improve availability?
5. Which CIA principles can ransomware affect?

---

Topic: Cybersecurity Fundamentals
Status: Completed – Theory and practical examples
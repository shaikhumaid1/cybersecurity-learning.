# Week 2 — Day 2: TCP/IP Model and Encapsulation

**Learning Track:** Cybersecurity & SOC Analyst  
**Topic:** TCP/IP Model, TCP vs UDP, Three-Way Handshake and Encapsulation  
**Assessment Score:** 6/7 (85.7%)  
**Status:** Theory and assessment completed

---

## 1. Introduction to TCP/IP

TCP/IP stands for Transmission Control Protocol / Internet Protocol.

It is the protocol suite used for communication across the internet and modern computer networks.

The TCP/IP reference model consists of four layers:

1. Application
2. Transport
3. Internet
4. Network Access

## 2. OSI vs TCP/IP Model

| OSI Layers | TCP/IP Layers |
|---|---|
| Application, Presentation, Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link, Physical | Network Access |

**Key Difference:** OSI is a seven-layer conceptual reference model, while the four-layer TCP/IP model describes the architecture of the Internet protocol suite.

## 3. TCP/IP Layers and Protocols

| Layer | Function | Protocols / Technologies |
|---|---|---|
| Application | Provides network services to applications | HTTP, HTTPS, DNS, SSH, SMTP |
| Transport | End-to-end communication and port addressing | TCP, UDP |
| Internet | Logical IP addressing and routing | IPv4, IPv6, ICMP |
| Network Access | Local network delivery and physical transmission | Ethernet, Wi-Fi |

## 4. TCP vs UDP

### TCP (Transmission Control Protocol)

- Connection-oriented.
- Uses a three-way handshake.
- Provides reliable, ordered byte-stream delivery.
- Uses acknowledgements and retransmissions.
- Commonly used for SSH, HTTPS and email protocols.

### UDP (User Datagram Protocol)

- Connectionless.
- Does not require a three-way handshake.
- Has lower protocol overhead.
- Does not guarantee delivery or ordering.
- Commonly used for DNS, VoIP, gaming and streaming.

**Important:** UDP-based applications can implement their own reliability mechanisms. HTTP/3 uses QUIC over UDP.

## 5. TCP Three-Way Handshake

TCP establishes a connection through three steps:

1. **SYN:** The client requests a connection.
2. **SYN-ACK:** The server acknowledges the request and sends its own SYN.
3. **ACK:** The client acknowledges the server.

Sequence:

```text
Client                    Server
   |                         |
   | -------- SYN ---------> |
   |                         |
   | <------ SYN-ACK ------- |
   |                         |
   | -------- ACK ---------> |
   |                         |
   |   Connection Established|
```

### SOC Relevance

A large number of SYN packets without completed handshakes may indicate scanning, a SYN flood, connectivity problems or other network conditions.

An analyst must correlate traffic patterns before confirming malicious activity.

## 6. Encapsulation and Decapsulation

Encapsulation is the process of adding protocol headers, and sometimes trailers, as data moves down the network stack.

```text
Application Data
       ↓
TCP Segment / UDP Datagram
       ↓
IP Packet
       ↓
Ethernet Frame
       ↓
Bits transmitted
```

Decapsulation is the reverse process at the receiving device.

### Protocol Data Units (PDUs)

| Layer | PDU |
|---|---|
| Application | Data |
| Transport | TCP Segment / UDP Datagram |
| Internet | Packet |
| Network Access | Frame / Bits |

## 7. Common Ports and Protocols

| Port | Protocol | Service |
|---|---|---|
| 21 | TCP | FTP control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67/68 | UDP | DHCP |
| 80 | TCP | HTTP |
| 123 | UDP | NTP |
| 443 | TCP/UDP | HTTPS (TCP) / HTTP/3 over QUIC (UDP) |
| 445 | TCP | SMB |

Port numbers help identify services, but applications can use nonstandard ports.

## 8. Practical Commands

### Windows PowerShell

Check IP configuration:

```powershell
ipconfig
```

Display the routing table:

```powershell
route print
```

Display network connections:

```powershell
netstat -ano
```

Check DNS resolution:

```powershell
nslookup example.com
```

Test TCP port 443:

```powershell
Test-NetConnection example.com -Port 443
```

### Linux

```bash
ip addr
ip route
ss -tuln
```

## 9. Wireshark Display Filters

```text
tcp
udp
dns
tcp.flags.syn == 1
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

These filters help identify TCP connections, UDP traffic, DNS queries and TCP handshake activity.

## 10. SOC Analyst Investigation Scenario

**Alert:** High-frequency outbound TCP connections.

```text
Source IP:        192.168.1.15
Destination IP:   45.80.XX.XX
Protocol:         TCP
Destination Port: 443
Connection State: Established
Frequency:        500 connections / 5 minutes
```

### Investigation Steps

1. Identify the affected endpoint and its owner.
2. Analyse the connection frequency and timestamps.
3. Investigate the destination IP and related domain.
4. Identify the originating process, PID and user.
5. Correlate firewall, DNS, network and endpoint logs.
6. Determine whether the behaviour is expected or suspicious.
7. Document the evidence and escalate to SOC Tier 2 when warranted.

### Key Learning

- High connection frequency does not automatically indicate an attack.
- A successful TCP handshake does not prove that traffic is safe.
- Endpoint and network evidence should be correlated.
- Escalation should include evidence and follow the organisation's incident response playbook.

## 11. MITRE ATT&CK Relevance

- **T1071 — Application Layer Protocol:** Adversaries may use application protocols for command-and-control communication.
- **T1046 — Network Service Discovery:** Adversaries may probe systems for available services.
- **T1498 — Network Denial of Service:** Adversaries may disrupt network availability through traffic-based attacks.

These are possible investigation mappings, not confirmed techniques based solely on a port or connection count.

## 12. Assessment and Progress

**Day 2 Quiz:** 6/7 — 85.7%

**SOC Scenario Evaluation:** 8/10

### Skills Practised in the Assessment

- Recognising abnormal connection frequency.
- Understanding TCP connection establishment.
- Investigating source and destination information.
- Identifying endpoint processes and reviewing logs.
- Knowing when to escalate an incident.

## 13. Practical Lab Record

Complete this section after performing the lab.

- **Operating System:** Windows 11 / Ubuntu WSL
- **Tools:** PowerShell, Linux terminal, Wireshark
- **IP Configuration:** Pending
- **TCP Connection Observations:** Pending
- **Wireshark Capture Findings:** Pending

## 14. Completion Checklist

- [x] Studied TCP/IP model
- [x] Compared OSI and TCP/IP
- [x] Understood TCP vs UDP
- [x] Studied the TCP three-way handshake
- [x] Understood encapsulation and decapsulation
- [x] Completed Day 2 assessment
- [x] Completed SOC investigation scenario
- [ ] Verified practical command outputs
- [ ] Completed Wireshark packet capture
- [ ] Documented practical observations

---

**Next Topic:** Week 2 — Day 3: IPv4 Addressing, Subnetting and CIDR

**Career Focus:** SOC Analyst Tier 1 — Network Traffic Analysis and Incident Triage

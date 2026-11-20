# Week 2 — Day 4: TCP, UDP and Common Ports

## Learning Objectives

By the end of this lesson, I should be able to:

- Explain what a port is and how it works with an IP address.
- Differentiate TCP and UDP.
- Explain the TCP three-way handshake.
- Recognize common service ports and their usual transport protocols.
- Inspect listening ports, active connections and process IDs on Windows.
- Apply port knowledge to basic SOC alert triage.

---

## 1. What Is a Port?

An IP address identifies a host/interface on an IP network. A transport-layer port identifies the application endpoint on that host.

A connection is commonly identified by a 5-tuple:

1. Source IP address
2. Source port
3. Destination IP address
4. Destination port
5. Transport protocol (TCP or UDP)

**Example**

A laptop `192.168.1.25` connects to a website at `203.0.113.20` using TCP port `443`.

- Source IP: `192.168.1.25`
- Source port: an ephemeral client port, for example `51524`
- Destination IP: `203.0.113.20`
- Destination port: `443`
- Protocol: TCP

The client-side port is usually selected dynamically. The server listens on a known service port.

**Analogy:** An IP address is like a building's street address; a port is like the room or department inside the building.

### Port number ranges

| Range | Name | Notes |
|---|---|---|
| 0–1023 | Well-known ports | Common standard services |
| 1024–49151 | Registered ports | Registered for particular applications/services |
| 49152–65535 | Dynamic/private ports | Commonly used as temporary client-side ports |

Operating systems may use different ephemeral port ranges, so the exact range can vary.

---

## 2. TCP (Transmission Control Protocol)

TCP is connection-oriented. It establishes a connection before normal application data transfer and provides mechanisms for reliable, ordered delivery.

Important TCP characteristics:

- Connection establishment and termination.
- Sequence numbers and acknowledgements.
- Retransmission when segments are not acknowledged.
- Flow control and congestion control.
- Ordered byte-stream delivery to the application.

### TCP three-way handshake

The normal connection setup uses three steps:

| Step | Packet | Meaning |
|---|---|---|
| 1 | SYN | Client requests a connection |
| 2 | SYN-ACK | Server acknowledges and responds |
| 3 | ACK | Client acknowledges the response |

```text
Client                                  Server
  | -------- SYN ----------------------> |
  | <----- SYN-ACK --------------------- |
  | -------- ACK ----------------------> |
  |                                      |
  |          Connection established      |
```

**Real-life example — opening a secure website**

When a browser connects to a website using HTTPS over TCP:

1. The client resolves the website name to an IP address.
2. The client establishes a TCP connection to the server's TCP port 443.
3. The client and server complete the TCP handshake.
4. TLS negotiation takes place to establish encryption and authenticate the server.
5. HTTPS application data is exchanged inside the protected connection.

Important: HTTP/3 uses QUIC over UDP, so not every HTTPS connection uses TCP.

### TCP flags to recognize

| Flag | Meaning |
|---|---|
| SYN | Starts a connection |
| ACK | Acknowledges received data/sequence |
| FIN | Graceful connection close |
| RST | Resets/aborts a connection |
| PSH | Requests prompt delivery of buffered data to the application |
| URG | Marks urgent data (rare in modern traffic) |

---

## 3. UDP (User Datagram Protocol)

UDP is connectionless. It sends datagrams without establishing a TCP-style connection first.

Important UDP characteristics:

- No TCP-style handshake.
- No built-in guarantee of delivery.
- No built-in guarantee of ordering.
- No TCP-style retransmission or flow control.
- Lower protocol overhead than TCP.

Applications may implement their own reliability, sequencing or recovery mechanisms where needed.

**Real-life examples**

- A DNS query commonly uses UDP port 53.
- Voice/video calls and live streams often use UDP because timely delivery can matter more than retransmitting delayed packets.
- DHCP uses UDP to allow a client to communicate before it has a normal IP configuration.

UDP is not inherently insecure. Security depends on the application, configuration and surrounding controls.

---

## 4. TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection model | Connection-oriented | Connectionless |
| Handshake | Yes | No TCP-style handshake |
| Delivery guarantee | Reliable transport mechanisms | No built-in delivery guarantee |
| Ordering | Ordered byte stream | No built-in ordering |
| Retransmission | Yes | No built-in retransmission |
| Overhead | Higher | Lower |
| Common examples | SSH, SMTP, HTTPS over TCP, SMB | DNS queries, DHCP, NTP, many real-time applications |

**Common exam trap:** UDP is not always faster in every situation, and TCP is not automatically more secure. They provide different transport behaviours; security is provided by protocols and controls above or around them.

---

## 5. Common Ports and Services

Know the purpose of these ports and their usual transport protocols. A service can sometimes be configured to use a non-default port.

| Port | Service | Usual transport | Purpose |
|---:|---|---|---|
| 20 | FTP data | TCP | FTP data channel in active mode |
| 21 | FTP control | TCP | FTP control/session commands |
| 22 | SSH | TCP | Secure remote administration |
| 23 | Telnet | TCP | Legacy remote terminal; plaintext |
| 25 | SMTP | TCP | Mail transfer between servers |
| 53 | DNS | UDP and TCP | Name resolution; TCP also used for larger responses and zone transfers |
| 67 | DHCP server | UDP | DHCP server/listener |
| 68 | DHCP client | UDP | DHCP client/listener |
| 69 | TFTP | UDP | Simple file transfer |
| 80 | HTTP | TCP | Web traffic without TLS |
| 110 | POP3 | TCP | Email retrieval |
| 123 | NTP | UDP | Time synchronization |
| 143 | IMAP | TCP | Email access and synchronization |
| 161 | SNMP | UDP | Network device management queries |
| 162 | SNMP trap | UDP | SNMP notifications/traps |
| 389 | LDAP | TCP/UDP | Directory services |
| 443 | HTTPS | TCP; QUIC uses UDP | Encrypted web traffic / HTTP over QUIC |
| 445 | SMB | TCP | Windows file and printer sharing |
| 514 | Syslog | UDP commonly; TCP also possible | Log forwarding |
| 587 | SMTP submission | TCP | Authenticated mail submission |
| 636 | LDAPS | TCP | LDAP over TLS |
| 993 | IMAPS | TCP | IMAP over TLS |
| 995 | POP3S | TCP | POP3 over TLS |
| 1433 | Microsoft SQL Server | TCP commonly | Database connectivity |
| 3306 | MySQL | TCP | Database connectivity |
| 3389 | RDP | TCP and UDP | Windows Remote Desktop |

### Important security observations

- Telnet (23) and FTP (21) are legacy protocols that may expose credentials or data without additional protection.
- SSH (22) is preferred over Telnet for secure remote administration.
- SMB (445) should generally be restricted to trusted network segments and required hosts.
- RDP (3389) should not be exposed directly to the public internet without a carefully designed secure access architecture.
- DNS (53) is not always UDP; TCP is also important.
- Port numbers are clues, not proof of the application or intent. Applications can use non-standard ports, and traffic can be tunnelled.

---

## 6. Practical Lab 1 — Inspect Listening Ports on Windows

**Environment:** Windows 11 PowerShell.

Open PowerShell and run:

```powershell
netstat -ano
```

What the columns mean:

| Column | Meaning |
|---|---|
| Proto | Transport protocol, such as TCP or UDP |
| Local Address | Local IP address and port |
| Foreign Address | Remote IP address and port |
| State | TCP state, where applicable |
| PID | Process ID associated with the socket |

Example (illustrative output):

```text
Proto  Local Address       Foreign Address       State         PID
TCP    192.168.1.25:51524  203.0.113.20:443      ESTABLISHED   4200
TCP    0.0.0.0:3389       0.0.0.0:0             LISTENING     1180
```

Interpretation:

- The first row shows an established TCP connection to remote port 443.
- The second row shows a process listening on local TCP port 3389 on all IPv4 interfaces.
- This is an illustrative example, not a claim about the current computer.

### Show TCP listening sockets

```powershell
netstat -ano -p tcp | findstr LISTENING
```

### Show connections involving a particular port

```powershell
netstat -ano | findstr :443
```

### Identify the process using a PID

Replace `4200` with a PID from your own output:

```powershell
tasklist /FI "PID eq 4200"
```

For more detail, use PowerShell:

```powershell
Get-Process -Id 4200
Get-NetTCPConnection
Get-NetTCPConnection -State Listen
```

**What to record:** Date/time, local address and port, remote address and port, state, PID, process name and whether the connection is expected.

Do not terminate or block a process only because its port looks unfamiliar. First identify the process and understand its role.

---

## 7. Practical Lab 2 — Test a TCP Port

Run:

```powershell
Test-NetConnection example.com -Port 443
```

Look for:

```text
TcpTestSucceeded : True
```

This indicates that the TCP connection test to the destination port succeeded at the time of the test.

Test another common port:

```powershell
Test-NetConnection example.com -Port 22
```

A failed test does not automatically mean the service is down. A firewall, routing policy, host policy or remote server configuration may block the connection.

---

## 8. Practical Lab 3 — Linux Socket Inspection

On Linux (for example, Kali or Ubuntu), run:

```bash
ss -tuln
```

Options:

- `-t`: TCP sockets
- `-u`: UDP sockets
- `-l`: listening sockets
- `-n`: show numeric addresses and port numbers

Show process information (may require elevated permissions):

```bash
sudo ss -tulpn
```

Show established TCP connections:

```bash
ss -tn state established
```

Alternative:

```bash
sudo lsof -i -P -n
```

---

## 9. Wireshark — Useful Display Filters

Capture traffic only on a network you own or are authorized to monitor.

| Display filter | What it shows |
|---|---|
| `tcp` | TCP packets |
| `udp` | UDP packets |
| `tcp.port == 443` | TCP traffic using port 443 |
| `udp.port == 53` | UDP traffic using port 53 |
| `dns` | DNS packets |
| `tcp.flags.syn == 1` | TCP packets with SYN set |
| `tcp.flags.reset == 1` | TCP packets with RST set |
| `tcp.port == 22` | TCP traffic using SSH's usual port |
| `tcp.port == 445` | TCP traffic using SMB's usual port |

### Mini exercise

1. Open Wireshark and select your active network interface.
2. Start a short capture.
3. Visit a website or run `nslookup example.com`.
4. Stop the capture.
5. Apply `dns` and inspect the query and response.
6. Apply `tcp.flags.syn == 1` and inspect connection attempts.
7. Apply `tcp.port == 443` to inspect HTTPS-over-TCP traffic, if present.

**Note:** Modern browsers may use HTTP/3 over QUIC (UDP 443). Therefore, a filter for `tcp.port == 443` may not show every HTTPS connection.

---

## 10. Real-World SOC Case Study — Suspicious Outbound Connection

### Scenario

A SIEM alert reports that workstation `192.168.1.25` made repeated outbound connections to `198.51.100.45` on TCP port `4444`.

Illustrative alert details:

| Field | Value |
|---|---|
| Source host | `WS-025` |
| Source IP | `192.168.1.25` |
| Destination IP | `198.51.100.45` |
| Destination port | `4444/TCP` |
| Event | Repeated outbound connection attempts |
| Time window | 5 minutes |

The address ranges `198.51.100.0/24` and `203.0.113.0/24` are reserved for documentation/examples. These are fictional case-study values, not live threat indicators.

### Tier 1 analyst investigation

**Step 1 — Validate the alert**

- Confirm source host and destination details.
- Check event timestamps and connection frequency.
- Determine whether the connection succeeded or failed.
- Check whether the activity is expected for this asset.

**Step 2 — Identify the process**

On the endpoint, correlate the connection with its PID and process:

```powershell
Get-NetTCPConnection -RemotePort 4444
Get-Process -Id <PID>
```

Use the actual PID returned by your investigation. Review executable path, signer, parent process, command line and user context where available.

**Step 3 — Correlate logs**

Review relevant sources:

- Windows Security and Sysmon events, if configured.
- EDR process and network telemetry.
- Firewall or proxy logs.
- DNS query logs.
- User and endpoint activity around the alert time.

**Step 4 — Assess destination and behaviour**

- Is the destination expected for the business?
- Is the destination reputation known and reliable?
- Did the same process contact other unfamiliar destinations?
- Did a suspicious process start shortly before the connection?
- Is there unusual persistence, privilege escalation or lateral movement evidence?

**Step 5 — Respond according to evidence and playbook**

- Document findings and timestamps.
- Escalate to L2/incident response if indicators support compromise.
- Request containment through approved procedures when warranted.
- Preserve relevant evidence.

**Important:** TCP port 4444 is sometimes associated with testing tools and can also be used by legitimate software. The port alone does not prove malware or compromise.

### MITRE ATT&CK context

Depending on what the investigation establishes, outbound command-and-control behaviour may be relevant to:

- **TA0011 — Command and Control**
- **T1071 — Application Layer Protocol** (only when the observed behaviour supports it)

Do not map an alert to a technique solely from the port number.

---

## 11. Real-World Example — RDP Exposure

A company notices repeated connection attempts to TCP port 3389 on a Windows server.

A SOC analyst should:

1. Identify the source IP addresses and time pattern.
2. Check Windows logon events, including successful and failed remote logons.
3. Correlate source IP, account, logon type and endpoint.
4. Determine whether the remote access is expected and authorized.
5. Look for unusual account use or follow-on activity.
6. Escalate and follow the organization's response procedure if suspicious.

Useful Windows Security Event IDs (when the relevant audit policy is enabled):

| Event ID | Meaning |
|---:|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4778 | Session reconnected |
| 4779 | Session disconnected |

Event IDs need context; a single failed logon is not sufficient to establish an attack.

---

## 12. Common Mistakes and Exam Traps

- TCP and UDP port numbers are separate transport namespaces; TCP 53 and UDP 53 are distinct endpoints.
- DNS commonly uses UDP 53, but also uses TCP 53.
- HTTPS commonly uses TCP 443, while HTTP/3 uses QUIC over UDP 443.
- A listening port is not automatically malicious.
- A high-numbered port is not automatically malicious; client ephemeral ports commonly use high numbers.
- A connection to port 4444 does not by itself prove malware.
- `netstat -ano` shows PIDs, not process names; correlate the PID with process information.
- A failed TCP connection test may reflect filtering or routing, not necessarily a down service.
- Telnet and FTP are legacy protocols; prefer secure alternatives where appropriate.

---

## 13. Day 4 Practical Checklist

- [x] Explain TCP and UDP in my own words.
- [x] Explain the TCP three-way handshake.
- [x] Identify at least 15 common ports and their services.
- [x] Run `netstat -ano` and identify a PID.
- [x] Run `Test-NetConnection example.com -Port 443`.
- [x] Run `ss -tuln` on Linux.
- [x] Apply at least three Wireshark display filters.
- [x] Write a short triage summary for the port 4444 case study.

### Progress Update

- [x] TCP and UDP concepts reviewed.
- [x] TCP three-way handshake understood.
- [x] Common ports and services reviewed.
- [x] Windows practical: `netstat -ano` and PID identification completed.
- [x] Windows practical: TCP port connectivity test completed.
- [x] Linux socket inspection practical completed.
- [x] Wireshark packet-capture exercise and display-filter practice.
- [x] Port 4444 SOC case study reviewed.

**Current status:** All Day 4 learning and practical tasks are marked complete, including the Wireshark exercise.

## 14. Quick Revision

- **IP address:** Identifies a host/interface on an IP network.
- **Port:** Identifies a transport-layer application endpoint.
- **TCP:** Connection-oriented, reliable and ordered transport.
- **UDP:** Connectionless datagram transport with no built-in delivery guarantee.
- **TCP handshake:** SYN → SYN-ACK → ACK.
- **SSH:** TCP 22.
- **DNS:** UDP/TCP 53.
- **HTTP:** TCP 80.
- **HTTPS:** TCP 443; HTTP/3 uses QUIC over UDP 443.
- **SMB:** TCP 445.
- **RDP:** TCP/UDP 3389.

---

## 15. Assessment — Test My Understanding

1. Which protocol provides reliable, ordered delivery?
2. What are the three steps of the TCP handshake?
3. Which service commonly uses TCP port 22?
4. Which ports are used by DHCP?
5. Why can DNS use both UDP and TCP?
6. What does `netstat -ano` show?
7. Which command helps identify the process associated with a PID?
8. Does a connection to TCP port 4444 automatically mean malware? Explain.
9. Why should internet-facing RDP be carefully restricted?
10. Why might `tcp.port == 443` fail to show some HTTPS traffic?

---

## SOC Analyst Takeaway

Port knowledge is a starting point for network investigation, not a verdict. A Tier 1 analyst correlates the 5-tuple with process, user, endpoint, DNS, firewall and SIEM evidence before deciding whether an event is normal, suspicious or requires escalation.

**Next:** Week 2 — Day 5: DNS, DHCP, HTTP, HTTPS and SSH.

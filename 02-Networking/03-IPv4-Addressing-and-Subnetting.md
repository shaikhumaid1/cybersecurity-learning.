# Week 2 — Day 3: IPv4 Addressing, Subnetting and CIDR

**Learning Track:** Cybersecurity & SOC Analyst  
**Topic:** IPv4, Subnet Masks, CIDR, Private/Public IPs and Subnetting  
**Difficulty:** Beginner to Intermediate

---

## 1. Learning Objectives

- Understand IPv4 addressing.
- Understand network and host portions.
- Identify public and private IP addresses.
- Understand subnet masks and CIDR notation.
- Calculate network and broadcast addresses.
- Calculate usable host ranges.
- Perform subnetting using the Block Size Method.
- Apply subnetting knowledge to SOC investigations.

## 2. What is an IPv4 Address?

IPv4 stands for Internet Protocol version 4.

It is a 32-bit logical address used to identify a network interface.

Example:

```text
192.168.1.10
```

An IPv4 address consists of four octets separated by dots.

Each octet contains 8 bits.

- Total bits: 32
- Number of octets: 4
- Each octet range: 0–255

### Binary representation

```text
192       168       1         10
11000000  10101000  00000001  00001010
```

Each octet contains 8 bits.

## 3. Network Portion and Host Portion

An IPv4 address has two logical portions:

1. Network portion — identifies the network.
2. Host portion — identifies an interface within that network.

Example:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
CIDR: /24
```

In this example:

- Network portion: 192.168.1
- Host portion: 10

The subnet mask determines which bits belong to the network and which belong to the host.

## 4. Types of IPv4 Addresses

### Private IPv4 Addresses

Private addresses are used within internal networks.

The three RFC 1918 private ranges are:

| Private range | CIDR |
|---|---|
| 10.0.0.0 – 10.255.255.255 | /8 |
| 172.16.0.0 – 172.31.255.255 | /12 |
| 192.168.0.0 – 192.168.255.255 | /16 |

Examples:

```text
10.0.0.10
172.16.5.20
192.168.1.100
```

### Public IPv4 Addresses

Public IP addresses are globally routable addresses assigned through internet address allocation.

Example:

```text
8.8.8.8
1.1.1.1
```

### Other Important IPv4 Addresses

| Address range | Purpose |
|---|---|
| 127.0.0.0/8 | Loopback |
| 169.254.0.0/16 | IPv4 link-local |
| 0.0.0.0 | Unspecified address |
| 255.255.255.255 | Limited broadcast |

**Important:** Not every address outside RFC 1918 ranges is necessarily a public unicast address. Special-purpose ranges also exist.

## 5. What is a Subnet Mask?

A subnet mask identifies the network portion and host portion of an IPv4 address.

Example:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
CIDR: /24
```

The binary representation of the subnet mask contains network bits (1s) followed by host bits (0s).

```text
11111111.11111111.11111111.00000000
```

This represents /24.

## 6. CIDR Notation

CIDR stands for Classless Inter-Domain Routing.

CIDR notation represents the number of network bits in an IPv4 address.

Example:

```text
192.168.1.10/24
```

The `/24` means:

- 24 network bits.
- 8 host bits.
- 32 total IPv4 bits.

Formula:

```text
Host Bits = 32 - CIDR Prefix
```

Example:

```text
32 - 24 = 8 host bits
```

## 7. Common Subnet Masks

| CIDR | Subnet Mask | Total Addresses | Usable Hosts* |
|---|---|---:|---:|
| /8 | 255.0.0.0 | 16,777,216 | 16,777,214 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |
| /31 | 255.255.255.254 | 2 | 2 |
| /32 | 255.255.255.255 | 1 | 1 |

*For conventional subnets, usable hosts exclude network and broadcast addresses. /31 is used for point-to-point links, while /32 represents one individual address.*

## 8. What is Subnetting?

Subnetting is the process of dividing a larger IP network into smaller logical networks.

### Why is subnetting important?

- Organises large networks.
- Separates departments and VLANs.
- Reduces broadcast domains.
- Helps manage IP addresses efficiently.
- Supports network segmentation.
- Helps implement security policies.

Example:

A company has 100 computers.

Instead of using one large network, it can divide devices into smaller networks:

```text
Main Network
    |
    |-- IT Department
    |
    |-- HR Department
    |
    |-- Finance Department
    |
    |-- Administration
```

Each department can have its own subnet and security policies.

## 9. Subnetting Formulas

### Formula 1 — Host Bits

```text
Host Bits = 32 - CIDR Prefix
```

### Formula 2 — Total Addresses

```text
Total Addresses = 2 ^ Host Bits
```

### Formula 3 — Conventional Usable Hosts

```text
Usable Hosts = 2 ^ Host Bits - 2
```

The subtraction accounts for the network and broadcast addresses.

### Formula 4 — Block Size

```text
Block Size = 256 - Interesting Subnet Mask Octet
```

The interesting octet is the subnet mask octet that is neither 255 nor 0.

## 10. The Block Size Method — Step by Step

The Block Size Method is an easy way to calculate subnet ranges.

### Example 1: /26

Given:

```text
IP Address: 192.168.1.70/26
```

**Step 1: Find the subnet mask**

```text
/26 = 255.255.255.192
```

**Step 2: Calculate block size**

```text
256 - 192 = 64
```

Block size = 64.

**Step 3: Write subnet ranges**

```text
0–63
64–127
128–191
192–255
```

**Step 4: Identify the range**

The IP address 70 belongs to:

```text
64–127
```

Therefore, the network starts at 64 and ends at 127.

**Step 5: Calculate final addresses**

| Property | Answer |
|---|---|
| Network address | 192.168.1.64 |
| First usable IP | 192.168.1.65 |
| Last usable IP | 192.168.1.126 |
| Broadcast address | 192.168.1.127 |
| Block size | 64 |
| Usable hosts | 62 |

### Example 2: /27

Given:

```text
IP Address: 192.168.10.150/27
```

**Step 1: Subnet mask**

```text
255.255.255.224
```

**Step 2: Block size**

```text
256 - 224 = 32
```

**Step 3: Write subnet ranges**

```text
0–31
32–63
64–95
96–127
128–159
160–191
192–223
224–255
```

**Step 4: Find the correct range**

150 belongs to:

```text
128–159
```

**Step 5: Final answer**

| Property | Answer |
|---|---|
| Network address | 192.168.10.128 |
| First usable IP | 192.168.10.129 |
| Last usable IP | 192.168.10.158 |
| Broadcast address | 192.168.10.159 |
| Block size | 32 |
| Usable hosts | 30 |

### Example 3: /27

Given:

```text
IP Address: 192.168.20.190/27
```

Block size:

```text
256 - 224 = 32
```

Subnet ranges:

```text
0–31
32–63
64–95
96–127
128–159
160–191
192–223
224–255
```

190 belongs to the range 160–191.

Final answer:

| Property | Answer |
|---|---|
| Network address | 192.168.20.160 |
| First usable IP | 192.168.20.161 |
| Last usable IP | 192.168.20.190 |
| Broadcast address | 192.168.20.191 |
| Block size | 32 |
| Usable hosts | 30 |

**Important:** The given IP address .190 is the last usable host in this subnet.

## 11. How to Calculate Subnet Masks Using Binary

Each octet contains eight bits.

Binary place values:

```text
128 64 32 16 8 4 2 1
```

Example:

```text
11000000
```

Calculation:

```text
128 + 64 = 192
```

Therefore:

```text
11000000 = 192
```

Common binary subnet octets:

| Binary | Decimal |
|---|---:|
| 00000000 | 0 |
| 10000000 | 128 |
| 11000000 | 192 |
| 11100000 | 224 |
| 11110000 | 240 |
| 11111000 | 248 |
| 11111100 | 252 |
| 11111110 | 254 |
| 11111111 | 255 |

## 12. Subnetting Cheat Sheet

| CIDR | Block Size | Total Addresses | Usable Hosts |
|---|---:|---:|---:|
| /24 | 256 | 256 | 254 |
| /25 | 128 | 128 | 126 |
| /26 | 64 | 64 | 62 |
| /27 | 32 | 32 | 30 |
| /28 | 16 | 16 | 14 |
| /29 | 8 | 8 | 6 |
| /30 | 4 | 4 | 2 |

### Quick Calculation Example

Given:

```text
192.168.5.100/26
```

1. Subnet mask: 255.255.255.192
2. Block size: 256 - 192 = 64
3. Network ranges: 0–63, 64–127, 128–191, 192–255
4. Network: 192.168.5.64
5. First host: 192.168.5.65
6. Last host: 192.168.5.126
7. Broadcast: 192.168.5.127
8. Usable hosts: 62

## 13. Variable Length Subnet Masking (VLSM)

VLSM stands for Variable Length Subnet Masking.

It allows a network administrator to use different subnet sizes within the same larger network.

Example:

A company has a network:

```text
192.168.10.0/24
```

Possible allocation:

| Department | Required Hosts | Example Subnet |
|---|---:|---|
| IT | 50 | /26 |
| HR | 25 | /27 |
| Finance | 12 | /28 |
| Point-to-point link | 2 | /30 |

VLSM helps allocate IP addresses according to actual requirements instead of wasting large address blocks.

These are example allocations; a complete VLSM plan must calculate and verify non-overlapping network ranges.

## 14. Practical Lab — Windows and Linux

### Windows PowerShell

Check network configuration:

```powershell
ipconfig /all
```

Display IPv4 routes:

```powershell
route print -4
```

Display active connections:

```powershell
netstat -ano
```

### Ubuntu / Kali Linux

Check IPv4 addresses:

```bash
ip -4 addr
```

Display routing table:

```bash
ip route
```

Display interfaces:

```bash
ip link
```

### Python Subnet Calculator

Python provides the ipaddress module for IPv4 network calculations.

```python
import ipaddress

network = ipaddress.ip_network("192.168.1.70/26", strict=False)

print("Network:", network.network_address)
print("Subnet Mask:", network.netmask)
print("Broadcast:", network.broadcast_address)
print("Total Addresses:", network.num_addresses)
print("Usable Hosts:", network.num_addresses - 2)
```

Expected output:

```text
Network: 192.168.1.64
Subnet Mask: 255.255.255.192
Broadcast: 192.168.1.127
Total Addresses: 64
Usable Hosts: 62
```

Note: This simple usable-host calculation is for conventional subnets such as /24 through /30, not /31 or /32.

## 15. SOC Analyst Application

Subnetting is useful in SOC operations because analysts need to identify where a device belongs within an organisation's network.

### Example SIEM Alert

```text
Timestamp: 2026-11-09 14:30:22
Source IP: 192.168.1.70
Destination IP: 203.0.113.50
Protocol: TCP
Destination Port: 4444
Action: ALLOW
```

The source IP belongs to:

```text
192.168.1.64/26
```

An analyst can investigate:

1. Which subnet or VLAN contains the affected endpoint?
2. Which department owns the endpoint?
3. Is the destination external?
4. Which process initiated the connection?
5. Are there other suspicious devices in the same subnet?
6. Are related firewall, DNS or endpoint events present?

### SOC Relevance

- Identify internal and external IP addresses.
- Determine affected network segments.
- Support incident scoping.
- Investigate lateral movement indicators.
- Understand firewall and network segmentation rules.

### MITRE ATT&CK Connection

Subnetting supports investigation of techniques such as:

- T1046 — Network Service Discovery.
- T1021 — Remote Services.
- T1071 — Application Layer Protocol.

These are investigation references, not proof of malicious activity from an IP address alone.

## 16. Common CCNA Exam Traps

1. /26 does not mean 26 host bits; it means 26 network bits.
2. A /27 subnet has 32 total addresses, not 32 usable hosts.
3. The network address cannot normally be assigned to a host in a conventional subnet.
4. The broadcast address is the last address of a conventional subnet.
5. Not every 172.x.x.x address is private.
6. The IP address must be checked against the subnet boundaries before choosing its network.
7. A /31 subnet is valid for point-to-point links and has two usable endpoint addresses.

## 17. Assessment and Progress

**Day 3 learning topics:**

- [x] IPv4 addressing
- [x] Public and private IP addresses
- [x] Subnet masks and CIDR
- [x] Block Size Method
- [x] Network and broadcast addresses
- [x] First and last usable IP addresses
- [x] Usable host calculations
- [x] Basic subnetting practice
- [ ] Practical command verification
- [ ] Record personal lab observations

**Subnetting practice results:**

| Practice | Result |
|---|---|
| 192.168.1.70/26 | 3/3 |
| 192.168.10.150/27 | Corrected after range mistake |
| 192.168.20.190/27 | Corrected after range mistake |
| 192.168.5.100/26 | Practice question |

## 18. Key Takeaways

- IPv4 uses 32-bit addresses.
- CIDR identifies the network prefix length.
- Subnet masks divide network and host portions.
- Block size helps identify subnet boundaries.
- Network and broadcast addresses are important for conventional subnet calculations.
- Subnetting helps with network segmentation and security investigations.

---

**Next Topic:** Week 2 — Day 4: TCP, UDP and Common Ports

**Career Focus:** SOC Analyst Tier 1 — IP Address Investigation, Network Segmentation and Traffic Analysis.

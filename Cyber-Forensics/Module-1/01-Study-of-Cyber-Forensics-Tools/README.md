# Practical 01: Study of Cyber Forensics Tools

> **Module:** 1 — Digital Forensics  
> **Practical:** 01 — Study of Cyber Forensics Tools  
> **Focus:** Tool purpose, forensic workflow, methods, techniques, alternatives and real-world use cases

## 1. Objective

To study commonly used digital forensic tools, understand which stage of an investigation each tool supports, compare their capabilities, and identify suitable tools for different evidence types.

## 2. What is Digital / Cyber Forensics?

Digital forensics is the disciplined process of identifying, preserving, acquiring, examining, analyzing and reporting data from digital devices or systems. The purpose is to answer investigative questions using reproducible methods and documented evidence.

Examples of potential evidence sources include:

- Desktop and laptop computers
- HDDs, SSDs, USB drives and memory cards
- Mobile devices and backups
- Emails and cloud exports
- Network packet captures
- Operating-system and application logs
- Images, videos and documents

Digital forensics is not simply file recovery. It includes evidence handling, acquisition, artifact interpretation, timeline reconstruction, validation and reporting.

## 3. Core stages of a forensic investigation

| Stage | Meaning | Example |
|---|---|---|
| Identification | Find potential evidence sources | Identify a USB drive involved in a data-theft allegation |
| Preservation | Protect evidence from avoidable changes | Isolate the device and use a suitable write blocker |
| Acquisition | Collect data using a documented method | Create an E01 or RAW image |
| Examination | Extract and organize relevant artifacts | Find deleted files, browser history or event logs |
| Analysis | Interpret artifacts in context | Correlate file timestamps with login events |
| Reporting | Record methods, findings and limitations | Produce a report with hashes and screenshots |

The exact workflow depends on the device, operating state, encryption, legal authority and investigation objectives. Live acquisition can change some system state; that impact must be documented.

## 4. Categories of digital forensic tools

### 4.1 Disk imaging and evidence acquisition

These tools acquire a physical device, logical data set, selected files or other digital evidence.

**Examples:** FTK Imager, Guymager, EnCase Imager, X-Ways Forensics, Magnet ACQUIRE and command-line tools such as dc3dd.

**Why this category matters:** Analysis should generally be performed on a verified forensic copy rather than repeatedly accessing original evidence.

### 4.2 Disk and file-system examination

These tools interpret partitions, file systems, allocated files, deleted entries, unallocated space and file-system artifacts.

**Examples:** Autopsy/The Sleuth Kit, EnCase, X-Ways Forensics and AXIOM.

### 4.3 File recovery and data carving

These tools attempt to restore deleted files or identify file content from storage structures and raw data.

**Examples:** Recuva, Disk Drill, PhotoRec, TestDisk, R-Studio and UFS Explorer.

### 4.4 Metadata analysis

These tools inspect metadata embedded in or associated with files.

**Examples:** ExifTool, MediaInfo, FFprobe and format-specific document metadata tools.

### 4.5 Network forensics

These tools capture or analyze network communications and packet data.

**Examples:** Wireshark, tshark, Zeek and NetworkMiner.

### 4.6 Memory forensics

These tools analyze volatile memory captures to investigate processes, connections, loaded modules and other runtime artifacts.

**Examples:** Volatility 3 and MemProcFS. FTK Imager can also capture RAM on supported Windows systems.

### 4.7 Mobile forensics

Mobile investigations may involve logical backups, file-system extraction, application artifacts and, where supported and authorized, deeper acquisition methods.

**Examples:** Cellebrite UFED, Magnet AXIOM, Oxygen Forensic Detective and MSAB XRY. Availability and access depend on device model, OS version, lock state and lawful authority.

## 5. Study of the four tools in this practical

### A. Autopsy

**Type:** Digital forensic examination platform  
**Project:** Open-source platform built around The Sleuth Kit

**Purpose:** Autopsy helps an examiner analyze disk images and other supported data sources. It is primarily an examination and analysis platform, rather than a replacement for every acquisition tool.

**Why use it?**
- Organize an investigation into cases
- Examine file systems and files
- Search for keywords
- Review web and other application artifacts
- Build timelines
- Calculate and look up hashes
- Tag findings and generate reports

**Typical method / workflow:**
1. Create a case and enter case details.
2. Add a forensic image or supported data source.
3. Select and configure ingest modules.
4. Allow background processing to identify artifacts.
5. Review files, deleted entries, search results and timelines.
6. Validate important findings manually.
7. Tag relevant items and generate a report.

**Techniques and concepts:**
- File-system analysis
- Keyword searching
- Hash-set comparison
- Timeline analysis
- Deleted-file examination
- Artifact extraction

**Real-world example:** During an employee data-theft investigation, an examiner receives a forensic image of a workstation. Autopsy can help locate USB-related artifacts, recently accessed files, browser history and relevant timestamps. Those artifacts must be correlated with other evidence before drawing conclusions.

**Limitations:** Results depend on the data source, supported file systems, ingest modules and artifact availability. A timestamp or recovered file alone does not prove who performed an action.

**Alternatives:** EnCase, X-Ways Forensics, Magnet AXIOM and other forensic suites.

### B. FTK Imager

**Type:** Forensic acquisition and preview utility  
**Developer:** Exterro

**Purpose:** FTK Imager creates forensic images, previews supported evidence and can verify image integrity. It is commonly used early in a computer-forensics workflow.

**Why use it?**
- Acquire storage media into forensic image formats
- Preview files and folders
- Review supported image contents
- Export selected files
- Calculate and verify hashes
- Capture volatile RAM on supported Windows systems

**Typical method / workflow:**
1. Identify and document the evidence device.
2. Use a hardware write blocker where appropriate.
3. Select the physical or logical source according to the objective.
4. Select an output format such as E01 or RAW/DD.
5. Enter case and evidence details.
6. Configure available hashing and verification options.
7. Acquire the evidence.
8. Review errors, hash results and the acquisition log.
9. Secure the original and analyze a working copy.

**Techniques and concepts:**
- Physical versus logical acquisition
- Bit-stream imaging
- E01 and RAW/DD formats
- Write blocking
- Cryptographic hash verification
- Live memory acquisition

**Real-world example:** An incident-response team receives an authorized USB drive suspected of containing stolen files. The examiner documents the device, acquires an image, records hash values and notes any unreadable sectors before analyzing a working copy.

**Limitations:** FTK Imager is primarily an acquisition and preview utility, not a complete advanced analysis suite. Its standard hash options and supported acquisition workflows should be checked against the version in use. It does not replace a mobile extraction platform.

**Alternatives:** Guymager, dc3dd, ewfacquire, EnCase Imager, X-Ways and Magnet ACQUIRE.

### C. EnCase

**Type:** Commercial digital forensic investigation platform  
**Vendor:** OpenText

**Purpose:** EnCase supports evidence acquisition, processing, examination and reporting in structured investigations.

**Why use it?**
- Manage evidence and case workflows
- Examine file systems and files
- Search and filter evidence
- Recover or inspect supported deleted-file artifacts
- Review metadata and timestamps
- Produce investigation reports

**Typical method / workflow:**
1. Create a case.
2. Add or acquire evidence using the available product workflow.
3. Process and index evidence.
4. Search for relevant files and artifacts.
5. Examine and validate findings.
6. Record evidence references and generate a report.

**Techniques:** File-system examination, keyword searching, hash analysis, artifact review, timeline work and reporting.

**Real-world example:** A corporate investigation team examines a workstation image after suspected unauthorized disclosure of confidential documents. EnCase can support structured examination and reporting of relevant files and artifacts.

**Limitations:** Commercial licensing, training requirements and product-specific workflows may apply. Features vary by product edition and version.

**Alternatives:** Autopsy, X-Ways Forensics, Magnet AXIOM and other forensic platforms.

### D. Wireshark

**Type:** Network protocol analyzer  
**Project:** Wireshark Foundation and contributors

**Purpose:** Wireshark captures and decodes network packets so an analyst can inspect protocols and communication behavior.

**Why use it?**
- Troubleshoot network communication
- Investigate suspicious connections
- Inspect DNS queries and responses
- Examine TCP handshakes and resets
- Review HTTP and other visible protocol data
- Filter and export relevant packets

**Typical method / workflow:**
1. Select an authorized network interface or open a supplied PCAP.
2. Capture or load traffic.
3. Apply display filters.
4. Inspect packet headers and protocol fields.
5. Follow supported streams where useful.
6. Identify indicators and preserve relevant packet evidence.
7. Correlate findings with endpoint and server logs.

**Useful display-filter examples:**

```wireshark
dns
tcp
http
ip.addr == 192.0.2.10
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

These are examples; substitute addresses from an authorized lab capture.

**Techniques:** Packet capture, protocol dissection, display filtering, stream reconstruction and conversation analysis.

**Real-world example:** A SOC analyst investigates a workstation that repeatedly contacts an unfamiliar domain. A PCAP can help identify DNS resolution, destination IPs, connection timing and visible protocol behavior. Encrypted payload contents generally cannot be read without appropriate decryption material.

**Limitations:** A packet capture shows only traffic visible at the capture point. Encryption, packet loss, asymmetric routing, capture filters and missing context can limit conclusions.

**Alternatives:** tshark, tcpdump, Zeek and NetworkMiner.

## 6. Comparison table

| Tool | Main evidence focus | Main role | License model |
|---|---|---|---|
| Autopsy | Disk images and artifacts | Examination and analysis | Open source |
| FTK Imager | Disks, files and supported memory capture | Acquisition and preview | Free utility |
| EnCase | Computer and digital evidence | Commercial investigation suite | Commercial |
| Wireshark | Network packets | Capture and protocol analysis | Open source |

## 7. How to choose the right tool

| Investigation question | Suitable starting tool |
|---|---|
| Need to acquire a USB drive | FTK Imager or Guymager |
| Need to examine a disk image | Autopsy or EnCase |
| Need to inspect packet traffic | Wireshark or tshark |
| Need to inspect image metadata | ExifTool |
| Need to attempt deleted-file recovery | Recuva, Disk Drill or PhotoRec |
| Need to analyze RAM | Volatility 3 |
| Need to examine a mobile extraction | A supported mobile-forensics platform |

Tool selection depends on evidence type, acquisition requirements, platform support, validation needs, budget and authorization.

## 8. Practical learning plan

- [x] Explore FTK Imager
- [x] Explore Recuva / Disk Drill
- [x] Explore ExifTool
- [ ] Create and document an Autopsy case
- [ ] Load a supplied or self-created forensic image into Autopsy
- [ ] Analyze a sample PCAP with Wireshark
- [ ] Compare findings and limitations between tools

## 9. Conclusion

Digital forensic tools have different roles across evidence acquisition, examination, analysis and reporting. FTK Imager supports acquisition and preview; Autopsy and EnCase support structured evidence examination; Wireshark focuses on network traffic. Selecting the correct tool and documenting its method and limitations are essential for reliable investigations.

## 10. References

- Autopsy documentation: https://www.autopsy.com/
- The Sleuth Kit: https://www.sleuthkit.org/
- Exterro FTK Imager: https://www.exterro.com/digital-forensics-software/ftk-imager
- OpenText: https://www.opentext.com/
- Wireshark: https://www.wireshark.org/
- NIST SP 800-86: https://csrc.nist.gov/pubs/sp/800/86/final
- NIST Digital Investigation Techniques: https://www.nist.gov/publications/digital-investigation-techniques-scientific-foundation-review

**Practical status:** Initial tool study completed. Hands-on use of each listed tool is not implied unless separately recorded.

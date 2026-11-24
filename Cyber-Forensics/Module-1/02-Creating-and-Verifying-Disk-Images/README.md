# Practical 02: Creating and Verifying Disk Images Using FTK Imager

> **Module:** 1 — Digital Forensics  
> **Tool used:** Exterro FTK Imager 8.3.1.6  
> **Evidence source:** SanDisk Cruzer Blade USB (personal lab evidence; identifying details redacted)

## 1. Objective

To understand digital evidence acquisition, create a forensic image from a physical USB storage device using FTK Imager, and review the acquisition and verification results using cryptographic hash values.

## 2. What is forensic imaging?

Forensic imaging is the acquisition of data from a digital storage source into an image or evidence container for examination.

A bit-stream image attempts to copy the addressable sectors of the selected source, including allocated and unallocated areas that can be read. A logical acquisition instead collects selected files, folders or logical data. The two methods answer different investigative needs.

A forensic image is not automatically proof that the source device was healthy, complete or authentic. The examiner must document the acquisition method, errors, hashes and limitations.

## 3. Why is disk imaging important?

Investigators should avoid unnecessary examination of original evidence because ordinary system activity can change data. A properly documented acquisition provides a working copy that can be examined while the original is preserved.

**Example scenario:** An organization suspects that confidential files were copied to a USB drive. An examiner documents the USB, uses suitable write protection, acquires an image, verifies the acquisition and examines a working copy for relevant files and artifacts.

## 4. Types and methods of acquisition

### 4.1 Physical / bit-stream acquisition

A physical acquisition targets the addressable storage media or selected physical device range. A full-device bit-stream image can include:
- Partition tables and partition structures
- Allocated file-system data
- Unallocated space
- File-system slack and other addressable sectors, depending on source and acquisition support

**Use:** When deleted data, unallocated space or broader device context may be relevant.

### 4.2 Logical acquisition

A logical acquisition collects selected files, directories or a logical volume's accessible contents.

**Use:** Targeted collection when the investigation concerns a known set of files or when full physical acquisition is not possible or necessary.

**Limitation:** A normal logical file copy generally does not preserve all unallocated space, slack space or deleted-file remnants.

### 4.3 Live versus dead acquisition

- **Live acquisition:** The system is powered on. Useful when volatile data, unlocked encrypted volumes or active state is important. Running tools and system activity can alter evidence; record the actions and changes.
- **Dead / offline acquisition:** The device is powered down and acquired using a separate forensic environment. This can reduce changes from the original operating system, but encryption and device configuration may affect access.

### 4.4 Targeted collection

Some workflows collect selected files, folders or artifacts rather than an entire disk. This can reduce collection time and storage, but the examiner must justify why the selected scope is sufficient.

## 5. Forensic image formats

| Format | Description | Considerations |
|---|---|---|
| RAW / DD | Raw sector data without a rich evidence-container structure | Simple and widely supported; can be large |
| E01 | EnCase Evidence File container | Supports metadata, segmentation and compression options |
| AFF / AFF4 | Advanced Forensic Format family | Supports forensic evidence storage and metadata; tool support varies |

The format does not itself make an acquisition forensically sound. Correct source selection, write protection, documentation and verification are still required.

## 6. Important concepts

### Write blocker

A hardware or software mechanism intended to prevent writes to the evidence source. A validated hardware write blocker is generally preferred for removable storage acquisition where compatible.

### Hashing

A cryptographic hash produces a fixed-length digest from input data. A digest can help detect changes when the same data is hashed using the same algorithm.

- MD5: 128-bit digest; legacy and collision-broken.
- SHA-1: 160-bit digest; legacy and collision-broken.
- SHA-256: 256-bit digest; preferred for modern integrity records where supported.

### Verification

Verification compares hashes or other integrity information according to the tool's workflow. A matching digest supports the conclusion that the compared data is identical for the relevant calculation. It does not prove that every source sector was readable or that the source was trustworthy.

### Chain of custody

A record of who collected, handled, transferred, stored and examined evidence, including dates, times, purpose and storage location.

## 7. Tool overview: FTK Imager

**Developer:** Exterro  
**Type:** Forensic acquisition and preview utility

FTK Imager can create forensic images, preview supported evidence, export files and perform hash-related verification. It is often used at the acquisition and initial triage stage.

**Why FTK Imager?**
- Accessible graphical workflow
- Supports common forensic image formats
- Can record case and evidence details
- Provides acquisition and verification reports
- Useful for preparing evidence for later analysis in tools such as Autopsy or FTK

**Alternatives:**
- Guymager — Linux-based forensic imaging
- dc3dd — forensic-oriented command-line acquisition
- ewfacquire — acquisition to the Expert Witness Format ecosystem
- EnCase Imager / X-Ways — commercial forensic acquisition workflows
- Magnet ACQUIRE — evidence collection workflows for supported sources

Tool choice should account for validation, supported device types, operating environment and case requirements.

## 8. Lab environment and actual case details

| Component | Recorded value |
|---|---|
| FTK Imager version | 8.3.1.6 |
| Host OS | Windows 11 Home Single Language |
| Processor | AMD Ryzen 5 7520U with Radeon Graphics |
| Installed RAM | 16 GB |
| Source model | SanDisk Cruzer Blade USB Device |
| Source type | Physical |
| Interface | USB |
| Removable media | Yes |
| Reported source data size | 7,467 MB |
| Bytes per sector | 512 |
| Sector count | 15,293,440 |
| Output format | E01 |
| Output filename | new ftk.E01 |

Case fields recorded in the lab:
- Case number: 001
- Evidence number: findind
- Description: testing
- Examiner: umaid
- Notes: for exam

Personal paths, account names and device serial numbers are intentionally not reproduced in this public report.

## 9. Acquisition methodology

The acquisition report records this workflow:

1. Identify the physical USB source.
2. Select the physical source in FTK Imager.
3. Configure an E01 image destination.
4. Enter case and evidence information.
5. Start acquisition.
6. Review the acquisition summary and read errors.
7. Run image verification.
8. Record the reported hashes and verification status.
9. Preserve the image and report for subsequent examination.

**Good-practice note:** The report supplied for this lab does not establish whether a hardware write blocker was used. Do not claim one was used unless that can be confirmed from the lab record.

## 10. Actual acquisition timeline

FTK Imager report date: 23 November 2026.

| Event | Time shown in report |
|---|---|
| Acquisition started | 22:23:15 |
| Acquisition finished | 22:27:03 |
| Verification started | 22:27:04 |
| Verification finished | 22:27:39 |

- Acquisition duration: 3 minutes 48 seconds
- Verification duration: 35 seconds

## 11. Actual hash verification results

### MD5

`d16e29d1251eef64efa84f09ec04a166`

### SHA-1

`b0193e76ddb73cdeded5f6840095f288af52b011`

| FTK Imager verification field | MD5 | SHA-1 |
|---|---|---|
| Computed hash | Verified | Verified |
| Stored verification hash | Verified | Verified |
| Report hash | Verified | Verified |

**Result:** All six verification entries in the supplied FTK Imager report were marked **verified**.

The report defines the computed hash as the hash calculated from the source media during creation or verification. It also records stored verification and report hashes from acquisition. The three matching entries for each algorithm support the reported verification result.

## 12. Critical acquisition finding: unreadable sectors

The source device contained sectors that FTK Imager could not read:

- Sector 728125
- Sector 728127
- Sectors 729540–729543
- Sectors 925300–925303

FTK Imager reports that the contents of these sectors were replaced with zeros in the image.

### Why this matters

The image was created and the reported hash verification checks passed, but some source-sector contents were unavailable during acquisition. The zeros in those locations are replacements, not recovered original data.

Possible causes can include media degradation, read errors or connection problems. The supplied report does not identify the exact cause, so no single cause should be asserted.

### Recommended examiner response

1. Preserve the original device and acquired image.
2. Record the exact unreadable sector ranges and tool messages.
3. Avoid repeated uncontrolled reads that could worsen a failing device.
4. If the case requires it, consult an experienced examiner and consider a controlled re-acquisition using suitable hardware and validated tools.
5. Keep each acquisition attempt and its results separately documented.
6. Disclose the limitation in any analysis or final report.

## 13. Results and interpretation

- An E01 image was created.
- FTK Imager reported successful MD5 and SHA-1 verification.
- All six reported verification checks passed.
- The source had unreadable sectors.
- Unreadable sector contents were zero-filled in the image.
- The image therefore has a documented acquisition limitation.

**Correct interpretation:** The verification result supports integrity of the acquired data as represented in the image and the tool's verification process. It does not prove a complete, error-free acquisition of all original source sectors.

## 14. Further integrity check: SHA-256

Where supported, record SHA-256 as an additional modern integrity digest. For example, PowerShell can calculate a hash of the resulting E01 file:

```powershell
Get-FileHash ".\new ftk.E01" -Algorithm SHA256
```

This hashes the E01 container file itself. It is not the same as comparing all acquired source sectors with the original device during acquisition verification.

## 15. Real-world applications

- Acquiring a USB drive in an insider-threat investigation
- Preserving a workstation after suspected malware execution
- Preparing a disk image for deleted-file analysis
- Capturing evidence before a device is returned or reimaged
- Supporting incident response and later independent examination

## 16. Limitations

- Unreadable sectors can lead to incomplete source data.
- Full physical images require time and storage capacity.
- Encryption may prevent meaningful examination without authorized keys or credentials.
- Live acquisition can change system state.
- Hashes do not establish authorship, intent or the truth of an artifact.
- A successful verification result does not erase acquisition errors.

## 17. Evidence screenshots to add

- FTK Imager main interface
- Physical source selection (redact serial numbers)
- E01 configuration
- Acquisition completion
- Verification report
- Unreadable-sector warning

Do not upload the E01 image, private data or screenshots containing personal paths and unnecessary device identifiers.

## 18. Conclusion

This practical demonstrated physical-source acquisition and image verification using FTK Imager 8.3.1.6. The E01 image was created, and all reported MD5 and SHA-1 verification entries passed. However, FTK Imager also reported unreadable source sectors and replaced their contents with zeros in the image. This is a material limitation and must be retained in the evidence record and any later interpretation.

## 19. References

- Exterro FTK Imager: https://www.exterro.com/digital-forensics-software/ftk-imager
- NIST SP 800-86: https://csrc.nist.gov/pubs/sp/800/86/final
- NIST Digital Investigation Techniques: https://www.nist.gov/publications/digital-investigation-techniques-scientific-foundation-review
- SWGDE Best Practices for Computer Forensic Acquisitions: https://www.swgde.org/

**Practical status:** Acquisition performed and report documented.  
**Verification status:** All reported hash checks passed.  
**Acquisition limitation:** Unreadable source sectors documented.

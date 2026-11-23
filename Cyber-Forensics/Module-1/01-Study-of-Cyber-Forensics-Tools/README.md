# Practical 01: Study of Cyber Forensics Tools

## 1. Objective

Study common tools used to acquire, examine and analyze digital evidence, and understand their purposes, workflows and applications.

## 2. Introduction

Digital forensics involves identifying, preserving, acquiring, examining and reporting digital evidence. Different tools support different stages of an investigation; no single tool performs every forensic task.

## 3. Tools studied

### A. Autopsy

**Purpose:** Open-source digital forensics platform for examining disk images and other supported data sources.

**Typical capabilities:**
- File-system and file analysis
- Keyword search
- Timeline analysis
- Hash-set searching
- Web and other digital artifact examination
- Ingest modules and reporting

**Typical workflow:** Create a case, add a data source, configure ingest modules, review artifacts, correlate findings and document results.

**Use case:** Examining a forensic image for relevant files and user-activity artifacts.

### B. FTK Imager

**Purpose:** Forensic acquisition and preview utility.

**Typical capabilities:**
- Acquire forensic images from supported sources
- Preview files and file systems
- Create RAW or E01 images
- Calculate and verify hashes

**Typical workflow:** Identify the evidence source, select the acquisition method and destination format, acquire the image, review errors and verify integrity.

**Use case:** Creating a forensic image of an authorized USB drive for later examination.

### C. EnCase

**Purpose:** Commercial digital forensic investigation platform.

**Typical capabilities:** Evidence acquisition and processing, file-system examination, searching, artifact analysis and reporting. Available features depend on the product and version.

**Typical workflow:** Create a case, add or acquire evidence, examine data and artifacts, search for relevant information, and report findings.

**Use case:** Structured enterprise or law-enforcement forensic investigations.

### D. Wireshark

**Purpose:** Open-source network protocol analyzer.

**Typical capabilities:**
- Capture and inspect packets
- Decode network protocols
- Apply capture and display filters
- Follow supported protocol streams
- Export selected packet data

**Typical workflow:** Select an authorized network interface, capture traffic, filter relevant packets, inspect protocol details and document suspicious or useful observations.

**Use case:** Investigating unusual network connections or analyzing a supplied packet capture.

## 4. Comparison

| Tool | Primary role |
|---|---|
| Autopsy | Disk and digital artifact examination |
| FTK Imager | Forensic acquisition and preview |
| EnCase | Commercial forensic investigation |
| Wireshark | Network packet analysis |

## 5. Forensic workflow concepts

1. Identification — determine potentially relevant evidence.
2. Preservation — protect evidence from avoidable changes.
3. Acquisition — collect data using a documented method.
4. Examination — extract and organize relevant artifacts.
5. Analysis — interpret findings in context.
6. Reporting — record methods, observations, limitations and conclusions.

## 6. Important considerations

- Use only devices and data you own or are authorized to examine.
- Preserve original evidence and use write-blocking where appropriate.
- Record tool versions, timestamps, acquisition settings and hashes.
- A tool's output requires interpretation and validation.
- Network captures may contain sensitive information; sanitize before sharing.

## 7. Conclusion

The tools studied support different parts of a digital investigation. FTK Imager focuses on acquisition and preview, Autopsy and EnCase support evidence examination, and Wireshark analyzes network traffic. Understanding their roles helps select an appropriate tool for an investigative task.

## 8. References

- Autopsy: https://www.autopsy.com/
- Exterro: https://www.exterro.com/
- OpenText: https://www.opentext.com/
- Wireshark: https://www.wireshark.org/

**Status:** Tool study completed.

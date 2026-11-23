# Practical 02: Creating and Verifying Disk Images Using FTK Imager

## 1. Objective

Create a forensic image of a physical USB storage device using FTK Imager and review the integrity verification results using the hashes recorded in the acquisition report.

## 2. Introduction

Forensic imaging creates an acquired representation of storage media for examination. Investigators generally work from the acquired image rather than repeatedly examining the original device. Acquisition settings, errors and limitations must be documented.

## 3. Tools and lab environment

| Component | Recorded value |
|---|---|
| Forensic software | Exterro FTK Imager 8.3.1.6 |
| Host operating system | Windows 11 Home Single Language |
| Processor | AMD Ryzen 5 7520U with Radeon Graphics |
| Installed RAM | 16 GB |
| Evidence source | SanDisk Cruzer Blade USB Device |
| Source type | Physical |
| Interface | USB |
| Image format | E01 |
| Image segment | new ftk.E01 |
| Reported source data size | 7,467 MB |
| Bytes per sector | 512 |
| Sector count | 15,293,440 |

## 4. Case information

| Field | Recorded value |
|---|---|
| Case number | 001 |
| Evidence number | findind |
| Unique description | testing |
| Examiner | umaid |
| Notes | for exam |
| Encrypted password | N/A |

## 5. Methodology

The acquisition report records the following workflow:

1. Select the physical source device.
2. Configure a forensic image acquisition.
3. Select E01 as the image format.
4. Enter case and evidence details.
5. Start acquisition and record the output image.
6. Review acquisition warnings.
7. Run image verification and record the reported hashes.

## 6. Acquisition timeline

Date recorded by FTK Imager: 23 November 2026.

| Event | Local time shown in report |
|---|---|
| Acquisition started | 22:23:15 |
| Acquisition finished | 22:27:03 |
| Verification started | 22:27:04 |
| Verification finished | 22:27:39 |

Acquisition duration: 3 minutes 48 seconds.  
Verification duration: 35 seconds.

## 7. Hash values recorded in the report

**MD5**

`d16e29d1251eef64efa84f09ec04a166`

**SHA-1**

`b0193e76ddb73cdeded5f6840095f288af52b011`

### Verification results

| Verification field | MD5 | SHA-1 |
|---|---|---|
| Computed hash | Verified | Verified |
| Stored verification hash | Verified | Verified |
| Report hash | Verified | Verified |

**Result:** All six reported verification checks were marked verified by FTK Imager.

Hash verification supports integrity checking of the acquired data that was successfully read. It does not establish that every source sector was readable or that the original device was uncompromised.

## 8. Acquisition warning: unreadable sectors

FTK Imager reported that these source sectors could not be read:

- Sector 728125
- Sector 728127
- Sectors 729540–729543
- Sectors 925300–925303

The report states that the contents of these sectors were replaced with zeros in the image.

### Forensic significance

Some source-sector contents were not successfully acquired. The resulting image therefore contains zero-filled replacements for those unreadable sectors. The reason for the read errors cannot be determined from this report alone.

This limitation must be disclosed in any subsequent examination or report. A successful hash verification does not mean the unreadable source data was recovered.

## 9. Results

- An E01 image named `new ftk.E01` was created.
- FTK Imager reported MD5 and SHA-1 verification as successful.
- All six verification entries were marked verified.
- Unreadable source sectors were reported and replaced with zeros in the image.
- Acquisition and verification timestamps were recorded.

## 10. Conclusion

The practical demonstrated physical-source acquisition and image verification using FTK Imager 8.3.1.6. The reported MD5 and SHA-1 verification checks passed. However, the acquisition report also identified unreadable sectors whose contents were zero-filled in the image. This limitation is important and must remain part of the evidence record.

## 11. Further integrity practice

MD5 and SHA-1 are legacy algorithms with known collision weaknesses. Where supported, use SHA-256 as an additional modern integrity check. A SHA-256 hash calculated over the E01 file itself identifies that image file; it is distinct from a source-versus-acquired-data verification.

## 12. Evidence screenshots to add

- FTK Imager main interface
- Physical source selection
- E01 acquisition configuration
- Acquisition completion
- Hash verification report

Redact personal paths, usernames and device serial numbers before publishing screenshots. Do not upload the E01 evidence image or private source data.

## 13. References

- Exterro: https://www.exterro.com/
- NIST: https://www.nist.gov/
- NIST Computer Forensics Tool Testing: https://www.nist.gov/itl/ssd/software-quality-group/computer-forensics-tool-testing-program-cftt

**Practical status:** Performed; acquisition report documented.  
**Verification status:** All reported hash checks passed.  
**Acquisition limitation:** Unreadable sectors documented.

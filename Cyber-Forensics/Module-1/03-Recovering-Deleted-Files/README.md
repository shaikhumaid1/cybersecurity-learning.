# Practical 03: Recovering Deleted Files Using Recuva and Disk Drill

> **Module:** 1 — Digital Forensics  
> **Tools explored:** Recuva and Disk Drill  
> **Focus:** Deletion behavior, file-system recovery, scanning methods, file carving, limitations and evidence handling

## 1. Objective

To understand how deleted files may remain recoverable, explore Recuva and Disk Drill, compare common recovery techniques, and learn how to document recovery attempts without altering original evidence unnecessarily.

## 2. Introduction: What happens when a file is deleted?

Deleting a file usually does not mean that every data block is immediately erased.

On many file systems, deletion first changes file-system metadata to mark the file's space as available for reuse. The file's content may remain on the storage medium until new data overwrites it or storage-device behavior makes it unavailable.

For example, on NTFS, file records are maintained in the Master File Table (MFT). A deleted file's record and data may remain available for some time, but the record or clusters can be reused.

Recovery is therefore conditional. It depends on file system, device type, deletion method, overwrite activity, fragmentation, TRIM behavior and the recovery software.

## 3. Why deleted-file recovery is used

Recovery can support investigations where an examiner needs to determine:
- Whether a relevant document or image existed
- Whether a deleted file can be restored for examination
- Whether a file name, path or metadata record remains
- Whether recoverable content is consistent with an incident timeline

**Real-world example:** An organization investigates suspected deletion of project documents before an employee's departure. An examiner creates a forensic image, works on a copy and attempts recovery. Recovered documents are hashed, examined and correlated with file-system records, access logs and other evidence. Recovery alone does not prove who deleted the files or why.

## 4. Types of file deletion

### 4.1 Normal deletion

A file is deleted through an operating-system interface. Depending on the action, it may be moved to the Recycle Bin or marked for deletion.

### 4.2 Recycle Bin deletion

On Windows, deleting through the Recycle Bin can remove the normal user-facing reference while some associated data may remain until reused or cleaned.

### 4.3 Permanent deletion

Using Shift+Delete or emptying the Recycle Bin generally removes the ordinary recovery path, but underlying data may remain until overwritten or otherwise unavailable.

### 4.4 Secure deletion / overwrite

A secure-delete operation attempts to overwrite data so normal recovery methods cannot restore the original content. Effectiveness depends on storage technology and implementation. Traditional overwrite assumptions do not map reliably to SSD flash translation layers.

### 4.5 Formatting

Quick formatting may recreate file-system structures while leaving some old data blocks untouched. A full format or subsequent writes may make recovery much harder. Exact behavior depends on operating system, file system and device.

## 5. Main file-recovery techniques

### 5.1 File-system metadata recovery

Recovery software examines file-system structures for records of deleted files and their data-cluster locations.

**Examples of structures:**
- NTFS: MFT records, directory entries and allocation information
- FAT/FAT32: directory entries and FAT allocation chains
- exFAT: directory entries and allocation bitmap
- Other file systems have their own metadata structures

**Strength:** Can preserve original file names, paths and some timestamps when metadata remains intact.

**Limitation:** Metadata may be overwritten, damaged or incomplete.

### 5.2 File carving / signature-based recovery

File carving searches raw data for known file signatures and structural patterns, often without relying on the original file-system directory entry.

Examples of recognizable formats include JPEG, PNG, PDF and ZIP.

**Strength:** May recover content when file-system metadata is missing.

**Limitations:**
- Original filenames and folder paths are often lost.
- Fragmented files may be incomplete or incorrectly reconstructed.
- False positives can occur.
- Not every format has a reliable signature.

### 5.3 Deep scanning

A deep scan may inspect more of the volume than a quick metadata-based scan and may combine file-system analysis with signature-based methods. Exact behavior is vendor- and version-dependent.

**Trade-off:** It may take longer and return more false positives or incomplete results.

### 5.4 Partition and file-system recovery

When partition or file-system structures are damaged, specialist tools may search for lost partitions or reconstruct file-system information. This is different from recovering a single deleted file.

## 6. Tool 1: Recuva

**Vendor:** Piriform / CCleaner  
**Type:** File-recovery utility

### Purpose

Recuva attempts to recover deleted files from supported storage media. Its results can include files identified through file-system information and scanning methods.

### Why use Recuva?

- Simple guided and advanced workflows
- Useful for basic recovery exercises
- Can scan supported local and removable media
- Provides file condition information in its results
- Offers a deep-scan option

### Typical methodology

1. Stop writing to the affected source.
2. Prefer a forensic image or working copy for an investigation.
3. Open Recuva and select the relevant file type or all files.
4. Select the original location or appropriate drive.
5. Run a normal scan.
6. If necessary, run a deep scan on the working copy.
7. Review filenames, paths and recovery condition.
8. Recover selected files to a different physical device.
9. Open and validate recovered files.
10. Record results, hashes and limitations.

### Recovery condition

A tool may report a file as recoverable or damaged based on its scan. This is an estimate, not proof that the restored file will be complete. Validate the recovered content.

### Limitations

- File-system and fragmentation conditions affect recovery.
- New writes may overwrite file data or metadata.
- SSD TRIM can make deleted content unavailable.
- Securely overwritten files are generally not recoverable through ordinary recovery.
- A recovered file does not establish who deleted it.

## 7. Tool 2: Disk Drill

**Vendor:** CleverFiles  
**Type:** Data-recovery utility

### Purpose

Disk Drill scans supported storage devices and attempts to recover deleted or lost files. Features depend on operating system, edition and version.

### Why use Disk Drill?

- Guided recovery workflow
- Scanning of supported storage devices
- File preview in supported cases
- Recovery of multiple file categories
- Additional disk and recovery utilities, depending on edition

### Typical methodology

1. Identify the affected device.
2. Avoid writing new data to it.
3. If appropriate, acquire an image and work from a copy.
4. Select the source in Disk Drill.
5. Choose the available scan or recovery method.
6. Review detected files and previews.
7. Recover selected data to a separate destination.
8. Validate the recovered files.
9. Record scan type, findings and limitations.

### Limitations

- Recovery support varies by platform, file system and product edition.
- SSD TRIM and overwritten sectors can prevent recovery.
- Reconstructed files may be incomplete.
- A scan result is not proof of file authenticity or user activity.

## 8. Comparison: Recuva vs Disk Drill

| Feature | Recuva | Disk Drill |
|---|---|---|
| Main purpose | Deleted-file recovery | Data recovery |
| Guided workflow | Yes | Yes |
| Deep / advanced scanning | Deep Scan option | Advanced scan methods vary by version |
| File preview | Supported file types | Supported file types |
| Platform support | Windows | Windows and macOS product offerings |
| Licensing | Free and paid offerings vary | Free and paid offerings vary |
| Forensic suite replacement? | No | No |

Check current vendor documentation for exact feature and edition differences.

## 9. Alternative recovery tools

| Tool | Main use | Notes |
|---|---|---|
| PhotoRec | Signature-based file carving | Open source; often loses original names and paths |
| TestDisk | Partition and file-system recovery | Useful for lost partitions and some file recovery |
| Autopsy / The Sleuth Kit | Forensic examination of images | Useful for examining deleted entries and artifacts |
| R-Studio | Advanced data recovery | Commercial product |
| UFS Explorer | Data recovery and file-system support | Commercial product family |
| X-Ways Forensics | Forensic examination and recovery | Professional commercial tool |

**Important distinction:** A general-purpose recovery utility is not automatically a validated forensic acquisition or examination platform. In evidentiary work, document tool versions, acquisition process, source protection and validation.

## 10. Storage concepts that affect recovery

### HDD

Hard disk drives store data magnetically. Deleted content may remain in sectors until overwritten, subject to drive and file-system behavior.

### SSD

Solid-state drives use flash memory and controller-managed translation. TRIM and garbage collection can make deleted data unavailable even when a file has not been overwritten in the traditional HDD sense.

### USB flash drives and memory cards

These often use flash storage and controller-level wear leveling. Recovery behavior varies by controller, file system and device condition.

### File fragmentation

A fragmented file is stored in multiple non-contiguous clusters. Metadata-based recovery may locate the fragments, but carving may struggle to reconstruct their correct order.

## 11. Safe forensic recovery methodology

For an actual investigation:

1. Confirm legal authority and scope.
2. Photograph and identify the evidence source.
3. Record device details and condition.
4. Use a suitable write blocker during acquisition where possible.
5. Acquire and verify a forensic image.
6. Preserve the original and create a working copy.
7. Perform recovery against the working copy.
8. Save recovered files to a separate destination.
9. Hash recovered files and record tool versions.
10. Document unrecoverable, partial and corrupted results.

Do not install recovery software onto the affected source drive or save recovered files back to that source.

## 12. Practical experiment to perform

Use a spare USB drive and synthetic test files only.

1. Create a folder containing sample text, JPEG and PDF files.
2. Record filenames and calculate baseline hashes.
3. Safely delete selected test files.
4. Stop using the source drive.
5. Create a forensic image if the lab setup supports it.
6. Scan a working copy using Recuva.
7. Repeat with Disk Drill if available.
8. Recover files to a separate destination.
9. Compare recovered files with the original baseline using hashes.
10. Record which files were recovered, damaged or not found.

Do not run this experiment on the only copy of important data.

## 13. Results record

**Tools explored:** Recuva and Disk Drill.

The exact scan mode, test files, recovery status, file hashes and screenshots have not yet been supplied for this practical. Add those values after recording the actual experiment. Do not invent recovery results.

## 14. Conclusion

Deleted-file recovery relies on surviving file-system metadata, raw data signatures or other recoverable structures. Recuva and Disk Drill provide accessible recovery workflows, while tools such as PhotoRec, TestDisk and Autopsy support different recovery and forensic-analysis tasks. Recovery is not guaranteed, especially after overwriting, SSD TRIM or secure deletion. Evidence preservation and honest reporting of incomplete results are essential.

## 15. References

- Recuva official site: https://www.ccleaner.com/recuva
- Recuva support: https://support.ccleaner.com/
- Disk Drill: https://www.cleverfiles.com/
- PhotoRec / TestDisk: https://www.cgsecurity.org/
- Autopsy: https://www.autopsy.com/
- NIST SP 800-86: https://csrc.nist.gov/pubs/sp/800/86/final

**Practical status:** Recuva and Disk Drill explored; detailed experiment results to be added.


## 16. Actual Disk Drill scan results (additional screenshots)

### Device and initial condition
- Source: SanDisk Cruzer Blade USB flash drive
- Drive letter: E:
- Capacity displayed in Disk Drill: 7.29 GB
- File system: FAT32
- Connection: USB
- Before scanning, Windows File Explorer displayed "This folder is empty."

### Scan completion
Disk Drill displayed the completion notification: **"All recovery methods complete — 143 files / 9.68 GB found."**

The 9.68 GB value is the application's reported total for found file entries. It exceeds the displayed physical USB capacity of 7.29 GB, so it must not be interpreted as 9.68 GB of unique, successfully restored data. Reconstructed candidates and overlapping or fragmented content can affect scan totals.

### Visible file categories

| Category | Count shown | Displayed size |
|---|---:|---:|
| JPG | 9 | 2.87 MB |
| PNG | 10 | 1.82 MB |
| Videos | 15 | 4.07 GB |
| 3GP (inside Videos) | 2 | 4.19 MB |
| AVI (inside Videos) | 1 | 293 MB |
| M1V (inside Videos) | 7 | 12.4 MB |
| MP4 (inside Videos) | 3 | 1.96 GB |
| WEBM (inside Videos) | 2 | 1.80 GB |

These are the values visible in the supplied screenshots. Nested video subcategories should not be added to the parent Videos total as if they were separate top-level categories.

### Recovery-chance indicators

The screenshots show Disk Drill's estimated recovery-chance labels:
- **High:** filled star; a more favorable estimate.
- **Average:** partly filled star; an intermediate estimate.
- **Low:** empty star; a less favorable estimate.

Visible JPG examples are marked Low, visible PNG examples are marked High, and the displayed M1V entries are marked Average. These are estimates, not guarantees. Disk Drill's vendor documentation explains that the application evaluates whether file content appears intact; preview and validation are still required before treating a file as successfully recovered. <https://www.cleverfiles.com/help/estimated-recovery-chances-in-disk-drill/>

### Reconstructed file names

The M1V list includes generated names such as `file000001.m1v` and `file000002.m1v`. The JPG list also includes generic names containing image dimensions. Such generic naming is consistent with signature-based reconstruction, where original filenames or directory metadata may not be available. <https://www.cleverfiles.com/help/scanning-faqs/>

### Interpretation and evidence limitations

The USB volume appeared empty in File Explorer, while Disk Drill's completed scan identified 143 file entries totalling 9.68 GB according to its notification. This demonstrates that a normal directory listing and a recovery scan can show different results.

The screenshots prove that Disk Drill detected these candidates; they do **not** prove that all candidates were restored, are complete, or open correctly. The screenshots do not show a completed recovery destination, file-opening validation, SHA-256 hashes, or whether scanning was performed against a forensic image or the original USB device. No Recuva results have been supplied.

**Evidence status:** Disk Drill scan completed; 143 files / 9.68 GB reported by the application. Actual file restoration and integrity verification remain unconfirmed.

### Further validation
1. Preview representative JPG, PNG and video candidates.
2. Select a small sample with High, Average and Low estimates.
3. Recover selected test files to a separate storage device, never back to the source USB.
4. Open the recovered files in their native applications or a suitable media player.
5. Calculate and record SHA-256 hashes.
6. Record which files open correctly, are partial/corrupt, or cannot be recovered.
7. If available, perform a separate Recuva scan on a forensic working copy.

**Updated practical status:** Disk Drill scan completed and screenshot findings documented. Recuva comparison, file restoration and integrity validation remain pending.

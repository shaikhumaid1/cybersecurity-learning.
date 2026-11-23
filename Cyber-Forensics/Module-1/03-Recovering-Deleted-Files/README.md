# Practical 03: Recovering Deleted Files

## 1. Objective

Explore file-recovery utilities and understand how deleted files may remain recoverable on storage media.

## 2. Tools explored

- Recuva
- Disk Drill

## 3. Why file recovery is useful

When a file is deleted, the file-system reference or allocation information may be changed. The underlying data may remain on the storage device until overwritten or otherwise altered. Recovery is not guaranteed.

## 4. General recovery methodology

1. Stop using the affected storage device to reduce the risk of overwriting recoverable data.
2. Where appropriate, create a forensic image and work from a copy.
3. Select the correct source in the recovery tool.
4. Run an appropriate scan.
5. Review the files and their recovery condition.
6. Recover selected files to a different storage device.
7. Record the scan method, findings and limitations.

## 5. Tool overview

### Recuva

Recuva is a file-recovery utility designed to help recover deleted files from supported storage media. Recovery depends on whether the file data remains available and has not been overwritten.

### Disk Drill

Disk Drill is a data-recovery application with scanning and recovery capabilities for supported storage devices and file systems. Features and recovery limits depend on the edition and platform.

## 6. Important limitations

- Recovery is not guaranteed.
- Continued use of the source device may overwrite deleted data.
- SSD TRIM and other storage behaviors can reduce recovery chances.
- Recovered files may be incomplete or corrupted.
- Recovery results do not by themselves establish who deleted a file or when.

## 7. Practical record

**Tools explored:** Recuva and Disk Drill.  
**Detailed scan settings, recovered-file list, screenshots and actual results:** To be added from the practical evidence.

## 8. Conclusion

Recuva and Disk Drill provide interfaces for attempting recovery of deleted files. A careful investigator minimizes writes to the source, records the recovery process and avoids claiming successful recovery without examining the recovered files.

**Status:** Tools explored; detailed practical evidence to be added.

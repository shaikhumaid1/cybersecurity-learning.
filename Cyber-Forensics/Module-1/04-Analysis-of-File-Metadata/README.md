# Practical 04: Analysis of File Metadata Using ExifTool

> **Module:** 1 — Digital Forensics  
> **Tool explored:** ExifTool  
> **Focus:** Metadata types, extraction methods, command-line techniques, investigative use and limitations

## 1. Objective

To understand file metadata, identify common metadata standards, use ExifTool to inspect metadata in supported files, and evaluate how metadata can support a digital investigation.

## 2. What is file metadata?

Metadata is information that describes a file, its content, creation process, device or processing history.

For an image, metadata may include camera make and model, image dimensions, capture date, exposure settings, GPS coordinates and editing software.

Metadata is not necessarily trustworthy. It may be absent, altered, copied from another file, generated incorrectly or interpreted using the wrong time zone.

## 3. Why metadata analysis is important

Metadata can provide investigative leads that are not immediately visible from the file's content.

It can help an examiner:
- Identify the device or software associated with a file
- Review date and time fields
- Identify possible GPS location data
- Compare files from different sources
- Detect inconsistencies between file content and metadata
- Organize evidence for timeline analysis

**Real-world example:** A company receives an image attached to a suspicious email. An examiner uses ExifTool to inspect camera, software, timestamp and GPS fields. These fields may help guide further investigation, but they do not independently prove who took the photograph or where it was taken.

## 4. Types of metadata

### 4.1 EXIF metadata

Exchangeable Image File Format metadata is commonly embedded in JPEG and other supported image formats.

Possible fields include:
- Make and Model
- DateTimeOriginal
- ExposureTime
- FNumber
- ISO
- FocalLength
- GPSLatitude and GPSLongitude
- Orientation

### 4.2 IPTC metadata

IPTC metadata is commonly used for descriptive and editorial information in photographs.

Examples:
- Caption or description
- Creator or byline
- Copyright notice
- Keywords
- Location descriptions

### 4.3 XMP metadata

Extensible Metadata Platform is an XML-based metadata framework used by many applications and file formats.

It may contain:
- Creator information
- Description
- Copyright
- Editing history or application-related fields
- Structured descriptive information

### 4.4 File-system metadata

This metadata is maintained by the file system rather than necessarily embedded inside the file.

Examples include:
- File name
- File size
- Directory path
- File-system timestamps
- File attributes
- Allocation information

**Important:** Embedded metadata and file-system timestamps are different evidence sources. Copying or downloading a file may change file-system timestamps while leaving embedded EXIF fields unchanged.

### 4.5 Document metadata

Documents may contain:
- Author
- Title
- Subject
- Company
- Application name
- Creation and modification fields
- Revision or template information

The available fields depend on file format and application.

## 5. Tool overview: ExifTool

**Developer:** Phil Harvey and contributors  
**Type:** Command-line metadata reader and writer  
**Official site:** https://exiftool.org/

ExifTool reads, writes and edits metadata across a large range of file formats. In forensic work, it is especially useful for extracting and comparing metadata.

### Why use ExifTool?

- Supports many file types and metadata formats
- Displays metadata in readable form
- Supports selected-tag extraction
- Can produce structured output such as JSON or CSV
- Can recursively process directories
- Can read, write or remove metadata

**Forensic caution:** For evidence examination, prefer read-only extraction commands and work on a forensic copy. Metadata-writing commands alter files and should only be used on disposable test copies when learning editing behavior.

## 6. Metadata extraction methodology

1. Identify the evidence file and record its source.
2. Preserve the original and work on a copy.
3. Calculate a hash of the working file before analysis.
4. Run ExifTool in read-only mode.
5. Review relevant metadata groups and tags.
6. Record the exact command and tool version.
7. Compare metadata with file-system timestamps and other evidence.
8. Hash the file again if there is any concern about modification.
9. Document missing, conflicting or suspicious fields.

## 7. ExifTool commands and techniques

### 7.1 Display basic metadata

```bash
exiftool sample.jpg
```

Displays available metadata in a readable format.

### 7.2 Display duplicate and unknown tags with group names

```bash
exiftool -a -u -g1 sample.jpg
```

- `-a`: show duplicate tags
- `-u`: show unknown tags
- `-g1`: organize output by metadata group

### 7.3 Extract selected fields

```bash
exiftool -Make -Model -DateTimeOriginal sample.jpg
```

Useful when an investigation focuses on camera and capture-time fields.

### 7.4 Inspect GPS metadata

```bash
exiftool -GPS:all sample.jpg
```

GPS tags may reveal coordinates if they exist. Treat location data as sensitive and do not publish private coordinates.

### 7.5 Recursively inspect a directory

```bash
exiftool -r ./evidence-images
```

Processes supported files in a directory tree. Use this only on an authorized evidence copy.

### 7.6 Export metadata as JSON

```bash
exiftool -json sample.jpg
```

Structured output can be useful for repeatable analysis and later comparison.

### 7.7 Export metadata to a text file

```bash
exiftool sample.jpg > metadata.txt
```

Stores the console output in a text file. Preserve the original evidence file.

### 7.8 Read metadata from multiple files

```bash
exiftool *.jpg
```

Useful for comparing a group of sample images.

### 7.9 Check the ExifTool version

```bash
exiftool -ver
```

Record the version in the investigation notes.

**Writing metadata:** ExifTool also supports commands to write or delete metadata. Do not use write operations on original evidence. Test them only on disposable copies and preserve a baseline hash.

## 8. Metadata interpretation example

Suppose a sample photograph reports:

| Field | Illustrative value |
|---|---|
| Make | Example Camera Co. |
| Model | Example Model |
| DateTimeOriginal | 2026:11:20 14:30:00 |
| Software | Example Editor |
| GPS | Not present |

Possible interpretation:
- The metadata claims a particular camera make and model.
- The capture-time field records a date and time.
- An editing-software field may indicate later processing.
- No GPS tag was found in the inspected metadata.

These are metadata observations, not proof of the actual device, time, location or person. Fields can be edited or copied.

## 9. Techniques used in metadata investigation

### 9.1 Metadata extraction

Read available tags from one or more files and preserve the output.

### 9.2 Metadata comparison

Compare tags across files to identify common devices, software, timestamps or inconsistencies.

### 9.3 Timeline correlation

Compare embedded capture dates with file-system timestamps, email timestamps, application logs and other independent records. Account for time zones and clock errors.

### 9.4 GPS examination

Review GPS tags where present. Verify coordinate format and consider whether the values are plausible. Handle location information as sensitive evidence.

### 9.5 Metadata anomaly review

Look for unexpected or inconsistent values, such as:
- Capture date later than a known event
- Different software fields across apparently related files
- Missing or contradictory metadata
- Unexpected GPS coordinates

An anomaly is a lead for further examination, not proof of tampering.

## 10. Alternative metadata tools

| Tool | Primary use |
|---|---|
| MediaInfo | Technical metadata for audio and video |
| FFprobe | Media streams, codecs, duration and container information |
| ExifTool | Broad metadata extraction and editing |
| pdfinfo | PDF document properties |
| strings | Extract readable text sequences from binary files; not a complete metadata parser |
| Autopsy | Examine file metadata in a broader forensic case |

Tool choice depends on the file type and the investigative question. A media-analysis tool may expose codec and stream details that are not the focus of an image EXIF tool.

## 11. Limitations and forensic considerations

- Metadata can be modified or removed.
- Some applications strip metadata during export or sharing.
- Time fields may use different time zones or incorrect device clocks.
- GPS fields can be absent, inaccurate or deliberately changed.
- Metadata may be copied from another file.
- A metadata field does not establish identity or authorship.
- File-system timestamps and embedded metadata must not be confused.
- ExifTool's ability to write metadata means examiners must use read-only commands on evidence copies.

## 12. Practical experiment to document

Use photographs you created yourself or public sample files with permission.

1. Select two or three sample images.
2. Record each file's SHA-256 hash.
3. Run `exiftool -ver`.
4. Run `exiftool sample.jpg`.
5. Run `exiftool -a -u -g1 sample.jpg`.
6. Extract selected EXIF, IPTC, XMP and GPS tags where present.
7. Export the results as JSON.
8. Compare metadata between the sample files.
9. Record fields that are absent or inconsistent.
10. Hash the files again and confirm that read-only inspection did not change them.

Do not publish private GPS coordinates, personal photographs or sensitive document metadata.

## 13. Practical experiment and recorded observations

### 13.1 Test environment and sample

| Item | Recorded value |
|---|---|
| Tool | ExifTool |
| ExifTool version | 13.55 |
| Operating environment | Linux command line |
| Sample file | `img1.jpeg` |
| File format | JPEG |
| Reported file size | 53 kB |
| Image dimensions | 738 × 414 pixels |
| Image size | 0.306 megapixels |

### 13.2 Command 1 — Basic metadata extraction

**Command executed:**

```bash
exiftool /home/umaid/Downloads/img1.jpeg
```

**Important observed output:**

| Metadata field | Actual value |
|---|---|
| File Name | img1.jpeg |
| Directory | /home/umaid/Downloads |
| File Size | 53 kB |
| File Modification Date/Time | 2026:11:25 21:51:46+05:30 |
| File Access Date/Time | 2026:11:25 21:51:46+05:30 |
| File Inode Change Date/Time | 2026:11:25 21:51:46+05:30 |
| File Permissions | -rw-r--r-- |
| File Type | JPEG |
| File Type Extension | jpg |
| MIME Type | image/jpeg |
| JFIF Version | 1.01 |
| Resolution Unit | None |
| X Resolution | 1 |
| Y Resolution | 1 |
| Image Width | 738 |
| Image Height | 414 |
| Encoding Process | Baseline DCT, Huffman coding |
| Bits Per Sample | 8 |
| Color Components | 3 |
| YCbCr Sub Sampling | YCbCr4:2:0 (2 2) |
| Image Size | 738x414 |
| Megapixels | 0.306 |

**Interpretation:**

- The file is identified as a JPEG image with JFIF version 1.01.
- The image contains 738 × 414 pixels, or approximately 0.306 megapixels.
- It uses 8 bits per sample and three colour components.
- The reported chroma subsampling is YCbCr 4:2:0.
- The filesystem-related fields show modification, access and inode-change times, along with Linux file permissions.
- The timestamp includes the UTC offset `+05:30`.

### 13.3 Command 2 — Grouped metadata, duplicate and unknown tags

**Command executed:**

```bash
exiftool -a -u -g1 /home/umaid/Downloads/img1.jpeg
```

**Purpose of the options:**

- `-a`: allows duplicate tags to be displayed.
- `-u`: requests unknown tags as well as recognized tags.
- `-g1`: organizes the output by metadata group (family 1).

**Groups actually displayed:**

| Group | Fields observed |
|---|---|
| ExifTool | ExifTool Version Number: 13.55 |
| System | File name, directory, file size, timestamps and permissions |
| File | JPEG type, extension, MIME type, dimensions, encoding, bits per sample, colour components and subsampling |
| JFIF | JFIF version, resolution unit and X/Y resolution |
| Composite | Image size and megapixels |

No camera make/model, original capture date, exposure, GPS, IPTC or XMP fields appeared in the supplied output. This means those fields were not exposed in this extraction; it does not prove that every possible metadata structure in the file is absent.

The second command reported the file access time as `2026:11:25 21:53:01+05:30`, later than the first command's access time. This is consistent with the file being accessed during the examination. Access timestamps may change as a result of reading a file, depending on the filesystem and mount settings.

### 13.4 Command 3 — JSON metadata export

**Command executed:**

```bash
exiftool -json img1.jpeg
```

The command returned a JSON array containing one object for `img1.jpeg`. It included the ExifTool version, source file, file-system information, JPEG/JFIF fields, image dimensions and composite image measurements.

Notable values in the JSON output:

- `ExifToolVersion`: 13.55
- `FileName`: img1.jpeg
- `Directory`: .
- `FileSize`: 53 kB
- `ImageWidth`: 738
- `ImageHeight`: 414
- `ImageSize`: 738x414
- `Megapixels`: 0.306

The JSON output reports `Directory` as `.`, whereas the first command reports `/home/umaid/Downloads`. The JSON command was entered with a relative filename, so the output records the current working directory as `.`. This is a path-context difference, not evidence that the image content changed.

**Why JSON is useful:** JSON provides structured output that can be parsed by scripts, stored in case notes, compared across files or imported into a data-analysis workflow.

### 13.5 Command 4 — Save and review a text report

**Command/output reviewed:**

```bash
cat metadata.txt
```

The displayed text report contains the metadata output for `img1.jpeg`, including the file-system fields, JPEG/JFIF properties, dimensions and composite values.

The `cat` command displays the saved text file; it does not perform a new metadata extraction. The report is useful for retaining a readable record of the earlier ExifTool output.

### 13.6 Overall findings

1. ExifTool 13.55 successfully identified and read the supplied JPEG file.
2. The image dimensions were 738 × 414 pixels and the reported size was 53 kB.
3. JPEG/JFIF technical properties were extracted, including baseline DCT encoding, 8 bits per sample, three colour components and YCbCr 4:2:0 subsampling.
4. Filesystem metadata included timestamps and Linux permissions.
5. The grouped output did not display camera-identification, GPS, IPTC or XMP fields.
6. JSON export and a text report were produced and reviewed.

### 13.7 Forensic interpretation and limitations

- The displayed file modification, access and inode-change timestamps describe filesystem-level information available to ExifTool on the examination system. They should not be treated as the photograph's original capture time.
- The file permissions `-rw-r--r--` represent read/write permission for the owner and read permission for group and others in the displayed Linux permission format.
- JFIF and JPEG technical fields describe the image encoding and dimensions; they do not identify the photographer.
- No GPS coordinates or camera make/model were present in the supplied output. Therefore, this result cannot be used to establish the photograph's capture location or camera.
- A missing metadata field does not prove that the file was never created by a camera or edited by software. Metadata may be stripped, omitted or unsupported.
- No SHA-256 hash or before-and-after integrity comparison was supplied. File integrity verification is therefore not claimed.
- The supplied commands are metadata-reading commands. The `cat` command reads the saved report.

**Evidence status:** Four ExifTool outputs supplied and documented. Hash verification and comparison against an original source image were not supplied.

## 14. Conclusion

In this practical, ExifTool 13.55 was used to inspect a JPEG image, display grouped metadata, export metadata in JSON format and review a saved text report. The examination identified a 53 kB JPEG image with dimensions of 738 × 414 pixels and several JPEG/JFIF technical properties. Filesystem timestamps and Linux permissions were also displayed. No camera, GPS, IPTC or XMP fields appeared in the supplied extraction. The results demonstrate how metadata can provide technical and filesystem context, while also showing why missing fields and timestamps must be interpreted carefully. Metadata alone cannot prove the identity of the photographer, the original capture time or the location of the image.

## 15. References

- ExifTool official documentation: https://exiftool.org/
- ExifTool command documentation: https://exiftool.org/exiftool_pod.html
- NIST SP 800-86: https://csrc.nist.gov/pubs/sp/800/86/final
- NIST Digital Investigation Techniques: https://www.nist.gov/publications/digital-investigation-techniques-scientific-foundation-review

**Practical status:** ExifTool extraction, grouped output, JSON export and text-report review completed and documented from the supplied command output.

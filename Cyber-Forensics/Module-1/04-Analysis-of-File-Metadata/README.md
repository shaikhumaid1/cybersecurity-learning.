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

## 13. Results record

**Tool explored:** ExifTool.

The actual sample filenames, command output, metadata fields observed, hashes and screenshots have not yet been supplied. Add the real observations after completing or reviewing the experiment. Do not use the illustrative values above as actual lab results.

## 14. Conclusion

Metadata analysis provides useful context about digital files. ExifTool supports broad extraction and examination of EXIF, IPTC, XMP and other metadata. However, metadata can be altered, removed or misinterpreted. A reliable examiner preserves the original, records exact commands, correlates metadata with independent evidence and clearly distinguishes observations from conclusions.

## 15. References

- ExifTool official documentation: https://exiftool.org/
- ExifTool command documentation: https://exiftool.org/exiftool_pod.html
- NIST SP 800-86: https://csrc.nist.gov/pubs/sp/800/86/final
- NIST Digital Investigation Techniques: https://www.nist.gov/publications/digital-investigation-techniques-scientific-foundation-review

**Practical status:** ExifTool explored; detailed sample outputs and screenshots to be added.

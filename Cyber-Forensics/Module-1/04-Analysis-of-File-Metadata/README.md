# Practical 04: Analysis of File Metadata Using ExifTool

## 1. Objective

Explore ExifTool and understand how metadata embedded in supported files can provide information useful during digital investigations.

## 2. Tool

**ExifTool** is a metadata-reading and editing utility that supports many image, audio, video and document formats.

Official website: https://exiftool.org/

## 3. What is metadata?

Metadata is information describing a file or its contents. Depending on the file format, it may include:

- File type and format details
- Camera make and model
- Image dimensions
- Date and time fields
- GPS coordinates, if present
- Software or device information

Metadata availability varies by format and may be missing, modified or inaccurate.

## 4. Why metadata analysis is useful

Metadata can provide investigative leads, help compare files and offer context about how a file was created or processed. It should be corroborated with other evidence and should not be treated as proof on its own.

## 5. Example commands

Read metadata from an image:

```bash
exiftool image.jpg
```

Display all available metadata:

```bash
exiftool -a -u -g1 image.jpg
```

Extract selected date and camera fields:

```bash
exiftool -DateTimeOriginal -Make -Model image.jpg
```

These commands are examples; record the actual command and output used in the lab.

## 6. Methodology

1. Use an authorized sample image or a working copy.
2. Open a terminal in the directory containing the sample.
3. Run ExifTool against the file.
4. Review available metadata fields.
5. Record relevant observations and missing fields.
6. Preserve the original sample and document any metadata changes separately.

## 7. Limitations

- Metadata can be edited or removed.
- Some file formats contain little or no metadata.
- Timestamps may reflect different time zones or device settings.
- GPS information may be absent or inaccurate.
- Metadata alone does not prove file origin, authenticity or user identity.

## 8. Practical record

**Tool explored:** ExifTool.  
**Actual sample filename, command output, observations and screenshots:** To be added from the practical evidence.

## 9. Conclusion

ExifTool helps investigators inspect metadata in supported files. Metadata can provide useful investigative context, but findings should be validated against other evidence and reported with appropriate limitations.

**Status:** Tool explored; detailed practical evidence to be added.

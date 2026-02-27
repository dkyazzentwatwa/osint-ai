# Metadata Extraction Module

> **Module ID:** META-EXT-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Metadata Analysis

---

## 1. Overview

Metadata extraction reveals hidden information embedded in files - creation details, authorship, software used, and sometimes sensitive data like GPS coordinates. This module provides systematic extraction procedures.

---

## 2. PDF Metadata Fields

### 2.1 Standard Document Information Dictionary

| Field | Description | Forensic Value |
|-------|-------------|----------------|
| `Title` | Document title | Subject matter identification |
| `Author` | Document creator | Attribution source |
| `Subject` | Document topic | Content categorization |
| `Keywords` | Search terms | Intent and scope |
| `Creator` | Creating application | Software fingerprint |
| `Producer` | PDF converter | Conversion chain |
| `CreationDate` | First creation | Timeline anchor |
| `ModDate` | Last modification | Edit detection |
| `Trapped` | Color trapping | Print production info |

### 2.2 PDF Date Format

```
Format: D:YYYYMMDDHHmmSSOHH'mm'

Example: D:20240115143000-05'00'
         │  │││││││││││└┬┘└┬┘
         │  │││││││││││ │  └── Minutes offset
         │  │││││││││││ └──── Hours offset (-05 = EST)
         │  │││││││││└────── Seconds
         │  │││││││└──────── Minutes
         │  │││││└────────── Hours (24h)
         │  │││└──────────── Day
         │  │└───────────── Month
         │  └────────────── Year
         └───────────────── Prefix
```

### 2.3 XMP Metadata Schema

**Namespaces:**
```xml
dc    - Dublin Core (title, creator, description)
xmp   - XMP Basic (CreateDate, CreatorTool)
pdf   - PDF Properties (PDFVersion, Producer)
xmpMM - XMP Media Management (History, Ingredients)
photoshop - Photoshop properties
```

**Example XMP Structure:**
```xml
<?xpacket begin="" id="W5M0MpCehiHzreSzNTczkc9d"?>
<x:xmpmeta xmlns:x="adobe:ns:meta/">
  <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
    <rdf:Description rdf:about=""
      xmlns:dc="http://purl.org/dc/elements/1.1/"
      xmlns:pdf="http://ns.adobe.com/pdf/1.3/"
      xmlns:xmp="http://ns.adobe.com/xap/1.0/">
      <dc:title><rdf:Alt><rdf:li xml:lang="x-default">Document Title</rdf:li></rdf:Alt></dc:title>
      <dc:creator><rdf:Seq><rdf:li>Author Name</rdf:li></rdf:Seq></dc:creator>
      <pdf:Producer>Adobe PDF Library 23.0</pdf:Producer>
      <xmp:CreateDate>2024-01-15T14:30:00-05:00</xmp:CreateDate>
      <xmp:CreatorTool>Microsoft Word for Mac</xmp:CreatorTool>
    </rdf:Description>
  </rdf:RDF>
</x:xmpmeta>
<?xpacket end="w"?>
```

---

## 3. Office Document Properties

### 3.1 Open XML Structure

Office documents (DOCX, XLSX, PPTX) contain:
```
docProps/core.xml     - Dublin Core metadata
docProps/app.xml      - Application-specific properties
docProps/custom.xml   - User-defined properties
```

### 3.2 Core Properties (core.xml)

```xml
<cp:coreProperties xmlns:cp="..." xmlns:dc="..." xmlns:dcterms="...">
  <dc:title>Project Proposal</dc:title>
  <dc:subject>Q4 Planning</dc:subject>
  <dc:creator>john.doe@company.com</dc:creator>
  <cp:lastModifiedBy>jane.smith@company.com</cp:lastModifiedBy>
  <cp:revision>12</cp:revision>
  <dcterms:created xsi:type="dcterms:W3CDTF">2024-01-10T09:00:00Z</dcterms:created>
  <dcterms:modified xsi:type="dcterms:W3CDTF">2024-01-15T16:30:00Z</dcterms:modified>
  <cp:category>Confidential</cp:category>
  <cp:contentStatus>Draft</cp:contentStatus>
</cp:coreProperties>
```

### 3.3 Application Properties (app.xml)

**Word-specific:**
```xml
<Properties xmlns="http://schemas.openxmlformats.org/officeDocument/2006/extended-properties">
  <Template>Normal.dotm</Template>
  <TotalTime>45</TotalTime>           <!-- Minutes editing -->
  <Pages>12</Pages>
  <Words>3450</Words>
  <Characters>19650</Characters>
  <Application>Microsoft Office Word</Application>
  <DocSecurity>0</DocSecurity>        <!-- 0=none, 8=password -->
  <Lines>450</Lines>
  <Paragraphs>127</Paragraphs>
  <Company>Acme Corp</Company>
  <HyperlinksChanged>false</HyperlinksChanged>
  <LinksUpToDate>false</LinksUpToDate>
  <ScaleCrop>false</ScaleCrop>
  <SharedDoc>false</SharedDoc>
</Properties>
```

**Excel-specific:**
```xml
<Properties>
  <Application>Microsoft Excel</Application>
  <DocSecurity>0</DocSecurity>
  <ScaleCrop>false</ScaleCrop>
  <HeadingPairs>...</HeadingPairs>
  <TitlesOfParts>...</TitlesOfParts>
  <Company>Acme Corp</Company>
  <LinksUpToDate>false</LinksUpToDate>
  <SharedDoc>false</SharedDoc>
  <HyperlinksChanged>false</HyperlinksChanged>
  <AppVersion>16.0000</AppVersion>
</Properties>
```

### 3.4 Custom Properties

```xml
<Properties xmlns="http://schemas.openxmlformats.org/officeDocument/2006/custom-properties">
  <property fmtid="{D5CDD505-2E9C-101B-9397-08002B2CF9AE}" pid="2" name="ProjectCode">
    <vt:lpwstr>PRJ-2024-001</vt:lpwstr>
  </property>
  <property fmtid="{D5CDD505-2E9C-101B-9397-08002B2CF9AE}" pid="3" name="Department">
    <vt:lpwstr>Engineering</vt:lpwstr>
  </property>
</Properties>
```

---

## 4. Image EXIF Fields

### 4.1 GPS Information (IFD_GPSInfo)

| Tag ID | Field | Description |
|--------|-------|-------------|
| 0x0000 | GPSVersionID | GPS tag version (2.2.0.0) |
| 0x0001 | GPSLatitudeRef | 'N' or 'S' |
| 0x0002 | GPSLatitude | Degrees, Minutes, Seconds |
| 0x0003 | GPSLongitudeRef | 'E' or 'W' |
| 0x0004 | GPSLongitude | Degrees, Minutes, Seconds |
| 0x0005 | GPSAltitudeRef | 0=above sea level, 1=below |
| 0x0006 | GPSAltitude | Meters |
| 0x0007 | GPSTimeStamp | Atomic clock time (UTC) |
| 0x001D | GPSDateStamp | Date (YYYY:MM:DD) |
| 0x0011 | GPSImgDirectionRef | 'T' (true) or 'M' (magnetic) |
| 0x0012 | GPSImgDirection | Direction camera faced |

### 4.2 Coordinate Format Conversion

**EXIF Format:**
```
GPSLatitude: [34, 3, 22.15]  → 34° 3' 22.15"
GPSLatitudeRef: N
GPSLongitude: [118, 15, 35.28] → 118° 15' 35.28"
GPSLongitudeRef: W
```

**Decimal Degrees:**
```
Latitude: 34 + (3/60) + (22.15/3600) = 34.0561528° N
Longitude: -(118 + (15/60) + (35.28/3600)) = -118.2598° W
```

### 4.3 Timestamp Fields

| Field | Description | Format |
|-------|-------------|--------|
| `DateTime` | File modification | YYYY:MM:DD HH:MM:SS |
| `DateTimeOriginal` | Capture time | YYYY:MM:DD HH:MM:SS |
| `DateTimeDigitized` | Digitization time | YYYY:MM:DD HH:MM:SS |
| `SubSecTime` | Fractional seconds for DateTime | String |
| `SubSecTimeOriginal` | Fractional seconds for Original | String |
| `GPSTimeStamp` | GPS atomic time | HH, MM, SS (array) |
| `GPSDateStamp` | GPS date | YYYY:MM:DD |
| `OffsetTime` | Timezone offset | ±HH:MM |
| `OffsetTimeOriginal` | Original timezone offset | ±HH:MM |

### 4.4 Camera Information Fields

```
Make                    - "Canon", "Nikon", "Apple", etc.
Model                   - "iPhone 14 Pro", "EOS R5"
Software                - "16.5.1" (firmware)
LensMake                - "Canon"
LensModel               - "RF 24-70mm F2.8 L IS USM"
LensSpecification       - [24, 70, 0, 0] (focal length range)
BodySerialNumber        - Camera serial
LensSerialNumber        - Lens serial
CameraOwnerName         - Owner string (Canon)
```

### 4.5 Shooting Condition Fields

```
ExposureTime            - 1/250 (shutter speed)
FNumber                 - 2.8 (aperture)
ExposureProgram         - 1=Manual, 2=Program, 3=Aperture, etc.
ISOSpeedRatings         - 400 (ISO)
SensitivityType         - 2=REI, 3=ISO
PhotographicSensitivity - Actual ISO used
ApertureValue           - APEX units
BrightnessValue         - APEX units
ExposureBiasValue       - ± EV compensation
MaxApertureValue        - Lens max aperture
SubjectDistance         - Meters to subject
MeteringMode            - 2=Center, 5=Pattern, etc.
LightSource             - 0=Unknown, 1=Daylight, etc.
Flash                   - Bit field (0x0=no, 0x1=fired)
FocalLength             - 35mm equivalent
SubjectArea             - Focus point coordinates
```

---

## 5. Archive Metadata

### 5.1 ZIP Archive Structure

**Local File Header:**
```
Offset  Size  Field
  0      4    Signature (0x04034b50)
  4      2    Version needed
  6      2    General purpose bit flag
  8      2    Compression method
  10     2    File last modification time
  12     2    File last modification date
  14     4    CRC-32
  18     4    Compressed size
  22     4    Uncompressed size
  26     2    File name length
  28     2    Extra field length
  30     n    File name
  30+n   m    Extra field
```

### 5.2 ZIP Metadata Extraction

```bash
# List with metadata
unzip -l archive.zip

# Verbose listing with extra fields
unzip -lv archive.zip

# Check for comments
zipinfo -c archive.zip

# Full metadata dump
zipinfo -v archive.zip
```

### 5.3 RAR Metadata

```bash
# List RAR contents
unrar l archive.rar

# Verbose listing
unrar lt archive.rar

# Technical information
unrar vt archive.rar
```

### 5.4 7-Zip Metadata

```bash
# List all information
7z l archive.7z

# Technical info
7z lt archive.7z

# Full property list
7z l -slt archive.7z
```

---

## 6. Metadata Extraction Procedures

### 6.1 General Extraction Workflow

```
Step 1: Identify file type
        └─→ file command, extension analysis

Step 2: Select appropriate tools
        ├─→ Images: ExifTool, exiv2
        ├─→ PDFs: pdfinfo, ExifTool
        ├─→ Office: oletools, direct XML extraction
        └─→ Archives: Built-in list commands

Step 3: Extract all metadata
        └─→ Preserve original timestamps

Step 4: Parse and normalize
        └─→ Convert dates to ISO 8601

Step 5: Cross-reference fields
        └─→ Identify inconsistencies

Step 6: Document findings
        └─→ Confidence ratings for each field
```

### 6.2 Tool-Specific Commands

**ExifTool (Comprehensive):**
```bash
# Extract all metadata to text
exiftool -a -u -g1 file.pdf > metadata.txt

# CSV export for multiple files
exiftool -csv -r directory/ > all_metadata.csv

# Specific fields only
exiftool -FileName -CreateDate -Author file.docx

# JSON output (structured)
exiftool -j file.jpg > metadata.json

# Extract binary data (thumbnails)
exiftool -b -ThumbnailImage image.jpg > thumb.jpg
```

**exiv2 (Images):**
```bash
# Print all EXIF data
exiv2 -pa image.jpg

# Extract specific key
exiv2 -g GPSInfo image.jpg

# Modify metadata (for testing)
exiv2 -M"set Exif.Photo.DateTimeOriginal 2024:01:15 10:30:00" image.jpg
```

**pdfinfo (PDFs):**
```bash
# Basic metadata
pdfinfo document.pdf

# Custom fields
pdfinfo -custom document.pdf

# Check for JavaScript
pdfinfo -js document.pdf
```

**oletools (Office):**
```bash
# Metadata extraction
olemeta document.doc

# VBA macro analysis
olevba document.docm

# Check for encrypted content
mraptor document.doc
```

### 6.3 Date Normalization

**Common Date Formats Found:**
```
PDF:      D:20240115143000-05'00'
EXIF:     2024:01:15 14:30:00
ISO 8601: 2024-01-15T14:30:00-05:00
Unix:     1705332600
Windows:  133491354000000000 (FILETIME)
```

**Conversion Reference:**
```python
# Python datetime conversion examples
from datetime import datetime

# PDF date
def parse_pdf_date(d):
    return datetime.strptime(d[2:14], "%Y%m%d%H%M%S")

# EXIF date
def parse_exif_date(d):
    return datetime.strptime(d, "%Y:%m:%d %H:%M:%S")
```

---

## 7. Privacy Implications

### 7.1 Sensitive Metadata Types

| Category | Risk Level | Examples |
|----------|-----------|----------|
| **GPS Coordinates** | CRITICAL | Exact location of photo |
| **Usernames** | HIGH | dc:creator, Author fields |
| **Software Versions** | MEDIUM | Fingerprint for exploits |
| **Timestamps** | MEDIUM | Activity patterns |
| **Serial Numbers** | MEDIUM | Device tracking |
| **Network Paths** | HIGH | \\server\share\document.docx |
| **Template Names** | LOW | Organizational structure |
| **Editing Time** | LOW | Work patterns |

### 7.2 Metadata Sanitization Guide

**Before Publishing Documents:**
```bash
# PDF sanitization
qpdf --linearize input.pdf output.pdf
exiftool -all:all= -tagsfromfile @ -all:all output.pdf

# Image sanitization
exiftool -all= -overwrite_original image.jpg

# Office document sanitization
# Save as PDF or use Document Inspector (Office built-in)
```

### 7.3 Privacy Protection Checklist

Before sharing files publicly:
- [ ] Remove GPS coordinates from photos
- [ ] Strip author/creator information
- [ ] Remove revision history
- [ ] Clear comments and annotations
- [ ] Remove hidden content and layers
- [ ] Delete custom properties
- [ ] Check for embedded files
- [ ] Remove printer paths and network info

---

## 8. Confidence Ratings

| Field Type | Confidence | Rationale |
|------------|-----------|-----------|
| EXIF GPS coordinates | HIGH | Hardware-generated, hard to fake accurately |
| PDF ModDate | HIGH | Standard field, widely supported |
| Office dc:creator | MEDIUM | User-configurable, may be fake |
| Archive timestamps | MEDIUM | System-dependent, easily modified |
| Camera serial number | HIGH | Burned into hardware |
| Software version | MEDIUM | Can be spoofed |
| Document revision count | HIGH | Automatic counter |

---

## 9. Limitations

### Technical Limitations

1. **Stripped metadata cannot be recovered**
2. **Encrypted files require decryption first**
3. **Corrupted files may give partial results**
4. **Custom/proprietary fields may be undocumented**
5. **Timezone handling varies by format**

### Interpretation Limitations

1. **Dates can be manually set to any value**
2. **Author fields accept any string**
3. **Software can spoof other software's signatures**
4. **GPS coordinates may be imprecise or wrong**
5. **Timezones may not reflect actual location**

---

*Metadata Extraction Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*

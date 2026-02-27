# Document Intelligence Module

> **Module ID:** DOC-INTEL-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Forensics Guide

---

## 1. Document Overview

This module provides forensic analysis guidance for document metadata, hidden content, and provenance verification. All analysis requires user-provided document data.

### 1.1 Supported Formats

| Format | Extensions | Metadata Level | Hidden Content Risk |
|--------|------------|----------------|---------------------|
| PDF | `.pdf` | High | Medium |
| Microsoft Word | `.docx`, `.doc` | High | High |
| Microsoft Excel | `.xlsx`, `.xls` | High | High |
| Microsoft PowerPoint | `.pptx`, `.ppt` | High | Medium |
| OpenDocument | `.odt`, `.ods`, `.odp` | High | Medium |
| Rich Text | `.rtf` | Medium | Low |
| Images | `.jpg`, `.png`, `.tiff` | Very High | Low |
| Archives | `.zip`, `.rar`, `.7z` | Medium | Variable |

---

## 2. EXIF Data Extraction Guide

### 2.1 What is EXIF?

Exchangeable Image File Format (EXIF) stores metadata in image files including camera settings, timestamps, and GPS coordinates.

### 2.2 EXIF Field Categories

#### Camera Information
```
Make                    - Camera manufacturer
Model                   - Camera model
Software                - Firmware/software version
LensModel               - Lens used
LensSpecification       - Focal length range
```

#### Image Properties
```
ImageWidth/ImageHeight  - Dimensions in pixels
Orientation             - Rotation/orientation
XResolution/YResolution - DPI settings
ColorSpace              - Color profile
```

#### Capture Settings
```
ExposureTime            - Shutter speed
FNumber                 - Aperture
ISOSpeedRatings         - ISO sensitivity
FocalLength             - Focal length in mm
Flash                   - Flash status
WhiteBalance            - White balance setting
MeteringMode            - Exposure metering method
```

#### Date/Time Fields
```
DateTimeOriginal        - When photo was taken
DateTimeDigitized       - When digitized
DateTime                - File modification date
SubSecTimeOriginal      - Fractional seconds
```

#### GPS Information (Critical)
```
GPSLatitude/GPSLatitudeRef      - Latitude coordinate
GPSLongitude/GPSLongitudeRef    - Longitude coordinate
GPSAltitude/GPSAltitudeRef      - Elevation
GPSTimeStamp                    - GPS time (atomic)
GPSDateStamp                    - GPS date
GPSImgDirection                 - Camera pointing direction
GPSDestLatitude/GPSDestLongitude - Destination coordinates
```

### 2.3 Extraction Procedures

#### Manual Extraction Steps

**On macOS:**
1. Right-click image → Get Info
2. Expand "More Info" section
3. View basic EXIF data
4. For GPS: Use Preview → Tools → Show Inspector

**On Windows:**
1. Right-click image → Properties
2. Select "Details" tab
3. Scroll through EXIF fields
4. GPS data under "GPS" section

**Command Line (ExifTool):**
```bash
# Extract all metadata
exiftool image.jpg

# Extract specific fields
exiftool -GPSLatitude -GPSLongitude image.jpg

# Export to CSV
exiftool -csv image.jpg > metadata.csv

# Extract from all images in directory
exiftool -r -csv /path/to/folder/ > all_metadata.csv
```

### 2.4 EXIF Analysis Checklist

- [ ] **GPS Coordinates Present** → Map location, verify consistency
- [ ] **Timestamp Analysis** → Check timezone, sequence with other photos
- [ ] **Camera Consistency** → Same device across photo set?
- [ ] **Software Sign** → Edited? (Photoshop, etc.)
- [ ] **Missing EXIF** → Stripped intentionally or screenshot?
- [ ] **Timestamp vs GPS Time** → Mismatch indicates manipulation

---

## 3. PDF Metadata Analysis

### 3.1 PDF Document Structure

```
PDF Header        - Version declaration
Body              - Objects (content, fonts, images)
Cross-Reference   - Object offsets
Trailer           - Root object, metadata
```

### 3.2 Metadata Fields

#### Document Properties
```
Title           - Document title
Author          - Creator name
Subject         - Document subject
Keywords        - Search keywords
Creator         - Software that created PDF
Producer        - PDF conversion software
CreationDate    - Original creation (D:YYYYMMDDHHmmSS)
ModDate         - Last modification
```

#### Extended Metadata (XMP)
```
xmp:CreatorTool         - Creation application
xmp:CreateDate          - ISO 8601 timestamp
xmp:ModifyDate          - Modification timestamp
xmp:MetadataDate        - Metadata modification
dc:creator              - Dublin Core author
dc:title                - Dublin Core title
dc:description          - Document description
pdf:Producer            - PDF producer
dc:format               - MIME type
```

### 3.3 Extraction Methods

#### Using PDF Metadata Tools
```bash
# Using pdfinfo (Poppler)
pdfinfo document.pdf

# Using exiftool
exiftool -a document.pdf

# Extract XMP metadata
exiftool -b -XMP document.pdf > xmp.xml

# Check for JavaScript
pdfinfo -js document.pdf
```

#### Manual Extraction
```bash
# View raw PDF structure
strings document.pdf | grep -E "(Author|Creator|Producer|Title)"

# Extract text streams
pdftotext -layout document.pdf -

# Check for embedded files
pdfdetach -list document.pdf
```

### 3.4 PDF Analysis Indicators

| Red Flag | Interpretation |
|----------|----------------|
| Recent ModDate, old CreationDate | Document was edited |
| Creator ≠ Producer | Converted from another format |
| Missing Author field | Anonymized or template |
| Embedded JavaScript | Potential security risk |
| Multiple producers | Document lifecycle complex |
| XMP metadata stripped | Intentional sanitization |

---

## 4. Office Document Forensics

### 4.1 Microsoft Office Structure

Modern Office files (`.docx`, `.xlsx`, `.pptx`) are ZIP archives containing XML:

```
document.docx
├── [Content_Types].xml
├── _rels/
├── docProps/
│   ├── app.xml          - Application properties
│   ├── core.xml         - Dublin Core metadata
│   └── custom.xml       - Custom properties
├── word/                - (or xl/, ppt/)
│   ├── document.xml     - Main content
│   ├── styles.xml       - Formatting
│   ├── settings.xml     - Document settings
│   └── media/           - Embedded images
└── _rels/
```

### 4.2 Core Metadata Fields

**docProps/core.xml:**
```xml
<dc:title>Document Title</dc:title>
<dc:subject>Subject</dc:subject>
<dc:creator>Author Name</dc:creator>
<cp:lastModifiedBy>Editor Name</cp:lastModifiedBy>
<dcterms:created>2024-01-15T10:30:00Z</dcterms:created>
<dcterms:modified>2024-01-20T14:45:00Z</dcterms:modified>
<cp:revision>5</cp:revision>
```

**docProps/app.xml:**
```xml
<Template>Template Name</Template>
<TotalTime>Editing time in minutes</TotalTime>
<Pages>Page count</Pages>
<Words>Word count</Words>
<Application>Microsoft Office Word</Application>
<DocSecurity>0 (none) to 8 (password)</DocSecurity>
```

### 4.3 Extraction Procedure

```bash
# Method 1: Unzip and inspect
unzip document.docx -d docx_extracted/
cat docx_extracted/docProps/core.xml

# Method 2: Using exiftool
exiftool document.docx

# Method 3: Using oletools (Python)
pip install oletools
olemeta document.docx

# Check for macros (security risk)
olevba document.docx
```

### 4.4 Excel-Specific Analysis

**Hidden Data Risks:**
- Hidden rows/columns
- Hidden sheets
- External links
- Named ranges
- Cached data

**Extraction:**
```bash
# Extract all sheets as CSV
libreoffice --headless --convert-to csv:"Text - txt - csv (StarCalc)":44,34,76 document.xlsx

# Check for external connections
unzip -p document.xlsx xl/connections.xml
```

---

## 5. Hidden Content Detection

### 5.1 Types of Hidden Content

#### Layer-Based Hiding
```
White text on white background
Hidden behind images
Off-page content (negative coordinates)
Overlapping objects
```

#### Metadata Hiding
```
Comments and annotations
Track changes (revisions)
Document variables
Custom XML
Previous versions
```

#### Technical Hiding
```
Steganography in images
Embedded files
Alternate data streams (ADS) - Windows
EXIF data in images
Zlib compressed streams
```

### 5.2 Detection Procedures

#### PDF Hidden Content
```bash
# Extract all images
pdfimages -all document.pdf output_prefix

# Extract embedded files
pdfdetach -saveall document.pdf

# Check for hidden text
pdftotext -layout document.pdf - | grep -v "^$"

# Analyze JavaScript
qpdf --qdf document.pdf unpacked.pdf
strings unpacked.pdf | grep -i "javascript\|js"
```

#### Office Hidden Content
```bash
# Check for revision tracking
unzip -p document.docx word/revisions.xml

# Check comments
unzip -p document.docx word/comments.xml

# Check for hidden text
unzip -p document.docx word/document.xml | grep -o 'w:vanish="1"'

# Check for embedded objects
unzip -l document.docx | grep -E "(embeddings|oleObject)"
```

### 5.3 Redaction Failures

**Common Redaction Mistakes:**
1. **Black boxes** - Content still in PDF, just covered
2. **White highlighting** - Text selectable and copyable
3. **Comment "redaction"** - Just comments, not actual removal
4. **Image overlay** - Original image data still present

**Verification:**
```bash
# Copy all text from PDF
pdftotext document.pdf -

# Check if "redacted" text appears
# If yes → Redaction failed
```

---

## 6. Document Comparison Methodology

### 6.1 Text Comparison

```bash
# Basic diff
diff <(pdftotext doc1.pdf -) <(pdftotext doc2.pdf -)

# Word-level diff
dwdiff doc1.txt doc2.txt

# Visual comparison (ImageMagick)
convert -density 150 doc1.pdf[0] doc1.png
convert -density 150 doc2.pdf[0] doc2.png
compare doc1.png doc2.png diff.png
```

### 6.2 Metadata Comparison Framework

| Field | Doc 1 | Doc 2 | Significance |
|-------|-------|-------|--------------|
| Author | John Doe | Jane Smith | Different creators |
| CreationDate | 2024-01-01 | 2024-06-01 | 5-month gap |
| Producer | Word 2019 | Word 2021 | Different software |
| ModDate | Same as Creation | 2024-06-02 | Doc 2 was edited |

### 6.3 Structural Comparison

```bash
# Compare internal structure
diff -r <(unzip -l doc1.docx) <(unzip -l doc2.docx)

# Compare XML content
for xml in docx1/word/*.xml; do
    diff "$xml" "docx2/word/$(basename $xml)"
done
```

---

## 7. Redaction Verification

### 7.1 Redaction Check Protocol

**Command:** `/redaction-check [pdf]`

**Procedure:**
```
Step 1: Open PDF in text editor (text extraction test)
Step 2: Select all text (Ctrl+A) and copy to text editor
Step 3: Search for keywords that should be redacted
Step 4: Check for selectable black boxes
Step 5: Examine file size (redaction should reduce size)
Step 6: Check for multiple PDF layers
```

### 7.2 Redaction Quality Levels

| Level | Method | Secure | Verification |
|-------|--------|--------|--------------|
| Poor | Black boxes | NO | Copy-paste test fails |
| Basic | White highlighting | NO | Invisible but present |
| Good | Image replacement | MAYBE | Check for metadata |
| Proper | Content removal + flatten | YES | Text extraction clean |

### 7.3 Redaction Verification Commands

```bash
# Full text extraction test
pdftotext redacted.pdf - | strings

# Check for hidden objects
qpdf --check redacted.pdf

# Check for multiple content streams
pdf-parser.py -a redacted.pdf | grep -c "stream"

# Examine structure for hidden content
mutool show redacted.pdf | less
```

---

## 8. Format-Specific Guides

### 8.1 Image Formats Deep Dive

**JPEG/JPG:**
- EXIF is standard
- Thumbnail may differ from main image
- Multiple APP segments possible
- Comment segments (COM) rare but possible

**PNG:**
- Textual chunks (tEXt, iTXt, zTXt) for metadata
- No EXIF by default (unless converted)
- Creation time in tIME chunk
- Physical dimensions in pHYs

**TIFF:**
- Extensive EXIF support
- Multiple IFD (Image File Directory) entries
- GPSInfo IFD for location data
- Often used in professional/archival contexts

**RAW Formats (CR2, NEF, ARW):**
- Manufacturer-specific metadata
- Thumbnail embedded
- Shooting information extensive
- Proprietary makernotes

### 8.2 Archive Analysis

```bash
# List contents with details
unzip -l archive.zip
7z l archive.7z

# Check for hidden files
unzip -l archive.zip | grep "^\.+"

# Extract metadata without extracting
zipinfo -v archive.zip

# Check for encrypted entries
unzip -t archive.zip  # Will prompt if encrypted
```

---

## 9. Confidence Ratings

| Finding | Confidence | Requirements |
|---------|-----------|--------------|
| GPS coordinates present | HIGH | Raw EXIF data provided |
| Document edited | HIGH | ModDate ≠ CreationDate |
| Author identity | MEDIUM | Metadata consistent with other sources |
| Software used | HIGH | Producer field present |
| Redaction complete | MEDIUM | Multiple verification tests passed |
| Hidden content found | HIGH | Visual inspection confirmed |

---

## 10. Limitations & Disclaimers

### What This Module Cannot Do

- Decrypt password-protected documents
- Recover deleted/overwritten metadata
- Bypass DRM or copy protection
- Extract data from corrupted files
- Verify document authenticity cryptographically

### Educational Purpose

This module is for:
- Understanding document metadata
- Learning forensic methodology
- Identifying sanitization failures
- Educational security awareness

**Not for:**
- Bypassing security controls
- Circumventing privacy protections
- Unauthorized document analysis
- Legal evidence extraction (requires certified tools)

---

## 11. Command Reference

### /analyze-doc [file]
**Purpose:** Guide document metadata extraction
**Input:** Document file path or metadata dump
**Output:** Structured analysis report

### /compare-docs [file1] [file2]
**Purpose:** Identify differences between documents
**Input:** Two document files
**Output:** Comparison report with changes highlighted

### /redaction-check [pdf]
**Purpose:** Verify PDF redaction completeness
**Input:** PDF file
**Output:** Redaction quality assessment

---

## 12. Quick Reference Card

```
EXIF PRIORITY FIELDS:
1. GPSLatitude/GPSLongitude (location)
2. DateTimeOriginal (timestamp)
3. Make/Model (device fingerprint)
4. Software (editing detection)

PDF PRIORITY FIELDS:
1. Author/Creator (attribution)
2. CreationDate/ModDate (timeline)
3. Producer (software chain)
4. Embedded files (hidden content)

OFFICE PRIORITY FIELDS:
1. dc:creator (author)
2. lastModifiedBy (editor)
3. Template (provenance)
4. revision (edit history)

HIDDEN CONTENT INDICATORS:
- File size larger than expected
- Selectable "redacted" areas
- Comments/revisions present
- Multiple content layers
- Embedded objects
```

---

*Document Intelligence Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*  
*For educational and authorized analysis purposes only*

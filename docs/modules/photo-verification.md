# Photo Verification Module

> **Module ID:** PHOTO-VERIFY-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Image Forensics

---

## 1. Overview

Photo verification determines the authenticity, location, and time of images through forensic analysis. This module covers shadow analysis, sun position, weather correlation, and provenance verification.

---

## 2. Shadow Analysis Guide

### 2.1 Shadow Direction Principles

**Sun Position Determines Shadow Direction:**
```
Morning (East)    → Shadows point West
Noon (South)*     → Shadows point North
Evening (West)    → Shadows point East

*Northern Hemisphere; reverse for Southern
```

### 2.2 Shadow Length Analysis

**Factors Affecting Shadow Length:**
- Sun elevation angle (higher = shorter shadows)
- Time of day (noon shortest)
- Season (summer shorter at noon)
- Latitude (polar regions extreme variation)

**Shadow Ratio Formula:**
```
Shadow Ratio = Shadow Length / Object Height

Example:
  Building height: 100m
  Shadow length: 150m
  Ratio: 1.5
  
  Corresponds to sun elevation: ~34°
  (atan(1/1.5) = 33.69°)
```

### 2.3 Shadow Analysis Procedure

**Step 1: Identify Reference Objects**
```
- People (average height reference)
- Buildings with known dimensions
- Vehicles (standard sizes)
- Street furniture (poles, signs)
```

**Step 2: Measure Shadow Angles**
```
- Use image editing software
- Draw lines along shadow edges
- Measure angle from north (compass direction)
- Note: Photo orientation may need correction
```

**Step 3: Calculate Sun Position**
```
Shadow azimuth = Object azimuth + 180°
(or Object azimuth - 180°)

Example:
  Shadow points 45° (NE)
  Sun is at 225° (SW)
```

**Step 4: Verify Consistency**
```
- Compare calculated sun position to claimed time
- Check if sun position is possible at that location/date
- Verify all shadows in image point consistently
```

### 2.4 Shadow Inconsistency Detection

**Red Flags:**
- Shadows pointing in multiple directions (multiple light sources)
- Shadow lengths inconsistent with sun angle
- Shadows "climbing" vertical surfaces incorrectly
- Contact shadows missing or wrong
- Reflections inconsistent with light source

---

## 3. Sun Position Calculation

### 3.1 Required Parameters

```
Latitude    - Location latitude (-90° to +90°)
Longitude   - Location longitude (-180° to +180°)
Date        - Day of year (affects declination)
Time        - Local apparent time
Timezone    - Offset from UTC
```

### 3.2 Solar Position Calculations

**Approximate Sun Elevation:**
```
Solar Noon Elevation = 90° - |Latitude - Solar Declination|

Solar Declination ≈ 23.45° × sin(360° × (284 + DayOfYear) / 365)

Example:
  Location: 40°N
  Date: June 21 (Day 172)
  Declination: ~23.45°
  Noon elevation: 90 - |40 - 23.45| = 73.45°
```

### 3.3 Using SunCalc Tools

**Online Tools:**
- suncalc.org - Interactive sun position
- sunearthtools.com - Solar calculator
- NOAA Solar Calculator - Official US data

**API Access:**
```bash
# Using suncalc npm package (conceptual)
# Install: npm install suncalc

const SunCalc = require('suncalc');
const times = SunCalc.getTimes(new Date(), lat, lon);
const position = SunCalc.getPosition(new Date(), lat, lon);

// position.azimuth - sun direction
// position.altitude - sun elevation
```

### 3.4 Verification Workflow

```
1. Extract GPS from EXIF → Location
2. Extract DateTimeOriginal → Timestamp
3. Calculate sun position at that time/location
4. Compare to shadow angles in photo
5. Evaluate consistency

Consistency Check:
  If shadows indicate sun at 135° (SE)
  And calculation shows sun at 45° (NE)
  Then: Photo timestamp or location is wrong
```

---

## 4. Weather Data Correlation

### 4.1 Weather Verification Sources

| Source | Data Available | Coverage |
|--------|---------------|----------|
| Weather Underground | Historical hourly | Global |
| NOAA/NWS | Official records | US primarily |
| Meteostat | API access | Global |
| OpenWeatherMap | Historical | Global |
| Local meteorological services | Varies | Regional |

### 4.2 Weather Elements to Verify

**Sky Conditions:**
```
Photo shows: Clear blue sky
Weather record: Overcast, 90% cloud cover
→ Inconsistency detected
```

**Precipitation:**
```
Photo shows: Dry surfaces, no rain
Weather record: Heavy rain at that time
→ Investigate further (sheltered location? wrong time?)
```

**Lighting Conditions:**
```
Photo shows: Bright sunlight
Weather record: Heavy fog
→ Possible manipulation
```

**Seasonal Indicators:**
```
Photo shows: Snow on ground
Weather record: Temperature 25°C (77°F)
→ Clear inconsistency
```

### 4.3 Weather Verification Procedure

**Step 1: Extract Location and Time**
```
From EXIF or context:
  GPS coordinates or city name
  Date and time (adjusted for timezone)
```

**Step 2: Query Historical Weather**
```bash
# Example using Weather Underground API (conceptual)
curl "https://api.weather.com/v1/location/${LAT},${LON}/observations/historical.json?apiKey=${KEY}&date=${YYYYMMDD}"
```

**Step 3: Compare Conditions**
```
Weather Data:
  Time: 14:00
  Condition: Partly cloudy
  Visibility: 10km
  
Photo Analysis:
  Shadows: Strong, defined
  Sky: Clear with few clouds
  Visibility: Excellent
  
Conclusion: Consistent ✓
```

---

## 5. Reverse Image Search Guidance

### 5.1 Search Engine Capabilities

| Engine | Strengths | Best For |
|--------|-----------|----------|
| Google Images | Largest index | General search |
| TinEye | Long history | Finding oldest version |
| Yandex | Face/object recognition | Non-Western content |
| Bing Visual Search | Microsoft ecosystem | Alternative results |
| Baidu | Chinese web | China-specific content |

### 5.2 Search Strategies

**Full Image Search:**
```
1. Upload original image
2. Review exact matches (same image)
3. Review similar matches (modified versions)
4. Note earliest publication date
5. Check context on source pages
```

**Cropped Region Search:**
```
When to crop:
- Background landmarks (identify location)
- Faces (find other appearances)
- Distinctive objects (find similar)
- Logos/signs (identify organization)

Crop regions:
- Top third (sky/background)
- Bottom third (ground/foreground)
- Left/right thirds (context)
- Center (main subject)
```

**Modified Image Search:**
```
Try variations:
- Flipped horizontally
- Grayscale
- Adjusted brightness/contrast
- Different crops
```

### 5.3 Interpreting Results

**Exact Match Found:**
```
→ Image exists elsewhere on web
→ Check publication date (earliest = closest to source)
→ Analyze context on source page
→ May indicate:
   - Reposted content
   - Stock image
   - Misattributed photo
```

**No Matches Found:**
```
→ Possibly original content
→ May be from private/dark web sources
→ Could be very recent
→ Not definitive proof of authenticity
```

**Multiple Versions Found:**
```
→ Track versions chronologically
→ Note modifications between versions
→ Identify likely original
→ Build provenance timeline
```

---

## 6. Provenance Checking

### 6.1 Provenance Verification Chain

```
Original Source → First Publication → Reposts → Current Context
      ↑                                              ↓
   Verify authenticity                      Verify consistency
```

### 6.2 Source Verification Checklist

**Original Photographer/Source:**
- [ ] Identify original uploader
- [ ] Check account history
- [ ] Verify account authenticity
- [ ] Review other content from same source
- [ ] Check for watermark/copyright

**Publication Context:**
- [ ] Original caption/description
- [ ] Associated text/article
- [ ] Publication date vs claimed date
- [ ] Publisher credibility
- [ ] Related images in series

**Technical Verification:**
- [ ] EXIF data present and consistent
- [ ] No signs of editing software
- [ ] Resolution appropriate for claimed source
- [ ] File format appropriate

### 6.3 Provenance Red Flags

| Indicator | Concern Level | Investigation |
|-----------|--------------|---------------|
| No EXIF data | MEDIUM | Screenshot or stripped? |
| Multiple "originals" | HIGH | Stolen/laundered content |
| Stock photo watermarks | HIGH | Misrepresented as real |
| Compression artifacts | LOW | Normal for web images |
| Inconsistent shadows | CRITICAL | Composite/manipulated |
| Reverse search shows older version | HIGH | Not original |
| Different crop but same content | MEDIUM | Check which is original |

---

## 7. EXIF Analysis for Verification

### 7.1 Verification Fields

| Field | Verification Use | Red Flag |
|-------|-----------------|----------|
| DateTimeOriginal | Establish timeline | Future date, mismatches context |
| GPS coordinates | Confirm location | Location contradicts claimed place |
| Make/Model | Verify capture device | Professional camera for "phone photo" |
| Software | Detect editing | Photoshop, GIMP in field |
| ImageSize | Assess quality | Inconsistent with claimed source |

### 7.2 EXIF Consistency Checks

**Timestamp vs GPS Time:**
```
DateTimeOriginal: 2024:01:15 14:30:00
OffsetTime: +05:00
→ UTC: 09:30:00

GPSTimeStamp: 09, 30, 00
GPSDateStamp: 2024:01:15
→ UTC: 09:30:00 ✓ Consistent
```

**GPS Location Verification:**
```
GPS Coordinates: 48.8566°N, 2.3522°E
Claimed location: Paris, France ✓

GPS Coordinates: 48.8566°N, 2.3522°E
Claimed location: London, UK ✗ Mismatch
```

### 7.3 Missing EXIF Analysis

**Common Reasons for Missing EXIF:**
1. **Screenshots** - No camera EXIF
2. **Social media download** - Stripped by platform
3. **Intentional stripping** - Privacy protection
4. **Image editing** - Some software strips EXIF
5. **Format conversion** - PNG typically has no EXIF
6. **Very old camera** - Pre-EXIF standard

**Interpretation:**
```
Screenshot detected:
  - No camera EXIF
  - Creation software shows "Screenshot"
  - Dimensions match common screen sizes
  → Image is secondary capture, not original
```

---

## 8. Cropping Strategies for Searches

### 8.1 Strategic Crop Regions

**Location Identification:**
```
Crop: Background elements
  - Landmarks
  - Signage
  - Architecture
  - Natural features
  - License plates (for region)
```

**Subject Identification:**
```
Crop: Distinctive features
  - Faces (if ethical/legal)
  - Tattoos/markings
  - Clothing logos
  - Vehicle details
  - Unique objects
```

**Context Gathering:**
```
Crop: Environmental clues
  - Language on signs
  - Vehicle types/styles
  - Vegetation types
  - Architectural style
  - Weather conditions
```

### 8.2 Crop Size Guidelines

| Purpose | Minimum Size | Recommended |
|---------|-------------|-------------|
| Face search | 100×100px | 250×250px |
| Object search | 150×150px | 300×300px |
| Landmark search | 200×200px | Full height/width |
| Logo/text | 50×50px | 100×100px |

### 8.3 Multi-Crop Search Strategy

```
Original Image
     ↓
┌────┴────┐
↓         ↓
Crop 1   Crop 2   Crop 3   Crop 4
(Full)   (Left)   (Right)  (Detail)
   ↓        ↓        ↓        ↓
 Search   Search   Search   Search
   ↓        ↓        ↓        ↓
Results  Results  Results  Results
   └────────┴────────┴────────┘
            ↓
     Compile findings
```

---

## 9. Confidence Ratings

| Verification Method | Confidence | Notes |
|--------------------|-----------|-------|
| Shadow angle match | HIGH | Mathematical verification |
| Sun position match | HIGH | Astronomical calculation |
| Weather correlation | MEDIUM | Historical data accuracy varies |
| Reverse image match | HIGH | Exact match definitive |
| EXIF GPS match | HIGH | Hardware-generated |
| Provenance chain | MEDIUM | Depends on source reliability |
| Visual consistency | MEDIUM | Subjective assessment |

---

## 10. Limitations

### Technical Limitations

1. **Sun calculations are approximate** - Atmospheric refraction affects actual position
2. **Weather data has gaps** - Not all locations have historical records
3. **Reverse search is incomplete** - No engine indexes entire web
4. **EXIF can be forged** - Tools exist to edit metadata
5. **Shadow analysis requires reference objects** - Need known dimensions

### Investigative Limitations

1. **Provenance may be unknowable** - Original source untraceable
2. **Edited images can be undetectable** - Expert manipulation
3. **Context may be missing** - Image cropped from larger scene
4. **Time zones complicate verification** - Multiple interpretations possible
5. **Private sources inaccessible** - Not all content is searchable

---

*Photo Verification Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*

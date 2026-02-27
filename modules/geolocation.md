# Geolocation Intelligence Module

> **Module ID:** GEO-INTEL-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Location Analysis

---

## 1. Overview

Geolocation intelligence extracts and analyzes location data from digital artifacts. This module covers GPS coordinate extraction, timezone analysis, location verification, and travel pattern detection.

---

## 2. GPS Coordinate Extraction from EXIF

### 2.1 GPS Coordinate Format

**EXIF stores GPS as rational numbers (fractions):**
```
GPSLatitude:  [34, 3, 222987/10000]  → 34° 3' 22.2987"
GPSLongitude: [118, 15, 352789/10000] → 118° 15' 35.2789"
```

**Conversion Formula:**
```
Decimal Degrees = Degrees + (Minutes/60) + (Seconds/3600)

Example:
  34 + (3/60) + (22.2987/3600) = 34.0561941° N
  -(118 + (15/60) + (35.2789/3600)) = -118.2597997° W
```

### 2.2 Extraction Procedure

**Step-by-step extraction:**
```bash
# Extract raw GPS data
exiftool -GPSLatitude -GPSLongitude -GPSAltitude -GPSDateStamp image.jpg

# Output format conversion
exiftool -c "%+.6f" -GPSLatitude -GPSLongitude image.jpg

# Bulk extraction with coordinates
exiftool -csv -GPSLatitude -GPSLongitude -GPSDateStamp -DateTimeOriginal /path/to/images/ > locations.csv
```

### 2.3 Coordinate Precision Analysis

| Source | Typical Precision | Use Case |
|--------|-------------------|----------|
| Smartphone GPS | ±3-5 meters | High-confidence location |
| Camera GPS | ±5-10 meters | Good location data |
| Network geolocation | ±100m-2km | Approximate area only |
| Manual entry | Variable | Unreliable |

---

## 3. Timezone Analysis

### 3.1 Timezone Indicators

**Sources of timezone information:**
- EXIF OffsetTime fields
- File system timestamps
- Email headers
- Log files
- Social media posts

### 3.2 Timezone Extraction from EXIF

```
OffsetTime:         +02:00  (camera timezone)
OffsetTimeOriginal: +02:00  (capture timezone)
DateTimeOriginal:   2024:01:15 14:30:00
GPSTimeStamp:       12, 30, 00  (UTC)
GPSDateStamp:       2024:01:15
```

**Inconsistency Detection:**
```
If OffsetTime = +02:00 and GPSTime = 12:30 UTC
Then LocalTime should be 14:30 ✓

If OffsetTime = +02:00 but LocalTime = 10:30
Then GPSTime should be 08:30 UTC → INCONSISTENCY
```

### 3.3 Timezone Consistency Checks

**Check 1: GPS vs Local Time**
```
Formula: GPS_Time + Offset = Local_Time

Example:
  GPS: 2024-01-15 12:30:00 UTC
  Local: 2024-01-15 14:30:00
  Expected Offset: +02:00
  EXIF OffsetTime: +02:00 ✓
```

**Check 2: DST Transition Detection**
```
If timezone offset changes between photos
  And location is constant
  Then DST transition occurred

Verification:
  Summer: OffsetTime = +02:00 (CEST)
  Winter: OffsetTime = +01:00 (CET)
  Same location confirmed by GPS
```

**Check 3: Impossible Timezones**
```
GPS coordinates in New York
But OffsetTime = +09:00 (Tokyo)
→ Possible manipulation or travel
```

---

## 4. Location Verification Methods

### 4.1 Verification Checklist

**Coordinate Validation:**
- [ ] Latitude between -90° and +90°
- [ ] Longitude between -180° and +180°
- [ ] Altitude reasonable for location
- [ ] Coordinates on land (if claimed)

**Logical Consistency:**
- [ ] Timestamp matches timezone offset
- [ ] GPS time matches capture time
- [ ] No impossible travel between photos
- [ ] Weather matches location/date

**Cross-Reference:**
- [ ] Satellite imagery matches
- [ ] Street view confirms features
- [ ] Landmarks identifiable
- [ ] Shadow angles match sun position

### 4.2 Reverse Geocoding

**Convert coordinates to address:**
```bash
# Using curl and OpenStreetMap
curl "https://nominatim.openstreetmap.org/reverse?format=json&lat=34.0561&lon=-118.2598&zoom=18&addressdetails=1"

# Response includes:
# - Street address
# - City, State, Country
# - Postcode
# - Neighbourhood
```

**Privacy Note:** Use offline databases or rate-limited APIs to avoid exposing investigation targets.

### 4.3 Coordinate Verification Tools

| Tool | Purpose | URL |
|------|---------|-----|
| Google Maps | Visual verification | maps.google.com |
| OpenStreetMap | Open data verification | openstreetmap.org |
| GeoNames | Location database | geonames.org |
| SunCalc | Sun position validation | suncalc.org |
| Sentinel Hub | Satellite imagery | sentinel-hub.com |

---

## 5. Travel Pattern Analysis

### 5.1 Timeline Construction

**Required Data:**
```
Timestamp (ISO 8601)    Location (Lat, Lon)     Source
2024-01-15T08:00:00Z    40.7128, -74.0060       Photo EXIF
2024-01-15T14:30:00Z    51.5074, -0.1278        Social media
2024-01-16T09:00:00Z    48.8566, 2.3522         Email header
```

### 5.2 Speed Calculations

**Distance Formula (Haversine):**
```python
import math

def haversine(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km
    
    lat1, lon1, lat2, lon2 = map(math.radians, [lat1, lon1, lat2, lon2])
    dlat = lat2 - lat1
    dlon = lon2 - lon1
    
    a = math.sin(dlat/2)**2 + math.cos(lat1) * math.cos(lat2) * math.sin(dlon/2)**2
    c = 2 * math.asin(math.sqrt(a))
    
    return R * c

# Example: NYC to London
distance = haversine(40.7128, -74.0060, 51.5074, -0.1278)
# Result: ~5570 km
```

**Speed Calculation:**
```
Speed = Distance / Time

NYC to London:
  Distance: 5570 km
  Time window: 6.5 hours (08:00 to 14:30)
  Required speed: 5570/6.5 = 857 km/h
  
Concorde max speed: ~2170 km/h ✓ Possible
Commercial jet: ~900 km/h ✓ Possible
Commercial flight time: ~7 hours ✓ Consistent
```

### 5.3 Impossible Travel Detection

**Maximum Plausible Speeds:**

| Transport | Max Speed | Detection Threshold |
|-----------|-----------|---------------------|
| Walking | 6 km/h | >10 km/h suspicious |
| Running | 25 km/h | >30 km/h suspicious |
| Car (highway) | 150 km/h | >200 km/h suspicious |
| Train | 350 km/h | >400 km/h suspicious |
| Commercial flight | 950 km/h | >1100 km/h suspicious |
| Supersonic | 2200 km/h | >2500 km/h suspicious |

**Detection Algorithm:**
```
For each consecutive location pair:
  Calculate distance (haversine)
  Calculate time difference
  Calculate speed
  
  If speed > threshold:
    Flag for review
    Check for:
      - Timezone errors
      - Timestamp errors
      - Multiple subjects
      - Spoofed data
```

---

## 6. Landmark Identification

### 6.1 Landmark Recognition Process

**Step 1: Visual Analysis**
```
- Identify distinctive structures
- Note architectural style
- Check for signage (text analysis)
- Observe natural features
```

**Step 2: Image Search**
```
- Reverse image search
- Crop distinctive features
- Search with descriptive terms
```

**Step 3: Coordinate Verification**
```
- Get landmark coordinates
- Compare with EXIF coordinates
- Verify distance is reasonable
```

### 6.2 Landmark Database Resources

| Resource | Type | Coverage |
|----------|------|----------|
| Wikidata | Structured data | Global |
| Wikipedia | Articles | Major landmarks |
| Google Places | API | Commercial |
| OpenStreetMap | Open data | Comprehensive |
| Landmark Recognition APIs | ML-based | Varies |

---

## 7. Geographic Correlation Techniques

### 7.1 Multi-Source Correlation

**Correlate data from:**
- Photo EXIF GPS
- Social media check-ins
- Email headers (IP geolocation)
- Cell tower data (if available)
- Transaction records

**Correlation Matrix:**
```
              Photo1   Photo2   Tweet    Email
Photo1         --      5km      100m     50km
Photo2         5km     --       5.1km    52km
Tweet          100m    5.1km    --       49km
Email          50km    52km     49km     --

Analysis: Photo1 and Tweet from same location
          Email from nearby city
          Consistent travel pattern
```

### 7.2 Clustering Analysis

**Density-Based Clustering:**
```
Group locations by proximity:
  - Cluster radius: 100 meters
  - Minimum points: 3
  
Result: Identify frequent locations
  - Home location (most points)
  - Workplace (business hours)
  - Regular routes
```

### 7.3 Temporal-Spatial Patterns

**Heatmap Generation:**
```
X-axis: Time of day
Y-axis: Day of week
Color: Location (home, work, travel)

Patterns to identify:
  - Commute routes
  - Weekend vs weekday locations
  - Travel frequency
  - Seasonal patterns
```

---

## 8. Geolocation Privacy Guide

### 8.1 Location Data Exposure Risks

| Data Type | Risk | Mitigation |
|-----------|------|------------|
| EXIF GPS | CRITICAL | Strip before sharing |
| Social check-ins | HIGH | Disable location services |
| IP geolocation | MEDIUM | Use VPN |
| Cell tower data | HIGH | Limited user control |
| WiFi network names | MEDIUM | Avoid personal SSIDs |

### 8.2 Removing GPS Data

```bash
# Strip all EXIF
exiftool -gps:all= -overwrite_original image.jpg

# Strip selectively (keep other metadata)
exiftool -GPSLatitude= -GPSLongitude= -GPSAltitude= image.jpg

# Verify removal
exiftool -gps:all image.jpg
# Should show "No GPS info"
```

---

## 9. Confidence Ratings

| Finding | Confidence | Factors |
|---------|-----------|---------|
| GPS coordinates present | HIGH | Direct hardware measurement |
| Location identification | MEDIUM | Landmark recognition accuracy |
| Travel pattern | HIGH | Multiple corroborating points |
| Impossible travel | HIGH | Mathematical certainty |
| Timezone consistency | HIGH | Algorithmic verification |
| Address from coordinates | MEDIUM | Geocoding accuracy varies |

---

## 10. Limitations

### Technical Limitations

1. **GPS can be spoofed** - Software-defined radio
2. **EXIF can be edited** - Tools widely available
3. **Indoor accuracy poor** - GPS signals weak
4. **Timezone database changes** - Historical accuracy varies
5. **Geocoding imprecision** - Rural areas less accurate

### Investigative Limitations

1. **Single point = limited context**
2. **Shared devices = attribution difficulty**
3. **VPN/proxy = IP geolocation unreliable**
4. **Private accounts = data access limited**
5. **Deleted content = recovery not guaranteed**

---

*Geolocation Intelligence Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*

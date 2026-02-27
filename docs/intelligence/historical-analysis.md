# Historical Analysis

Archive.org integration and historical data reconstruction techniques.

---

## 1. Wayback Machine Search Strategies

### Basic URL Search

**Direct URL Lookup:**
```
https://web.archive.org/web/*/https://target-url.com
```

**Specific Date Range:**
```
https://web.archive.org/web/20200101-20201231*/https://target-url.com
```

**Single Snapshot:**
```
https://web.archive.org/web/20200615000000*/https://target-url.com
```

### Google Dork for Archive Discovery

```
# Find archived versions of a site
site:web.archive.org target-domain.com

# Find specific content in archives
site:web.archive.org "target phrase" "target-domain.com"

# Find profile archives
site:web.archive.org/web/*/twitter.com/username
site:web.archive.org/web/*/instagram.com/username
```

### Search Parameters

| Parameter | Format | Example |
|-----------|--------|---------|
| Single date | YYYYMMDDhhmmss | 20240115120000 |
| Date range | YYYYMMDDhhmmss-YYYYMMDDhhmmss | 20200101-20201231 |
| Year only | YYYY | 2023 |
| Closest to date | YYYYMMDD* | 20240101* |

---

## 2. Historical Snapshot Retrieval

### Snapshot Extraction

**Available Snapshot Discovery:**
```
# Get all snapshots for a URL
https://web.archive.org/cdx/search/cdx?url=example.com&output=json

# Filter by date range
https://web.archive.org/cdx/search/cdx?url=example.com&from=202001&to=202012&output=json

# Filter by MIME type
https://web.archive.org/cdx/search/cdx?url=example.com&filter=mimetype:text/html&output=json
```

### Snapshot Types

| Type | URL Pattern | Use Case |
|------|-------------|----------|
| Original | `web/20200101/https://...` | As captured |
| Without JS | `web/20200101if_/https://...` | Without Wayback JS |
| Text only | `web/20200101id_/https://...` | Text extraction |
| Screenshot | N/A (separate service) | Visual archive |

### Social Media Archive Strategies

**Twitter/X:**
```
https://web.archive.org/web/*/https://twitter.com/username
https://web.archive.org/web/*/https://x.com/username

# Specific tweet (if archived)
https://web.archive.org/web/*/https://twitter.com/username/status/tweetid
```

**Instagram:**
```
https://web.archive.org/web/*/https://instagram.com/username
https://web.archive.org/web/*/https://instagram.com/p/postid
```

**LinkedIn:**
```
https://web.archive.org/web/*/https://linkedin.com/in/username
```

**Facebook:**
```
https://web.archive.org/web/*/https://facebook.com/username
```

**Reddit:**
```
https://web.archive.org/web/*/https://reddit.com/user/username
https://web.archive.org/web/*/https://reddit.com/r/subreddit/comments/postid
```

---

## 3. Content Change Detection

### Change Detection Methodology

**Step 1: Baseline Establishment**
```
1. Capture earliest available snapshot
2. Extract key elements:
   - Page text content
   - Profile information
   - Posted content
   - Images/media
   - Links
3. Store as baseline
```

**Step 2: Comparative Analysis**
```
FOR each_snapshot IN chronological_snapshots:
    current = extract_content(snapshot)
    
    # Text changes
    text_diff = diff(baseline.text, current.text)
    
    # Profile changes
    IF baseline.profile != current.profile:
        log_profile_change(timestamp, diff)
    
    # Content changes
    new_content = current.posts - baseline.posts
    deleted_content = baseline.posts - current.posts
    
    # Link changes
    new_links = current.links - baseline.links
    broken_links = baseline.links - current.links
    
    UPDATE baseline = current
```

**Step 3: Pattern Recognition**
```
# Detect mass deletions
IF deleted_content_count > 10 AND time_window < 24_hours:
    FLAG = "mass_deletion_event"

# Detect rebranding
IF profile_changes > 5 AND time_window < 7_days:
    FLAG = "rebranding_event"

# Detect compromise
IF content_style_changes > 50% AND impossible_location:
    FLAG = "possible_compromise"
```

### Change Types

| Change Type | Detection Method | Significance |
|-------------|------------------|--------------|
| Text edit | Character diff | Low |
| Content addition | New elements | Low |
| Content deletion | Missing elements | High |
| Profile update | Field comparison | Medium |
| Link changes | URL diff | Medium |
| Image changes | Visual diff | Medium |
| Complete removal | 404 in archive | Critical |

---

## 4. Domain History Tracking

### Domain Lifecycle Analysis

**Registration History:**
```
# WHOIS history (use web-based WHOIS)
site:whois.domaintools.com target-domain.com
site:whois.icann.org target-domain.com

# Registration changes
site:web.archive.org target-domain.com whois
```

**DNS History:**
```
# Historical DNS records
site:securitytrails.com/domain/target-domain.com/history/a
site:viewdns.info/iphistory/?domain=target-domain.com
```

**Subdomain Discovery:**
```
# Find subdomains in archives
site:web.archive.org "subdomain.target-domain.com"

# Common subdomains to check
www, mail, ftp, blog, shop, api, dev, staging, admin, portal
```

### Domain Age Assessment

```
IF domain_age < 30_days:
    risk_score += 20
    
IF domain_age < 90_days:
    risk_score += 10
    
IF domain_age > 5_years AND consistent_content:
    risk_score -= 15
```

---

## 5. Timeline Reconstruction from Archives

### Reconstruction Process

**Phase 1: Data Collection**
```
1. Query Wayback CDX API for all snapshots
2. Download representative snapshots (monthly/quarterly)
3. Extract content from each snapshot
4. Build timeline database
```

**Phase 2: Event Identification**
```
EVENT_TYPES = {
    "account_creation": "first_snapshot",
    "profile_updates": "profile_field_changes",
    "content_posts": "new_content_detected",
    "content_deletions": "content_missing_in_later",
    "platform_migrations": "url_changes",
    "suspension_events": "sudden_archive_gaps",
    "rebranding": "major_profile_changes"
}
```

**Phase 3: Timeline Assembly**
```
timeline = []

FOR snapshot IN sorted_snapshots:
    date = extract_date(snapshot)
    
    # Detect events
    events = detect_events(snapshot, previous_snapshot)
    
    FOR event IN events:
        timeline.append({
            "date": date,
            "type": event.type,
            "description": event.description,
            "evidence": event.evidence_url,
            "confidence": event.confidence
        })

RETURN chronological(timeline)
```

### Timeline Visualization

```
2019-01-15 [●] Account created (first snapshot)
2019-03-22 [+] First content posted
2019-06-10 [~] Profile updated
2020-01-20 [+] Major content campaign
2020-05-15 [-] Content deleted (3 posts)
2021-02-01 [~] Username changed
2021-08-30 [■] Account suspended (gap in archives)
2022-03-10 [●] New account created
```

---

## 6. Historical Analysis Commands

### Command: `/history [url]`

```
SYNTAX: /history <url> [--from date] [--to date] [--type content_type]

OUTPUT:
📜 Historical Snapshots for: example.com
═══════════════════════════════════════════

Available Snapshots: 47
Date Range: 2015-03-10 to 2024-01-15

Key Milestones:
├─ 2015-03-10: First snapshot (account creation)
├─ 2018-07-22: Major redesign detected
├─ 2020-11-05: Mass deletion event (15 posts removed)
├─ 2021-09-12: Profile rebranding
└─ 2023-02-28: Last available snapshot

Snapshot Density:
2015: ████ (12 snapshots)
2016-2019: ████████ (24 snapshots)
2020-2023: ████ (11 snapshots)

RECOMMENDED SNAPSHOTS:
• Earliest: https://web.archive.org/web/20150310*/https://example.com
• Most complete: https://web.archive.org/web/20180722*/https://example.com
• Most recent: https://web.archive.org/web/20230228*/https://example.com
```

### Command: `/what-changed [url] [date1] [date2]`

```
SYNTAX: /what-changed <url> <date1> <date2>

OUTPUT:
🔍 Changes Detected: example.com
══════════════════════════════════
Comparing: 2020-01-01 vs 2020-06-01

PROFILE CHANGES:
├─ Username: johndoe → johndoe_official
├─ Bio: "Developer" → "Senior Developer at TechCorp"
├─ Location: Added "San Francisco, CA"
└─ Website: Added "https://johndoe.dev"

CONTENT CHANGES:
├─ New posts: 23
├─ Deleted posts: 5
└─ Modified posts: 2

DELETED CONTENT:
• Post from 2019-08-12: "Just tried..." [View Archive]
• Post from 2019-11-03: "Can't believe..." [View Archive]
• (3 more...)

LINK CHANGES:
├─ New links: 8
├─ Removed links: 3
└─ Broken links: 2

SIGNIFICANCE: Medium - Profile rebranding with content cleanup
```

### Command: `/deleted-content [url]`

```
SYNTAX: /deleted-content <url> [--since date]

OUTPUT:
🗑️ Deleted Content Recovery: example.com
═══════════════════════════════════════════

Deleted Posts Found: 12
Recovery Rate: 85% (10/12 recovered from archives)

RECENT DELETIONS (Last 6 months):
├─ 2024-01-10: "Controversial opinion post..." [RECOVERED]
├─ 2023-12-28: "Personal announcement..." [RECOVERED]
├─ 2023-11-15: "Product review..." [RECOVERED]
└─ (9 more...)

DELETION PATTERNS:
• Mass deletion: 2023-11-01 (5 posts in 1 hour)
• Selective deletion: 2024-01 (political content)
• Account wipe: None detected

ARCHIVE SOURCES:
• Wayback Machine: 8 snapshots
• Archive.today: 2 snapshots
• Google Cache: 0 snapshots

VIEW ALL: [Link to detailed report]
```

---

## 7. Archive Verification

### Authenticity Checks

**Timestamp Verification:**
```
1. Verify snapshot timestamp in URL
2. Check page content for date references
3. Cross-reference with other archives
4. Validate against known events
```

**Content Integrity:**
```
1. Check for Wayback Machine banners/JS
2. Verify no injected content
3. Compare multiple snapshots for consistency
4. Validate image hashes
```

### Confidence Ratings

| Source | Confidence | Notes |
|--------|-----------|-------|
| Wayback Machine | 90% | Reliable, comprehensive |
| Archive.today | 85% | Good alternative |
| Google Cache | 70% | Limited, ephemeral |
| Screenshots | 60% | Visual only, no text extraction |
| Third-party archives | 50% | Verify authenticity |

---

## 8. Advanced Techniques

### Content Recovery from Partial Archives

**Reconstruction from Fragments:**
```
1. Collect all available snapshots
2. Extract unique content from each
3. Merge into complete dataset
4. Identify gaps
5. Flag missing content
```

**Cross-Archive Correlation:**
```
# Check multiple archive sources
wayback_url = f"https://web.archive.org/web/*/{target}"
archive_today = f"https://archive.today/{target}"
google_cache = f"https://webcache.googleusercontent.com/search?q={target}"

# Merge results
all_snapshots = merge_snapshots(wayback, archive_today, google_cache)
```

### Historical Search Dorks

```
# Find old content by keyword
site:web.archive.org "keyword" "target-site.com"

# Find historical profiles
site:web.archive.org/web/*/twitter.com/username

# Find historical mentions
site:web.archive.org "username" "platform.com"

# Find deleted pages (404 in current, exist in archive)
site:web.archive.org/web/*/https://target-site.com/deleted-page
```

---

## Quick Reference: Historical Analysis Commands

```bash
/history [url]
  └─ Show all available snapshots with timeline

/history [url] --from YYYY-MM-DD --to YYYY-MM-DD
  └─ Show snapshots in date range

/what-changed [url] [date1] [date2]
  └─ Compare two snapshots and show differences

/deleted-content [url]
  └─ Find and recover deleted content

/deleted-content [url] --since YYYY-MM-DD
  └─ Find deletions since specific date

/timeline [url]
  └─ Reconstruct complete timeline from archives
```

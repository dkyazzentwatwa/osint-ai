# Change Detection

System for detecting changes over time and recovering deleted content.

---

## 1. What-Changed Detection Logic

### Change Detection Framework

**Monitoring Setup:**
```
1. Establish baseline snapshot
2. Define monitoring scope:
   - Profile fields
   - Content/posts
   - Connections/followers
   - Metadata
   - Links
3. Set monitoring frequency
4. Configure change thresholds
```

### Change Categories

| Category | Change Types | Detection Method |
|----------|--------------|------------------|
| Profile | Bio, name, photo, location | Field comparison |
| Content | Posts, comments, media | Hash/content diff |
| Network | Followers, following, mutuals | Set operations |
| Metadata | Timestamps, device info | EXIF/header analysis |
| Visibility | Public/private, deleted | Availability check |

### Change Detection Algorithm

```python
def detect_changes(baseline, current, scope):
    changes = []
    
    FOR field IN scope.profile_fields:
        IF baseline[field] != current[field]:
            changes.append({
                "type": "profile_update",
                "field": field,
                "old": baseline[field],
                "new": current[field],
                "significance": assess_significance(field, baseline[field], current[field])
            })
    
    FOR content IN scope.monitored_content:
        IF content NOT IN current.content:
            changes.append({
                "type": "content_deletion",
                "content_id": content.id,
                "last_seen": content.timestamp,
                "significance": "high" IF content.engagement > 100 ELSE "medium"
            })
    
    FOR content IN current.content:
        IF content NOT IN baseline.content:
            changes.append({
                "type": "content_addition",
                "content_id": content.id,
                "timestamp": content.timestamp,
                "significance": assess_content_significance(content)
            })
    
    FOR connection IN scope.connections:
        IF connection IN current.followers AND connection NOT IN baseline.followers:
            changes.append({
                "type": "new_follower",
                "account": connection,
                "significance": assess_connection_significance(connection)
            })
    
    RETURN changes
```

### Change Significance Scoring

```
SIGNIFICANCE_MATRIX = {
    "profile_update": {
        "username": 90,      # High - identity change
        "display_name": 40,   # Medium - name change
        "bio": 30,           # Medium - description change
        "location": 50,      # Medium-High - location change
        "profile_image": 35, # Medium - visual identity
        "website": 25        # Low-Medium - link update
    },
    "content_deletion": {
        "single_post": 20,
        "multiple_posts": 50,
        "mass_deletion": 80,
        "selective_deletion": 70
    },
    "network_change": {
        "single_follow": 5,
        "mass_unfollow": 40,
        "account_privatization": 60,
        "account_deletion": 95
    }
}
```

---

## 2. Deletion Recovery Strategies

### Recovery Sources

| Source | Recovery Rate | Reliability | Speed |
|--------|--------------|-------------|-------|
| Wayback Machine | 60-80% | High | Medium |
| Archive.today | 40-60% | High | Medium |
| Google Cache | 20-40% | Medium | Fast |
| Screenshots (others) | 10-30% | Low | Slow |
| Cached APIs | 30-50% | Medium | Fast |

### Recovery Priority

```python
def recover_deleted_content(url, content_id, timestamp):
    recovery_attempts = []
    
    # Priority 1: Wayback Machine
    wayback_url = f"https://web.archive.org/web/{timestamp}*/{url}"
    result = try_recovery(wayback_url)
    IF result.success:
        recovery_attempts.append({"source": "wayback", "content": result.content})
    
    # Priority 2: Archive.today
    archive_today = f"https://archive.today/{url}"
    result = try_recovery(archive_today)
    IF result.success:
        recovery_attempts.append({"source": "archive.today", "content": result.content})
    
    # Priority 3: Google Cache
    google_cache = f"https://webcache.googleusercontent.com/search?q={url}"
    result = try_recovery(google_cache)
    IF result.success:
        recovery_attempts.append({"source": "google_cache", "content": result.content})
    
    # Priority 4: Other archives
    other_archives = search_other_archives(url, timestamp)
    FOR archive IN other_archives:
        result = try_recovery(archive)
        IF result.success:
            recovery_attempts.append({"source": archive.name, "content": result.content})
    
    RETURN merge_recoveries(recovery_attempts)
```

### Recovery Dorks

```
# Find archived version
site:web.archive.org {url}
site:archive.today {url}

# Find cached version
cache:{url}

# Find screenshots
site:reddit.com "{url}" screenshot OR archive
site:twitter.com "{url}" screenshot

# Find mentions with quotes
"exact text from deleted post" -site:original-site.com
```

---

## 3. Content Diff Methodology

### Text Diff Algorithm

**Line-Based Diff:**
```python
def text_diff(old_text, new_text):
    old_lines = old_text.split('\n')
    new_lines = new_text.split('\n')
    
    diff = unified_diff(old_lines, new_lines, lineterm='')
    
    changes = {
        "added": [],
        "removed": [],
        "modified": []
    }
    
    FOR line IN diff:
        IF line.startswith('+'):
            changes["added"].append(line[1:])
        ELIF line.startswith('-'):
            changes["removed"].append(line[1:])
    
    RETURN changes
```

**Word-Level Diff:**
```python
def word_diff(old_text, new_text):
    old_words = old_text.split()
    new_words = new_text.split()
    
    matcher = SequenceMatcher(None, old_words, new_words)
    
    changes = []
    FOR tag, i1, i2, j1, j2 IN matcher.get_opcodes():
        IF tag == 'replace':
            changes.append({
                "type": "modified",
                "old": old_words[i1:i2],
                "new": new_words[j1:j2]
            })
        ELIF tag == 'delete':
            changes.append({
                "type": "removed",
                "content": old_words[i1:i2]
            })
        ELIF tag == 'insert':
            changes.append({
                "type": "added",
                "content": new_words[j1:j2]
            })
    
    RETURN changes
```

### Visual Diff for Profiles

```python
def visual_diff(old_image, new_image):
    # Image comparison
    diff = ImageChops.difference(old_image, new_image)
    
    IF diff.getbbox() IS None:
        RETURN "identical"
    
    # Calculate difference percentage
    diff_percentage = calculate_diff_percentage(diff)
    
    IF diff_percentage < 5%:
        RETURN "minor_changes"
    ELIF diff_percentage < 30%:
        RETURN "moderate_changes"
    ELSE:
        RETURN "major_changes"
```

### Structured Data Diff

```python
def json_diff(old_json, new_json):
    changes = []
    
    FOR key IN old_json.keys() | new_json.keys():
        IF key NOT IN new_json:
            changes.append({"type": "removed", "key": key, "value": old_json[key]})
        ELIF key NOT IN old_json:
            changes.append({"type": "added", "key": key, "value": new_json[key]})
        ELIF old_json[key] != new_json[key]:
            changes.append({
                "type": "modified",
                "key": key,
                "old": old_json[key],
                "new": new_json[key]
            })
    
    RETURN changes
```

---

## 4. Version Comparison Rules

### Comparison Scope Definition

```
SCOPE_LEVELS = {
    "minimal": ["username", "display_name", "profile_image"],
    "basic": ["username", "display_name", "bio", "location", "profile_image"],
    "standard": ["username", "display_name", "bio", "location", "profile_image", "recent_posts"],
    "comprehensive": ["all_profile_fields", "all_content", "connections", "metadata"],
    "forensic": ["all_profile_fields", "all_content", "connections", "metadata", "timestamps", "device_info"]
}
```

### Comparison Rules

**Rule 1: Username Change Detection**
```
IF old.username != new.username:
    SIGNIFICANCE = CRITICAL
    ACTION = log_identity_change
    ALERT = immediate
```

**Rule 2: Mass Deletion Detection**
```
deletion_rate = (old.post_count - new.post_count) / old.post_count

IF deletion_rate > 0.8:
    SIGNIFICANCE = CRITICAL
    ACTION = trigger_recovery_protocol
    
ELIF deletion_rate > 0.5:
    SIGNIFICANCE = HIGH
    ACTION = log_mass_deletion
    
ELIF deletion_rate > 0.2:
    SIGNIFICANCE = MEDIUM
    ACTION = log_selective_deletion
```

**Rule 3: Selective Content Removal**
```
deleted_posts = old.posts - new.posts

IF deleted_posts HAVE common_theme:
    SIGNIFICANCE = HIGH
    ACTION = flag_selective_cleanup
    
IF deleted_posts CONTAINS controversial_content:
    SIGNIFICANCE = HIGH
    ACTION = flag_reputation_management
```

**Rule 4: Network Change Detection**
```
follower_change = (new.follower_count - old.follower_count) / old.follower_count

IF follower_change > 0.5:
    SIGNIFICANCE = MEDIUM
    ACTION = log_viral_growth
    
IF follower_change < -0.5:
    SIGNIFICANCE = HIGH
    ACTION = log_mass_unfollow
    
IF following_count == 0 AND old.following_count > 0:
    SIGNIFICANCE = HIGH
    ACTION = log_account_cleanup
```

### Temporal Comparison Rules

```python
def temporal_comparison(snapshots):
    patterns = {
        "regular_updates": [],
        "sudden_changes": [],
        "gradual_shifts": [],
        "deletion_events": []
    }
    
    FOR i IN range(1, len(snapshots)):
        old = snapshots[i-1]
        new = snapshots[i]
        time_diff = new.timestamp - old.timestamp
        
        changes = detect_changes(old, new)
        
        # Detect sudden changes
        IF len(changes) > 10 AND time_diff < timedelta(days=1):
            patterns["sudden_changes"].append({
                "timestamp": new.timestamp,
                "changes": changes
            })
        
        # Detect deletion events
        deletions = [c FOR c IN changes IF c.type == "content_deletion"]
        IF len(deletions) > 5:
            patterns["deletion_events"].append({
                "timestamp": new.timestamp,
                "deletions": deletions
            })
    
    RETURN patterns
```

---

## 5. Change Detection Commands

### Command: `/what-changed [url] [date1] [date2]`

```
SYNTAX: /what-changed <url> <date1> <date2> [--scope level]

OUTPUT:
🔍 CHANGE DETECTION REPORT
════════════════════════════════════════════════════
Target: https://twitter.com/username
Comparison: 2024-01-01 vs 2024-02-01

CHANGE SUMMARY:
├─ Total Changes: 23
├─ Additions: 18
├─ Deletions: 3
├─ Modifications: 2
└─ Significance Score: 45/100 (Medium)

PROFILE CHANGES (2):
├─ [MODIFIED] Display Name
│  ├─ Old: "John Doe"
│  └─ New: "John Doe | Developer"
│  └─ Significance: Low
│
└─ [MODIFIED] Bio
   ├─ Old: "Software developer"
   └─ New: "Senior Software Developer at TechCorp | Open source contributor"
   └─ Significance: Medium

CONTENT CHANGES (18):
├─ New Posts: 18
│  └─ View: [List all new posts]
│
└─ Deleted Posts: 3
   ├─ 🔴 Post from 2023-12-15 (High engagement)
   │  └─ Recovery: [Archive link]
   ├─ 🟡 Post from 2023-11-20
   │  └─ Recovery: [Archive link]
   └─ 🟢 Post from 2023-10-05
      └─ Recovery: Not available

NETWORK CHANGES (3):
├─ New Followers: +245 (+12.5%)
├─ New Following: +12
└─ Lost Followers: -18

RECOMMENDED ACTIONS:
1. Review deleted posts (2 recovered, 1 missing)
2. Verify new bio claims
3. Monitor follower quality
```

### Command: `/deleted-content [url]`

```
SYNTAX: /deleted-content <url> [--since date] [--type content_type]

OUTPUT:
🗑️ DELETED CONTENT RECOVERY REPORT
═══════════════════════════════════════
Target: https://twitter.com/username
Analysis Period: Last 12 months

DELETION SUMMARY:
├─ Total Deletions Found: 47
├─ Successfully Recovered: 38 (81%)
├─ Partially Recovered: 5 (11%)
└─ Unrecoverable: 4 (8%)

RECOVERY BY SOURCE:
├─ Wayback Machine: 28 recovered
├─ Archive.today: 7 recovered
├─ Google Cache: 2 recovered
├─ Third-party archives: 1 recovered
└─ Screenshots: 0 recovered

DELETION PATTERNS:
├─ Mass Deletion Event: 2023-11-15 (12 posts in 2 hours)
├─ Selective Deletion: Political content (8 posts)
├─ Routine Cleanup: Low engagement posts (15 posts)
└─ Single Deletions: Various (12 posts)

TOP RECOVERED CONTENT:
1. [HIGH] 2023-11-10 - Controversial statement [View]
2. [HIGH] 2023-10-22 - Personal announcement [View]
3. [MED] 2023-09-15 - Product review [View]
4. (35 more...)

EVIDENCE PACKAGE:
└─ [Download All Recovered Content]
```

---

## 6. Automated Monitoring Setup

### Monitoring Configuration

```yaml
monitoring_config:
  target: "https://twitter.com/username"
  frequency: "daily"
  scope: "comprehensive"
  
  alerts:
    - condition: "username_change"
      action: "immediate_notification"
    - condition: "mass_deletion > 5"
      action: "recovery_protocol"
    - condition: "follower_change > 20%"
      action: "daily_digest"
  
  storage:
    baseline_retention: "90_days"
    snapshot_retention: "1_year"
    change_log_retention: "permanent"
```

### Change Alert Template

```
🚨 CHANGE DETECTED ALERT
═══════════════════════════════════
Target: @username (Twitter)
Detection Time: 2024-02-15 14:32:00 UTC

CHANGE TYPE: Mass Deletion Event
SEVERITY: HIGH

DETAILS:
• 12 posts deleted in last 24 hours
• Includes 3 high-engagement posts
• Selective deletion pattern detected

RECOVERY STATUS:
• 9 posts recovered from archives
• 3 posts unrecoverable

RECOMMENDED ACTIONS:
1. Review recovered content for context
2. Investigate deletion trigger
3. Monitor for additional deletions

[View Full Report] [Dismiss Alert]
```

---

## Quick Reference: Change Detection Commands

```bash
/what-changed [url] [date1] [date2]
  └─ Compare two versions and show all changes
  
  Options:
  --scope [minimal|basic|standard|comprehensive|forensic]
  --format [summary|detailed|json]
  --include-recovered

/deleted-content [url]
  └─ Find and recover deleted content
  
  Options:
  --since [YYYY-MM-DD]
  --until [YYYY-MM-DD]
  --type [posts|profile|media|all]
  --recovery-source [wayback|archive.today|all]

/monitor [url]
  └─ Set up automated change monitoring
  
  Options:
  --frequency [hourly|daily|weekly]
  --scope [level]
  --alert [conditions]

/changelog [url]
  └─ View complete change history
  
  Options:
  --since [date]
  --filter [change_type]
  --export [format]
```

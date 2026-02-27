# Auto-Pivot Rules

Rules for automatically suggesting investigation pivots based on discovered data.

---

## 1. Username Variation Pivot Rules

### Trigger Conditions

**Rule 1.1: Character Substitution Detected**
```
IF entity.username CONTAINS leet_chars (0,1,3,4,5,7,@,$,!)
AND similar_username_exists(platforms)
THEN SUGGEST_PIVOT(
    type: "username_variant",
    target: normalized_username,
    priority: HIGH,
    confidence: 85%,
    rationale: "Leet substitution suggests same user avoiding detection"
)
```

**Rule 1.2: Platform Presence Gap**
```
IF entity.active_on CONTAINS platform_A
AND similar_username INACTIVE on platform_A
AND similar_username ACTIVE on platform_B
THEN SUGGEST_PIVOT(
    type: "cross_platform_presence",
    target: platform_B_similar_username,
    priority: MEDIUM,
    confidence: 70%,
    rationale: "Username migration between platforms"
)
```

**Rule 1.3: Incremental Numbering**
```
IF entity.username MATCHES pattern(name + number)
AND name + (number+1) EXISTS
THEN SUGGEST_PIVOT(
    type: "incremental_username",
    target: name + (number+1),
    priority: MEDIUM,
    confidence: 60%,
    rationale: "Sequential numbering suggests related accounts"
)
```

### Username Pivot Dork

```
# Search for variants
"johndoe" OR "john_doe" OR "johndoe1" OR "johndoe2" site:twitter.com

# Search with wildcards
"john*" site:instagram.com "software engineer"
```

---

## 2. Email-Name Mismatch Detection

### Mismatch Patterns

**Rule 2.1: Name in Email Mismatch**
```
IF entity.display_name = "John Smith"
AND entity.email CONTAINS different_name
AND different_name NOT IN aliases
THEN SUGGEST_PIVOT(
    type: "email_name_mismatch",
    target: different_name,
    priority: HIGH,
    confidence: 75%,
    rationale: "Email contains different name - possible alias or account compromise"
)
```

**Rule 2.2: Email Domain Pivot**
```
IF entity.email_domain = "company-A.com"
AND entity.affiliation != "Company A"
THEN SUGGEST_PIVOT(
    type: "email_domain_investigation",
    target: entity.email_domain,
    priority: MEDIUM,
    confidence: 70%,
    rationale: "Email domain doesn't match stated affiliation"
)
```

**Rule 2.3: Disposable Email Usage**
```
IF entity.email_domain IN disposable_domains
AND entity.platform IN professional_platforms
THEN SUGGEST_PIVOT(
    type: "disposable_email_investigation",
    target: entity.email_prefix,
    priority: HIGH,
    confidence: 80%,
    rationale: "Disposable email on professional platform suggests intentional obfuscation"
)
```

### Email Pivot Dork

```
# Find other accounts with same email pattern
"johnsmith@" site:haveibeenpwned.com OR site:pastebin.com

# Find email on other platforms
"johnsmith@company.com" -site:linkedin.com
```

---

## 3. Co-Mention Triggers

### Mention-Based Pivots

**Rule 3.1: Frequent Co-Mention**
```
IF entity_A.mentions(entity_B) > 5
AND entity_B.mentions(entity_A) > 3
AND timeframe = RECENT
THEN SUGGEST_PIVOT(
    type: "frequent_comention",
    target: entity_B,
    priority: MEDIUM,
    confidence: 75%,
    rationale: "Frequent mutual mentions suggest relationship"
)
```

**Rule 3.2: Tagged Together**
```
IF entity_A.tagged_with(entity_B) IN photos > 3
THEN SUGGEST_PIVOT(
    type: "photo_tag_relationship",
    target: entity_B,
    priority: HIGH,
    confidence: 85%,
    rationale: "Multiple photo tags indicate personal relationship"
)
```

**Rule 3.3: Reply Thread Participation**
```
IF entity_A.replies_to(entity_B) > 10
AND conversation_depth > 5
THEN SUGGEST_PIVOT(
    type: "conversation_participant",
    target: entity_B,
    priority: MEDIUM,
    confidence: 70%,
    rationale: "Extended conversations suggest connection"
)
```

### Co-Mention Dork

```
# Find mentions together
"@entityA" "@entityB" site:twitter.com

# Find tagged photos
site:instagram.com "entityA" "entityB"
```

---

## 4. Shared Connection Alerts

### Network-Based Pivots

**Rule 4.1: High Mutual Connection Overlap**
```
IF mutual_connections(entity_A, entity_B) > 10
AND mutual_percentage > 30%
THEN SUGGEST_PIVOT(
    type: "mutual_network_overlap",
    target: entity_B,
    priority: MEDIUM,
    confidence: 70%,
    rationale: "High mutual connection overlap suggests same social circle"
)
```

**Rule 4.2: Same Community Membership**
```
IF entity_A.communities INTERSECT entity_B.communities > 3
AND communities ARE niche
THEN SUGGEST_PIVOT(
    type: "shared_community_membership",
    target: entity_B,
    priority: LOW,
    confidence: 60%,
    rationale: "Multiple shared niche communities suggest connection"
)
```

**Rule 4.3: Coordinated Following Pattern**
```
IF entity_A.followed_by set INTERSECT entity_B.followed_by set > 50
AND accounts ARE suspicious OR bots
THEN SUGGEST_PIVOT(
    type: "coordinated_follower_pattern",
    target: "investigate_follower_quality",
    priority: HIGH,
    confidence: 80%,
    rationale: "Shared suspicious followers suggest coordination"
)
```

### Connection Pivot Dork

```
# Analyze mutual connections
site:twitter.com/entityA/following AND site:twitter.com/entityB/following

# Find shared community
site:reddit.com/r/subreddit "entityA" "entityB"
```

---

## 5. Example Scenarios

### Scenario 1: Username Variation Discovery

**Input:** Primary target `j0hndoe` on Twitter

**Detection:**
- System finds `johndoe` on Instagram with similar content
- Profile pictures match (visual similarity 95%)
- Bio text has 80% overlap
- Posting times correlate (r=0.85)

**Pivot Suggestion:**
```
🔄 PIVOT SUGGESTED
Type: Username Variation
Target: johndoe (Instagram)
Confidence: 90%
Priority: HIGH

Evidence:
• Leet substitution pattern (0 for o)
• Matching profile pictures
• Similar bio content
• Correlated activity times

Action: Merge entities and investigate Instagram account
```

### Scenario 2: Email-Name Mismatch

**Input:** Target claims name "John Smith", email is `mike.jones@email.com`

**Detection:**
- Email name doesn't match display name
- No alias "Mike Jones" documented
- Account created recently

**Pivot Suggestion:**
```
🔄 PIVOT SUGGESTED
Type: Email Name Mismatch
Target: "Mike Jones"
Confidence: 70%
Priority: HIGH

Evidence:
• Email contains different name
• No known alias matches
• Recent account creation

Action: Investigate "Mike Jones" as possible real identity or alias
Warning: Could indicate account purchase/compromise
```

### Scenario 3: Coordinated Activity

**Input:** Investigating `@suspicious_account`

**Detection:**
- Account posts within 2 minutes of `@related_account` 15 times
- Same hashtags used consistently
- Identical image posts (different order)
- 80% follower overlap

**Pivot Suggestion:**
```
🔄 PIVOT SUGGESTED
Type: Coordinated Activity
Target: @related_account
Confidence: 85%
Priority: HIGH

Evidence:
• Temporal correlation (15 synchronized posts)
• Identical hashtag patterns
• Shared image content
• High follower overlap

Action: Investigate as potential sock puppet or coordinated network
Expand to identify additional coordinated accounts
```

### Scenario 4: Geographic Impossibility

**Input:** Target posts from "New York" at 2pm

**Detection:**
- Same account posts from "Tokyo" at 2:15pm
- 15-minute gap
- 6,700km distance
- Would require 26,800 km/h travel

**Pivot Suggestion:**
```
🔄 PIVOT SUGGESTED
Type: Geographic Impossibility
Target: Investigate VPN/Proxy usage or account sharing
Confidence: 90%
Priority: HIGH

Evidence:
• Impossible travel detected (26,800 km/h required)
• 15-minute gap between posts
• 6,700km distance

Action:
• Check for VPN usage patterns
• Investigate potential account sharing
• Analyze posting style differences by location
• Look for additional geographic anomalies
```

---

## 6. Pivot Priority Matrix

| Pivot Type | Confidence | Priority | Action Timeline |
|------------|-----------|----------|-----------------|
| Username exact match | 95% | Critical | Immediate |
| Email exact match | 95% | Critical | Immediate |
| Profile picture match | 90% | High | Within 24h |
| Username variation | 85% | High | Within 24h |
| Geographic impossibility | 90% | High | Within 24h |
| Coordinated activity | 85% | High | Within 24h |
| Frequent co-mention | 75% | Medium | Within 48h |
| Mutual network overlap | 70% | Medium | Within 48h |
| Shared community | 60% | Low | Within 1 week |
| Temporal correlation | 70% | Medium | Within 48h |

---

## 7. Pivot Suppression Rules

### When NOT to Suggest Pivots

**Suppression Rule 1: Known False Positive**
```
IF pivot_type IN known_false_positives
AND entity_matches_suppression_pattern
THEN SUPPRESS_PIVOT
LOG: "Pivot suppressed - known false positive pattern"
```

**Suppression Rule 2: Insufficient Evidence**
```
IF confidence < 50%
AND corroborating_evidence < 2
THEN SUPPRESS_PIVOT
LOG: "Pivot suppressed - insufficient evidence"
```

**Suppression Rule 3: Privacy Protection**
```
IF pivot_target IN privacy_protected_entities
AND investigation_scope != "authorized"
THEN SUPPRESS_PIVOT
LOG: "Pivot suppressed - privacy protection"
```

---

## Quick Reference: Pivot Command

```
/pivot-suggest [entity]
  └─ Analyzes entity and suggests investigation pivots
  
/pivot-accept [pivot_id]
  └─ Adds pivot target to investigation
  
/pivot-ignore [pivot_id]
  └─ Dismisses pivot suggestion with reason
  
/pivot-list
  └─ Shows all pending pivot suggestions
```

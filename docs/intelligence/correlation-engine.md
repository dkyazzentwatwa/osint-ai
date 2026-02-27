# Correlation Engine

Smart connection discovery system for identifying relationships between entities.

---

## 1. Auto-Pivot Suggestion Logic

### Pivot Trigger Conditions

**High-Confidence Pivot Triggers:**

| Trigger Type | Condition | Suggested Pivot | Priority |
|--------------|-----------|-----------------|----------|
| Username variant found | Similar username on different platform | Investigate variant account | High |
| Email domain match | Multiple accounts share email domain | Domain investigation | High |
| Mutual connections | >5 shared connections | Network analysis | Medium |
| Content similarity | >80% content overlap | Cross-reference content | Medium |
| Temporal alignment | Posts within minutes on different platforms | Timeline correlation | High |
| Geographic overlap | Same locations, different accounts | Location-based pivot | Medium |

### Pivot Decision Tree

```
ON entity_discovered:
    
    # Check for username variations
    IF similar_username_exists(platforms):
        SUGGEST_PIVOT(username_variation_investigation)
        PRIORITY = HIGH
    
    # Check for email patterns
    IF email_domain IN known_domains:
        SUGGEST_PIVOT(domain_correlation)
        PRIORITY = MEDIUM
    
    # Check for shared connections
    IF mutual_connections > threshold:
        SUGGEST_PIVOT(network_analysis)
        PRIORITY = MEDIUM
    
    # Check for content patterns
    IF content_similarity > 0.8:
        SUGGEST_PIVOT(content_cross_reference)
        PRIORITY = HIGH
    
    # Check for temporal patterns
    IF post_time_correlation > 0.9:
        SUGGEST_PIVOT(timeline_correlation)
        PRIORITY = HIGH
```

### Pivot Suggestion Dork

```
# Find related accounts by content
"exact phrase from target" site:twitter.com OR site:instagram.com -from:target

# Find by image/hash
site:images.google.com (reverse image search for profile pictures)

# Find by mutual connections
site:twitter.com/following (analyze shared following)
```

---

## 2. Cross-Reference Matching Algorithms

### Exact Matching

**String Matching:**
```
MATCH_TYPES:
- Exact: "johndoe" == "johndoe"
- Case-insensitive: "JohnDoe" == "johndoe"
- Normalized: "john_doe" == "johndoe" (remove special chars)
```

**Email Matching:**
```
# Normalize emails for comparison
normalize_email(email):
    lower = email.lower()
    remove_dots = lower.replace(".", "")
    remove_plus = remove_dots.split("+")[0]
    return remove_plus

# Gmail equivalence: john.doe+test@gmail.com == johndoe@gmail.com
```

### Fuzzy Matching

**Username Similarity:**
```python
def username_similarity(u1, u2):
    # Levenshtein distance
    distance = levenshtein(u1, u2)
    max_len = max(len(u1), len(u2))
    similarity = 1 - (distance / max_len)
    
    # Adjust for common substitutions
    if has_leet_substitution(u1, u2):
        similarity += 0.15
    
    return similarity

THRESHOLD = 0.85  # Consider match if >= 85% similar
```

**Content Similarity:**
```
# TF-IDF + Cosine Similarity
def content_similarity(text1, text2):
    # Normalize
    t1 = normalize(text1)
    t2 = normalize(text2)
    
    # Calculate TF-IDF vectors
    vec1 = tfidf_vectorize(t1)
    vec2 = tfidf_vectorize(t2)
    
    # Cosine similarity
    similarity = cosine_similarity(vec1, vec2)
    
    return similarity
```

### Multi-Attribute Matching

```
CORRELATION_SCORE = (
    username_match * 0.25 +
    email_match * 0.30 +
    content_match * 0.20 +
    temporal_match * 0.15 +
    network_match * 0.10
)

IF CORRELATION_SCORE >= 0.75:
    CONFIDENCE = "High - likely same entity"
ELSE IF CORRELATION_SCORE >= 0.50:
    CONFIDENCE = "Medium - possible connection"
ELSE IF CORRELATION_SCORE >= 0.25:
    CONFIDENCE = "Low - weak connection"
ELSE:
    CONFIDENCE = "No significant correlation"
```

---

## 3. Shared Attribute Discovery

### Profile Attribute Comparison

**Attributes to Compare:**

| Attribute | Match Type | Weight |
|-----------|-----------|--------|
| Username | Exact/Similar | 0.20 |
| Display name | Exact | 0.15 |
| Bio/Description | Similarity | 0.15 |
| Profile picture | Hash/Image match | 0.20 |
| Location | Exact/Close | 0.10 |
| Email domain | Exact | 0.15 |
| Phone pattern | Similar | 0.05 |

### Shared Attribute Dork

```
# Find by bio text
"exact bio text" site:twitter.com OR site:instagram.com OR site:linkedin.com

# Find by location + interest
"software engineer" "Seattle" site:linkedin.com/in

# Find by profile image (use reverse image search)
images.google.com (upload profile picture)
```

### Attribute Correlation Matrix

```
                    Username  Email   Bio    Image   Location
Username              1.0     0.3    0.2     0.1      0.1
Email                 0.3     1.0    0.4     0.2      0.1
Bio                   0.2     0.4    1.0     0.3      0.2
Image                 0.1     0.2    0.3     1.0      0.1
Location              0.1     0.1    0.2     0.1      1.0

# Higher correlation = stronger evidence of connection
```

---

## 4. Temporal Correlation Rules

### Posting Time Analysis

**Synchronization Detection:**

```
RULE: Posting Synchronization
IF |post_time_A - post_time_B| < 5 minutes
AND content_similarity > 0.7
AND occurs > 3 times
THEN temporal_correlation = HIGH

RULE: Activity Pattern Match
IF daily_activity_pattern_A correlates > 0.8 with pattern_B
AND timezone_difference < 2 hours
THEN likely_same_operator = TRUE

RULE: Coordinated Campaign
IF multiple_accounts_post_within(minutes: 30)
AND content_contains(common_hashtags)
AND occurs > 5 times
THEN coordinated_activity = TRUE
```

### Timeline Correlation Dork

```
# Find posts at same time
site:twitter.com since:2024-01-01T14:00:00 until:2024-01-01T14:30:00 "hashtag"

# Find by date patterns
site:twitter.com "on this day" OR "anniversary" "event name"
```

---

## 5. Network Proximity Detection

### Connection Path Analysis

**Degrees of Separation:**

```
DEGREE_1: Direct connection (follows/followed by)
DEGREE_2: Friend of friend (mutual connection)
DEGREE_3: Connection through shared community

SCORING:
Direct connection: 1.0
Mutual connection: 0.7
Shared community: 0.4
Same follower cluster: 0.3
No apparent connection: 0.0
```

### Network Cluster Detection

```
# Detect clusters using community detection
FOR each_account IN network:
    connections = get_followers_following(account)
    
    # Find densely connected groups
    clusters = detect_communities(connections)
    
    FOR cluster IN clusters:
        IF target_account IN cluster:
            cluster_members = cluster.accounts
            cluster_cohesion = calculate_density(cluster)
            
            IF cluster_cohesion > 0.6:
                FLAG = "tight_knit_community"
                CONFIDENCE = 80%
```

### Network Analysis Dork

```
# Analyze following/followers
site:twitter.com/user/followers
site:twitter.com/user/following

# Find shared communities
site:reddit.com/r/subreddit "username" "othertarget"
```

---

## 6. Likelihood Scoring Formulas

### Entity Correlation Score

```
ENTITY_CORRELATION(entity_A, entity_B):
    
    score = 0
    
    # Username similarity (0-25 points)
    username_sim = username_similarity(A.username, B.username)
    score += username_sim * 25
    
    # Email correlation (0-30 points)
    IF A.email_domain == B.email_domain:
        score += 30
    ELSE IF domains_related(A.email, B.email):
        score += 15
    
    # Content similarity (0-20 points)
    content_sim = content_similarity(A.posts, B.posts)
    score += content_sim * 20
    
    # Temporal correlation (0-15 points)
    IF posting_times_correlate(A, B) > 0.8:
        score += 15
    ELSE IF posting_times_correlate(A, B) > 0.5:
        score += 8
    
    # Network proximity (0-10 points)
    network_score = calculate_network_proximity(A, B)
    score += network_score * 10
    
    RETURN score  # 0-100 scale
```

### Correlation Interpretation

| Score | Classification | Recommended Action |
|-------|----------------|-------------------|
| 90-100 | Same entity (confirmed) | Merge entities in investigation |
| 75-89 | Same entity (likely) | Deep correlation analysis |
| 60-74 | Related entities | Investigate relationship type |
| 40-59 | Possible connection | Light investigation |
| 20-39 | Weak connection | Monitor for additional signals |
| 0-19 | No connection | Disregard |

---

## 7. Correlation Report Template

```
CORRELATION ANALYSIS REPORT
============================
Primary Entity: [entity_A]
Secondary Entity: [entity_B]
Analysis Date: [date]

CORRELATION SCORE: [X/100]
CLASSIFICATION: [Same/Related/Weak/None]

EVIDENCE BREAKDOWN:
├─ Username Similarity: [X%] ([details])
├─ Email Correlation: [X%] ([details])
├─ Content Match: [X%] ([details])
├─ Temporal Correlation: [X%] ([details])
└─ Network Proximity: [X%] ([details])

KEY FINDINGS:
• [Finding 1]
• [Finding 2]
• [Finding 3]

RECOMMENDED PIVOTS:
1. [Pivot suggestion with priority]
2. [Pivot suggestion with priority]

CONFIDENCE ASSESSMENT: [X%]
VERIFICATION STATUS: [Verified/Unverified/Needs verification]
```

---

## Quick Reference: Correlation Dorks

```
# Multi-platform username search
"johndoe" (site:twitter.com OR site:instagram.com OR site:github.com)

# Email domain correlation
"@company.com" site:linkedin.com/in OR site:twitter.com

# Content similarity
"exact phrase from bio" site:social_media_platforms

# Reverse image search for profile pictures
images.google.com (upload image)

# Find by mutual connections
site:twitter.com/followers (analyze overlap)

# Temporal correlation
site:twitter.com since:YYYY-MM-DDTHH:MM:SS until:YYYY-MM-DDTHH:MM:SS
```

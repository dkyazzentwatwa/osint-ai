# Anomaly Detector

System for detecting unusual patterns and behaviors in OSINT investigations.

---

## 1. Timeline Anomaly Detection

### Activity Timeline Analysis

**Normal Activity Pattern:**
- Consistent posting frequency
- Gradual follower growth
- Regular engagement patterns
- Predictable login times

**Anomaly Detection Rules:**

```
RULE: Inactivity Spike
IF posts_in_last_30_days = 0 AND posts_in_previous_30_days > 30
THEN flag = "sudden_silence"
CONFIDENCE = 75%

RULE: Burst Activity
IF posts_in_single_day > (average_daily_posts * 10)
THEN flag = "activity_burst"
CONFIDENCE = 85%

RULE: Irregular Timing
IF posts_clustered_between(hours: 2am-5am, days: 7+) 
AND timezone_claimed != "night_owl_timezone"
THEN flag = "suspicious_timing"
CONFIDENCE = 70%
```

### Timeline Reconstruction Dork

```
site:twitter.com/username since:YYYY-MM-DD until:YYYY-MM-DD
site:reddit.com/u/username after:YYYY-MM-DD before:YYYY-MM-DD
```

---

## 2. Activity Pattern Analysis

### Baseline Establishment

**Week 1-2: Baseline Collection**
- Average posts per day
- Typical posting hours
- Common content types
- Engagement rate average

**Anomaly Thresholds:**

| Metric | Normal Range | Anomaly Threshold |
|--------|--------------|-------------------|
| Posts/day | ±30% of average | >2x or <0.1x |
| Posting time | Within usual hours | >3 hours outside |
| Content type | 80% match pattern | <50% match |
| Engagement rate | ±20% average | >2x or <0.5x |
| Follower change | ±5%/week | >15%/week |

### Pattern Recognition Algorithm

```
FOR each_week IN timeline:
    Calculate_weekly_metrics()
    
    IF weekly_posts > (baseline_avg * 2):
        anomaly_score += 25
        
    IF new_posting_hours NOT IN baseline_hours:
        anomaly_score += 15
        
    IF content_category_shift > 50%:
        anomaly_score += 20
        
    IF follower_change > 15%:
        anomaly_score += 10
        
FINAL_SCORE = anomaly_score / number_of_weeks

IF FINAL_SCORE > 50:
    FLAG = "significant_anomaly"
```

---

## 3. Geographic Inconsistency Logic

### Impossible Travel Detection

**Physics-Based Rules:**

```
MAX_TRAVEL_SPEED = 900 km/h (commercial flight average)

FOR each_login_pair IN chronological_logins:
    distance = calculate_distance(location1, location2)
    time_diff = timestamp2 - timestamp1
    
    IF distance > 100:
        required_speed = distance / time_diff
        
        IF required_speed > MAX_TRAVEL_SPEED:
            IMPOSSIBLE_TRAVEL = TRUE
            CONFIDENCE = 90%
        ELSE IF required_speed > 500:
            SUSPICIOUS_TRAVEL = TRUE
            CONFIDENCE = 70%
```

### Location Pattern Analysis

**Consistency Checks:**

| Check Type | Normal | Suspicious | Anomalous |
|------------|--------|------------|-----------|
| Primary location | 80%+ posts from 1-2 cities | 3-5 cities regularly | >10 cities, random |
| Timezone alignment | Post times match claimed TZ | 2-3 hours off | 8+ hours off |
| Language/geo | Matches location | Minor mismatch | Complete mismatch |
| IP consistency | Same ISP/region | Multiple ISPs | Global distribution |

### Geographic Dork

```
site:twitter.com username "location" OR "visited" OR "traveling"
site:instagram.com/username "checked in" OR "at"
```

---

## 4. Bot Detection Patterns

### Automation Indicators

**High-Confidence Bot Signals:**

| Indicator | Weight | Confidence |
|-----------|--------|------------|
| Posts every X minutes (exact interval) | 25 | 90% |
| Identical posts across multiple accounts | 30 | 95% |
| Only retweets, no original content | 20 | 75% |
| Always posts links, never text | 15 | 70% |
| Never responds to mentions | 10 | 60% |
| Account created during spike event | 15 | 80% |
| Default profile image + no bio | 20 | 85% |

**Bot Score Calculation:**

```
BOT_SCORE = 0

IF posting_interval_variance < 5 minutes:
    BOT_SCORE += 25
    
IF duplicate_content_across_accounts > 3:
    BOT_SCORE += 30
    
IF original_content_ratio < 5%:
    BOT_SCORE += 20
    
IF link_only_posts > 90%:
    BOT_SCORE += 15
    
IF engagement_response_rate < 1%:
    BOT_SCORE += 10
    
IF profile_completeness < 20%:
    BOT_SCORE += 20

IF BOT_SCORE >= 60:
    CLASSIFICATION = "likely_bot"
IF BOT_SCORE >= 80:
    CLASSIFICATION = "confirmed_bot"
```

### Bot Detection Dork

```
site:twitter.com "follow us" "retweet" "tag friends" (bot giveaway pattern)
site:twitter.com account created:2024-01-01 posts:>1000 (high volume new)
```

---

## 5. Unusual Connection Patterns

### Network Anomaly Detection

**Follower/Following Anomalies:**

| Pattern | Description | Risk Level |
|---------|-------------|------------|
| Ratio inversion | Following 10x followers | Medium |
| Sudden mutuals | 50+ new mutuals in 1 day | High |
| Geographic clustering | 80% connections from 1 country | Low-Medium |
| Interest divergence | Connections don't match stated interests | Medium |
| Age disparity | Only connects to very new/very old accounts | Medium |

### Connection Timeline Analysis

```
FOR each_connection:
    connection_date = extract_date(followed/following)
    
    # Detect coordinated following
    IF connections_in_1_hour > 10:
        FLAG = "coordinated_connection_spike"
    
    # Detect purchased followers
    IF connections_in_1_day > 100 AND follower_quality_score < 30:
        FLAG = "likely_purchased_followers"
        
    # Detect circle patterns
    IF connection_set_A follows connection_set_B AND vice versa:
        FLAG = "mutual_circle_pattern"
```

---

## 6. Anomaly Scoring Methodology

### Multi-Factor Anomaly Score

```
ANOMALY_SCORE = (
    timeline_anomaly * 0.30 +
    activity_anomaly * 0.25 +
    geographic_anomaly * 0.20 +
    connection_anomaly * 0.15 +
    content_anomaly * 0.10
)

# Normalize to 0-100 scale
FINAL_SCORE = min(100, ANOMALY_SCORE)
```

### Interpretation Guide

| Score Range | Classification | Action |
|-------------|----------------|--------|
| 0-25 | Normal | No action |
| 26-50 | Unusual | Monitor, low priority |
| 51-70 | Suspicious | Investigate further |
| 71-85 | Highly Anomalous | Priority investigation |
| 86-100 | Critical | Immediate attention |

### Confidence Adjustment

```
FINAL_CONFIDENCE = BASE_CONFIDENCE

# Increase confidence
+ Multiple anomaly types detected: +15%
+ Cross-platform confirmation: +10%
+ Temporal correlation: +10%

# Decrease confidence
- Single data point: -20%
- Known legitimate event (vacation, new job): -30%
- Privacy protection (VPN): -25%
```

---

## 7. False Positive Mitigation

### Common False Positives

| Anomaly | Legitimate Reason | Verification |
|---------|-------------------|--------------|
| Geographic jump | Business travel | Check for travel posts |
| Activity spike | Viral moment | Check engagement source |
| Night posting | Night shift worker | Check consistency |
| Following spike | New account growth | Check quality of connections |
| Content shift | Life event | Check for announcements |

### Validation Questions

Before flagging as anomalous:

1. **Is there a known explanation?**
   - Recent life events
   - Job changes
   - Moving/travel

2. **Is the pattern consistent?**
   - One-time vs. recurring
   - Platform-specific vs. global

3. **Can it be verified?**
   - Cross-platform check
   - Timestamp correlation
   - Content context

---

## Quick Reference: Anomaly Detection Commands

```bash
# Analyze timeline
site:twitter.com/username since:2024-01-01 until:2024-06-30

# Check for burst activity
site:twitter.com/username after:2024-01-01 before:2024-01-02

# Detect geographic patterns
site:instagram.com/username "New York" "London" "Tokyo"

# Find bot patterns
site:twitter.com "follow everyone who retweets" "tag 3 friends"

# Check connection quality
site:twitter.com/following (analyze follower/following ratios)
```

---

## Anomaly Report Template

```
ANOMALY DETECTION REPORT
========================
Subject: [username/handle]
Date Analyzed: [date]
Baseline Period: [date range]

ANOMALIES DETECTED:
1. [Type] - [Description] - Score: [X/100] - Confidence: [X%]
2. [Type] - [Description] - Score: [X/100] - Confidence: [X%]

OVERALL ANOMALY SCORE: [X/100]
CLASSIFICATION: [Normal/Unusual/Suspicious/Highly Anomalous/Critical]

VERIFICATION STEPS:
- [Step 1]
- [Step 2]
- [Step 3]

RECOMMENDATIONS:
- [Action 1]
- [Action 2]
```

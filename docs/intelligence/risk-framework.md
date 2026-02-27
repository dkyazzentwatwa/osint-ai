# Risk Framework

Comprehensive risk assessment system for OSINT investigations.

---

## 1. Risk Categories

### 1.1 Security Risk

**Definition:** Threats to digital or physical security

| Sub-Category | Indicators | Severity |
|--------------|------------|----------|
| Account compromise | Sudden behavior changes, impossible travel, password dumps | Critical |
| Malware distribution | Link patterns, suspicious attachments, reported content | Critical |
| Social engineering | Impersonation, pretexting patterns, manipulation | High |
| Credential exposure | Password reuse, plaintext storage, breach involvement | High |
| Privacy violations | Doxxing, unauthorized data sharing, stalking | High |

**Security Risk Dorks:**
```
# Check breach exposure
site:haveibeenpwned.com "username" OR "email"

# Find exposed credentials
"username" "password" site:pastebin.com OR site:ghostbin.co

# Check for malware links
site:twitter.com/username "suspicious-link-domain"
```

### 1.2 Privacy Risk

**Definition:** Threats to personal privacy

| Sub-Category | Indicators | Severity |
|--------------|------------|----------|
| Oversharing | Location data, personal details, routine exposure | Medium |
| Metadata leakage | EXIF data, document properties, timestamp exposure | Medium |
| Contact information | Phone, email, address publicly visible | Medium |
| Family exposure | Children's info, family member details | High |
| Professional crossover | Personal content on professional accounts | Medium |

**Privacy Risk Dorks:**
```
# Find phone numbers
"username" "phone" OR "call me" OR "text me"

# Find addresses
"username" "lives in" OR "address" OR "moved to"

# Find family connections
"username" "my mom" OR "my dad" OR "my wife" OR "my husband"
```

### 1.3 Reputation Risk

**Definition:** Threats to professional or personal reputation

| Sub-Category | Indicators | Severity |
|--------------|------------|----------|
| Controversial content | Offensive posts, inflammatory content, harassment | High |
| Professional misconduct | Conflicts of interest, false credentials, fraud | Critical |
| Inconsistent persona | Contradictory claims, fake achievements | Medium |
| Association risk | Links to controversial figures/groups | Medium-High |
| Content permanence | Deleted but archived content | Medium |

**Reputation Risk Dorks:**
```
# Find controversial content
site:twitter.com/username "controversial term"

# Check deleted content
site:webcache.googleusercontent.com "username"

# Find professional claims
site:linkedin.com/in/username "title" "company"
```

### 1.4 Legal Risk

**Definition:** Potential legal implications

| Sub-Category | Indicators | Severity |
|--------------|------------|----------|
| Copyright infringement | Unauthorized use, plagiarism, piracy | Medium |
| Defamation | False statements, libel, slander | High |
| Threats/intimidation | Direct threats, implied violence, harassment | Critical |
| Fraud indicators | Scam patterns, financial deception, false claims | Critical |
| Regulatory violations | Industry-specific violations, non-compliance | High |

**Legal Risk Dorks:**
```
# Find potential legal issues
"username" lawsuit OR arrested OR charged OR investigation

# Find copyright issues
"username" "not mine" OR "stolen" OR "repost"

# Check court records
site:courts.state.gov "username" OR "real name"
```

---

## 2. Risk Factor Definitions

### Risk Factor Matrix

| Factor | Category | Weight | Detection Method |
|--------|----------|--------|------------------|
| Breach exposure | Security | 0.20 | HaveIBeenPwned, paste sites |
| Credential reuse | Security | 0.15 | Pattern analysis, dumps |
| Impossible travel | Security | 0.15 | Geographic analysis |
| Oversharing | Privacy | 0.10 | Content analysis |
| Metadata exposure | Privacy | 0.08 | EXIF analysis, document review |
| Controversial content | Reputation | 0.12 | Sentiment analysis, keyword detection |
| Persona inconsistency | Reputation | 0.08 | Cross-reference validation |
| Legal mentions | Legal | 0.08 | Court records, news mentions |
| Platform violations | Legal | 0.04 | ToS violations, bans |

### Risk Factor Details

**Breach Exposure (Weight: 20%)**
```
DATA_SOURCES:
- HaveIBeenPwned
- Pastebin/dump sites
- Breach databases

SCORING:
1 breach: +20 points
2-3 breaches: +40 points
4+ breaches: +60 points
Recent breach (<1 year): +20 points
Password exposed: +30 points
```

**Credential Reuse (Weight: 15%)**
```
DETECTION:
- Same password across platforms (from dumps)
- Similar password patterns
- Security question reuse

SCORING:
Same password: +50 points
Similar pattern: +30 points
Password in breach: +40 points
```

**Impossible Travel (Weight: 15%)**
```
DETECTION:
- Login location analysis
- Post geolocation correlation
- Device fingerprinting

SCORING:
Confirmed impossible: +60 points
Suspicious pattern: +30 points
VPN suspected: +10 points
```

---

## 3. Scoring Methodology

### Base Risk Score Calculation

```
RISK_SCORE = Σ(factor_score × factor_weight)

CATEGORY_SCORES:
Security_Risk = (breach × 0.20) + (credentials × 0.15) + (travel × 0.15)
Privacy_Risk = (overshare × 0.10) + (metadata × 0.08)
Reputation_Risk = (content × 0.12) + (persona × 0.08)
Legal_Risk = (legal × 0.08) + (violations × 0.04)

TOTAL_RISK = Security_Risk + Privacy_Risk + Reputation_Risk + Legal_Risk
```

### Score Normalization

```
# Raw score is 0-100
NORMALIZED_SCORE = min(100, TOTAL_RISK)

# Category normalization
FOR each_category:
    category_normalized = (category_score / category_max) × 100
```

### Risk Level Classification

| Score Range | Risk Level | Color Code | Action Required |
|-------------|-----------|------------|-----------------|
| 0-20 | Minimal | Green | None |
| 21-40 | Low | Light Green | Monitor |
| 41-60 | Moderate | Yellow | Investigate |
| 61-80 | High | Orange | Immediate action |
| 81-100 | Critical | Red | Urgent response |

---

## 4. Risk Trend Analysis

### Trend Detection

**Improving Trend:**
```
IF current_score < previous_score × 0.8
AND duration > 30 days
THEN trend = "improving"
```

**Worsening Trend:**
```
IF current_score > previous_score × 1.3
AND new_factors > 2
THEN trend = "deteriorating"
```

**Stable Trend:**
```
IF |current_score - previous_score| < 10
AND duration > 60 days
THEN trend = "stable"
```

### Trend Velocity Calculation

```
VELOCITY = (current_score - score_30_days_ago) / 30

IF VELOCITY > 2:
    trend_rate = "rapidly deteriorating"
ELSE IF VELOCITY > 0.5:
    trend_rate = "gradually deteriorating"
ELSE IF VELOCITY > -0.5:
    trend_rate = "stable"
ELSE IF VELOCITY > -2:
    trend_rate = "gradually improving"
ELSE:
    trend_rate = "rapidly improving"
```

---

## 5. Threshold Alerts

### Alert Configuration

| Threshold | Condition | Alert Type | Recipients |
|-----------|-----------|------------|------------|
| Critical risk | Score > 80 | Immediate | Investigation lead |
| High risk | Score > 60 | Daily digest | Investigation team |
| Trend alert | Velocity > 2 | Weekly | Assigned analyst |
| Category spike | Any category > 70 | Immediate | Investigation lead |
| New factor | Previously unseen factor | Real-time | Investigation team |

### Alert Triggers

```
TRIGGER: Critical_Risk_Alert
IF TOTAL_RISK > 80:
    SEND_ALERT(
        priority: CRITICAL,
        message: "Risk score exceeds critical threshold",
        action_required: "Immediate investigation",
        factors: list_high_risk_factors()
    )

TRIGGER: Category_Spike_Alert
IF Security_Risk > 70 OR Privacy_Risk > 70 OR Reputation_Risk > 70 OR Legal_Risk > 70:
    SEND_ALERT(
        priority: HIGH,
        message: f"{category} risk exceeds threshold",
        details: get_category_details(category)
    )

TRIGGER: Trend_Alert
IF VELOCITY > 2:
    SEND_ALERT(
        priority: MEDIUM,
        message: "Risk deteriorating rapidly",
        trend: trend_rate,
        velocity: VELOCITY
    )
```

---

## 6. Mitigation Priority Ranking

### Priority Calculation

```
MITIGATION_PRIORITY = (
    risk_score × 0.4 +
    exploitability × 0.3 +
    impact × 0.2 +
    time_sensitivity × 0.1
)
```

### Exploitability Factors

| Factor | Score | Description |
|--------|-------|-------------|
| Publicly accessible | +30 | Information is public |
| No authentication | +20 | No login required |
| Automated discoverable | +25 | Can be found via search |
| Active exploitation | +40 | Currently being exploited |
| Skill required | -20 | Requires advanced skills |

### Impact Factors

| Factor | Score | Description |
|--------|-------|-------------|
| Financial impact | +30 | Monetary loss potential |
| Reputation damage | +25 | Public perception harm |
| Legal consequences | +35 | Lawsuit/prosecution risk |
| Operational impact | +20 | Business disruption |
| Personal safety | +40 | Physical harm potential |

### Mitigation Priority Matrix

| Priority | Score Range | Response Time | Resources |
|----------|-------------|---------------|-----------|
| P1 - Critical | 90-100 | < 4 hours | Full team |
| P2 - High | 70-89 | < 24 hours | Assigned team |
| P3 - Medium | 50-69 | < 1 week | Analyst |
| P4 - Low | 30-49 | < 1 month | As available |
| P5 - Minimal | 0-29 | Monitor | Automated |

---

## 7. Risk Report Template

```
RISK ASSESSMENT REPORT
======================
Subject: [Entity/Investigation Name]
Assessment Date: [Date]
Analyst: [Name]

EXECUTIVE SUMMARY
-----------------
Overall Risk Score: [X/100]
Risk Level: [Minimal/Low/Moderate/High/Critical]
Trend: [Improving/Stable/Deteriorating]

CATEGORY BREAKDOWN
------------------
├─ Security Risk: [X/100] [████████░░]
├─ Privacy Risk: [X/100] [████░░░░░░]
├─ Reputation Risk: [X/100] [██████░░░░]
└─ Legal Risk: [X/100] [██░░░░░░░░]

KEY RISK FACTORS
----------------
1. [Factor Name] - Score: [X] - Severity: [Level]
   Details: [Description]
   Evidence: [Source/Link]

2. [Factor Name] - Score: [X] - Severity: [Level]
   Details: [Description]
   Evidence: [Source/Link]

MITIGATION RECOMMENDATIONS
--------------------------
Priority 1: [Action item]
Priority 2: [Action item]
Priority 3: [Action item]

MONITORING REQUIREMENTS
-----------------------
- [Monitoring action 1]
- [Monitoring action 2]

NEXT ASSESSMENT: [Date]
```

---

## Quick Reference: Risk Assessment Commands

```bash
# Generate comprehensive risk profile
/risk-assessment [entity]
  └─ Analyzes all risk factors and generates score

# Show risk changes over time
/risk-trend [entity] [timeframe]
  └─ Displays risk score trends and velocity

# Category-specific assessment
/risk-category [entity] [security|privacy|reputation|legal]
  └─ Deep dive into specific risk category

# Compare risk profiles
/risk-compare [entity1] [entity2]
  └─ Side-by-side risk comparison
```

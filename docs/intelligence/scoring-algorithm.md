# Scoring Algorithm

Detailed scoring logic for multi-factor risk assessment (1-100 scale).

---

## 1. Multi-Factor Risk Formula

### Base Formula

```
TOTAL_RISK_SCORE = Σ(Base_Factor_Score × Factor_Weight × Context_Modifier)

WHERE:
- Base_Factor_Score: 0-100 points per factor
- Factor_Weight: 0.0-1.0 (sum of all weights = 1.0)
- Context_Modifier: 0.5-1.5 based on temporal/contextual factors
```

### Factor Weight Distribution

```
SECURITY_FACTORS (Total Weight: 0.50)
├── Data Breach Exposure: 0.20
├── Credential Security: 0.15
└── Account Security: 0.15

PRIVACY_FACTORS (Total Weight: 0.18)
├── Information Exposure: 0.10
└── Metadata Leakage: 0.08

REPUTATION_FACTORS (Total Weight: 0.20)
├── Content Analysis: 0.12
└── Persona Consistency: 0.08

LEGAL_FACTORS (Total Weight: 0.12)
├── Legal Exposure: 0.08
└── Compliance Violations: 0.04
```

---

## 2. Weight Calculations Per Factor

### Security Factors

**Data Breach Exposure (Weight: 20%)**

```python
def calculate_breach_score(entity):
    score = 0
    
    # Breach count
    breach_count = get_breach_count(entity)
    if breach_count == 0:
        score += 0
    elif breach_count == 1:
        score += 25
    elif breach_count <= 3:
        score += 45
    else:
        score += 65
    
    # Recent breach (within 1 year)
    if has_recent_breach(entity, days=365):
        score += 20
    
    # Password exposed
    if password_exposed_in_breach(entity):
        score += 15
    
    # Sensitive data exposed
    if sensitive_data_exposed(entity):
        score += 10
    
    return min(100, score)
```

**Credential Security (Weight: 15%)**

```python
def calculate_credential_score(entity):
    score = 0
    
    # Password reuse across platforms
    reuse_count = detect_password_reuse(entity)
    score += min(50, reuse_count * 10)
    
    # Weak password patterns
    if has_weak_password_pattern(entity):
        score += 25
    
    # Security question exposure
    if security_questions_exposed(entity):
        score += 15
    
    # 2FA status
    if not has_2fa(entity):
        score += 10
    
    return min(100, score)
```

**Account Security (Weight: 15%)**

```python
def calculate_account_security_score(entity):
    score = 0
    
    # Impossible travel incidents
    impossible_travel = count_impossible_travel(entity)
    score += min(40, impossible_travel * 20)
    
    # Suspicious login patterns
    if has_suspicious_logins(entity):
        score += 25
    
    # Session anomalies
    if has_session_anomalies(entity):
        score += 20
    
    # Device fingerprint changes
    if rapid_device_changes(entity):
        score += 15
    
    return min(100, score)
```

### Privacy Factors

**Information Exposure (Weight: 10%)**

```python
def calculate_exposure_score(entity):
    score = 0
    
    # Personal information found
    if phone_number_public(entity):
        score += 20
    if home_address_public(entity):
        score += 25
    if email_public(entity):
        score += 15
    if family_info_public(entity):
        score += 20
    
    # Routine exposure
    if routine_exposed(entity):
        score += 15
    
    # Location history
    location_posts = count_location_posts(entity)
    score += min(20, location_posts * 2)
    
    return min(100, score)
```

**Metadata Leakage (Weight: 8%)**

```python
def calculate_metadata_score(entity):
    score = 0
    
    # EXIF data in images
    if images_have_exif(entity):
        score += 30
    
    # Document metadata
    if documents_have_metadata(entity):
        score += 25
    
    # Timestamp analysis
    if precise_timestamps_exposed(entity):
        score += 20
    
    # Software/hardware fingerprints
    if device_info_exposed(entity):
        score += 25
    
    return min(100, score)
```

### Reputation Factors

**Content Analysis (Weight: 12%)**

```python
def calculate_content_score(entity):
    score = 0
    
    # Controversial content
    controversial = analyze_content_sentiment(entity)
    score += min(40, controversial * 20)
    
    # Harassment indicators
    if harassment_detected(entity):
        score += 30
    
    # Offensive language patterns
    if offensive_language_detected(entity):
        score += 20
    
    # False information
    if misinformation_detected(entity):
        score += 25
    
    # Deleted content (attempted cover-up)
    if mass_deletion_detected(entity):
        score += 15
    
    return min(100, score)
```

**Persona Consistency (Weight: 8%)**

```python
def calculate_persona_score(entity):
    score = 0
    
    # Contradictory claims
    contradictions = find_contradictions(entity)
    score += min(35, contradictions * 15)
    
    # False credentials
    if credentials_unverified(entity):
        score += 30
    
    # Inconsistent biographical details
    if bio_inconsistencies(entity):
        score += 20
    
    # Timeline impossibilities
    if timeline_contradictions(entity):
        score += 15
    
    return min(100, score)
```

### Legal Factors

**Legal Exposure (Weight: 8%)**

```python
def calculate_legal_score(entity):
    score = 0
    
    # Court record mentions
    if court_records_found(entity):
        score += 35
    
    # Legal threats received
    if legal_threats_documented(entity):
        score += 25
    
    # Copyright violations
    if copyright_issues(entity):
        score += 20
    
    # Defamation indicators
    if defamation_detected(entity):
        score += 30
    
    # Regulatory mentions
    if regulatory_actions(entity):
        score += 25
    
    return min(100, score)
```

**Compliance Violations (Weight: 4%)**

```python
def calculate_compliance_score(entity):
    score = 0
    
    # Platform ToS violations
    if tos_violations(entity):
        score += 40
    
    # Industry regulation violations
    if regulatory_violations(entity):
        score += 35
    
    # Data protection violations
    if privacy_violations(entity):
        score += 25
    
    return min(100, score)
```

---

## 3. Context Modifiers

### Temporal Modifiers

| Condition | Modifier | Rationale |
|-----------|----------|-----------|
| Recent incident (< 30 days) | ×1.3 | Higher urgency |
| Ongoing pattern | ×1.2 | Active threat |
| Historical (> 2 years) | ×0.8 | Aged out |
| Resolved/mitigated | ×0.5 | No longer active |
| Seasonal spike | ×1.1 | Temporary elevation |

### Entity Type Modifiers

| Entity Type | Modifier | Rationale |
|-------------|----------|-----------|
| High-profile individual | ×1.2 | Higher exposure |
| Corporate account | ×1.1 | Business impact |
| Anonymous/pseudonymous | ×0.9 | Lower direct risk |
| Verified public figure | ×1.3 | Reputation sensitive |
| Minor/child | ×1.4 | Vulnerable population |

### Platform Modifiers

| Platform Type | Modifier | Rationale |
|---------------|----------|-----------|
| Professional (LinkedIn) | ×1.2 | Career impact |
| Personal (Instagram) | ×1.0 | Standard risk |
| Anonymous (Reddit) | ×0.8 | Lower attribution |
| Ephemeral (Snapchat) | ×0.7 | Temporary content |
| Professional + Personal | ×1.3 | Cross-contamination |

### Modifier Application

```python
def apply_context_modifiers(base_score, entity, context):
    modifier = 1.0
    
    # Temporal modifier
    if context.has_recent_incident(days=30):
        modifier *= 1.3
    elif context.is_historical(years=2):
        modifier *= 0.8
    
    # Entity type modifier
    if entity.type == "high_profile":
        modifier *= 1.2
    elif entity.type == "anonymous":
        modifier *= 0.9
    
    # Platform modifier
    if context.platform == "professional":
        modifier *= 1.2
    elif context.platform == "anonymous":
        modifier *= 0.8
    
    return min(100, base_score * modifier)
```

---

## 4. Comparative Benchmarking

### Industry Benchmarks

| Industry | Average Risk | High Risk Threshold |
|----------|-------------|---------------------|
| Technology | 35 | 55 |
| Finance | 40 | 60 |
| Healthcare | 45 | 65 |
| Legal | 30 | 50 |
| Entertainment | 50 | 70 |
| Government | 35 | 55 |
| Education | 30 | 50 |

### Platform Benchmarks

| Platform | Average Risk | High Risk Threshold |
|----------|-------------|---------------------|
| LinkedIn | 25 | 45 |
| Twitter/X | 40 | 60 |
| Instagram | 35 | 55 |
| Facebook | 30 | 50 |
| Reddit | 45 | 65 |
| TikTok | 45 | 65 |

### Percentile Ranking

```python
def calculate_percentile(score, benchmark_data):
    """Calculate percentile rank within comparable entities"""
    sorted_scores = sorted(benchmark_data)
    position = bisect_left(sorted_scores, score)
    percentile = (position / len(sorted_scores)) * 100
    return percentile

# Interpretation
# 90th percentile = Higher risk than 90% of similar entities
# 50th percentile = Average risk for entity type
# 10th percentile = Lower risk than 90% of similar entities
```

---

## 5. Score Interpretation Guide

### Overall Score Interpretation

| Score | Classification | Interpretation | Recommended Action |
|-------|----------------|----------------|-------------------|
| 0-15 | Minimal | Excellent security posture | Routine monitoring |
| 16-30 | Low | Good posture, minor issues | Quarterly review |
| 31-45 | Low-Moderate | Acceptable with gaps | Bi-annual assessment |
| 46-60 | Moderate | Concerning patterns | Monthly monitoring |
| 61-75 | High | Significant issues | Weekly review |
| 76-90 | Very High | Critical vulnerabilities | Daily monitoring |
| 91-100 | Critical | Immediate threat | Emergency response |

### Category Score Interpretation

| Category | Low | Moderate | High | Critical |
|----------|-----|----------|------|----------|
| Security | 0-25 | 26-50 | 51-75 | 76-100 |
| Privacy | 0-20 | 21-40 | 41-60 | 61-100 |
| Reputation | 0-25 | 26-50 | 51-70 | 71-100 |
| Legal | 0-20 | 21-40 | 41-65 | 66-100 |

---

## 6. Example Calculations

### Example 1: Low Risk Profile

```
Entity: Standard professional user

Factor Scores:
├── Breach Exposure: 10 (no breaches)
├── Credential Security: 25 (weak password, no 2FA)
├── Account Security: 5 (no issues)
├── Info Exposure: 20 (some personal info public)
├── Metadata Leakage: 15 (some EXIF data)
├── Content Analysis: 10 (clean content)
├── Persona Consistency: 5 (consistent)
├── Legal Exposure: 0 (no issues)
└── Compliance: 5 (minor ToS issues)

Weighted Calculation:
Security: (10×0.20) + (25×0.15) + (5×0.15) = 2.0 + 3.75 + 0.75 = 6.5
Privacy: (20×0.10) + (15×0.08) = 2.0 + 1.2 = 3.2
Reputation: (10×0.12) + (5×0.08) = 1.2 + 0.4 = 1.6
Legal: (0×0.08) + (5×0.04) = 0 + 0.2 = 0.2

TOTAL: 6.5 + 3.2 + 1.6 + 0.2 = 11.5

CONTEXT: Professional platform, stable history
Modifier: 1.0

FINAL SCORE: 11.5 → 12 (Minimal Risk)
```

### Example 2: High Risk Profile

```
Entity: Controversial public figure

Factor Scores:
├── Breach Exposure: 60 (3 breaches, 1 recent)
├── Credential Security: 45 (password reuse, no 2FA)
├── Account Security: 40 (impossible travel detected)
├── Info Exposure: 55 (significant doxxing)
├── Metadata Leakage: 30 (frequent EXIF exposure)
├── Content Analysis: 70 (controversial posts, harassment)
├── Persona Consistency: 50 (multiple contradictions)
├── Legal Exposure: 35 (court mentions, threats)
└── Compliance: 40 (multiple ToS violations)

Weighted Calculation:
Security: (60×0.20) + (45×0.15) + (40×0.15) = 12 + 6.75 + 6 = 24.75
Privacy: (55×0.10) + (30×0.08) = 5.5 + 2.4 = 7.9
Reputation: (70×0.12) + (50×0.08) = 8.4 + 4.0 = 12.4
Legal: (35×0.08) + (40×0.04) = 2.8 + 1.6 = 4.4

TOTAL: 24.75 + 7.9 + 12.4 + 4.4 = 49.45

CONTEXT: Recent controversy, high-profile
Modifier: 1.3

FINAL SCORE: 49.45 × 1.3 = 64.3 → 64 (High Risk)
```

### Example 3: Critical Risk Profile

```
Entity: Compromised corporate account

Factor Scores:
├── Breach Exposure: 80 (5+ breaches, passwords exposed)
├── Credential Security: 75 (widespread reuse)
├── Account Security: 90 (confirmed compromise)
├── Info Exposure: 70 (corporate data leaked)
├── Metadata Leakage: 45 (internal docs exposed)
├── Content Analysis: 60 (malicious posts)
├── Persona Consistency: 80 (attacker behavior)
├── Legal Exposure: 55 (regulatory issues)
└── Compliance: 65 (data protection violations)

Weighted Calculation:
Security: (80×0.20) + (75×0.15) + (90×0.15) = 16 + 11.25 + 13.5 = 40.75
Privacy: (70×0.10) + (45×0.08) = 7.0 + 3.6 = 10.6
Reputation: (60×0.12) + (80×0.08) = 7.2 + 6.4 = 13.6
Legal: (55×0.08) + (65×0.04) = 4.4 + 2.6 = 7.0

TOTAL: 40.75 + 10.6 + 13.6 + 7.0 = 71.95

CONTEXT: Active compromise, corporate account
Modifier: 1.4

FINAL SCORE: 71.95 × 1.4 = 100.7 → 100 (Critical Risk)
```

---

## Quick Reference: Score Commands

```bash
# Calculate risk score
/calculate-risk [entity]
  └─ Returns detailed breakdown with all factors

# Show factor breakdown
/score-factors [entity]
  └─ Shows individual factor scores

# Compare to benchmark
/score-compare [entity] [benchmark_type]
  └─ Compares to industry/platform averages

# Simulate score changes
/score-simulate [entity] --mitigate [factor]
  └─ Shows how score would improve with mitigation
```

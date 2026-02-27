# Pattern Library

Comprehensive pattern definitions for OSINT investigations.

---

## 1. Username Obfuscation Patterns

### Common Character Substitutions

| Pattern | Variation Type | Example |
|---------|---------------|---------|
| `0` → `o` | Zero substitution | j0hn_doe |
| `1` → `l/i` | One substitution | j0hn_d0e |
| `3` → `e` | Three substitution | j3nnif3r |
| `4` → `a` | Four substitution | m4rk |
| `5` → `s` | Five substitution | j5on |
| `7` → `t` | Seven substitution | chris7 |
| `@` → `a` | At substitution | @lex |
| `$` → `s` | Dollar substitution | jame$ |
| `!` → `i/l` | Exclamation substitution | m!ke |

### Google Dork for Leet Detection

```
intext:"j0hn" OR intext:"j0hnn" OR intext:"johnn" site:instagram.com
intext:"j0hn_doe" OR intext:"john_d0e" OR intext:"j0hn_d0e"
```

### Username Variation Generator

Base: `johndoe`

**Level 1 - Simple additions:**
- johndoe1, johndoe2, johndoe123
- thejohndoe, realjohndoe, officialjohndoe
- johndoe_official, johndoe_real

**Level 2 - Separators:**
- john_doe, john.doe, john-doe
- johndoe_, _johndoe, _john_doe

**Level 3 - Modifiers:**
- johndoe2024, johndoe2023, johndoe2022
- mrjohndoe, johndoesmith, johnnydoe

**Level 4 - Leet/Obfuscation:**
- j0hnd0e, j0hn_d0e, j0hndoe
- johnd03, j0hndoe123

---

## 2. Temporary Email Domains

### High-Risk Temporary Email Providers

| Domain Type | Examples |
|-------------|----------|
| 10-minute mail | 10minutemail.com, mailinator.com, guerrillamail.com |
| Disposable | tempmail.com, throwawaymail.com, getairmail.com |
| Burner | burnermail.io, temp-mail.org, fakemail.net |
| Forwarding | anonaddy.com, simplelogin.io, firefoxrelay.com |
| Free anonymous | protonmail (anonymous signups), tutanota, mailfence |

### Detection Patterns

```
email domain contains: temp, disposable, throwaway, 10minute, burner
email pattern: randomstring@tempdomain.com
username: 8-12 random alphanumeric characters
```

### Google Dork for Temp Email Discovery

```
intext:"@mailinator.com" OR intext:"@guerrillamail.com" OR intext:"@10minutemail.com"
site:haveibeenpwned.com "temp" OR "disposable"
```

---

## 3. Suspicious Activity Patterns

### Activity Gap Detection

**Definition:** Unusual periods of inactivity followed by sudden activity

**Indicators:**
- Account dormant > 6 months, then active
- Posting frequency: < 1/month → > 10/day
- Login location changes after long gap

**Google Dork:**
```
site:twitter.com/johndoe since:2020-01-01 until:2020-06-30
site:twitter.com/johndoe since:2023-01-01 (check for gap)
```

### Activity Spike Detection

**Definition:** Sudden dramatic increase in posting frequency

**Scoring:**
| Spike Multiplier | Risk Level |
|------------------|------------|
| 3x average | Low |
| 5x average | Medium |
| 10x average | High |
| 50x+ average | Critical |

**Temporal Context:**
- Spike during off-hours (3am local time)
- Coordinated posting across platforms
- Identical/similar content across accounts

---

## 4. Geographic Impossibility Detection

### Detection Logic

**Impossible Travel:** Login locations that require impossible travel speeds

| Scenario | Detection Rule |
|----------|----------------|
| Same account | 2 locations > 500km apart within 1 hour |
| Same account | 3+ countries in 24 hours |
| Same account | Timezone impossible sequence |

### Location Verification Patterns

**Self-Reported vs. IP-based:**
- Profile says "New York" but posts geotagged "London"
- Consistent timezone vs. claimed location
- Language patterns inconsistent with location

### Google Dork for Location Analysis

```
site:instagram.com/johndoe "New York" "London"
site:twitter.com/johndoe geocode:40.7128,-74.0060,10km
```

---

## 5. Platform Presence Gaps

### Cross-Platform Analysis Patterns

**Normal Presence:**
- Consistent username across major platforms
- Similar bio/description
- Profile creation dates within reasonable timeframe

**Suspicious Patterns:**

| Pattern | Indicators |
|---------|-----------|
| Platform cherry-picking | Only on platforms with weak verification |
| Account age disparity | Twitter 2010, Instagram 2024 |
| Selective deletion | Reddit posts deleted, Twitter preserved |
| Platform avoidance | No LinkedIn despite professional claims |

### Detection Query

```
"johndoe" (site:twitter.com OR site:instagram.com OR site:linkedin.com)
Compare creation dates, activity patterns
```

---

## 6. Email Pattern Recognition

### Catch-All Detection

**Indicators:**
- Any username@domain reaches a mailbox
- Subdomain variations work (test@sub.domain.com)
- No bounce-back on invalid addresses

**Test Pattern:**
```
Send to: random12345@targetdomain.com
Send to: nonexistent@targetdomain.com
Both deliver = catch-all enabled
```

### Email Forwarding Detection

**Patterns:**
- Email headers show forwarding chain
- Reply-to differs from from-address
- Multiple Received headers from different domains

**Google Dork:**
```
intext:"forwarded message" from:johndoe@company.com
intext:"original message" intext:"forwarded by"
```

### Pattern Recognition Matrix

| Pattern Type | Confidence | False Positive Risk |
|--------------|------------|---------------------|
| Character substitution | 85% | Medium - common legit use |
| Temp email domain | 95% | Low - clear indicator |
| Activity gap | 70% | High - legitimate reasons |
| Geographic impossibility | 90% | Low - unless VPN use |
| Catch-all email | 80% | Medium - business standard |
| Platform gap | 60% | High - personal preference |

---

## 7. Pattern Confidence Ratings

### Confidence Calculation

```
Base Confidence: 50%
+ Multiple indicators match: +20%
+ Cross-platform verification: +15%
+ Temporal correlation: +10%
- Single data point: -20%
- Possible legitimate reason: -15%
- No corroborating evidence: -25%
```

### False Positive Mitigation

**Username Obfuscation:**
- Cross-reference with account creation context
- Check if variation exists on other platforms
- Verify posting patterns match primary account

**Temp Email:**
- Verify domain is actually disposable
- Check if used for legitimate privacy
- Context matters (signup vs. long-term use)

**Geographic:**
- Consider VPN/proxy usage
- Business travel patterns
- Multiple device usage

---

## Quick Reference: Pattern Detection Dorks

```
# Username variations
"j0hn" OR "johnn" OR "john_doe" site:twitter.com

# Temp emails
intext:"@tempmail" OR intext:"@mailinator" site:pastebin.com

# Geographic inconsistencies
site:instagram.com "location:New York" "London" "johndoe"

# Activity spikes
site:twitter.com/johndoe since:2024-01-01 until:2024-01-31
site:twitter.com/johndoe since:2024-02-01 until:2024-02-28 (compare counts)

# Email patterns
intext:"forwarded" intext:"original message" "johndoe"
```

# Breach Intelligence Module

> **Module ID:** BREACH-INTEL-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Compromised Data Analysis

---

## 1. Overview

Breach intelligence involves detecting and analyzing compromised data from security incidents. This module covers breach database searches, paste site monitoring, and exposure assessment.

**Educational Purpose Only:** This module teaches awareness and verification techniques. Never use compromised data for unauthorized access.

---

## 2. HaveIBeenPwned Search Patterns

### 2.1 HIBP Service Overview

HaveIBeenPwned (HIBP) is a free service tracking data breaches:
- Email address search
- Username search
- Domain search (for administrators)
- Password search (k-anonymity model)
- Breach notification service

### 2.2 Search Methods

**Email Address Search:**
```
URL: https://haveibeenpwned.com/
API: https://haveibeenpwned.com/api/v3/breachedaccount/{account}

Process:
1. Enter email address
2. Service checks against breach database
3. Returns list of breaches containing email
4. Does NOT return actual password/data
```

**Username Search:**
```
Some breaches contain usernames without emails
Check both: username@domain.com and standalone username

Pattern: https://haveibeenpwned.com/unifiedsearch/{username}
```

**Domain Search (Admin Only):**
```
Requires: Domain ownership verification
Useful for: Organization breach assessment
Returns: All breached addresses @domain.com
```

### 2.3 Understanding HIBP Results

**Breach Entry Format:**
```
Breach Name: LinkedIn
Date: May 2016
Compromised Accounts: 164,611,595
Compromised Data: Email addresses, Passwords
Description: Professional networking platform breach
```

**Data Classes to Note:**
```
- Email addresses
- Usernames
- Passwords (hashed or plaintext)
- Names
- Phone numbers
- Physical addresses
- IP addresses
- Dates of birth
- Security questions/answers
- Credit cards (rare, usually redacted)
```

### 2.4 Interpreting Exposure Severity

| Data Type | Risk Level | Recommended Action |
|-----------|-----------|-------------------|
| Email only | LOW | Monitor for phishing |
| Email + hashed password | MEDIUM | Change password, enable 2FA |
| Email + plaintext password | HIGH | Change password immediately, check reuse |
| Full PII | CRITICAL | Credit monitoring, identity protection |
| Financial data | CRITICAL | Bank notification, card replacement |

---

## 3. Paste Site Monitoring

### 3.1 Paste Site Landscape

**Major Paste Sites:**
| Site | URL | Content Types |
|------|-----|---------------|
| Pastebin | pastebin.com | General dumps, code |
| GitHub Gist | gist.github.com | Code, configs |
| Ghostbin | ghostbin.co | Anonymous pastes |
| PrivateBin | various instances | Encrypted pastes |
| Hastebin | hastebin.com | Code sharing |
| Paste.ee | paste.ee | General purpose |

### 3.2 Paste Site Search Strategies

**Search Patterns:**
```
"target@domain.com"
"domain.com" password
"target-username" dump
"company name" breach
leak "email domain"
```

**Google Dorking for Pastes:**
```
site:pastebin.com "target@example.com"
site:pastebin.com intext:"password" "example.com"
site:pastebin.com "database dump" "company name"
site:pastebin.com intitle:"leak" "example.com"
```

### 3.3 Paste Content Indicators

**What to Look For:**
```
Email:password combinations
Username:hash pairs
Database dumps
Configuration files
API keys and tokens
Internal documents
Credential lists
```

**Format Patterns:**
```
Standard formats:
  email@domain.com:password
  username:password
  email@domain.com:$hash$hashvalue
  "email","password","name"
```

### 3.4 Automated Monitoring

**HIBP Paste Monitoring:**
```
HIBP monitors major paste sites automatically
Sign up for notifications on haveibeenpwned.com
API available for programmatic access
```

**Manual Monitoring Strategy:**
```
1. Set up Google Alerts for patterns
2. Check major paste sites weekly
3. Search for organization domain
4. Search for key personnel emails
5. Monitor security researcher posts
```

---

## 4. Breach Data Pattern Recognition

### 4.1 Common Breach Formats

**SQL Dump Format:**
```sql
INSERT INTO `users` VALUES 
(1,'john.doe','john@example.com','$2y$10$hash...'),
(2,'jane.smith','jane@example.com','$2y$10$hash...');
```

**CSV Format:**
```csv
id,username,email,password_hash,created_at
1,johndoe,john@example.com,5f4dcc3b5aa765d61d8327deb882cf99,2020-01-15
```

**JSON Format:**
```json
{
  "users": [
    {"id": 1, "email": "john@example.com", "password": "hash123"},
    {"id": 2, "email": "jane@example.com", "password": "hash456"}
  ]
}
```

### 4.2 Hash Type Identification

| Hash Pattern | Type | Cracking Difficulty |
|-------------|------|-------------------|
| `5f4dcc3b5aa765d61d8327deb882cf99` | MD5 | Very Easy |
| `b109f3bbbc244eb82441917ed06d618b` | SHA1 | Easy |
| `$2y$10$...` | bcrypt | Very Hard |
| `$argon2id$...` | Argon2 | Very Hard |
| `$6$rounds=5000$...` | SHA512-crypt | Hard |
| `{SHA256}...` | LDAP SHA256 | Moderate |
| `0x0100...` | MSSQL | Moderate |

### 4.3 Breach Source Clues

**Database-Specific Signatures:**
```
MySQL:      INSERT INTO, backtick quoted names
PostgreSQL: COPY FROM, double quoted names
MongoDB:    JSON format with ObjectId
SQL Server: TOP keyword, square brackets
Oracle:     FROM DUAL, TO_DATE functions
```

---

## 5. Credential Exposure Indicators

### 5.1 Password Reuse Indicators

**High-Risk Patterns:**
```
Same password across multiple breached services
Common password patterns (Password123, Company2024!)
Username as password or base
Email local-part as password
```

### 5.2 Exposure Timeline Construction

**Building the Timeline:**
```
Step 1: List all known breaches for target
Step 2: Note breach dates (when breach occurred)
Step 3: Note disclosure dates (when made public)
Step 4: Check for credential overlap
Step 5: Identify window of exposure

Example Timeline:
2016-05: LinkedIn breach (email + hashed password)
2017-08: Target changes LinkedIn password
2019-01: Credential stuffing attack using old LinkedIn password
→ Password reuse vulnerability identified
```

### 5.3 Credential Stuffing Risk

**Attack Pattern:**
```
1. Attacker obtains breach database
2. Extracts email:password pairs
3. Tests same credentials on other services
4. Successful when user reuses passwords
```

**Risk Assessment:**
```
HIGH RISK indicators:
- Same password in multiple breaches
- Simple/common passwords
- No 2FA on important accounts
- Old passwords never changed
```

---

## 6. Leak Severity Assessment

### 6.1 Severity Matrix

| Factor | Low (1) | Medium (2) | High (3) | Critical (4) |
|--------|---------|------------|----------|--------------|
| **Data Sensitivity** | Emails only | Names + emails | Full PII | Financial + SSN |
| **Password State** | Not included | Strong hash | Weak hash | Plaintext |
| **Account Type** | Forum account | Social media | Email provider | Banking |
| **Verification** | Unverified | Partially verified | Verified | Official confirmation |
| **Current Relevance** | 5+ years old | 2-5 years old | <2 years old | <6 months old |

**Score Calculation:**
```
Total = Data + Password + Account + Verification + Age

5-8:   Low severity - Monitor
9-12:  Medium severity - Review and update
13-16: High severity - Immediate action required
17-20: Critical severity - Full incident response
```

### 6.2 Impact Assessment Questions

**For Individuals:**
```
1. What accounts use the same password?
2. Are security questions compromised?
3. Is 2FA enabled on critical accounts?
4. What services use the breached email?
5. Can attackers reset other passwords with this info?
```

**For Organizations:**
```
1. How many employee emails are exposed?
2. Are corporate credentials reused?
3. What systems use breached passwords?
4. Is there evidence of credential stuffing?
5. What data was in the compromised system?
```

---

## 7. Breach Timeline Construction

### 7.1 Timeline Components

```
Discovery Timeline:
├── Breach Occurs (unknown date)
├── Data Harvested (breach to sale period)
├── First Appearance (dark web/paste sites)
├── Public Disclosure (company announcement)
├── HIBP Inclusion (service adds to database)
└── Target Notification (user learns of breach)
```

### 7.2 Timeline Research Methods

**Step 1: Identify Initial Breach Date**
```
Sources:
- Company disclosure notice
- Security researcher reports
- Dark web monitoring services
- Forum post dates
```

**Step 2: Track Data Movement**
```
Indicators:
- Paste site upload dates
- Dark web marketplace listings
- Forum discussions
- Researcher disclosures
```

**Step 3: Map Exposure Duration**
```
Calculation:
Exposure Window = Discovery Date - Breach Date

Example:
Breach: 2023-03-15
Discovered: 2023-09-20
Exposure Window: 6 months
```

---

## 8. Commands Reference

### /breach-check [email]

**Purpose:** Check email against known breach databases
**Input:** Email address or username
**Process:**
1. Search HaveIBeenPwned
2. Check paste site references
3. Analyze exposure timeline
4. Assess severity
**Output:** Breach exposure report

### /exposure-timeline [identifier]

**Purpose:** Build breach history timeline
**Input:** Email, username, or domain
**Process:**
1. Compile all known breaches
2. Order chronologically
3. Identify patterns
4. Assess cumulative risk
**Output:** Timeline visualization and risk assessment

---

## 9. Ethical Guidelines

### 9.1 Permitted Uses

✅ **Educational:** Understanding breach landscape  
✅ **Self-Assessment:** Checking personal exposure  
✅ **Authorized:** Organization security assessment  
✅ **Research:** Academic security research  
✅ **Incident Response:** Legitimate security work  

### 9.2 Prohibited Uses

❌ **Unauthorized Access:** Using credentials without permission  
❌ **Harassment:** Targeting individuals with breach data  
❌ **Identity Theft:** Using PII for fraudulent purposes  
❌ **Extortion:** Threatening disclosure of private data  
❌ **Commercial Exploitation:** Selling breached data  

### 9.3 Responsible Disclosure

**If you discover a breach:**
1. Do not download or distribute data
2. Notify affected organization privately
3. Allow reasonable time for response
4. Follow coordinated disclosure practices
5. Report to relevant authorities if required

---

## 10. Limitations

### 10.1 Data Limitations

1. **Not all breaches are public** - Many never disclosed
2. **Breach databases incomplete** - Only known breaches indexed
3. **Paste sites ephemeral** - Content deleted quickly
4. **False positives exist** - Test data, hoaxes, old data
5. **Attribution difficult** - Source of breach often unknown

### 10.2 Search Limitations

1. **Recent breaches may not be indexed yet**
2. **Niche/small breaches may be missed**
3. **Private/dark web sources inaccessible**
4. **API rate limits restrict bulk searches**
5. **Some services require payment for deep searches**

---

*Breach Intelligence Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*  
*For authorized security awareness and educational purposes only*

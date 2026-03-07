# Example Investigation: Domain Security Audit (v2.0)

> Canonical example: this file in `examples/` is the source of truth. `docs/examples/domain-security-audit.md` is a navigation mirror that should link here rather than maintain a second long-form copy.

## Example Navigation

- Canonical path: [`examples/domain-security-audit.md`](./domain-security-audit.md)
- Docs mirror: [`docs/examples/domain-security-audit.md`](../docs/examples/domain-security-audit.md)
- Related examples: [`examples/person-investigation.md`](./person-investigation.md) and [`examples/username-correlation.md`](./username-correlation.md)

## Target: example-startup.com

**Scenario:** You're a security consultant doing due diligence before a client invests in a startup. This example shows v2.0 features including templates, risk assessment, visualization, and remediation workflows.

---

## Method 1: Using Security Audit Template (Fastest)

**Command:**
```
/template run security-audit
```

**Interactive Session:**
```
Agent: Running Security Audit Template
This will perform a 5-step comprehensive security assessment.

Step 1/5: Enter target domain
You: example-startup.com

Step 2/5: Include subdomain enumeration? (yes/no)
You: yes

Step 3/5: Check for credential leaks? (yes/no)
You: yes

Step 4/5: Generate remediation report? (yes/no)
You: yes

Step 5/5: Select report format:
  [1] Executive (for C-suite)
  [2] Technical (for security team)
  [3] Both
You: 3

Agent: Starting security audit...
[████████████████████] 100%

Template execution complete!
✓ Domain reconnaissance
✓ Subdomain enumeration (7 found)
✓ Security dorking (12 tests)
✓ Credential leak check
✓ Risk assessment
✓ Remediation report

Critical findings: 4
High findings: 3
Medium findings: 2
Low findings: 5

Risk Score: 72/100 (HIGH)

Reports generated:
- example-startup-executive-report.md
- example-startup-technical-report.md
```

---

## Method 2: Full Security Sweep

**Command:**
```
/full example-startup.com
```

**Results:**
```
=== COMPREHENSIVE SECURITY INVESTIGATION ===
Target: example-startup.com
Assessment Type: External Security Reconnaissance
Started: 2026-02-27 15:45:22

⚠️  HIGH RISK DETECTED (72/100)

PHASE 1: Reconnaissance ✓
  Entities: 12 (domains, IPs, services)
  Technologies: AWS, Node.js, MongoDB
  
PHASE 2: Security Analysis ✓
  Tests performed: 18
  Critical issues: 4
  High issues: 3
  Medium issues: 2
  
PHASE 3: Pivot Analysis ✓
  Related domains: 7 subdomains
  Connected entities: GitHub org, S3 buckets
  
PHASE 4: Timeline ✓
  Security events: 3 breaches (HaveIBeenPwned)
  
PHASE 5: Reports ✓
  Technical report generated
  Simple report generated

CRITICAL ACTIONS REQUIRED:
🔴 Exposed credentials in GitHub
🔴 Open S3 bucket with customer data
🔴 Publicly accessible staging environment
🔴 Unprotected API documentation
```

---

## Method 3: Step-by-Step Professional Assessment

### Step 1: Initial Domain Recon

**Command:**
```
/recon example-startup.com
```

**Results:**
```
🟢 HIGH: Main website operational (example-startup.com)
🟢 HIGH: Found LinkedIn company page (50 employees)
🟡 MEDIUM: ~50 employees on LinkedIn
🔴 LOW: Mentioned in 2 tech blog articles
🟢 HIGH: GitHub organization exists
🟢 HIGH: Active social media presence

Initial Assessment: Legitimate business entity
```

### Step 2: Risk Assessment

**Command:**
```
/risk-assessment
```

**Results:**
```
=== SECURITY RISK ASSESSMENT ===
Target: example-startup.com
Overall Risk Score: 72/100 (HIGH) ⚠️

Risk Breakdown:
┌─────────────────┬───────┬────────┐
│ Category        │ Score │ Level  │
├─────────────────┼───────┼────────┤
│ Security        │ 85    │ HIGH   │ 🔴
│ Privacy         │ 65    │ HIGH   │ 🟡
│ Reputation      │ 60    │ MODERATE│ 🟡
│ Legal           │ 80    │ HIGH   │ 🔴
└─────────────────┴───────┴────────┘

Security Issues:
🔴 Exposed credentials (GitHub)
🔴 Open S3 bucket
🔴 Public staging environment
🟡 Exposed API docs
🟡 Outdated software versions

Privacy Issues:
🟡 Customer data exposed (CSV)
🟡 Employee emails harvestable

Legal/Compliance:
🔴 Potential GDPR violation (exposed customer data)
🔴 No security headers on main site

CRITICAL: Immediate action required
HIGH: Address within 30 days
```

### Step 3: Deep Dork Analysis

**Command:**
```
/dork example-startup.com
```

**Results:**

#### Exposed Documents
```
site:example-startup.com filetype:pdf
  └── 🟡 FOUND: /reports/2024-q1-financial.pdf
      Location: example-startup.com/reports/
      Size: 2.3MB
      Exposure: Financial data publicly accessible
      
  └── 🟡 FOUND: /presentations/investor-deck-2023.pdf
      Location: example-startup.com/presentations/
      Size: 15MB
      Exposure: Investor information, roadmap
      
site:example-startup.com filetype:xls OR filetype:csv
  └── 🔴 FOUND: /data/customer-list-backup.csv
      Location: example-startup.com/data/customer-list-backup.csv
      Size: 45KB
      Exposure: CUSTOMER DATA BREACH
      Contents: Names, emails, phone numbers (estimated 500+ records)
      GDPR/CCPA Violation: YES
```

#### Admin Panels & Login Pages
```
site:example-startup.com inurl:admin OR inurl:login
  └── 🟢 No exposed admin panels on main domain
  
site:example-startup.com inurl:dashboard OR inurl:portal
  └── 🔴 FOUND: staging.example-startup.com/dashboard
      Status: 200 OK (accessible)
      Authentication: NONE
      Data visible: Production customer data
      Risk: CRITICAL
      
  └── 🔴 FOUND: dev.example-startup.com/admin
      Status: 200 OK
      Authentication: Basic auth (easily bypassed)
      Risk: HIGH
```

#### API Endpoints
```
site:example-startup.com inurl:api OR inurl:swagger OR inurl:docs
  └── 🟡 FOUND: api.example-startup.com/v1/
      Status: Active
      Authentication: API key required (but docs exposed)
      
  └── 🔴 FOUND: api.example-startup.com/v1/docs (Swagger UI)
      Status: Publicly accessible
      Authentication: NONE
      Endpoints documented: 47
      Includes: User management, payment processing, admin functions
      Risk: CRITICAL - Attackers can map entire API surface
```

#### Configuration Files
```
site:example-startup.com ext:env OR ext:config OR ext:yaml OR ext:yml
  └── 🟢 No exposed config files on main domain
  
site:example-startup.com ext:sql OR ext:bak OR ext:dump
  └── 🔴 FOUND: dev.example-startup.com/db-backup-2024-01.sql
      Size: 2.1GB
      Exposure: FULL DATABASE DUMP
      Contains: Customer data, passwords (hashed), payment info
      Risk: CRITICAL
```

#### Cloud Storage
```
site:s3.amazonaws.com "example-startup"
  └── 🔴 FOUND: s3.amazonaws.com/example-startup-backups/
      Status: PUBLIC (listable)
      Contents:
        - customer-backups/ (500MB)
        - financial-records/ (2GB)
        - employee-data/ (150MB)
      Risk: CRITICAL
      
  └── 🔴 FOUND: s3.amazonaws.com/example-startup-logs/
      Status: PUBLIC
      Contents: Application logs with user data
      Risk: HIGH
```

### Step 4: Subdomain Enumeration

**Command:**
```
/dork site:*.example-startup.com
```

**Results:**
```
SUBDOMAIN ENUMERATION
=====================

🟢 www.example-startup.com (main site)
   Status: 200 OK
   Server: nginx/1.18.0
   
🟢 app.example-startup.com (customer portal)
   Status: 200 OK
   Authentication: Required
   Security: HTTPS valid
   
🔴 staging.example-startup.com (staging env)
   Status: 200 OK
   Authentication: NONE
   Exposure: Production data visible
   Risk: CRITICAL
   
🔴 dev.example-startup.com (development)
   Status: 200 OK
   Authentication: Weak
   Exposure: Debug mode enabled
   Risk: HIGH
   
🟢 api.example-startup.com (API)
   Status: 200 OK
   Rate limiting: No
   Documentation: Publicly exposed
   Risk: HIGH
   
🔴 legacy.example-startup.com (old site)
   Status: 200 OK
   Maintenance: None
   Known vulnerabilities: 12 (outdated CMS)
   Risk: HIGH
   
🟡 blog.example-startup.com (blog)
   Status: 200 OK
   Platform: WordPress 5.8 (outdated)
   Risk: MEDIUM
```

### Step 5: GitHub Investigation

**Command:**
```
Search: site:github.com/example-startup
```

**Results:**
```
GITHUB SECURITY ANALYSIS
========================

🟢 Organization: github.com/example-startup
   Public repositories: 12
   Private repositories: 3 (estimated)
   
🔴 Repository: example-startup/legacy-api
   Status: Public
   Issues found:
   
   1. database-credentials.json
      Path: /config/database-credentials.json
      Content: {"host": "prod-db...", "user": "admin", "pass": "SuperSecret123!"}
      Committed: 2024-01-15
      Still present: YES
      Risk: CRITICAL
      
   2. .env file
      Path: /.env
      Content: API keys, AWS credentials, JWT secrets
      Committed: 2024-02-01
      Still present: YES
      Risk: CRITICAL
      
   3. docker-compose.yml
      Path: /docker-compose.yml
      Issue: Hardcoded passwords
      Risk: HIGH

🟡 Repository: example-startup/mobile-app
   Status: Public
   Issues found:
   - API endpoints hardcoded
   - Debug mode enabled in production build
   Risk: MEDIUM

⚠️ CREDENTIAL EXPOSURE SUMMARY:
   - Database passwords: 1
   - API keys: 3
   - AWS credentials: 2
   - JWT secrets: 1
   
   All credentials are PRODUCTION credentials.
   Immediate rotation required.
```

### Step 6: Breach & Leak Check

**Command:**
```
/breach-check admin@example-startup.com
```

**Results:**
```
BREACH INTELLIGENCE
===================

Email: admin@example-startup.com
Status: ⚠️ COMPROMISED

Breach History:
1. HaveIBeenPwned - LinkedIn (2012)
   Date: 2012-05-05
   Data: Email, password (hashed)
   
2. HaveIBeenPwned - Dropbox (2012)
   Date: 2012-07-01
   Data: Email, password (hashed)

3. Custom breach - StartupDB (2024)
   Date: 2024-11-12
   Data: Email, password (plaintext!), phone
   Source: HaveIBeenPwned

Password Reuse Risk: HIGH
Recommendation: Force password reset, enable 2FA
```

**Command:**
```
/leak-search "example-startup.com"
```

**Results:**
```
LEAK SEARCH RESULTS
===================

Pastebin:
🟢 No direct mentions

GitHub Gists:
🟡 1 mention in technical discussion (neutral)

Dark web monitoring (via search):
🔴 3 mentions on hacker forums
   - Database dump being sold (2024-12)
   - API credentials shared (2024-11)
   - Employee list leaked (2024-10)

Conclusion: Company has been targeted by attackers
```

### Step 7: Visualization

**Command:**
```
/visualize risk
```

**Results:**
```
RISK HEATMAP
============

                  Low   Med   High  Crit
Security         [  ]  [  ]  [##]  [##]  🔴
Privacy          [  ]  [##]  [##]  [  ]  🟡
Reputation       [  ]  [##]  [  ]  [  ]  🟡
Legal            [  ]  [  ]  [  ]  [##]  🔴
Operational      [  ]  [##]  [  ]  [  ]  🟡

Legend: [  ] None  [==] Present  [##] Critical
```

**Command:**
```
/visualize network
```

**Results:**
```
INFRASTRUCTURE NETWORK
======================

example-startup.com (MAIN)
├── www (Production)
│   └── ✅ Secure
├── app (Customer Portal)
│   └── ✅ Secure
├── api (API Server)
│   └── ⚠️  Public docs exposed
├── staging (Staging)
│   └── 🔴 NO AUTHENTICATION
├── dev (Development)
│   └── 🔴 Weak security
├── legacy (Old Site)
│   └── 🔴 12 vulnerabilities
└── blog (WordPress)
    └── 🟡 Outdated

External Services:
├── GitHub
│   └── 🔴 Credentials exposed
├── AWS S3
│   ├── 🔴 Backups bucket public
│   └── 🔴 Logs bucket public
└── MongoDB
    └── 🔴 Direct exposure via dev site
```

### Step 8: Dashboard Overview

**Command:**
```
/dashboard
```

**Results:**
```
╔════════════════════════════════════════════════╗
║  SECURITY AUDIT: example-startup.com           ║
╠════════════════════════════════════════════════╣
║ Risk Score: 72/100          Level: HIGH 🔴     ║
║ Critical Issues: 4                             ║
║ Duration: 28 minutes                           ║
║ Status: CRITICAL ACTION REQUIRED               ║
╠════════════════════════════════════════════════╣
║ Critical Findings:                             ║
║ 🔴 Exposed database credentials               ║
║ 🔴 Open S3 bucket (customer data)             ║
║ 🔴 Public staging environment                 ║
║ 🔴 Database dump accessible                   ║
╠════════════════════════════════════════════════╣
║ Quick Actions:                                 ║
║ [1] View remediation plan [2] Export report   ║
║ [3] Check compliance      [4] Save session    ║
╚════════════════════════════════════════════════╝
```

### Step 9: Generate Comprehensive Reports

**Command:**
```
/report
```

**Output:**
```markdown
=== SECURITY ASSESSMENT REPORT ===
Target: example-startup.com
Date: 2026-02-27
Classification: CONFIDENTIAL
Risk Rating: 72/100 (HIGH)

EXECUTIVE SUMMARY:
Multiple critical security vulnerabilities identified
through external reconnaissance. Immediate action 
required to prevent data breach and regulatory penalties.

CRITICAL FINDINGS (Fix within 24 hours):
🔴 GitHub repository contains exposed credentials
   Location: github.com/example-startup/legacy-api
   Files: database-credentials.json, .env
   Impact: Full database access, AWS account compromise
   
🔴 Open S3 bucket exposing customer data
   Location: s3.amazonaws.com/example-startup-backups/
   Data: 500+ customer records, financial data
   Impact: GDPR violation, $20M potential fine
   
🔴 Staging environment publicly accessible
   URL: staging.example-startup.com/dashboard
   Auth: None required
   Data: Production customer data visible
   Impact: Data breach, compliance violation
   
🔴 Database dump publicly accessible
   URL: dev.example-startup.com/db-backup-2024-01.sql
   Size: 2.1GB
   Impact: Complete data compromise

HIGH FINDINGS (Fix within 7 days):
🟡 API documentation exposed without authentication
🟡 Development environment accessible
🟡 Legacy site with 12 known vulnerabilities
🟡 Outdated WordPress installation (blog)

COMPLIANCE IMPLICATIONS:
- GDPR Article 32 violation (security measures)
- Potential fines: up to €20M or 4% annual revenue
- Required breach notification: within 72 hours
- Customer notification: may be required

REMEDIATION ROADMAP:

Phase 1 - Immediate (24 hours):
1. Rotate ALL exposed credentials
2. Secure S3 buckets (make private)
3. Remove database dump from web
4. Add authentication to staging

Phase 2 - Short term (7 days):
1. Implement API authentication
2. Secure development environment
3. Update WordPress and plugins
4. Remove or secure legacy site

Phase 3 - Long term (30 days):
1. Implement security scanning in CI/CD
2. Conduct penetration test
3. Security awareness training
4. Implement DLP solution
```

**Command:**
```
/simple-report
```

**Output:**
```markdown
=== SIMPLE SECURITY SUMMARY ===
THE BOTTOM LINE:
This startup has serious security problems that need 
immediate attention. Their customer data is exposed 
on the internet where anyone can access it.

WHAT WE FOUND (The Bad News):
🔴 Database passwords are publicly visible on GitHub
🔴 Customer data is in an open cloud storage bucket
🔴 Anyone can access their test website with real data
🔴 A full copy of their database is downloadable
🔴 API documentation is public (hackers love this)

WHAT THIS MEANS:
This is like leaving your office unlocked with customer
files sitting on the front desk. Hackers could:
• Steal all customer information
• Access the company's bank accounts
• Impersonate employees
• Shut down their systems

LEGAL PROBLEMS:
They may have violated data privacy laws (GDPR), which
could result in fines up to $20 million.

RISK LEVEL: HIGH (72/100)

WHAT THEY SHOULD DO RIGHT NOW:
1. Change ALL passwords immediately (today!)
2. Lock down the cloud storage buckets
3. Delete that database copy from the website
4. Add passwords to the test environment
5. Hire a security professional to review everything

WHAT THIS MEANS FOR INVESTORS:
High risk. Do not invest until these issues are fixed
and verified by an independent security audit.

TIMELINE TO FIX:
• Critical issues: 24 hours
• High issues: 7 days
• Full remediation: 30 days
```

### Step 10: Save Investigation

**Command:**
```
/save example-startup-security-audit
```

**Results:**
```
✓ Investigation saved successfully

Name: example-startup-security-audit
Date: 2026-02-27
Duration: 28 minutes
Entities: 12
Risk Score: 72/100 (HIGH)
Critical Findings: 4
Reports: 2 generated

Files saved:
- example-startup-technical-report.md
- example-startup-simple-report.md
- investigation-state.json
```

---

## Key Security Issues Summary

| Issue | Severity | Location | Evidence |
|-------|----------|----------|----------|
| Exposed credentials in GitHub | Critical | github.com/.../legacy-api | database-credentials.json |
| Open S3 bucket | Critical | s3.amazonaws.com/...backups/ | Public customer data |
| Database dump accessible | Critical | dev.example.../db-backup.sql | 2.1GB of data |
| Public staging environment | Critical | staging.example.../dashboard | No authentication |
| API docs exposed | High | api.example.../v1/docs | Swagger UI public |
| Dev environment accessible | High | dev.example... | Weak security |
| Customer CSV exposed | High | /data/customer-list.csv | 500+ records |
| Legacy site vulnerabilities | High | legacy.example... | 12 CVEs |
| Outdated WordPress | Medium | blog.example... | WP 5.8 (EOL) |

---

## Investment Impact Assessment

### Risk Factors
- ⚠️ Potential $20M GDPR fine
- ⚠️ Data breach could destroy customer trust
- ⚠️ Regulatory compliance issues
- ⚠️ Technical debt in legacy systems

### Due Diligence Questions
1. When was the last security audit?
2. Do they have a CISO or security team?
3. Are they aware of the exposed S3 bucket?
4. Have they had previous breaches?
5. What cyber insurance do they carry?
6. What's their incident response plan?

### Recommendation
**DO NOT INVEST** until:
- All critical issues resolved
- Independent security audit completed
- Compliance review passed
- 30-day monitoring period completed

---

## Techniques Demonstrated

| Feature | Command | v2.0 Feature |
|---------|---------|--------------|
| Template execution | `/template run` | ✅ New |
| Risk assessment | `/risk-assessment` | ✅ New |
| Full sweep | `/full` | ✅ Enhanced |
| Visualization | `/visualize risk/network` | ✅ New |
| Dashboard | `/dashboard` | ✅ New |
| Breach check | `/breach-check` | ✅ New |
| Leak search | `/leak-search` | ✅ New |
| Session save | `/save` | ✅ New |
| Dual reports | `/report` + `/simple-report` | ✅ Enhanced |

---

**This investigation demonstrates v2.0's professional-grade security assessment capabilities.**

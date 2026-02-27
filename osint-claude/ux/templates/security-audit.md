# Domain Security Audit Template

A comprehensive security assessment workflow for domain infrastructure.

---

## Template Overview

**Purpose:** Assess domain security posture and identify vulnerabilities  
**Use Cases:** Security assessments, due diligence, compliance checks, vendor evaluation  
**Duration:** 10-20 minutes  
**Complexity:** Intermediate to Advanced  
**Output:** Security score with remediation roadmap  

### Required Information

- Domain name (required)
- Permission to scan (recommended/ethical)
- Scope definition (optional - subdomains, depth)

---

## Step-by-Step Workflow

### Step 1: Reconnaissance

**Objective:** Gather basic domain information and infrastructure overview

**WHOIS and Registration:**
```
/recon {{domain}} --whois
/dork "{{domain}}" whois OR registration
```

**DNS Enumeration:**
```
/recon {{domain}} --dns
/dns-lookup {{domain}} --all-records
/dork site:{{domain}} DNS OR nameserver
```

**IP and Hosting Discovery:**
```
/recon {{domain}} --hosting
/host-info {{domain}}
/ip-lookup {{domain}}
```

**Expected Findings:**

| Check | Information Gathered | Risk Indicator |
|-------|---------------------|----------------|
| WHOIS | Registrar, dates, contacts | Privacy protection off |
| DNS | Nameservers, MX records | Vulnerable DNS servers |
| IP | Hosting provider, geolocation | High-risk hosting |
| CDN | Content delivery network | None (missing CDN) |

**Reconnaissance Risk Flags:**
```
🔴 Critical:
  • Registration expired or expiring soon
  • No privacy protection on personal domains
  • Suspicious registrar (known for abuse)

🟡 Warning:
  • DNS servers in single location
  • Self-hosted nameservers
  • Recently transferred registration

🟢 Good:
  • Long registration history
  • Reputable registrar
  • DNS redundancy
```

---

### Step 2: Subdomain Discovery

**Objective:** Map all subdomains and entry points

**Subdomain Enumeration:**
```
/recon {{domain}} --subdomains
/subdomain-scan {{domain}} --comprehensive
/dork site:{{domain}} -www -mail -ftp
```

**Common Subdomain Checks:**
```
# Standard prefixes
www.{{domain}}
mail.{{domain}}
ftp.{{domain}}
admin.{{domain}}
test.{{domain}}
dev.{{domain}}
staging.{{domain}}

# Service subdomains
api.{{domain}}
app.{{domain}}
blog.{{domain}}
shop.{{domain}}
support.{{domain}}
cdn.{{domain}}
```

**Wildcard Detection:**
```
/wildcard-check {{domain}}
# Tests if *.domain.com responds
```

**Expected Findings:**

| Subdomain Type | Security Relevance | Risk Level |
|----------------|-------------------|------------|
| Admin panels | Management interfaces | High if exposed |
| Test/staging | Often less secure | High if discoverable |
| API endpoints | Data access points | Variable |
| Legacy systems | Outdated software | High |
| Development | Incomplete security | High |

**Subdomain Risk Assessment:**
```
Risk Factors:
  +5 points: Admin panel without auth
  +5 points: Test environment exposed
  +3 points: Development server public
  +3 points: Database admin interface
  +2 points: Unused subdomain pointing to IP
  +1 point: Each wildcard response

Score:
  0-5: Low risk
  6-15: Medium risk
  16+: High risk
```

---

### Step 3: Dork Execution Sequence

**Objective:** Find exposed information and potential vulnerabilities

**Information Disclosure Dorks:**
```
# Configuration files
/dork site:{{domain}} ext:env OR ext:config OR ext:ini
/dork site:{{domain}} (config.json OR .env OR web.config)

# Backup files
/dork site:{{domain}} ext:bak OR ext:backup OR ext:old
/dork site:{{domain}} (backup.zip OR dump.sql OR .git)

# Log files
/dork site:{{domain}} ext:log filetype:log
/dork site:{{domain}} intitle:"index of" "logs"

# Database exposure
/dork site:{{domain}} ext:sql OR ext:db OR ext:sqlite
/dork site:{{domain}} "phpmyadmin" OR "adminer"
```

**Sensitive Document Dorks:**
```
# Documents with metadata
/dork site:{{domain}} filetype:pdf "author"
/dork site:{{domain}} filetype:doc OR filetype:docx
/dork site:{{domain}} filetype:xls OR filetype:xlsx

# Email/communication
/dork site:{{domain}} filetype:eml OR filetype:msg
/dork site:{{domain}} "@{{domain}}" filetype:txt
```

**Vulnerability Indicator Dorks:**
```
# Common vulnerable paths
/dork site:{{domain}} inurl:admin OR inurl:login OR inurl:wp-admin
/dork site:{{domain}} inurl:phpmyadmin OR inurl:webmail
/dork site:{{domain}} inurl:api OR inurl:swagger OR inurl:graphql

# Error messages
/dork site:{{domain}} "error" "sql syntax" OR "mysql" OR "database"
/dork site:{{domain}} "warning" "failed to open stream"
/dork site:{{domain}} intitle:"error" "stack trace"
```

**Dork Results Prioritization:**

| Priority | Finding | Action |
|----------|---------|--------|
| P0 | Database dump exposed | Immediate alert |
| P0 | Admin panel unprotected | Immediate alert |
| P1 | Source code (.git) exposed | Urgent |
| P1 | Backup files accessible | Urgent |
| P2 | Config files with credentials | High priority |
| P2 | Error messages revealing structure | High priority |
| P3 | Documents with metadata | Medium priority |
| P3 | Directory listing enabled | Low priority |

---

### Step 4: Technology Fingerprinting

**Objective:** Identify technologies and versions for vulnerability assessment

**Technology Detection:**
```
/tech-detect {{domain}}
/wappalyzer {{domain}}
/recon {{domain}} --technologies
```

**Version Identification:**
```
# Web server version
/recon {{domain}} --server-version

# Framework/CMS detection
/dork site:{{domain}} "powered by" OR "generator"
/dork site:{{domain}} wp-content OR wp-includes

# JavaScript libraries
/analyze-js {{domain}}
```

**Third-Party Services:**
```
/dork site:{{domain}} analytics OR tracking
/recon {{domain}} --third-party
```

**Technology Risk Matrix:**

| Technology | Version Check | Known Vulnerabilities |
|------------|---------------|----------------------|
| Web server (Apache/Nginx) | Version exposed? | CVE database check |
| CMS (WordPress/Drupal) | Core version | WPScan/drupal-check |
| Framework (React/Vue) | Detect version | NPM security audit |
| Database (MySQL/Postgres) | Port exposed? | Default credentials? |
| Server language (PHP/Python) | Version exposed? | Known CVEs |

---

### Step 5: Vulnerability Assessment

**Objective:** Identify exploitable security weaknesses

**Header Security Analysis:**
```
/security-headers {{domain}}

Check for:
  ✓ Content-Security-Policy
  ✓ X-Frame-Options
  ✓ X-Content-Type-Options
  ✓ Strict-Transport-Security
  ✓ X-XSS-Protection
  ✓ Referrer-Policy
```

**SSL/TLS Assessment:**
```
/ssl-check {{domain}}
/certificate-info {{domain}}

Check:
  ✓ Certificate validity
  ✓ Protocol versions (TLS 1.2+)
  ✓ Cipher strength
  ✓ Certificate transparency
  ✓ Expiration date
```

**Common Vulnerability Checks:**
```
# Injection vulnerabilities
/vuln-check {{domain}} --sqli
/vuln-check {{domain}} --xss
/vuln-check {{domain}} --lfi

# Authentication issues
/vuln-check {{domain}} --auth-bypass
/vuln-check {{domain}} --session-management

# Configuration issues
/vuln-check {{domain}} --security-headers
/vuln-check {{domain}} --cors-misconfig
```

**Vulnerability Severity Scale:**

| Severity | CVSS Score | Example | Response Time |
|----------|------------|---------|---------------|
| Critical | 9.0-10.0 | SQL injection, RCE | Immediate |
| High | 7.0-8.9 | Auth bypass, LFI | 24 hours |
| Medium | 4.0-6.9 | XSS, CSRF | 7 days |
| Low | 0.1-3.9 | Info disclosure | 30 days |
| Info | 0.0 | Best practice | As scheduled |

---

### Step 6: Risk Scoring

**Objective:** Calculate overall security posture score

**Scoring Categories:**

```
Category Weights:
  Infrastructure: 20%
  Exposure: 25%
  Technology: 20%
  Vulnerabilities: 25%
  Response: 10%
```

**Infrastructure Score (20%):**
```
DNS Security:
  ✓ DNSSEC enabled: +20 points
  ✓ Multiple nameservers: +10 points
  ✓ Redundant DNS providers: +10 points
  ✗ Single nameserver: -20 points
  ✗ Vulnerable DNS software: -30 points

Hosting Security:
  ✓ CDN protection: +15 points
  ✓ WAF enabled: +15 points
  ✓ DDoS protection: +10 points
  ✗ Shared hosting: -10 points
  ✗ Known bad host: -40 points
```

**Exposure Score (25%):**
```
Information Disclosure:
  ✗ Database exposed: -50 points
  ✗ Admin panel public: -40 points
  ✗ Source code accessible: -30 points
  ✗ Backup files found: -25 points
  ✗ Config files exposed: -20 points
  ✗ Error messages detailed: -10 points
  ✗ Directory listing: -5 points

Subdomain Security:
  ✗ Test env public: -20 points
  ✗ Dev server exposed: -15 points
  ✗ Admin subdomain: -10 points
```

**Technology Score (20%):**
```
Software Currency:
  ✓ All software current: +30 points
  ⚠ Minor versions behind: +15 points
  ✗ Major versions behind: -20 points
  ✗ End-of-life software: -40 points

Security Features:
  ✓ Security headers complete: +25 points
  ✓ Modern TLS only: +20 points
  ✓ HSTS enabled: +10 points
  ✗ Missing critical headers: -15 each
  ✗ Weak TLS versions: -20 points
```

**Vulnerability Score (25%):**
```
Active Vulnerabilities:
  ✗ Critical vuln unpatched: -50 points
  ✗ High vuln unpatched: -30 points
  ✗ Medium vuln unpatched: -15 points
  ✗ Low vuln unpatched: -5 points

Historical Incidents:
  ✗ Breach in last year: -40 points
  ✗ Breach in last 3 years: -20 points
  ✓ No known breaches: +10 points
```

**Response Score (10%):**
```
Security Responsiveness:
  ✓ Security contact listed: +10 points
  ✓ Bug bounty program: +15 points
  ✓ Responsible disclosure: +10 points
  ✗ No security contact: -10 points
  ✗ Hostile to researchers: -20 points
```

**Final Score Calculation:**
```
Total possible: 100 points
Score = Sum of all category scores

Grade Scale:
  90-100: A (Excellent)
  80-89:  B (Good)
  70-79:  C (Fair)
  60-69:  D (Poor)
  <60:    F (Critical)
```

---

### Step 7: Remediation Recommendations

**Objective:** Provide actionable fixes for identified issues

**Critical Issues (Fix Immediately):**
```
🚨 CRITICAL - Immediate Action Required

1. [ISSUE: Database publicly accessible]
   RISK: Complete data compromise
   FIX: Restrict database to localhost/VPN only
   EFFORT: 30 minutes
   GUIDE: /guide secure-database

2. [ISSUE: Admin panel without authentication]
   RISK: Full system compromise
   FIX: Implement strong auth + IP restrictions
   EFFORT: 1 hour
   GUIDE: /guide secure-admin-panel
```

**High Priority (Fix This Week):**
```
⚠️ HIGH PRIORITY - Fix Within 7 Days

1. [ISSUE: Outdated CMS with known vulnerabilities]
   RISK: Site compromise via known exploits
   FIX: Update to latest version
   EFFORT: 2-4 hours
   GUIDE: /guide cms-update

2. [ISSUE: Missing security headers]
   RISK: XSS, clickjacking attacks
   FIX: Add CSP, X-Frame-Options headers
   EFFORT: 1 hour
   GUIDE: /guide security-headers
```

**Medium Priority (Fix This Month):**
```
📋 MEDIUM PRIORITY - Fix Within 30 Days

1. [ISSUE: Information disclosure in error messages]
   RISK: System information leakage
   FIX: Configure generic error pages
   EFFORT: 2 hours

2. [ISSUE: Subdomain takeover risk]
   RISK: Subdomain hijacking
   FIX: Audit and remove unused DNS entries
   EFFORT: 1 hour
```

**Remediation Roadmap:**
```
Security Roadmap for {{domain}}

Week 1 (Critical):
  ☐ Secure exposed database
  ☐ Implement admin authentication
  ☐ Remove exposed backups

Week 2 (High Priority):
  ☐ Update CMS and plugins
  ☐ Implement security headers
  ☐ Configure WAF rules

Month 1 (Medium Priority):
  ☐ Fix error handling
  ☐ Audit subdomains
  ☐ Implement monitoring

Month 2-3 (Ongoing):
  ☐ Security audit schedule
  ☐ Penetration testing
  ☐ Staff security training
```

---

## Report Generation

### Report Sections

```
1. Executive Summary
   • Overall security grade
   • Critical findings count
   • Remediation priority

2. Infrastructure Assessment
   • DNS configuration
   • Hosting analysis
   • Network topology

3. Attack Surface
   • Subdomain inventory
   • Exposed services
   • Entry point analysis

4. Findings Detail
   • Vulnerability listings
   • Evidence/screenshots
   • Severity ratings

5. Remediation Guide
   • Prioritized fix list
   • Step-by-step instructions
   • Effort estimates

6. Appendices
   • Technical data
   • Tool outputs
   • Methodology
```

### Export Commands

```
/report security-audit
  → Interactive report builder

/report export pdf --executive
  → 1-page summary for leadership

/report export technical
  → Full technical details for IT

/report export remediation
  → Action items only

/report export csv
  → Vulnerability tracking spreadsheet
```

---

## Template Execution

### Run Command

```
/template run security-audit

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Domain Security Audit
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This template performs a comprehensive security assessment
of a domain and its infrastructure.

⚠️ IMPORTANT: Only scan domains you own or have explicit
    permission to test. Unauthorized scanning may violate
    laws and terms of service.

Target domain: _

Scope options:
  [ ] Include subdomain enumeration
  [ ] Check for exposed files
  [ ] Test security headers
  [ ] SSL/TLS assessment

Depth:
  (•) Standard (10-15 minutes)
  ( ) Deep (30-45 minutes)
  ( ) Quick (3-5 minutes)

[/ ethical-guidelines ] [/ begin ] [/ cancel ]
```

---

## Ethical Guidelines

### Responsible Disclosure

```
⚠️ ETHICAL USE REQUIRED

You must:
  ✓ Have permission to scan the target
  ✓ Use findings responsibly
  ✓ Report vulnerabilities appropriately
  ✓ Protect discovered data

You must NOT:
  ✗ Exploit found vulnerabilities
  ✗ Access systems without authorization
  ✗ Share findings publicly without consent
  ✗ Use for malicious purposes

Found a vulnerability?
  1. Document your findings
  2. Contact the organization privately
  3. Allow reasonable time for fix
  4. Never demand payment for disclosure

Legal notice: Unauthorized access to computer systems
is illegal in most jurisdictions.
```

# Domain Audit Wizard

A guided step-by-step workflow for auditing domain security.

---

## Wizard Overview

**Purpose:** Guide users through comprehensive domain security assessment  
**Use Cases:** Website security checks, pre-purchase due diligence, compliance  
**Duration:** 10-15 minutes  
**Complexity:** Intermediate  
**Output:** Security score with prioritized fixes  

### When to Use

- Before purchasing a website
- Monthly security checkups
- After security incidents
- Compliance requirements
- Vendor security assessments

---

## Step 1: Target Definition & Scope

### Question 1a: Domain to Audit

**System asks:**
```
Step 1 of 6: Domain Security Audit

What domain would you like to audit?

Enter the domain name (e.g., example.com)
Domain: _
```

**Expected User Input:**
- Domain name (with or without www)
- Validates format automatically

**System Validates:**
```
Validating domain format...
✓ Format valid: example.com
✓ Domain resolves to IP: 203.0.113.45
✓ Web server responds

Target confirmed. Proceeding...
```

---

### Question 1b: Audit Scope

**System asks:**
```
What should this audit include?

Scope options:
  1. Quick check (5 min) - Basic security only
     • SSL certificate
     • Security headers
     • Basic info exposure

  2. Standard audit (10-15 min) - Recommended
     • All quick checks
     • Subdomain discovery
     • Technology detection
     • Common vulnerability checks

  3. Deep audit (30-45 min) - Comprehensive
     • All standard checks
     • Extended subdomain scan
     • Directory enumeration
     • Advanced vulnerability testing

Select scope (1, 2, or 3): _
```

**Expected User Input:**
- 1, 2, or 3

**Scope Determines:**

| Scope | Duration | Checks Included |
|-------|----------|-----------------|
| Quick | 5 min | SSL, headers, basic exposure |
| Standard | 10-15 min | + Subdomains, tech, common vulns |
| Deep | 30-45 min | + Full enumeration, advanced tests |

---

### Question 1c: Permission Confirmation

**System asks:**
```
⚠️ IMPORTANT: Permission Check

You are about to scan: example.com

Do you have permission to security test this domain?
  • You own it ✓
  • You work for the organization ✓
  • Client authorized the test ✓
  • Public bug bounty program ✓
  • No explicit permission ✗

Type: yes (I have permission) or no (uncertain)
```

**Expected User Input:**
- yes / no

**If No:**
```
Without explicit permission, security scanning may:
  • Violate terms of service
  • Be illegal in some jurisdictions
  • Result in your IP being blocked
  • Cause legal issues

Recommendation:
  1. Get written permission first
  2. Check if they have a bug bounty program
  3. Use external reconnaissance only

Continue anyway? (not recommended) (yes/no)
```

---

## Step 2: Infrastructure Discovery

### System Performs:

```
Step 2 of 6: Infrastructure Discovery

[Scanning... ████████░░] 80%

Discovering:
  • Domain registration details
  • DNS configuration
  • IP addresses and hosting
  • CDN and cloud services
  • Nameserver setup
```

### Results Presentation

**Registration Information:**
```
📋 Domain Registration

Domain: example.com
Registrar: Namecheap, Inc.
Registered: March 15, 2018
Expires: March 15, 2025 (⚠️ Expires in 45 days!)
Last updated: January 10, 2024
Status: Active

Registrant: Privacy protection enabled ✓
Nameservers:
  • ns1.cloudflare.com
  • ns2.cloudflare.com

⚠️ NOTE: Domain expires soon. Renew to avoid takeover.
```

**DNS Configuration:**
```
🌐 DNS Records

A Record:     203.0.113.45
AAAA Record:  2001:db8::1
MX Records:   5 mail.example.com
              10 backup-mail.example.com
TXT Records:  "v=spf1 include:_spf.google.com ~all"
              "google-site-verification=abc123"
CNAME:        www → example.com

DNSSEC: ✓ Enabled
CDN: ✓ CloudFlare detected
```

**Hosting Analysis:**
```
🖥️ Hosting Information

IP Address: 203.0.113.45
Hosting Provider: Amazon Web Services (AWS)
Region: us-east-1 (Virginia)
Server Type: EC2 instance
Web Server: nginx/1.24.0

Reverse DNS: ec2-203-0-113-45.compute-1.amazonaws.com

Geographic Distribution:
  ✓ Multi-region setup detected
  ✓ CDN edge locations: 23 worldwide
```

---

### Question 2: Infrastructure Review

**System asks:**
```
Infrastructure Discovery Complete

Findings summary:
  ✓ Domain properly registered
  ✓ DNSSEC enabled (good security)
  ✓ Privacy protection on
  ⚠️ Domain expires in 45 days
  ✓ Using reputable CDN
  ✓ Multi-region hosting

Any concerns about this infrastructure setup? (yes/no)
```

**Expected User Input:**
- yes / no
- Optional: specific concerns

---

## Step 3: Security Headers & SSL

### System Performs:

```
Step 3 of 6: Security Configuration Check

[Testing... ██████░░░░] 60%

Checking:
  • SSL/TLS certificate validity
  • Security headers
  • HTTPS enforcement
  • Protocol versions
  • Cipher suites
```

### SSL/TLS Results

```
🔒 SSL/TLS Assessment

Certificate:
  Status: ✓ Valid
  Issuer: Let's Encrypt Authority X3
  Valid from: Dec 15, 2023
  Expires: Mar 15, 2024 (89 days)
  Type: Domain Validated (DV)
  SANs: example.com, www.example.com

Protocol Support:
  ✓ TLS 1.3
  ✓ TLS 1.2
  ✗ TLS 1.1 (disabled - good!)
  ✗ TLS 1.0 (disabled - good!)
  ✗ SSLv3 (disabled - good!)

Cipher Strength:
  ✓ Only strong ciphers enabled
  ✓ Forward secrecy supported
  ✓ HSTS enabled (max-age=31536000)

Grade: A+ (Excellent)
```

### Security Headers Analysis

```
🛡️ Security Headers

Header                          Status      Value
─────────────────────────────────────────────────────────
Strict-Transport-Security       ✓ Present   max-age=31536000
Content-Security-Policy         ✓ Present   default-src 'self'
X-Frame-Options                 ✓ Present   DENY
X-Content-Type-Options          ✓ Present   nosniff
Referrer-Policy                 ✓ Present   strict-origin
Permissions-Policy              ✗ Missing   -
X-XSS-Protection                ✓ Present   1; mode=block

Score: 6/7 headers present
Grade: A (Very Good)
```

**Missing Headers Impact:**
```
⚠️ Missing: Permissions-Policy

Risk: Low
Impact: Browser features not restricted
Fix: Add Permissions-Policy header
Effort: 5 minutes

Example:
  Permissions-Policy: camera=(), microphone=()
```

---

### Question 3: Security Configuration

**System asks:**
```
Security Configuration Results:

SSL/TLS: A+ (Excellent)
Headers: A (Very Good - 1 minor missing)

Overall security posture: STRONG

This domain has good security fundamentals.

Do you want to see detailed fix instructions
for the missing header? (yes/no)
```

---

## Step 4: Exposure Assessment

### System Performs:

```
Step 4 of 6: Information Exposure Check

[Scanning... ████████░░] 80%

Searching for:
  • Exposed configuration files
  • Backup files
  • Directory listings
  • Sensitive documents
  • Admin interfaces
  • Source code repositories
```

### Exposure Results

**Clean Result:**
```
✓ Exposure Check Results

Good news! No significant exposures found.

Checked for:
  ✗ .env files - None found
  ✗ .git directories - None found
  ✗ Backup files (.bak, .old) - None found
  ✗ Config files exposed - None found
  ✗ Directory indexing - Not enabled
  ✗ Database interfaces - Not exposed
  ✗ Admin panels - Not publicly accessible

Information exposure risk: LOW
```

**Issues Found:**
```
⚠️ Exposure Check Results

Issues discovered:

🟡 MEDIUM RISK:
  1. Backup file exposed
     URL: example.com/config.php.bak
     Risk: May contain database credentials
     Fix: Remove backup files from web root

  2. Directory listing enabled
     URL: example.com/assets/
     Risk: Exposes file structure
     Fix: Disable directory indexing in server config

🔵 LOW RISK:
  3. Robots.txt reveals paths
     Found: /admin/, /api/, /internal/
     Risk: Attackers know where to look
     Fix: Don't list sensitive paths

Total exposure score: 3.5/10 (Medium)
```

---

### Question 4: Exposure Review

**System asks:**
```
Found [N] potential exposures.

Severity breakdown:
  • Critical: [0]
  • High: [0]
  • Medium: [2]
  • Low: [1]

Would you like:
  1. Detailed remediation guide
  2. Quick fix commands
  3. Continue to next step
  4. Export findings now

Select (1-4): _
```

---

## Step 5: Technology & Vulnerability Assessment

### System Performs:

```
Step 5 of 6: Technology Detection

[Analyzing... ██████░░░░] 60%

Identifying:
  • CMS and frameworks
  • JavaScript libraries
  • Server software
  • Third-party services
  • Known vulnerabilities
```

### Technology Stack

```
💻 Technology Stack

Server-Side:
  Web Server: nginx 1.24.0 ✓ (current)
  Language: PHP 8.1.27 ✓ (supported)
  Framework: Laravel 10.40.0 ✓ (current)
  Database: MySQL 8.0 (detected)

Client-Side:
  jQuery: 3.7.1 ✓ (current)
  Bootstrap: 5.3.2 ✓ (current)
  Vue.js: 3.4.5 ✓ (current)

Third-Party:
  Google Analytics 4
  CloudFlare CDN
  AWS S3 (assets)
  reCAPTCHA v3
```

### Vulnerability Check

```
🔍 Vulnerability Assessment

Software Version Analysis:
  ✓ nginx 1.24.0 - No known CVEs
  ✓ PHP 8.1.27 - No known CVEs
  ✓ Laravel 10.40.0 - No known CVEs
  ✓ jQuery 3.7.1 - No known CVEs

CVE Database Check:
  Total CVEs checked: 47
  Affecting your stack: 0 ✓

Security Note:
  All detected software versions are current
  and have no known vulnerabilities.

Score: 10/10 (Excellent)
```

---

### Question 5: Technology Review

**System asks:**
```
Technology Assessment Complete

Stack Health: EXCELLENT
  • All software current
  • No known vulnerabilities
  • Modern, supported versions

Third-Party Services: 4 detected
  • Consider reviewing privacy implications
  • Ensure each service is necessary

Any questions about the technology stack? (yes/no)
```

---

## Step 6: Final Report & Recommendations

### System Compiles Report:

```
Step 6 of 6: Final Report Generation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DOMAIN SECURITY AUDIT REPORT
example.com
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Overall Security Grade: A- (Excellent)
Risk Score: 2.3/10 (Very Low Risk)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CATEGORY SCORES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Infrastructure:     9/10  ✓
SSL/TLS:           10/10  ✓
Security Headers:   9/10  ✓
Information Exposure: 8/10  ✓
Technology:        10/10  ✓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Priority Actions

```
🎯 PRIORITY ACTIONS

No critical issues found!

Recommended Improvements:

1. Renew domain (45 days remaining)
   Priority: Medium
   Timeline: Within 30 days

2. Add Permissions-Policy header
   Priority: Low
   Timeline: Next maintenance window
   
3. Remove backup file (config.php.bak)
   Priority: Medium
   Timeline: This week

4. Disable directory listing
   Priority: Low
   Timeline: Next deployment
```

### Security Roadmap

```
📋 RECOMMENDED SECURITY ROADMAP

Immediate (This Week):
  ☐ Remove exposed backup file
  ☐ Renew domain registration

Short-term (This Month):
  ☐ Add Permissions-Policy header
  ☐ Review third-party service access
  ☐ Enable automated security scanning

Ongoing:
  ☐ Monthly security audits
  ☐ Keep software updated
  ☐ Monitor security advisories
  ☐ Review access logs regularly
```

---

## Report Export

### Export Options

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
EXPORT OPTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  1. Executive Summary (PDF)
     → 1-page overview for leadership
     → High-level findings only

  2. Technical Report (PDF)
     → Complete findings
     → Remediation instructions
     → Evidence screenshots

  3. Spreadsheet (CSV)
     → Findings in table format
     → Priority sorting
     → Import to project management

  4. JSON Export
     → Machine-readable format
     → Integration with other tools

  5. Remediation Only
     → Action items list
     → Quick reference guide

Select export format (1-5): _
```

---

## Wizard Command

### Activation

```
/wizard domain

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🌐 Domain Security Audit Wizard
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

I'll guide you through a comprehensive security audit
of any domain you own or have permission to test.

Duration: 10-15 minutes for standard audit

You'll learn:
  • Security strengths and weaknesses
  • Specific vulnerabilities
  • Prioritized fix list
  • Security roadmap

Great for:
  • Monthly security checkups
  • Pre-purchase website audits
  • Compliance assessments
  • Vendor security reviews

⚠️ IMPORTANT: Only audit domains you have
    permission to test.

Ready to begin? (yes/no)
```

---

## Alternative Verdict Examples

### Poor Security Example

```
Overall Security Grade: D (Poor)
Risk Score: 7.8/10 (High Risk)

Critical Issues:
  🚨 Admin panel accessible without auth
  🚨 Outdated software with known CVEs
  🚨 No HTTPS enforcement

Immediate Actions Required:
  1. Secure admin panel NOW
  2. Update all software
  3. Enable HTTPS redirects
```

### Medium Risk Example

```
Overall Security Grade: C (Fair)
Risk Score: 5.2/10 (Medium Risk)

Concerns:
  ⚠️ Missing security headers
  ⚠️ Information disclosure in errors
  ⚠️ Self-signed SSL certificate

Recommended Actions:
  • Add HSTS and CSP headers
  • Use valid SSL certificate
  • Configure error handling
```

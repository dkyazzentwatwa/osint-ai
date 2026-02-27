# Example Investigation: Domain Security Audit

## Target: example-startup.com

### Scenario
You're doing due diligence before investing in a startup. You want to check their security posture based on publicly available information.

### Step 1: Initial Domain Recon

**Command:**
```
/recon example-startup.com
```

**Results:**
🟢 HIGH: Main website operational
🟢 HIGH: Found LinkedIn company page
🟡 MEDIUM: ~50 employees on LinkedIn
🔴 LOW: Mentioned in 2 tech blog articles
🟢 HIGH: GitHub organization exists

### Step 2: Deep Dork Analysis

**Command:**
```
/dork example-startup.com
```

**Results:**

#### Exposed Documents
```
site:example-startup.com filetype:pdf
  └── 🟡 FOUND: /reports/2024-q1-financial.pdf
  └── 🟡 FOUND: /presentations/investor-deck-2023.pdf
  
site:example-startup.com filetype:xls OR filetype:csv
  └── 🔴 FOUND: /data/customer-list-backup.csv
      ⚠️ POTENTIAL DATA EXPOSURE
```

#### Admin Panels & Login Pages
```
site:example-startup.com inurl:admin OR inurl:login
  └── 🟢 No exposed admin panels found
  
site:example-startup.com inurl:dashboard
  └── 🔴 FOUND: staging.example-startup.com/dashboard
      ⚠️ Staging environment exposed
```

#### API Endpoints
```
site:example-startup.com inurl:api OR inurl:swagger OR inurl:docs
  └── 🟡 FOUND: api.example-startup.com/v1/
  └── 🔴 FOUND: api.example-startup.com/v1/docs (Swagger UI)
      ⚠️ API documentation publicly accessible
```

#### Configuration Files
```
site:example-startup.com ext:env OR ext:config OR ext:yaml
  └── 🟢 No config files exposed

site:example-startup.com "index of /"
  └── 🔴 FOUND: s3.amazonaws.com/example-startup-backups/
      ⚠️ OPEN S3 BUCKET
```

### Step 3: Subdomain Enumeration

**Command:**
```
Search: site:*.example-startup.com
```

**Results:**
```
🟢 www.example-startup.com (main site)
🟢 app.example-startup.com (customer portal)
🔴 staging.example-startup.com (staging env)
🔴 dev.example-startup.com (development)
🟢 api.example-startup.com (API endpoint)
🔴 legacy.example-startup.com (old site)
```

### Step 4: GitHub Investigation

**Command:**
```
Search: site:github.com/example-startup
```

**Results:**
```
🟢 Organization: github.com/example-startup
🟢 Public repositories: 12
🔴 Repository: example-startup/legacy-api
  └── Contains: database-credentials.json (committed)
  └── Contains: .env files with API keys
  
⚠️ CREDENTIAL EXPOSURE DETECTED
```

### Step 5: Paste Site Check

**Command:**
```
Search: site:pastebin.com OR site:github.com/gists "example-startup.com"
```

**Results:**
```
🟢 No leaked credentials found
🟡 One mention in technical discussion
```

### Step 6: Generate Security Report

**Command:**
```
/report
```

**Output:**
```markdown
=== SECURITY ASSESSMENT REPORT ===
Target: example-startup.com
Date: 2026-02-27
Assessment Type: External Reconnaissance

EXECUTIVE SUMMARY:
Multiple security issues identified through passive 
reconnaissance. Immediate attention required for 
exposed credentials and open S3 bucket.

CRITICAL FINDINGS:
🔴 GitHub repository contains exposed credentials
   - File: legacy-api/database-credentials.json
   - File: legacy-api/.env (API keys)
   
🔴 Open S3 bucket: s3.amazonaws.com/example-startup-backups/
   - Potential customer data exposure
   
🔴 Staging environment publicly accessible
   - staging.example-startup.com/dashboard
   
🔴 API documentation exposed without authentication
   - api.example-startup.com/v1/docs

MEDIUM FINDINGS:
🟡 Financial documents publicly accessible
🟡 Exposed customer list CSV
🟡 Development environment accessible

POSITIVE FINDINGS:
🟢 No exposed configuration files on main domain
🟢 No admin panels publicly accessible
🟢 Main application requires authentication

RISK RATING: HIGH

RECOMMENDATIONS:
1. IMMEDIATE: Remove exposed credentials from GitHub
2. IMMEDIATE: Secure or remove open S3 bucket
3. HIGH: Add authentication to staging environment
4. HIGH: Restrict API documentation access
5. MEDIUM: Remove sensitive documents from public access
6. ONGOING: Implement security scanning in CI/CD

COMPLIANCE NOTES:
Exposed customer data (CSV) may trigger breach notification
requirements under GDPR/CCPA depending on content.
```

## Key Security Issues Found

| Issue | Severity | Evidence |
|-------|----------|----------|
| Exposed credentials in GitHub | Critical | database-credentials.json |
| Open S3 bucket | Critical | Public bucket listing |
| Exposed customer data | High | customer-list-backup.csv |
| Staging environment public | High | No authentication |
| API docs public | Medium | Swagger UI accessible |

## Due Diligence Impact

**Investment Risk Factors:**
- Poor security practices
- Potential data breach exposure
- Regulatory compliance issues
- Technical debt (legacy systems)

**Questions to Ask:**
1. When was the last security audit?
2. Do you have a security team or external consultant?
3. Are you aware of the exposed S3 bucket?
4. What data was in the customer CSV file?
5. Have you had any previous breaches?

## Techniques Used

- Google Dorking for exposed files
- Subdomain enumeration
- Repository security scanning
- Paste site monitoring
- Cloud storage reconnaissance
- Confidence rating for findings

# Leak Detection Module

> **Module ID:** LEAK-DETECT-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Data Exposure Detection

---

## 1. Overview

Data leak detection involves finding exposed sensitive information in public repositories, paste sites, and misconfigured services. This module covers search patterns for credentials, configurations, and proprietary data.

**Warning:** Only search for data you have authorization to investigate. Respect privacy and intellectual property.

---

## 2. GitHub Credential Search Patterns

### 2.1 Search Scope

GitHub contains vast amounts of code, including accidentally committed:
- API keys and tokens
- Database credentials
- Private configuration files
- Internal documentation
- Proprietary algorithms

### 2.2 Search Operators

**Basic Operators:**
```
term                    - Contains term
"exact phrase"          - Exact match
user:username           - Specific user
org:organization        - Specific organization
repo:owner/name         - Specific repository
path:directory          - Specific path
extension:json          - File extension
filename:.env           - Specific filename
```

**Boolean Operators:**
```
term1 AND term2         - Both required
term1 OR term2          - Either acceptable
-term                   - Exclude term
```

### 2.3 Credential Search Patterns

**API Keys:**
```
"api_key" OR "api-key" OR "apikey"
"api_secret" OR "api-secret"
"api_token" OR "api-token"
"access_token" extension:json
"private_key" extension:pem
"aws_access_key_id"
"aws_secret_access_key"
"AZURE_CLIENT_SECRET"
"GOOGLE_API_KEY"
```

**Database Credentials:**
```
"mongodb://" 
"mysql://" 
"postgres://" 
"redis://"
"DATABASE_URL" 
"DB_PASSWORD" 
"DB_HOST" 
"connection_string"
"jdbc:mysql://"
"mongodb+srv://"
```

**Authentication Tokens:**
```
"bearer " 
"token: " 
"Authorization:"
"x-api-key:"
"slack_token" 
"github_token"
"heroku_api_key"
"npm_token"
"docker_password"
```

### 2.4 Configuration File Searches

**Sensitive Files:**
```
filename:.env
filename:.env.local
filename:.env.production
filename:config.json
filename:credentials.json
filename:secrets.json
filename:.aws/credentials
filename:id_rsa
filename:.htpasswd
filename:web.config
```

**Content Patterns:**
```
"password" AND ("prod" OR "production")
"secret" AND extension:json
"credentials" AND path:config
"-----BEGIN RSA PRIVATE KEY-----"
"-----BEGIN OPENSSH PRIVATE KEY-----"
"-----BEGIN PGP PRIVATE KEY BLOCK-----"
```

### 2.5 Organization-Specific Searches

```
org:companyname "password"
org:companyname "secret"
org:companyname "api_key"
org:companyname filename:.env
org:companyname extension:sql
```

---

## 3. Paste Site Search Strategies

### 3.1 Targeted Search Patterns

**Email-Credential Pairs:**
```
"@company.com" "password"
"@company.com" "login"
"@company.com" "pwd"
username "company" "pass"
```

**Configuration Exposures:**
```
"company.com" "config"
"company.com" "database"
"company.com" "internal"
"company.com" "vpn"
"company.com" "credentials"
```

**Database Dumps:**
```
"company_name" "dump"
"company_name" "leak"
"company_name" "breach"
"company_name" "database"
"company_name" "users"
```

### 3.2 Multi-Site Search

**Search Engines for Paste Sites:**
```
Google:
site:pastebin.com "company.com"
site:pastebin.com "company" "password"
site:gist.github.com "company" "config"

DuckDuckGo:
site:ghostbin.co "company"
site:paste.ee "company"

Specialized:
https://psbdmp.ws/ - Pastebin search engine
https://www.pastebinsearch.com/
```

---

## 4. Cloud Storage Exposure Detection

### 4.1 Amazon S3 Buckets

**Common Bucket Naming:**
```
company-name
companyname-backup
companyname-dev
companyname-prod
companyname-assets
```

**Search Patterns:**
```
"s3.amazonaws.com/company"
"s3://company"
"https://s3" "company"
".s3.amazonaws.com" " confidential"
```

**Dorking Patterns:**
```
site:s3.amazonaws.com "company"
site:s3.amazonaws.com "company" intitle:index
site:s3.amazonaws.com inurl:company
```

### 4.2 Google Cloud Storage

```
"storage.googleapis.com/company"
"storage.cloud.google.com/company"
".storage.googleapis.com" " confidential"
```

### 4.3 Azure Blob Storage

```
"blob.core.windows.net/company"
".blob.core.windows.net"
"account.blob.core.windows.net"
```

### 4.4 Misconfiguration Indicators

**Public Listings:**
```
- Directory indexing enabled
- No authentication required
- World-readable permissions
- Public ACL settings
```

**Sensitive Content Signs:**
```
- Database backups (.sql, .bak)
- Configuration files (.env, .config)
- Source code repositories
- Customer data files
- Internal documents
```

---

## 5. Database Dump Indicators

### 5.1 Dump File Characteristics

**File Extensions:**
```
.sql         - SQL dump files
.dump        - Database dump
.bak         - Backup files
.db          - SQLite database
.accdb       - Access database
.mdb         - Access database (old)
.backup      - Generic backup
```

**Content Signatures:**
```
-- MySQL dump
-- PostgreSQL database dump
-- Table structure for table
INSERT INTO `users`
INSERT INTO `accounts`
COPY table_name FROM stdin;
```

### 5.2 SQL Dump Patterns

**MySQL Dump Indicators:**
```sql
-- MySQL dump
-- Host: localhost    Database: database_name
-- Server version: 8.0.25

CREATE TABLE `users` (
INSERT INTO `users` VALUES
```

**PostgreSQL Dump Indicators:**
```sql
-- PostgreSQL database dump
SET statement_timeout = 0;
CREATE TABLE users (
COPY users (id, email, password) FROM stdin;
```

**MongoDB Export Indicators:**
```json
{"_id":{"$oid":"..."},"email":"..."}
```

---

## 6. Sensitive File Exposure

### 6.1 Environment Files

**.env File Contents:**
```
DB_HOST=localhost
DB_USER=admin
DB_PASS=secretpassword123
API_KEY=sk_live_abcdefghijklmnopqrstuvwxyz
SECRET_KEY=supersecretkey
AWS_ACCESS_KEY_ID=AKIA...
AWS_SECRET_ACCESS_KEY=...
```

**Search Patterns:**
```
extension:env
filename:.env
filename:.env.local
filename:.env.production
"DB_PASSWORD" extension:env
"API_KEY" extension:env
```

### 6.2 Configuration Files

**Web Config Files:**
```
filename:web.config
filename:appsettings.json
filename:config.json
filename:application.yml
filename:database.yml
```

**Server Config:**
```
filename:.htaccess
filename:httpd.conf
filename:nginx.conf
filename:php.ini
```

### 6.3 Key Files

**SSH Keys:**
```
filename:id_rsa
filename:id_dsa
filename:id_ecdsa
filename:id_ed25519
filename:.ssh/id_rsa
"-----BEGIN OPENSSH PRIVATE KEY-----"
```

**TLS/SSL Certificates:**
```
filename:.crt
filename:.key
filename:.pem
extension:key
```

---

## 7. Leak Verification Methodology

### 7.1 Verification Steps

**Step 1: Confirm Authenticity**
```
- Check file creation dates
- Verify against known configurations
- Look for internal references
- Validate email domains
```

**Step 2: Assess Scope**
```
- Count affected records
- Identify data types exposed
- Determine time window
- Check for production data
```

**Step 3: Check Current Validity**
```
- Are credentials still active?
- Is the configuration current?
- Have keys been rotated?
- Is the data still sensitive?
```

### 7.2 Verification Commands

**Command:** `/leak-search [term]`

**Input:** Search term (domain, company, product)
**Process:**
1. Search GitHub for code/config leaks
2. Search paste sites for dumps
3. Search for cloud storage exposure
4. Check for credential patterns
**Output:** Potential leak report with verification steps

### 7.3 False Positive Detection

**Common False Positives:**
```
- Test/demo data
- Documentation examples
- Placeholder values ("your-api-key-here")
- Intentionally public data
- Already revoked credentials
- Decoy/honeypot data
```

**Verification Indicators:**
```
✓ Internal email domains
✓ Production hostnames
✓ Current credentials (verified)
✓ Recent timestamps
✓ Internal terminology
✓ Valid API responses
```

---

## 8. Search Automation

### 8.1 Google Alerts Setup

```
Alert queries:
- "company.com" site:pastebin.com
- "company" "database dump"
- "company" "credentials leaked"
- companyname filetype:sql
```

### 8.2 GitHub Monitoring

```
Save searches:
- org:company "password"
- org:company "secret"
- org:company "api_key"

Enable notifications for new results
```

### 8.3 Shodan Queries

```
For exposed services:
- hostname:"company.com"
- ssl:"company.com"
- org:"Company Name"
```

---

## 9. Ethical Guidelines

### 9.1 Responsible Search Practices

**DO:**
- Search only authorized targets
- Report findings through proper channels
- Respect rate limits and robots.txt
- Verify before alerting
- Handle data responsibly

**DO NOT:**
- Download exposed data without authorization
- Test credentials without permission
- Share findings publicly
- Use discovered data maliciously
- Ignore responsible disclosure

### 9.2 Disclosure Process

```
1. Document findings
2. Notify affected organization privately
3. Allow time for remediation (typically 90 days)
4. Escalate if no response
5. Public disclosure only after fix or timeout
```

---

## 10. Limitations

### 10.1 Search Limitations

1. **GitHub search limited** - Only indexed content
2. **Deleted content** - Not searchable
3. **Private repositories** - Inaccessible
4. **Real-time coverage** - Delays in indexing
5. **API rate limits** - Restricts automated searching

### 10.2 Detection Limitations

1. **Encrypted data** - Unreadable even if exposed
2. **Hashed credentials** - Requires cracking
3. **Temporary exposure** - May be missed
4. **Access controls** - Some leaks require authentication
5. **Volume** - Too much data to monitor everything

---

*Leak Detection Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*  
*For authorized security research only*

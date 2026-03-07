# Example Investigation: Username Correlation

> Canonical example: this file in `examples/` is the source of truth. `docs/examples/username-correlation.md` is a navigation mirror that should link here rather than maintain a second long-form copy.

## Example Navigation

- Canonical path: [`examples/username-correlation.md`](./username-correlation.md)
- Docs mirror: [`docs/examples/username-correlation.md`](../docs/examples/username-correlation.md)
- Related examples: [`examples/domain-security-audit.md`](./domain-security-audit.md) and [`examples/person-investigation.md`](./person-investigation.md)

## Target: Username "cyberhacker99"

### Scenario
You're investigating a potential threat actor or researcher using the handle "cyberhacker99". You want to find all platforms where this username appears.

### Step 1: Initial Username Search

**Command:**
```
/recon cyberhacker99
```

**Results:**
🟢 HIGH: GitHub - github.com/cyberhacker99
🟢 HIGH: Twitter/X - @cyberhacker99
🟡 MEDIUM: Reddit - u/cyberhacker99 (low activity)
🔴 LOW: Forum mention on hackforums.net
🟢 HIGH: LinkedIn - /in/cyberhacker99

### Step 2: Cross-Platform Analysis

**Command:**
```
/pivot cyberhacker99
```

**Results:**

#### GitHub Profile Analysis
```
🟢 User: cyberhacker99
🟢 Joined: 2019
🟢 Repos: 23
🟢 Followers: 156
🟡 Activity: Security tools, POCs, educational content

Repository Highlights:
- network-scanner (Python)
- password-audit-tool (Bash)
- wifi-security-guide (Markdown)
- vulnerable-app-demo (PHP)

⚠️ Note: Repositories marked "for educational purposes only"
```

#### Twitter/X Analysis
```
🟢 Account: @cyberhacker99
🟢 Created: 2018
🟢 Following: 400
🟢 Followers: 1,200
🟢 Bio: "Security researcher | CTF player | Bug hunter"

Tweet Analysis:
- Posts about CTF competitions
- Shares security articles
- Occasional tool releases
- No direct threats or illegal activity
```

#### LinkedIn Analysis
```
🟢 Profile: /in/cyberhacker99
🟢 Name: Alex Johnson (name changed for example)
🟢 Title: Security Consultant
🟢 Company: CyberSec Solutions LLC
🟢 Location: San Francisco, CA
🟢 Connections: 500+

Employment History:
- Security Consultant (2020-present)
- Junior Penetration Tester (2018-2020)
```

### Step 3: Email Discovery

**Command:**
Search GitHub commits for email patterns

**Results:**
```
🟢 Found in GitHub commits: alex.j@cybersec-solutions.com
🟢 Found in GitHub profile: cyberhacker99@protonmail.com

Pivot Analysis:
- alex.j@cybersec-solutions.com → Corporate email
- cyberhacker99@protonmail.com → Personal secure email
```

### Step 4: Domain Association

**Command:**
```
/pivot cybersec-solutions.com
```

**Results:**
```
🟢 Domain: cybersec-solutions.com
🟢 Registered: 2019
🟢 Business: Cybersecurity consulting firm
🟢 Location: San Francisco, CA (matches LinkedIn)
🟢 Employees: ~10 (per LinkedIn)

Contact Info:
- Phone: +1-555-0123
- Email: info@cybersec-solutions.com
- Address: 123 Market St, San Francisco
```

### Step 5: Timeline Construction

**Command:**
```
/timeline cyberhacker99
```

**Results:**
```
2018:
- Twitter account created
- Started sharing security content

2019:
- GitHub account created
- Joined CyberSec Solutions
- First repository published

2020:
- Promoted to Security Consultant
- LinkedIn profile created
- Started CTF competitions

2021-2023:
- Consistent tool releases on GitHub
- Growing Twitter following
- Multiple speaking engagements (per LinkedIn)

2024-Present:
- Active bug hunting
- Continued educational content
```

### Step 6: Correlation Analysis

**Entity Map:**
```
cyberhacker99 (Username)
├── Alex Johnson (Real Name) 🟢 HIGH
├── alex.j@cybersec-solutions.com (Corporate Email) 🟢 HIGH
├── cyberhacker99@protonmail.com (Personal Email) 🟢 HIGH
├── CyberSec Solutions LLC (Employer) 🟢 HIGH
├── San Francisco, CA (Location) 🟢 HIGH
├── github.com/cyberhacker99 (GitHub) 🟢 HIGH
├── @cyberhacker99 (Twitter) 🟢 HIGH
├── u/cyberhacker99 (Reddit) 🟡 MEDIUM
└── /in/cyberhacker99 (LinkedIn) 🟢 HIGH
```

### Step 7: Generate Investigation Report

**Command:**
```
/report
```

**Output:**
```markdown
=== USERNAME CORRELATION REPORT ===
Target: cyberhacker99
Date: 2026-02-27
Classification: Security Research Assessment

EXECUTIVE SUMMARY:
Username "cyberhacker99" is associated with Alex Johnson,
a legitimate security consultant based in San Francisco.
Multiple data points confirm identity across platforms.
Assessment: Likely legitimate security researcher, not 
malicious actor.

IDENTITY VERIFICATION:
🟢 HIGH: LinkedIn profile confirms real identity
🟢 HIGH: GitHub email matches corporate domain
🟢 HIGH: Location consistent across all platforms
🟢 HIGH: Employment history verifiable
🟡 MEDIUM: Reddit activity minimal/unconfirmed

PLATFORM PRESENCE:
✅ GitHub: Active, educational security tools
✅ Twitter/X: Professional security content
✅ LinkedIn: Verified professional profile
⚠️ Reddit: Account exists, minimal activity
⚠️ Forums: Single mention on hackforums.net

CONTENT ANALYSIS:
- Tools published: Educational/POC only
- Disclaimers present: "for educational purposes"
- No malware distribution detected
- No direct threats or illegal activity
- CTF participation documented

EMPLOYMENT VERIFICATION:
Company: CyberSec Solutions LLC
Status: Active business entity
Location: San Francisco, CA
Role: Security Consultant
Tenure: 2020-present

RISK ASSESSMENT: LOW
Subject appears to be a legitimate cybersecurity 
professional with appropriate disclosures on tools.

CONFIDENCE LEVEL: HIGH
Multiple corroborating sources confirm identity
and professional status.

RECOMMENDATIONS:
1. No further investigation warranted
2. Tools reviewed appear legitimate and educational
3. Professional references check positive
4. Consider for security consulting engagement

CAVEATS:
- Single mention on hackforums.net (context unclear)
- Uses ProtonMail (privacy-conscious, not suspicious)
- Minimal Reddit activity could be different user
```

## Key Findings

| Platform | Account | Activity | Confidence |
|----------|---------|----------|------------|
| GitHub | cyberhacker99 | High | 🟢 |
| Twitter/X | @cyberhacker99 | High | 🟢 |
| LinkedIn | /in/cyberhacker99 | High | 🟢 |
| Reddit | u/cyberhacker99 | Low | 🟡 |
| HackForums | Mention | Unknown | 🔴 |

## Pivot Opportunities

1. **Corporate**: Investigate CyberSec Solutions LLC
2. **Email**: Search for cyberhacker99@protonmail.com mentions
3. **Domain**: Analyze cybersec-solutions.com infrastructure
4. **Real Name**: Search "Alex Johnson" + San Francisco + security

## Techniques Used

- Username enumeration across platforms
- Cross-reference correlation
- Email pattern analysis
- Timeline construction
- Content analysis for intent assessment
- Employment verification
- Confidence rating system

## Lessons Learned

1. **Same username = Strong correlation**: Consistent use across professional platforms suggests single individual
2. **Context matters**: Hack forums mention could be research, not participation
3. **Privacy tools ≠ suspicious**: ProtonMail use is common in security field
4. **Employment verification anchors identity**: Company website confirms professional status
5. **Confidence builds with corroboration**: Each matching data point increases certainty

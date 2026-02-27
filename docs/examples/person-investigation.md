# Example Investigation: Person Reconnaissance

## Target: Tech Influencer "ExampleCorp"

### Step 1: Initial Reconnaissance

**Command:**
```
/recon ExampleCorp
```

**Results:**
🟢 HIGH: Found Substack - examplecorp.substack.com (The Tech Weekly)
🟢 HIGH: Found LinkedIn - janesmith  
🟢 HIGH: YouTube channel @examplecorp-tech
🟡 MEDIUM: Instagram @examplecorp
🟢 HIGH: Co-founder of Tech Community

### Step 2: Pivot on Findings

**Command:**
```
/pivot janesmith
```

**Results:**
- Username appears consistently across LinkedIn, Substack, YouTube
- Real name appears to be Jane Smith
- Associated with John Doe-Smith (co-founder, likely spouse)

**Command:**
```
/pivot examplecorp.com
```

**Results:**
🟢 HIGH: Main domain registered and active
🟢 HIGH: Subdomain community.examplecorp.com (membership portal)
🟡 MEDIUM: Uses Discord platform
🟢 HIGH: Contact email hello@examplecorp.com

### Step 3: Domain Security Check

**Command:**
```
/dork examplecorp.com
```

**Results:**
```
site:examplecorp.com filetype:pdf
  └── No exposed documents

site:examplecorp.com inurl:admin OR inurl:login
  └── No exposed admin panels

site:examplecorp.com inurl:api
  └── No exposed API docs

"examplecorp.com" -site:examplecorp.com
  └── Mentions on LinkedIn, Substack, Facebook
```

### Step 4: Timeline Building

**Command:**
```
/timeline "Jane Smith"
```

**Results:**
- Substack launched ~10 months ago
- 250,000+ followers across platforms
- Tech Community co-founded with John
- Featured industry collaborator status

### Step 5: Generate Report

**Command:**
```
/report
```

**Output:**
```markdown
=== INTELLIGENCE SUMMARY ===
Subject: Jane Smith ("ExampleCorp")
Date: 2026-02-27

EXECUTIVE SUMMARY:
Technology consultant operating under the 
brand "ExampleCorp". Co-founder of Tech Community.

KEY FINDINGS:
🟢 Founder of The Tech Weekly (18K+ Substack subscribers)
🟢 250K+ followers across TikTok, Instagram, YouTube
🟢 Co-founder Tech Community with John Doe-Smith
🟢 Featured industry collaborator
🟢 Business operates at examplecorp.com

DIGITAL FOOTPRINT:
- Substack: examplecorp.substack.com
- YouTube: @examplecorp-tech
- LinkedIn: janesmith
- Domain: examplecorp.com
- Email: hello@examplecorp.com

CONFIDENCE ASSESSMENT:
High confidence on all major findings due to consistent
self-identification across authoritative sources.

RECOMMENDED NEXT STEPS:
- Monitor examplecorp.com for new subdomain creation
- Track Tech Community membership growth claims
- Cross-reference with John Doe-Smith findings
```

## Key Takeaways

1. **Consistent Branding**: Same username across all platforms
2. **Business Association**: Clear link to John Doe-Smith
3. **Clean Security**: No exposed sensitive files or endpoints
4. **Growing Influence**: Large, engaged following
5. **Multiple Revenue Streams**: Substack, courses, consulting

## Techniques Used

- Username correlation across platforms
- Domain reconnaissance
- Subdomain enumeration
- Timeline construction from dated content
- Confidence rating for each finding

# Example Investigation: Person Reconnaissance

## Target: Tech Influencer "TechTiff"

### Step 1: Initial Reconnaissance

**Command:**
```
/recon TechTiff
```

**Results:**
🟢 HIGH: Found Substack - techtiff.substack.com (The AI Creator Drop)
🟢 HIGH: Found LinkedIn - tiffanykyazze  
🟢 HIGH: YouTube channel @techtiff-ai
🟡 MEDIUM: Instagram @techtiff
🟢 HIGH: Co-founder of AI Flow Club

### Step 2: Pivot on Findings

**Command:**
```
/pivot tiffanykyazze
```

**Results:**
- Username appears consistently across LinkedIn, Substack, YouTube
- Real name appears to be Tiffany Kyazze
- Associated with David Kyazze-Ntwatwa (co-founder, likely spouse)

**Command:**
```
/pivot techtiff.ai
```

**Results:**
🟢 HIGH: Main domain registered and active
🟢 HIGH: Subdomain flow-club.techtiff.ai (membership portal)
🟡 MEDIUM: Uses Circle.so platform
🟢 HIGH: Contact email hello@techtiff.ai

### Step 3: Domain Security Check

**Command:**
```
/dork techtiff.ai
```

**Results:**
```
site:techtiff.ai filetype:pdf
  └── No exposed documents

site:techtiff.ai inurl:admin OR inurl:login
  └── No exposed admin panels

site:techtiff.ai inurl:api
  └── No exposed API docs

"techtiff.ai" -site:techtiff.ai
  └── Mentions on LinkedIn, Substack, Facebook
```

### Step 4: Timeline Building

**Command:**
```
/timeline "Tiffany Kyazze"
```

**Results:**
- Substack launched ~10 months ago
- 250,000+ followers across platforms
- AI Flow Club co-founded with David
- Featured ChatGPT Collaborator status

### Step 5: Generate Report

**Command:**
```
/report
```

**Output:**
```markdown
=== INTELLIGENCE SUMMARY ===
Subject: Tiffany Kyazze ("TechTiff")
Date: 2026-02-27

EXECUTIVE SUMMARY:
AI educator and systems strategist operating under the 
brand "TechTiff". Co-founder of AI Flow Club community.

KEY FINDINGS:
🟢 Founder of The AI Creator Drop (18K+ Substack subscribers)
🟢 250K+ followers across TikTok, Instagram, YouTube
🟢 Co-founder AI Flow Club with David Kyazze-Ntwatwa
🟢 Featured ChatGPT Collaborator
🟢 Business operates at techtiff.ai

DIGITAL FOOTPRINT:
- Substack: techtiff.substack.com
- YouTube: @techtiff-ai
- LinkedIn: tiffanykyazze
- Domain: techtiff.ai
- Email: hello@techtiff.ai

CONFIDENCE ASSESSMENT:
High confidence on all major findings due to consistent
self-identification across authoritative sources.

RECOMMENDED NEXT STEPS:
- Monitor techtiff.ai for new subdomain creation
- Track AI Flow Club membership growth claims
- Cross-reference with David Kyazze-Ntwatwa findings
```

## Key Takeaways

1. **Consistent Branding**: Same username across all platforms
2. **Business Association**: Clear link to David Kyazze-Ntwatwa
3. **Clean Security**: No exposed sensitive files or endpoints
4. **Growing Influence**: Large, engaged following
5. **Multiple Revenue Streams**: Substack, courses, consulting

## Techniques Used

- Username correlation across platforms
- Domain reconnaissance
- Subdomain enumeration
- Timeline construction from dated content
- Confidence rating for each finding

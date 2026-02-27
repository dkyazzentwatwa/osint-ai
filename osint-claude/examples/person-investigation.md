# Example Investigation: Person Reconnaissance (v2.0)

## Target: Tech Influencer "ExampleCorp"

This example demonstrates the v2.0 features including wizards, risk assessment, visualization, and session management.

---

## Method 1: Using the Wizard (Recommended for Beginners)

**Command:**
```
/wizard person
```

**Interactive Session:**
```
Agent: Welcome to the Person Investigation Wizard!
This will guide you through investigating someone step by step.

Step 1/6: What's the person's full name or username?
You: ExampleCorp

Step 2/6: Do you know their location? (press Enter to skip)
You: 

Step 3/6: Do you know their email or website? (press Enter to skip)
You: examplecorp.com

Step 4/6: Any specific platforms to focus on? (all/social/professional)
You: all

Step 5/6: What report format do you want?
  [1] Simple (easy to understand)
  [2] Technical (detailed)
  [3] Both
You: 3

Step 6/6: Review your inputs:
  - Name: ExampleCorp
  - Website: examplecorp.com
  - Platforms: All
  - Reports: Both formats

Proceed with investigation? (yes/no)
You: yes

Agent: [Running investigation...]
[████████████░░░░░░░░] 60% complete

✓ Reconnaissance complete (8 entities found)
✓ Domain analysis complete (1 domain)
✓ Timeline building complete (12 events)
✓ Report generation complete
```

---

## Method 2: The "Full Sweep" (Fastest)

**Command:**
```
/full examplecorp.com
```

**Results:**
```
=== COMPREHENSIVE INVESTIGATION ===
Target: examplecorp.com
Started: 2026-02-27 14:32:15

PHASE 1: Initial Reconnaissance ✓ (2.3s)
  Entities found: 8
  - Person: Jane Smith
  - Domain: examplecorp.com
  - Domain: community.examplecorp.com
  - Username: @examplecorp (multiple platforms)
  - Organization: Tech Community
  - Email: hello@examplecorp.com

PHASE 2: Security Analysis ✓ (8.7s)
  Dorks executed: 12
  Risk score: 15/100 (LOW)
  Findings:
  🟢 No exposed sensitive files
  🟢 No admin panels found
  🟢 No API documentation exposed
  🟢 Clean security posture

PHASE 3: Pivot Analysis ✓ (15.2s)
  Pivots executed: 5
  Connections found:
  - Jane Smith → John Doe
  - examplecorp.com → community.examplecorp.com
  - @examplecorp → LinkedIn, YouTube, Substack

PHASE 4: Timeline Construction ✓ (4.1s)
  Events mapped: 12
  Date range: 2024-02 to 2026-02

PHASE 5: Entity Mapping ✓ (1.8s)
  Relationship graph: 8 entities, 14 connections

PHASE 6: Report Generation ✓ (3.2s)
  ✓ Technical report: examplecorp-technical-report.md
  ✓ Simple report: examplecorp-simple-report.md

=== INVESTIGATION COMPLETE ===
Duration: 35.3 seconds
Entities: 8
Risk Score: 15/100 (LOW)
Reports: 2 generated

Key Findings:
🟢 Clean security posture
🟢 Legitimate business operation
🟢 Consistent branding across platforms
🟢 No red flags identified
```

**Save the investigation:**
```
/save examplecorp-investigation-2026-02-27

Agent: ✓ Investigation saved as "examplecorp-investigation-2026-02-27"
  - 8 entities
  - 12 timeline events
  - 2 reports
  - 35.3 seconds of work preserved
```

---

## Method 3: Step-by-Step (For Learning)

### Step 1: Initial Reconnaissance

**Command:**
```
/recon ExampleCorp
```

**Results:**
```
🟢 HIGH: Found Substack - examplecorp.substack.com (Tech Insights)
🟢 HIGH: Found LinkedIn - janesmith  
🟢 HIGH: YouTube channel @examplecorp-tech (18K subscribers)
🟡 MEDIUM: Instagram @examplecorp (45K followers)
🟢 HIGH: Co-founder of Tech Community
🟢 HIGH: TikTok @examplecorp (200K+ followers)

Total reach: ~250,000+ followers across platforms
```

### Step 2: Risk Assessment

**Command:**
```
/risk-assessment
```

**Results:**
```
=== RISK ASSESSMENT ===
Target: ExampleCorp / Jane Smith
Overall Risk Score: 15/100 (LOW)

Breakdown by Category:
┌─────────────────┬───────┬────────┐
│ Category        │ Score │ Level  │
├─────────────────┼───────┼────────┤
│ Security        │ 10    │ LOW    │
│ Privacy         │ 20    │ LOW    │
│ Reputation      │ 15    │ LOW    │
│ Legal           │ 5     │ LOW    │
└─────────────────┴───────┴────────┘

Analysis:
🟢 Low security risk - clean digital footprint
🟢 No exposed credentials or vulnerabilities
🟢 Legitimate business operation
🟢 Positive reputation across platforms
🟢 No legal issues identified

Recommendation: LOW RISK - Safe to engage
```

### Step 3: Pivot on Findings

**Command:**
```
/pivot tiffanykyazze
```

**Results:**
```
PIVOT ANALYSIS: tiffanykyazze
Searching across platforms...

🟢 LinkedIn: /in/janesmith
   - Title: AI & Automation Specialist
   - Company: ExampleCorp (Founder)
   - Location: United States
   - Connections: 1,200+

🟢 Substack: @janesmith (redirects to examplecorp)
🟢 YouTube: Channel owner
🟢 Instagram: @examplecorp (confirmed match)

Auto-Correlation:
⚠️ Found similar username: "janesmith" → "jane_smith"
  Investigate this variation? (y/n)

Connection Strength: 95% (same person across platforms)
```

**Command:**
```
/pivot examplecorp.com
```

**Results:**
```
🟢 HIGH: Main domain registered and active
🟢 HIGH: Subdomain community.examplecorp.com (membership portal)
🟡 MEDIUM: Uses Circle.so platform
🟢 HIGH: Contact email hello@examplecorp.com
🟢 HIGH: Privacy policy at /privacy
🟢 HIGH: Terms of service at /terms

Domain Health: EXCELLENT
SSL: Valid
Uptime: 99.9% (per public monitors)
```

### Step 4: Domain Security Check

**Command:**
```
/dork examplecorp.com
```

**Results:**
```
GOOGLE DORK ANALYSIS: examplecorp.com

Exposed Documents:
site:examplecorp.com filetype:pdf
  └── 🟢 No exposed PDF documents

Admin Panels:
site:examplecorp.com inurl:admin OR inurl:login
  └── 🟢 No exposed admin panels

API Endpoints:
site:examplecorp.com inurl:api OR inurl:swagger
  └── 🟢 No exposed API documentation

Configuration Files:
site:examplecorp.com ext:env OR ext:config
  └── 🟢 No config files exposed

Directory Listings:
site:examplecorp.com "index of /"
  └── 🟢 No open directories

Off-site Mentions:
"examplecorp.com" -site:examplecorp.com
  └── 🟢 Mentions on LinkedIn, Substack, Facebook (all positive)

SECURITY VERDICT: CLEAN
No vulnerabilities found via public reconnaissance.
```

### Step 5: Visualization

**Command:**
```
/visualize entities
```

**Results:**
```
ENTITY RELATIONSHIP GRAPH
=========================

ExampleCorp (Brand)
├── Jane Smith (Founder) 🟢
│   ├── LinkedIn: /in/janesmith
│   ├── Substack: @examplecorp
│   └── Email: hello@examplecorp.com
│
├── John Doe (Co-founder) 🟢
│   ├── LinkedIn: /in/johndoe
│   └── GitHub: exampleuser123
│
├── examplecorp.com (Domain) 🟢
│   ├── community.examplecorp.com (Subdomain)
│   └── hello@examplecorp.com (Contact)
│
├── Tech Community (Product) 🟢
│   ├── 800+ members
│   ├── community.examplecorp.com
│   └── Co-founded by Jane & John
│
└── Social Media Presence 🟢
    ├── YouTube: @examplecorp-tech (18K)
    ├── Instagram: @examplecorp (45K)
    └── TikTok: @examplecorp (200K+)

Legend: 🟢 HIGH 🟡 MEDIUM 🔴 LOW
```

**Command:**
```
/visualize timeline
```

**Results:**
```
TIMELINE: ExampleCorp Growth
=========================

2024-03  [====] ExampleCorp brand launched
         [====] First Substack post
         
2024-05  [====] YouTube channel created
          
2024-08  [====] Tech Community launched
         [====] 100 members milestone
          
2024-11  [====] 1,000 members milestone
         [====] Featured Industry Collaborator
          
2025-01  [====] TikTok growth surge
          
2025-06  [====] 10,000 Substack subscribers
          
2026-02  [====] 250K+ total followers
         [====] 800+ Tech Community members

Activity: ████████████████████ Very Active
```

### Step 6: Dashboard Overview

**Command:**
```
/dashboard
```

**Results:**
```
╔════════════════════════════════════════════════╗
║     INVESTIGATION: ExampleCorp                 ║
╠════════════════════════════════════════════════╣
║ Entities: 8           High Confidence: 7       ║
║ Risk Score: 15/100    Level: LOW 🟢            ║
║ Duration: 12 minutes                           ║
║ Status: Active                                 ║
╠════════════════════════════════════════════════╣
║ Recent Findings:                               ║
║ • 2 min ago: LinkedIn profile verified         ║
║ • 5 min ago: Domain security check complete    ║
║ • 8 min ago: Co-founder identified             ║
║ • 10 min ago: YouTube channel found            ║
╠════════════════════════════════════════════════╣
║ Quick Actions:                                 ║
║ [1] /visualize network  [2] Generate report    ║
║ [3] /pivot on email     [4] Save session       ║
╚════════════════════════════════════════════════╝
```

### Step 7: Quality Check

**Command:**
```
/qa-check
```

**Results:**
```
QUALITY ASSURANCE CHECK
=======================

Coverage Analysis:
✓ Person reconnaissance: COMPLETE
✓ Domain analysis: COMPLETE
✓ Social media platforms: COMPLETE
✓ Timeline construction: COMPLETE
✓ Risk assessment: COMPLETE
✓ Entity relationships: COMPLETE

Gaps Identified:
⚠️ Financial records: Not checked (private)
⚠️ Deep web mentions: Not checked (requires specialized tools)

Source Verification:
✓ All sources currently active
✓ No broken links detected
✓ 7/7 primary sources verified

Investigation Quality: EXCELLENT (95%)
Recommendation: Investigation is thorough. Minor gaps are acceptable.
```

### Step 8: Generate Reports

**Command:**
```
/simple-report
```

**Output:**
```markdown
=== SIMPLE SUMMARY ===
THE BOTTOM LINE:
ExampleCorp is an AI education company run by Jane Smith 
and her partner John Doe. They teach people how to use AI tools 
through online courses and a community called Tech Community.

WHAT WE FOUND:
✅ Website: examplecorp.com (active and secure)
✅ 800+ member community called "Tech Community"
✅ Jane has 250,000+ followers on social media
✅ No security problems found on the website
✅ Legitimate business with clear offerings

WHAT THIS MEANS:
This appears to be a legitimate AI education business. 
The security is good, the founders are transparent about 
their identities, and there's a real community of learners.

RISK LEVEL: LOW (15/100)
Safe to engage with this business.

WHAT YOU SHOULD DO NEXT:
• If interested in AI courses, it's safe to explore
• Check their free content first (Substack, YouTube)
• Join Tech Community if you want community support
• No red flags or concerns identified

SIMPLE EXPLANATIONS:
Substack: A newsletter platform where writers share content
Tech Community: An online community for learning AI tools
Co-founder: Someone who started a business with another person
```

**Command:**
```
/report brief
```

**Output:**
```markdown
=== EXECUTIVE BRIEF ===
One-Page Summary

TARGET: ExampleCorp / examplecorp.com
DATE: 2026-02-27
RISK: 15/100 (LOW)

KEY FINDINGS:
1. Legitimate AI education business
2. 250K+ social media followers
3. 800+ member paid community
4. Clean security posture
5. Transparent founders

BUSINESS MODEL:
• Free content (Substack, YouTube, TikTok)
• Paid community (Tech Community - $97/month)
• AI consulting services

SECURITY: ✅ CLEAN
No exposed credentials, APIs, or sensitive files.

RECOMMENDATION: ✅ LOW RISK
Safe for business engagement.

NEXT REVIEW: 6 months
```

---

## Key Takeaways

1. **Consistent Branding**: Same username across all platforms builds trust
2. **Business Association**: Clear link to John Doe (co-founder)
3. **Clean Security**: No exposed sensitive files or endpoints (Risk: 15/100)
4. **Growing Influence**: Large, engaged following (250K+)
5. **Multiple Revenue Streams**: Substack, courses, consulting
6. **Legitimate Operation**: Professional website, privacy policy, terms of service

---

## Techniques Demonstrated

| Technique | Command | Purpose |
|-----------|---------|---------|
| Wizard-guided investigation | `/wizard person` | Step-by-step for beginners |
| Full automated sweep | `/full` | Complete investigation fast |
| Risk assessment | `/risk-assessment` | Automated risk scoring |
| Username correlation | `/pivot username` | Cross-platform matching |
| Domain reconnaissance | `/dork domain` | Security analysis |
| Entity visualization | `/visualize entities` | Relationship mapping |
| Timeline construction | `/visualize timeline` | Chronological view |
| Quality assurance | `/qa-check` | Verify coverage |
| Session management | `/save` | Preserve investigation |
| Dual reporting | `/simple-report` + `/report` | Multiple formats |

---

## v2.0 Features Used

- ✅ **Wizard** - Guided step-by-step investigation
- ✅ **Full sweep** - One-command complete investigation
- ✅ **Risk scoring** - Automated 1-100 risk assessment
- ✅ **Visualization** - ASCII entity graphs and timelines
- ✅ **Dashboard** - Real-time investigation overview
- ✅ **QA Check** - Quality assurance verification
- ✅ **Save/Load** - Session persistence
- ✅ **Multiple reports** - Technical and simple formats

---

**This investigation demonstrates the full v2.0 capabilities from beginner-friendly wizards to professional-grade analysis.**

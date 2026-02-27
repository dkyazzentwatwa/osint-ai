# Progressive Disclosure System

Progressive disclosure presents information in layers, starting with essentials and revealing details as needed.

---

## Simple vs Detailed Output Rules

### Default Output Levels

```
Level 1 (Simple):    Core finding only
Level 2 (Summary):   Finding + key context
Level 3 (Detailed):  Full analysis with sources
Level 4 (Complete):  Raw data + all metadata
```

### Output Expansion Commands

| Starting Level | Expand Command | Result |
|----------------|----------------|--------|
| Simple | `/more` or `/details` | Summary level |
| Summary | `/full` or `/expand` | Detailed level |
| Detailed | `/raw` or `/sources` | Complete level |
| Any | `/simple` | Return to simple view |

### Simple Output Format (Beginner Default)

```
[ICON] Finding Title
Brief description (1 sentence)

[Confidence: High/Medium/Low]
[Source count: N]

Type '/more' for details or '/sources' to see where this came from.
```

Example:
```
📧 Email Found: john.doe@example.com
This email appears on 3 professional profiles.

Confidence: High
Sources: 3

Type '/more' for details or '/sources' to see where this came from.
```

### Detailed Output Format (Expert Available)

```
================================================
FINDING: Email Address
================================================

Value: john.doe@example.com
Type: Professional
Confidence: 87%
First Seen: 2019-03-15
Last Verified: 2024-01-20

Sources:
  [1] LinkedIn profile (verified 2024-01-20)
  [2] Company website staff page (2023-11-10)
  [3] Conference speaker bio (2019-03-15)

Context:
  Associated with: Example Corp, TechConf 2019
  Format: firstname.lastname@domain.com
  Pattern: Corporate standard naming

Risk Indicators: None
Related Findings: 2 phone numbers, 1 address

================================================
Commands: /simple | /raw | /export | /timeline
================================================
```

---

## Command Explanation System

### Explanation Levels

**Level 1: Quick Definition**
```
/recon - Short for "reconnaissance." Gathers information 
about a target from public sources.
```

**Level 2: Usage Context**
```
/recon - Gathers publicly available information about 
a target (person, company, domain, or email).

When to use:
  • Starting a new investigation
  • Verifying online presence
  • Finding related accounts

Example: /recon johndoe@email.com

This will search social media, public records, and 
websites to build a profile.
```

**Level 3: Technical Deep-Dive**
```
/recon [target] [options]

Technical description:
  Executes targeted queries across 15+ public data 
  sources including search engines, social APIs, and 
  public record databases.

Parameters:
  target    - Entity to investigate (required)
  --deep    - Include archived/deleted content
  --social  - Focus on social media only
  --web     - Focus on websites only

Rate limits: 100 req/min
Timeout: 30s per source
Output: Structured JSON + human summary
```

### Explanation Triggers

```
Automatic Level 1 explanation:
  - Unknown command entered
  - First use of command in session

Automatic Level 2 explanation:
  - Command with >2 parameters
  - Command taking >30 seconds
  - After error recovery

Manual triggers:
  /explain [command]  → Level 2 explanation
  /explain [finding]  → Explain a result
  /how [command]      → Step-by-step guide
  /why [result]       → Explain reasoning
```

---

## Terminology Translation

### Technical Terms Dictionary

| Expert Term | Beginner Translation | Context |
|-------------|---------------------|---------|
| Reconnaissance | Information gathering | /recon command |
| Pivot | Follow a lead | Investigation flow |
| Dork | Special search query | /dork command |
| Metadata | Hidden file information | Photo verification |
| EXIF | Photo location/data | Image analysis |
| WHOIS | Domain registration info | Domain lookup |
| DNS | Website address system | Domain records |
| Subdomain | Section of website | Domain structure |
| OSINT | Public information research | General concept |
| Sock puppet | Fake online identity | Social analysis |
| Digital footprint | Online trail | Investigation target |
| PII | Personal information | Privacy warning |
| TTPs | Behavior patterns | Threat analysis |
| IOCs | Warning signs | Security analysis |
| Geolocation | Location tracking | Photo analysis |

### Auto-Translation Rules

```
When in Beginner mode:
  1. First occurrence of term → Show translation
  2. Subsequent uses → Show term with tooltip hint
  3. Hover/tap available → Show translation
  4. Glossary link always appended

Format:
  "We found METADATA (hidden information) in the photo."
  
  or
  
  "We found metadata* in the photo.
   
   *Metadata means hidden information stored in files"
```

### Context-Aware Translation

```
Term: "Pivot"
Context: /pivot command
Translation: "Follow this lead to discover more connections"

Term: "Pivot table"
Context: Data analysis
Translation: "A spreadsheet view that summarizes data"

Term: "Pivot point"
Context: Timeline analysis
Translation: "A key moment where the situation changed"
```

---

## Auto-Explanation Triggers

### Trigger Conditions

```
Trigger Table:
┌─────────────────────┬───────────┬───────────────┬────────┐
│ Condition           │ Beginner  │ Intermediate  │ Expert │
├─────────────────────┼───────────┼───────────────┼────────┤
│ New command         │ Always    │ First session │ Never  │
│ Error occurs        │ Full help │ Quick fix     │ Code   │
│ Slow operation      │ Progress  │ Status        │ None   │
│ Complex result      │ Explain   │ Summarize     │ Raw    │
│ Risk detected       │ Warn+help │ Warn          │ Alert  │
│ Multiple options    │ Guide     │ List          │ Menu   │
│ Ambiguous input     │ Clarify   │ Confirm       │ Error  │
└─────────────────────┴───────────┴───────────────┴────────┘
```

### Smart Explanation Timing

```
DO explain immediately when:
  - Command will modify data
  - Finding affects safety/privacy
  - User appears confused (hesitation patterns)
  - Error prevents progress

DO NOT explain when:
  - User is in flow (rapid commands)
  - Same explanation given <2 min ago
  - User has declined help 3+ times
  - Expert mode with no errors
```

---

## Example Mode Comparisons

### Example 1: Domain Reconnaissance

**Beginner Mode:**
```
I searched for "example.com" and found:

✓ This website was registered in 2010
✓ It's hosted in California, USA
✓ I found 5 related email addresses
⚠ One email uses a different domain (possible subsidiary)

Would you like me to:
  1. Look at those email addresses
  2. Check the subsidiary domain
  3. See all technical details

Just tell me the number or type '/help' for more options.
```

**Intermediate Mode:**
```
Domain: example.com

Registration:
  Created: 2010-06-15
  Registrar: GoDaddy
  
Infrastructure:
  Hosting: AWS us-west-1
  IP: 203.0.113.45
  
Contacts Found: 5 emails
  - admin@example.com
  - support@example.com
  - [3 additional]

Anomaly: Domain variant "example-inc.com" detected

Next: /emails | /variant | /dns
```

**Expert Mode:**
```
example.com
━━━━━━━━━━━━━━━━━━━━━━━━
REG: 2010-06-15 | R:GoDaddy | EXP:2025-06-15
HOST: 203.0.113.45 (AWS/us-west-1)
CDN: CloudFlare
MX: 5 records | SPF: v=spf1 +mx ~all

Emails: 5 | Variants: 1 | Subdomains: 23
━━━━━━━━━━━━━━━━━━━━━━━━
[CMD] /emails /variants /subdomains /export
```

### Example 2: Photo Verification

**Beginner Mode:**
```
📷 Photo Analysis Complete

I checked your photo and found:

✓ No location data embedded
✓ No camera information found
⚠ This image appears elsewhere online

What this means:
  The photo doesn't have hidden GPS coordinates (good for 
  privacy), but it has been posted on other websites. This 
  could mean it's a stock photo or someone else's image.

Recommendation: Be cautious. Ask for a current photo that 
shows something specific (like today's newspaper).

Want me to show you where else this image appears?
```

**Intermediate Mode:**
```
Photo Analysis: image_001.jpg

Metadata Check:
  ✗ GPS: Not present
  ✗ EXIF: Stripped
  ✗ Camera: Unknown

Reverse Image Search:
  ⚠ 12 matches found
  Sources: Shutterstock, 3 social profiles, news article

Assessment: Likely stock photo or reused image
Risk Level: Medium

Actions: /search-results | /metadata-deep | /compare
```

**Expert Mode:**
```
IMG: image_001.jpg | 2400x1600 | 2.3MB
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
METADATA
  GPS: ✗ | EXIF: ✗ | XMP: ✗
  Modify Date: 2024-01-15 14:32:11
  Software: Photoshop 2024
  
RIS (TinEye): 12 matches
  [0] shutterstock.com/img_12345 (stock)
  [1] twitter.com/userA/status/123
  [2] facebook.com/userB/posts/456
  [3-11] ...

VERDICT: STOCK/REUSED | RISK: 6/10
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
/view-matches | /timeline-check | /export
```

### Example 3: Person Investigation

**Beginner Mode:**
```
I looked up "Jane Smith" and here's what I found:

I discovered 3 people with this name:

Person 1 - Jane Smith in Boston
  • Works as a teacher
  • Active on LinkedIn
  • No red flags found

Person 2 - Jane Smith in London
  • Works in marketing
  • Professional profiles only
  • No red flags found

Person 3 - Jane "Janie" Smith in Austin
  ⚠ Multiple name variations found
  ⚠ Limited online presence
  • May be using a nickname

Which person were you looking for? Tell me the number 
(1, 2, or 3) or give me more details to narrow it down.
```

**Intermediate Mode:**
```
Search: "Jane Smith"
Results: 3 distinct individuals identified

[1] Jane Smith - Boston, MA
    Occupation: Educator (Boston Public Schools)
    Presence: LinkedIn, school website
    Confidence: High
    Risk: None

[2] Jane Smith - London, UK
    Occupation: Marketing Manager (TechCorp)
    Presence: LinkedIn, Twitter, company site
    Confidence: High
    Risk: None

[3] Jane "Janie" Smith - Austin, TX
    Occupation: Unknown
    Presence: Minimal (1 dating profile)
    Anomaly: Name variation, sparse footprint
    Confidence: Medium
    Risk: Low-Unknown

Select: /profile 1 | /profile 2 | /profile 3 | /refine
```

**Expert Mode:**
```
QUERY: "Jane Smith" | RESULTS: 3 | TIME: 3.2s
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[1] UID:jsmith-bos-1985 | C:0.94
    LOC:42.3601,-71.0589 | EDU:BU,MS-Ed
    EMP:BPS | SOC:LI,TW=0
    
[2] UID:jsmith-ldn-1990 | C:0.91
    LOC:51.5074,-0.1278 | EMP:TechCorp,MktDir
    SOC:LI,TW,IG | LAST:2024-01-15
    
[3] UID:jsmith-aus-1992 | C:0.67 ⚠
    ALIAS:"Janie" | LOC:30.2672,-97.7431
    SOC:DTG(1) | FOOTPRINT:MINIMAL ⚠
    
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
/pivot 1 | /pivot 2 | /deep 3 | /dork "Janie Smith Austin"
```

---

## Progressive Disclosure Best Practices

### Information Layering Principles

1. **Answer First**: Start with the core answer
2. **Context Second**: Add meaning and implications
3. **Details Last**: Provide granular data only when requested
4. **Exit Always**: Every screen shows how to get less/more info

### User Control

```
Every output includes:
  ✓ Current information level indicator
  ✓ Commands to adjust detail level
  ✓ Estimated time for deeper analysis
  ✓ Option to skip/cancel

Example footer:
  [Simple view] /more → Detailed | /raw → Complete | /export → Save
```

### Memory of Preferences

```
System remembers:
  - Preferred detail level per command type
  - Recently viewed detail expansions
  - Explanation dismissal patterns
  - Time spent at each level

Adapts by:
  - Defaulting to preferred level
  - Pre-fetching likely expansions
  - Reducing repetitive explanations
```

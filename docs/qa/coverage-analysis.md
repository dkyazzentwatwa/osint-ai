# Investigation Coverage Analysis System

## Overview

The coverage analysis system ensures comprehensive investigations by tracking what vectors have been checked and identifying gaps.

---

## Coverage Matrix

### Vector Categories

| Category | Vectors | Status |
|----------|---------|--------|
| **Identity** | Real name, aliases, usernames | ☐ |
| **Digital Presence** | Websites, social media, forums | ☐ |
| **Professional** | LinkedIn, company records, patents | ☐ |
| **Financial** | Property records, business filings, donations | ☐ |
| **Legal** | Court records, lawsuits, sanctions | ☐ |
| **Technical** | Domains, IPs, infrastructure | ☐ |
| **Geographic** | Addresses, locations, movements | ☐ |
| **Associates** | Family, colleagues, organizations | ☐ |
| **Historical** | Timeline events, archived content | ☐ |
| **Media** | Photos, videos, news mentions | ☐ |

### Coverage Scoring

```
Coverage Score = (Checked Vectors / Total Vectors) × 100

0-25%:   Minimal coverage - significant gaps
26-50%:  Partial coverage - major gaps remain  
51-75%:  Good coverage - minor gaps to address
76-90%:  Strong coverage - few gaps remain
91-100%: Comprehensive - near-complete picture
```

---

## Gap Identification Logic

### Automatic Gap Detection

When analyzing coverage, check for these patterns:

**Identity Gaps:**
- No real name found, only username
- Multiple usernames with no connection established
- Nickname/alias without origin identified

**Digital Presence Gaps:**
- Active on Twitter but no Instagram/Facebook found
- Professional site exists but no personal presence
- Forum activity found but no social media

**Professional Gaps:**
- Current job listed but employment history missing
- Company affiliation known but role unclear
- Skills mentioned but no portfolio/evidence

**Financial Gaps:**
- Property ownership suggested but unconfirmed
- Business entity mentioned but no registration found
- High-value transactions implied but no records

**Legal Gaps:**
- Litigation history incomplete
- Regulatory actions not checked
- International sanctions lists unchecked

**Technical Gaps:**
- Domain found but subdomains not enumerated
- Website exists but no WHOIS data retrieved
- Infrastructure changes not tracked over time

**Geographic Gaps:**
- Current address unknown
- Location history incomplete
- Travel patterns not established

**Associate Gaps:**
- Family connections unidentified
- Professional network incomplete
- Organizational affiliations missing

---

## Missing Vector Detection

### Systematic Check Process

```
FOR each vector category:
  1. List all discovered entities
  2. Cross-reference with search sources
  3. Identify unverified connections
  4. Flag negative search results
  5. Mark as: CHECKED | NOT_CHECKED | NOT_APPLICABLE
END
```

### Red Flag Indicators

These patterns indicate potential coverage gaps:

1. **Single-Source Dependencies** - Critical finding relies on only one source
2. **Temporal Gaps** - Large periods with no activity detected
3. **Platform Absence** - No presence on expected platforms for demographic
4. **Contradictory Data** - Conflicting information without resolution
5. **Unverified Pivots** - Connections suggested but not confirmed

---

## Investigation Completeness Scoring

### Calculation Formula

```
COMPLETENESS = Σ(Vector Weight × Verification Level)

Vector Weights:
- Identity: 1.5x (critical)
- Digital Presence: 1.2x (high)
- Professional: 1.0x (standard)
- Financial: 1.3x (high)
- Legal: 1.4x (critical)
- Technical: 1.0x (standard)
- Geographic: 1.1x (standard)
- Associates: 0.9x (supporting)
- Historical: 0.8x (context)
- Media: 0.9x (supporting)

Verification Levels:
- Confirmed (multiple sources): 1.0
- Likely (strong evidence): 0.8
- Possible (single source): 0.5
- Unverified (claim only): 0.2
- Not checked: 0.0
```

### Minimum Thresholds

| Investigation Type | Minimum Score | Required Vectors |
|-------------------|---------------|------------------|
| Quick Check | 35% | Identity, 2 presence platforms |
| Background Check | 60% | Identity, presence, professional, legal |
| Due Diligence | 75% | All except financial/technical |
| Full Investigation | 85% | All applicable categories |
| Deep Investigation | 90% | All categories + historical/media |

---

## Coverage Visualization

### Text-Based Heatmap

```
IDENTITY        [████████████████░░░░] 80%
DIGITAL         [████████████░░░░░░░░] 60%
PROFESSIONAL    [████████████████████] 100%
FINANCIAL       [████████░░░░░░░░░░░░] 40%
LEGAL           [██████████████░░░░░░] 70%
TECHNICAL       [████████████░░░░░░░░] 60%
GEOGRAPHIC      [██████████████░░░░░░] 70%
ASSOCIATES      [████████░░░░░░░░░░░░] 40%
HISTORICAL      [████████████░░░░░░░░] 60%
MEDIA           [██████████████░░░░░░] 70%

OVERALL: 65% (Partial Coverage)
```

### Radar Chart (Text Representation)

```
                    Identity
                       5
                       |
            Technical  |  Digital
                 4     |     5
                   \   |   /
                    \  |  /
             Legal 3 \ | / 3 Professional
                      \|/
            2----------+----------2
                      /|\
                     / | \
            Media 3 /  |  \ 3 Geographic
                   /   |   \
                  /    |    \
            Historical   Associates
                 4          2
```

---

## Recommendations for Missing Areas

### Priority Matrix

| Priority | Score Range | Action |
|----------|-------------|--------|
| **Critical** | 0-20% | Immediate attention required |
| **High** | 21-40% | Address before report finalization |
| **Medium** | 41-60% | Include in next phase |
| **Low** | 61-80% | Nice-to-have additions |
| **Optional** | 81-100% | Supplementary only |

### Automated Recommendations

**When Identity < 40%:**
- Suggest username correlation searches
- Recommend real name lookup services
- Propose alias/nickname research

**When Digital Presence < 50%:**
- Cross-platform username search
- Archived website review (Wayback Machine)
- Forum/deep web presence check

**When Professional < 60%:**
- LinkedIn deep dive
- Company registry searches
- Professional association lookups

**When Financial < 30%:**
- Property record searches
- Business registration checks
- Campaign finance lookups (if applicable)

**When Legal < 40%:**
- Court record database searches
- Regulatory action lookups
- International sanctions screening

**When Technical < 50% (domain targets):**
- Subdomain enumeration
- Historical WHOIS analysis
- Infrastructure mapping

**When Geographic < 50%:**
- Address history research
- Location-based social media analysis
- Check-in/review site searches

**When Associates < 40%:**
- Social graph analysis
- Co-occurrence searches
- Organizational membership checks

---

## /coverage Command Output

### Standard Display Format

```
═══════════════════════════════════════════════════════════════
           INVESTIGATION COVERAGE ANALYSIS
═══════════════════════════════════════════════════════════════

Target: [Investigation Subject]
Generated: [Date/Time]

┌─────────────────────────────────────────────────────────────┐
│ OVERALL COMPLETENESS: 68% (GOOD COVERAGE)                   │
└─────────────────────────────────────────────────────────────┘

COVERAGE BY CATEGORY:

✅ IDENTITY          85%  [████████████████████░░]  Verified
🟡 DIGITAL           60%  [████████████░░░░░░░░]  Partial
✅ PROFESSIONAL      95%  [███████████████████░]  Strong
🔴 FINANCIAL         25%  [█████░░░░░░░░░░░░░░░]  Minimal
🟡 LEGAL             55%  [███████████░░░░░░░░░]  Partial
🟡 TECHNICAL         60%  [████████████░░░░░░░░]  Partial
🟡 GEOGRAPHIC        65%  [█████████████░░░░░░░]  Partial
🔴 ASSOCIATES        30%  [██████░░░░░░░░░░░░░░]  Minimal
🟡 HISTORICAL        50%  [██████████░░░░░░░░░░]  Partial
🟡 MEDIA             70%  [██████████████░░░░░░]  Good

CRITICAL GAPS IDENTIFIED:
⚠️  Financial records not checked (property, business filings)
⚠️  Associate network incomplete (family, close connections)
⚠️  Historical timeline gaps 2019-2021

RECOMMENDED ACTIONS:
1. [HIGH] Search county property records for real estate holdings
2. [HIGH] Cross-reference known usernames for family connections
3. [MEDIUM] Expand timeline with social media archive searches
4. [MEDIUM] Check state business registration databases
5. [LOW] Deep search for media mentions in local press

ESTIMATED TIME TO COMPLETE: 45-60 minutes
```

---

## Usage with /gaps Command

### Gap Analysis Output

```
═══════════════════════════════════════════════════════════════
              INVESTIGATION GAP ANALYSIS
═══════════════════════════════════════════════════════════════

SUMMARY: 4 Critical Gaps | 3 High-Priority Gaps | 2 Medium Gaps

DETAILED BREAKDOWN:

🔴 CRITICAL GAPS:

1. Legal/Criminal History
   Status: NOT CHECKED
   Impact: High - Could reveal disqualifying information
   Sources to Check:
   - PACER (federal court records)
   - State court databases
   - Sex offender registries
   - International sanctions lists
   Time Required: 20 minutes

2. Financial/Business Ownership
   Status: MINIMAL COVERAGE (2/8 sources checked)
   Impact: High - May reveal undisclosed interests
   Sources to Check:
   - Secretary of State business filings
   - County property assessor records
   - UCC lien filings
   - Campaign finance disclosures
   Time Required: 25 minutes

3. Identity Verification
   Status: PARTIAL (username only, real name unverified)
   Impact: Critical - Cannot confirm target identity
   Actions:
   - Cross-reference username with real name databases
   - Verify through multiple social platforms
   - Check professional directories
   Time Required: 15 minutes

4. Technical Infrastructure (Domain Targets Only)
   Status: NOT APPLICABLE (non-domain target)
   Impact: N/A

🟡 HIGH-PRIORITY GAPS:
...

═══════════════════════════════════════════════════════════════
```

---

## Integration with Reports

### Coverage Appendix Template

Append to all reports:

```markdown
## Investigation Coverage Statement

This investigation achieved **[XX]% coverage** across [N] vector categories.

### Coverage Breakdown
- **Fully Covered**: [List categories]
- **Partially Covered**: [List categories with %]
- **Not Covered**: [List categories]

### Limitations
[Explicit statement of what was NOT found and why]

### Confidence Impact
[How coverage gaps affect overall confidence in findings]

### Recommended Follow-up
[Specific next steps to address gaps]
```

---

## Best Practices

1. **Run coverage analysis early** - Identify gaps while investigation is active
2. **Document negative searches** - "Checked X database, no results" is valuable
3. **Prioritize by risk** - Focus gaps that could change conclusions
4. **Set time limits** - Don't let gap chasing derail timeline
5. **Be explicit** - Clearly state what wasn't checked in final report

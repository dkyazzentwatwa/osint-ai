# Quality Metrics & Assurance Framework

## Overview

Quality assurance ensures investigation findings are accurate, verified, and properly attributed. This framework provides systematic quality checks throughout the OSINT process.

---

## Quality Scoring Methodology

### Quality Dimensions

| Dimension | Weight | Description |
|-----------|--------|-------------|
| **Source Reliability** | 25% | Authority and trustworthiness of sources |
| **Verification Level** | 25% | Multi-source confirmation |
| **Attribution Quality** | 20% | Proper citation and traceability |
| **Analytical Rigor** | 15% | Logical soundness of conclusions |
| **Presentation Clarity** | 15% | Clear communication of findings |

### Quality Score Calculation

```
Quality Score = Σ(Dimension Weight × Dimension Score)

Scoring Scale (per dimension):
- Excellent (90-100): Exceeds standards
- Good (75-89): Meets standards
- Acceptable (60-74): Minor issues
- Poor (40-59): Significant issues
- Unacceptable (0-39): Fails standards
```

### Overall Quality Rating

| Score | Rating | Action Required |
|-------|--------|-----------------|
| 90-100 | 🟢 **EXCELLENT** | None - report ready |
| 75-89 | 🟢 **GOOD** | Minor polishing |
| 60-74 | 🟡 **ACCEPTABLE** | Address specific issues |
| 40-59 | 🔴 **POOR** | Major revisions needed |
| 0-39 | 🔴 **UNACCEPTABLE** | Re-do investigation |

---

## Source Verification Procedures

### Source Reliability Hierarchy

#### Tier 1: Primary/Authoritative (Score: 1.0)
- Government databases (official records)
- Court filings and legal documents
- Official company registrations
- Verified social media accounts (blue check/official)
- Direct statements from subject

#### Tier 2: Professional/Reputable (Score: 0.9)
- Major news outlets (NYT, WSJ, Reuters, AP)
- Academic journals and institutions
- Professional licensing boards
- Established industry publications
- Non-partisan research organizations

#### Tier 3: Commercial/Established (Score: 0.7)
- LinkedIn profiles
- Company websites
- Industry databases
- Professional directories
- Established blogs with track record

#### Tier 4: User-Generated/Unverified (Score: 0.4)
- Anonymous forum posts
- Unverified social media accounts
- Wikipedia (use as lead only)
- Review sites (Yelp, Glassdoor)
- Personal blogs without credentials

#### Tier 5: Unreliable/Questionable (Score: 0.1)
- Rumor sites
- Unverified leaks
- Anonymous tips without evidence
- Biased advocacy sources
- Known disinformation outlets

### Verification Checklist

For each source used, verify:

- [ ] **Authenticity** - Source is what it claims to be
- [ ] **Currency** - Information is up-to-date
- [ ] **Authority** - Source has relevant expertise
- [ ] **Objectivity** - Minimal bias detected
- [ ] **Corroboration** - Supported by other sources
- [ ] **Accessibility** - URL/archive link available
- [ ] **Context** - Information properly contextualized

### Cross-Verification Requirements

| Finding Type | Minimum Sources | Verification Method |
|--------------|-----------------|---------------------|
| Critical fact | 3+ independent | All Tier 1-2 |
| Important detail | 2+ independent | Tier 1-3 acceptable |
| Supporting info | 1+ | Tier 1-4 acceptable |
| Background context | 1 | Any tier with caveat |

---

## Redundancy Detection

### Duplicate Identification

Check for these redundancy types:

**Source Redundancy:**
- Same article syndicated across outlets
- Press releases republished verbatim
- Aggregator sites copying original content

**Finding Redundancy:**
- Same fact stated multiple times in report
- Circular references (A cites B cites A)
- Multiple sources quoting same original

**Analysis Redundancy:**
- Repetitive conclusions
- Overlapping evidence presented multiple ways
- Duplicate entity mappings

### Redundancy Scoring

```
Redundancy Impact = (Duplicate Findings / Total Findings) × 100

< 10%: Excellent - minimal redundancy
10-20%: Good - acceptable levels
20-30%: Fair - some consolidation needed
> 30%: Poor - significant redundancy
```

### Consolidation Process

```
1. Identify duplicate sources
2. Select most authoritative version
3. Archive alternatives for reference
4. Update citations to primary source
5. Note source variations if material
```

---

## Bias Detection Rules

### Bias Types to Monitor

#### Confirmation Bias
- **Sign**: Only searching for supporting evidence
- **Detection**: Review search query logs for one-sided terms
- **Mitigation**: Actively search for contradictory evidence

#### Selection Bias
- **Sign**: Cherry-picking sources that support narrative
- **Detection**: Compare source diversity to claim confidence
- **Mitigation**: Include sources across spectrum

#### Attribution Bias
- **Sign**: Crediting sources based on agreement level
- **Detection**: Review if disliked sources were dismissed
- **Mitigation**: Evaluate sources on merits, not alignment

#### Anchoring Bias
- **Sign**: Over-relying on first information found
- **Detection**: Check if early findings dominated analysis
- **Mitigation**: Re-evaluate with fresh perspective

#### Availability Bias
- **Sign**: Overweighting recent or vivid examples
- **Detection**: Verify if search recency affected results
- **Mitigation**: Include historical/archived sources

### Bias Audit Checklist

For each major finding:

- [ ] Searched for contradictory evidence?
- [ ] Included sources from multiple perspectives?
- [ ] Evaluated sources on authority, not agreement?
- [ ] Re-examined initial assumptions?
- [ ] Considered what information might be missing?
- [ ] Separated facts from interpretations?
- [ ] Acknowledged uncertainty appropriately?

### Bias Score

```
Bias Risk = (Bias Indicators Found / Total Checks) × 100

< 10%: Low bias risk
10-25%: Moderate bias - review findings
25-50%: High bias - significant concerns
> 50%: Severe bias - investigation compromised
```

---

## Investigation Health Check

### Health Metrics Dashboard

```
═══════════════════════════════════════════════════════════════
           INVESTIGATION HEALTH CHECK
═══════════════════════════════════════════════════════════════

OVERALL HEALTH: 🟢 HEALTHY / 🟡 CAUTION / 🔴 CRITICAL

METRICS:

Source Quality     ████████████░░░░░░░░ 62%  🟡
├─ Tier 1 sources: 12
├─ Tier 2 sources: 18
├─ Tier 3 sources: 15
└─ Tier 4+ sources: 5

Verification       ████████████████░░░░ 80%  🟢
├─ Triple-verified: 8 findings
├─ Double-verified: 15 findings
├─ Single-source: 12 findings
└─ Unverified: 3 findings

Citation Quality   ██████████████░░░░░░ 70%  🟡
├─ Full citations: 28
├─ Partial citations: 8
└─ Missing citations: 2

Bias Risk          ████████████░░░░░░░░ 60%  🟡
├─ Confirmation checks: 6/10
├─ Perspective diversity: Moderate
└─ Contradictory findings: 3 noted

Redundancy         ██████████████████░░ 90%  🟢
├─ Unique findings: 95%
├─ Consolidated duplicates: 5
└─ Remaining redundancy: Low

RECOMMENDATIONS:
⚠️  Increase Tier 1 sources (target: 20+)
⚠️  Verify 3 single-source findings
✅  Citation quality acceptable
⚠️  Complete 4 remaining bias checks
✅  Redundancy well managed
```

### Health Thresholds

| Metric | Healthy | Caution | Critical |
|--------|---------|---------|----------|
| Source Quality | >70% | 50-70% | <50% |
| Verification | >75% | 60-75% | <60% |
| Citation | >80% | 65-80% | <65% |
| Bias Risk | <15% | 15-30% | >30% |
| Redundancy | <15% | 15-25% | >25% |

---

## Quality Improvement Suggestions

### Automated Recommendations

Based on health check, suggest:

**If Source Quality < 60%:**
- Prioritize Tier 1-2 sources
- Replace Tier 4-5 sources with better alternatives
- Seek official documentation

**If Verification < 60%:**
- Identify critical single-source findings
- Conduct additional corroboration searches
- Downgrade confidence ratings appropriately

**If Citation < 65%:**
- Review all findings without URLs
- Add archive links for all web sources
- Document offline source retrieval method

**If Bias Risk > 30%:**
- Conduct adversarial search (look for contradictory evidence)
- Add sources from opposing perspectives
- Re-examine initial hypothesis

**If Redundancy > 25%:**
- Consolidate similar findings
- Eliminate duplicate sources
- Streamline analysis sections

### Quality Improvement Workflow

```
1. Run /qa-check to assess current quality
2. Review flagged issues by priority
3. Address critical issues immediately
4. Schedule medium issues for next phase
5. Document low-priority suggestions
6. Re-run /qa-check after improvements
7. Repeat until quality thresholds met
```

---

## /qa-check Command Output

### Standard Format

```
═══════════════════════════════════════════════════════════════
         QUALITY ASSURANCE CHECK
═══════════════════════════════════════════════════════════════

Investigation: [Subject/Case]
Check Date: [Date/Time]
Analyst: [Name/System]

┌─────────────────────────────────────────────────────────────┐
│ OVERALL QUALITY SCORE: 74/100 (ACCEPTABLE)                 │
│ STATUS: 🟡 Minor improvements recommended                   │
└─────────────────────────────────────────────────────────────┘

DETAILED SCORING:

Source Reliability      18/25  🟡
├─ Tier 1-2 sources: 45% (target: 60%)
├─ Tier 3 sources: 35%
└─ Tier 4-5 sources: 20% (reduce to <15%)

Verification Level      22/25  🟢
├─ Multi-source confirmed: 65%
├─ Single-source: 30%
└─ Unverified: 5%

Attribution Quality     18/20  🟢
├─ Full URLs: 85%
├─ Archive links: 60%
└─ Missing citations: 2

Analytical Rigor        10/15  🟡
├─ Logical conclusions: Strong
├─ Caveats noted: Partial
└─ Speculation labeled: Yes

Presentation Clarity    12/15  🟢
├─ Clear organization: Yes
├─ Jargon explained: Mostly
└─ Visual aids: Present

═══════════════════════════════════════════════════════════════
                        ISSUES FOUND
═══════════════════════════════════════════════════════════════

🔴 CRITICAL (Must Fix):
None

🟡 HIGH (Should Fix):
1. Finding #14 relies on single Tier 4 source
   → Find additional verification
   
2. Missing archive links for 8 web sources
   → Add archive.today or Wayback links

🟢 MEDIUM (Nice to Have):
1. Consider adding opposing perspective sources
2. Consolidate redundant findings #22 and #23

═══════════════════════════════════════════════════════════════
                    IMPROVEMENT ACTIONS
═══════════════════════════════════════════════════════════════

To reach GOOD quality (75+):
- Replace 3 Tier 4 sources with Tier 2-3
- Verify 1 critical single-source finding
- Add archive links for 8 sources

Estimated time: 30 minutes
Expected new score: 82/100

═══════════════════════════════════════════════════════════════
```

---

## /verify-sources Command

### Source Validation Output

```
═══════════════════════════════════════════════════════════════
              SOURCE VERIFICATION CHECK
═══════════════════════════════════════════════════════════════

Checking [N] sources for accessibility and validity...

VALID SOURCES:     28 ✅
├─ Direct accessible: 22
└─ Archive accessible: 6

BROKEN SOURCES:    3 ❌
1. https://example.com/page (404 Not Found)
   → Archive available: https://archive.today/abc123
   
2. https://old-site.com/article (Domain expired)
   → Archive available: https://web.archive.org/web/...
   
3. https://temp-link.com/doc (Timeout)
   → Alternative: https://new-site.com/doc

CONTENT CHANGED:   2 ⚠️
1. https://example.com/profile
   → Content modified since citation
   → Archive original: https://archive.today/xyz789
   
2. https://company.com/about
   → Page restructured
   → Original content preserved in archive

═══════════════════════════════════════════════════════════════
```

---

## Quality Assurance Best Practices

1. **Run QA check at milestones** - Not just at end
2. **Document quality decisions** - Why was X source accepted?
3. **Be transparent about limitations** - State quality issues upfront
4. **Prioritize by impact** - Focus on critical findings first
5. **Maintain QA log** - Track quality improvements over time
6. **Peer review when possible** - Second set of eyes catches bias
7. **Continuous improvement** - Learn from each investigation

---

## Integration with Reports

### Quality Statement Template

Append to reports:

```markdown
## Quality Assurance Statement

This investigation has been evaluated against quality standards:

**Quality Score:** [X]/100 ([Rating])

**Source Breakdown:**
- Authoritative sources: [N] ([%])
- Professional sources: [N] ([%])
- Commercial/UGC sources: [N] ([%])

**Verification Status:**
- Multi-source confirmed: [N] findings
- Single-source: [N] findings (caveated)
- Unverified: [N] findings (noted)

**Limitations:**
[Explicit quality limitations affecting findings]

**Analyst Confidence:**
[Overall confidence statement based on quality metrics]
```

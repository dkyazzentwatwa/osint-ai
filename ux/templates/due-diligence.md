# Company Due Diligence Template

A comprehensive 7-step investigation workflow for evaluating companies.

---

## Template Overview

**Purpose:** Evaluate company background, legitimacy, and risk factors  
**Use Cases:** Vendor vetting, investment research, partnership verification  
**Duration:** 15-25 minutes  
**Complexity:** Intermediate  
**Output:** Risk-scored report with findings  

### Required Information

- Company name (required)
- Website domain (required)
- Location/country (optional but recommended)
- Industry (optional)

---

## Step-by-Step Workflow

### Step 1: Company Registration Verification

**Objective:** Confirm legal existence and registration status

**Questions Answered:**
- Is this a legally registered entity?
- When was it established?
- What is its registration number?
- Who are the registered officers?

**Search Queries:**
```
/recon {{company_name}} --type registration
/dork "{{company_name}}" ("registered" OR "incorporated" OR "LLC" OR "Ltd")
/dork "{{company_name}}" "business registration" {{location}}
```

**Expected Findings:**
| Finding | Good Sign | Red Flag |
|---------|-----------|----------|
| Registration record | Found in official registry | No record found |
| Registration date | 2+ years old | Very recent (<6 months) |
| Registered address | Physical location | PO box only |
| Registered agent | Named individual | Third-party only |
| Status | Active/Good standing | Dissolved/Suspended |

**Risk Indicators:**
- No official registration found
- Recently registered with high activity claims
- Multiple name changes in short period
- Registered in high-risk jurisdictions

---

### Step 2: Digital Presence Mapping

**Objective:** Map online footprint and verify consistency

**Questions Answered:**
- What is their web presence?
- Are social media accounts legitimate?
- Is branding consistent across platforms?
- How long have they been active online?

**Search Queries:**
```
/recon {{domain}}
/dork site:{{domain}} "about us" OR "team" OR "contact"
/dork "{{company_name}}" (LinkedIn OR Twitter OR Facebook) 
/timeline "{{domain}}" --years 5
```

**Expected Findings:**

| Platform | What to Check | Good Sign | Red Flag |
|----------|---------------|-----------|----------|
| Website | Domain age, professionalism | 2+ years, complete info | New domain, placeholder content |
| LinkedIn | Employee count, activity | 10+ employees, recent posts | 0-2 employees, no posts |
| Twitter | Followers, engagement | Gradual growth, replies | Fake followers, no engagement |
| Facebook | Reviews, activity | Active with customer reviews | No reviews, inactive |

**Risk Indicators:**
- Domain registered < 6 months ago
- Social media accounts created recently
- No verifiable employees on LinkedIn
- Inconsistent branding across platforms
- Copied content from other websites

---

### Step 3: Leadership Team Research

**Objective:** Verify key personnel exist and have credible backgrounds

**Questions Answered:**
- Who are the founders/executives?
- Do they have verifiable backgrounds?
- Have they been associated with other companies?
- Any concerning history?

**Search Queries:**
```
/recon "{{ceo_name}}" OR "{{founder_name}}"
/dork "{{executive_name}}" {{company_name}} LinkedIn
/pivot "{{executive_name}}" --previous-companies
/dork "{{executive_name}}" (lawsuit OR fraud OR scandal)
```

**Expected Findings:**

| Check | Positive | Negative |
|-------|----------|----------|
| Executive presence | Found on LinkedIn with history | No online presence |
| Previous experience | Verifiable at past companies | Gaps or unverifiable claims |
| Education | Confirmed institutions | Unknown or fake schools |
| Other affiliations | Related industry experience | Unrelated or no experience |
| Media mentions | Positive industry coverage | Negative press |

**Risk Indicators:**
- Executives with no prior verifiable work history
- Fake LinkedIn profiles (stock photos, few connections)
- History of failed companies
- Undisclosed conflicts of interest
- Criminal or civil litigation history

---

### Step 4: Legal Record Check

**Objective:** Identify lawsuits, regulatory actions, or legal issues

**Questions Answered:**
- Are there active lawsuits?
- Any regulatory violations?
- Bankruptcy filings?
- Intellectual property disputes?

**Search Queries:**
```
/dork "{{company_name}}" (lawsuit OR litigation OR "legal action")
/dork "{{company_name}}" (SEC OR "securities" OR "regulatory")
/dork "{{company_name}}" bankruptcy OR insolvency
/dork "{{company_name}}" trademark OR copyright OR patent dispute
```

**Expected Findings:**

| Record Type | Normal | Concerning |
|-------------|--------|------------|
| Minor litigation | 1-2 small claims | Multiple significant lawsuits |
| Regulatory filings | Standard disclosures | Violations or penalties |
| IP disputes | Occasional | Frequent infringement claims |
| Bankruptcy | None for operating companies | Recent or repeated filings |

**Risk Indicators:**
- Multiple ongoing lawsuits
- Regulatory enforcement actions
- Recent bankruptcy or restructuring
- Pattern of contract disputes
- Consumer complaint patterns

---

### Step 5: Reputation Analysis

**Objective:** Assess public perception and customer experiences

**Questions Answered:**
- What do customers say?
- Industry reputation?
- Any scandals or controversies?
- Media coverage tone?

**Search Queries:**
```
/dork "{{company_name}}" review OR rating OR testimonial
/dork "{{company_name}}" scam OR fraud OR complaint
/dork "{{company_name}}" BBB OR "Better Business Bureau"
/news "{{company_name}}" --sentiment
```

**Expected Findings:**

| Source | What to Look For | Risk Signs |
|--------|------------------|------------|
| BBB rating | A+ to F rating | F rating, unresolved complaints |
| Trustpilot/G2 | 3.5+ stars, recent reviews | <2 stars, no recent reviews |
| Reddit/forums | Honest customer discussions | Multiple complaint threads |
| News articles | Balanced coverage | Negative investigative pieces |
| Glassdoor | 3+ stars, consistent reviews | <2.5 stars, frequent CEO complaints |

**Risk Indicators:**
- Pattern of unresolved customer complaints
- Accusations of fraud or scam
- Negative investigative journalism
- High employee turnover complaints
- Fake review patterns (all 5-star, similar wording)

---

### Step 6: Financial Snapshot

**Objective:** Assess financial health and stability indicators

**Questions Answered:**
- Is the company financially stable?
- Revenue claims verifiable?
- Funding history legitimate?
- Any financial red flags?

**Search Queries:**
```
/dork "{{company_name}}" (funding OR investment OR "raised" OR "Series")
/dork "{{company_name}}" revenue OR "annual report" OR "financial results"
/dork "{{company_name}}" Crunchbase OR Pitchbook OR "funding round"
/recon {{domain}} --financial-indicators
```

**Expected Findings:**

| Indicator | Positive | Negative |
|-----------|----------|----------|
| Funding history | Verifiable rounds, named investors | Unverifiable claims |
| Revenue claims | Consistent across sources | Wildly varying numbers |
| Growth trajectory | Steady growth | Declining or stagnant |
| Financial disclosures | Transparent | Secretive or evasive |

**Risk Indicators:**
- Unverifiable funding claims
- Revenue numbers that don't add up
- Unpaid vendor or tax liens
- Sudden changes in financial reporting
- Refusal to provide basic financial info

---

### Step 7: Risk Assessment & Scoring

**Objective:** Synthesize findings into actionable risk score

**Scoring Matrix:**

| Category | Weight | Score 1-10 |
|----------|--------|------------|
| Legal/Registration | 20% | _ |
| Digital Presence | 15% | _ |
| Leadership | 20% | _ |
| Legal Issues | 20% | _ |
| Reputation | 15% | _ |
| Financial | 10% | _ |

**Risk Levels:**

| Score | Level | Recommendation |
|-------|-------|----------------|
| 0-2 | Very Low | Proceed with standard precautions |
| 3-4 | Low | Minor concerns, document verification |
| 5-6 | Medium | Additional due diligence recommended |
| 7-8 | High | Significant concerns, legal review advised |
| 9-10 | Very High | Avoid or extreme caution |

**Report Generation Triggers:**
- Automatically generated upon completion
- Triggered by /report command
- Auto-saved to investigation history
- Exportable to PDF/CSV

---

## Risk Assessment Integration

### Automatic Risk Flags

```
Critical Flags (Auto-highlight):
  🛑 No legal registration found
  🛑 Active fraud lawsuits
  🛑 Fake executive profiles
  🛑 Recent bankruptcy
  🛑 Regulatory sanctions

Warning Flags (Review recommended):
  ⚠️ Recent company formation
  ⚠️ Minimal digital footprint
  ⚠️ Unverifiable funding claims
  ⚠️ Pattern of complaints
  ⚠️ High employee turnover

Info Flags (Note for context):
  ℹ️ Young company (under 2 years)
  ℹ️ Small team (under 10)
  ℹ️ Limited public information
  ℹ️ Privately held
```

### Risk Calculation Example

```
Company: Example Corp

Category Scores:
  Legal/Registration: 2/10 (Properly registered)
  Digital Presence: 4/10 (New website, small social presence)
  Leadership: 3/10 (Verifiable but limited history)
  Legal Issues: 1/10 (No issues found)
  Reputation: 5/10 (Mixed reviews, mostly positive)
  Financial: 6/10 (Unverifiable revenue claims)

Weighted Calculation:
  (2×0.20) + (4×0.15) + (3×0.20) + (1×0.20) + (5×0.15) + (6×0.10) = 3.25

Final Risk Score: 3.3/10 (LOW RISK)

Recommendation: Proceed with standard verification.
              Document unverifiable revenue claims.
```

---

## Report Generation

### Report Sections

```
1. Executive Summary
   - Overall risk score
   - Key findings (3-5 bullets)
   - Recommendation

2. Company Profile
   - Legal name and registration
   - Business address
   - Industry and size

3. Digital Footprint
   - Website analysis
   - Social media presence
   - Online activity timeline

4. Leadership Assessment
   - Key personnel identified
   - Background verification
   - Experience review

5. Legal and Compliance
   - Litigation summary
   - Regulatory status
   - IP portfolio

6. Reputation Analysis
   - Customer reviews summary
   - Media coverage
   - Industry standing

7. Financial Overview
   - Funding history
   - Revenue indicators
   - Stability assessment

8. Risk Factors
   - Identified concerns
   - Mitigation recommendations
   - Red flag summary

9. Recommendations
   - Next steps
   - Additional verification needed
   - Timeline considerations

10. Appendix
    - Source list
    - Search methodology
    - Data snapshot date
```

### Export Options

```
/report export pdf
  → Professional formatted PDF
  → Include all sections
  → Charts and graphs

/report export csv
  → Structured data format
  → Findings in table format
  → Import to spreadsheet

/report export json
  → Machine-readable format
  → Complete metadata
  → API integration ready
```

---

## Template Execution

### Run Command

```
/template run due-diligence

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Company Due Diligence Investigation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This will perform a 7-step investigation of a company.
Estimated time: 15-25 minutes.

Required information:
  • Company name: _
  • Website domain: _
  • Location (optional): _
  • Industry (optional): _

Your answers will be saved for this investigation.

[Begin] [Cancel]
```

### Customization Options

```
Add flags for specific focus:

/template run due-diligence --deep
  → Extended investigation (30-45 min)
  → Includes executive deep-dives
  → Financial analysis expanded

/template run due-diligence --quick
  → Abbreviated investigation (5-10 min)
  → Core checks only
  → Surface-level assessment

/template run due-diligence --legal-focus
  → Emphasis on legal/compliance
  → Extended litigation search
  → Regulatory database checks
```

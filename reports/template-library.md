# Template Library v2.0

Comprehensive report templates for OSINT investigations.

---

## 1. Executive Brief (1 Page)

### Purpose
Ultra-condensed summary for decision-makers with limited time.

### Template
```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                           EXECUTIVE BRIEF                                     ║
║                      OSINT Investigation Summary                              ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  CASE: [CASE NAME]                              DATE: [DATE]                  ║
║  CLASSIFICATION: [LEVEL]                        PAGES: 1 of 1                 ║
║                                                                               ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  SITUATION                                                                    ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  [2-3 sentences describing what was investigated and why]                     ║
║                                                                               ║
║  KEY FINDINGS                                                                 ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  1. [Most important finding - one sentence]                                   ║
║  2. [Second most important finding - one sentence]                            ║
║  3. [Third most important finding - one sentence]                             ║
║                                                                               ║
║  RISK ASSESSMENT                                                              ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Overall Risk: [CRITICAL/HIGH/MEDIUM/LOW]                                     ║
║                                                                               ║
║  [█] Financial        [█] Reputational        [░] Physical                    ║
║  [█] Legal            [░] Operational         [░] Cyber                       ║
║                                                                               ║
║  █ = Significant risk identified    ░ = No significant risk identified        ║
║                                                                               ║
║  IMMEDIATE ACTIONS REQUIRED                                                   ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  □ [Action 1 - highest priority]                                              ║
║  □ [Action 2 - second priority]                                               ║
║  □ [Action 3 - third priority]                                                ║
║                                                                               ║
║  NEXT REVIEW: [DATE]    REPORT BY: [ANALYST]    CONFIDENCE: [X%]             ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

### Usage Notes
- Maximum 1 page
- No technical jargon
- Focus on actionable intelligence
- Include confidence level prominently

---

## 2. Technical INTSUM (Intelligence Summary)

### Purpose
Detailed intelligence summary for analysts and technical staff.

### Template
```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                    TECHNICAL INTELLIGENCE SUMMARY                             ║
║                       OSINT Investigation Report                              ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  DOCUMENT CONTROL                                                             ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Report ID:     [UNIQUE-ID]                                                   ║
║  Case Name:     [CASE NAME]                                                   ║
║  Classification:[LEVEL]                                                       ║
║  Date:          [YYYY-MM-DD]                                                  ║
║  Analyst:       [NAME]                                                        ║
║  Reviewer:      [NAME]                                                        ║
║  Distribution:  [LIST]                                                        ║
║                                                                               ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  1. EXECUTIVE SUMMARY                                                         ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  1.1 Key Judgments                                                            ║
║      • [Judgment 1 with confidence level]                                     ║
║      • [Judgment 2 with confidence level]                                     ║
║      • [Judgment 3 with confidence level]                                     ║
║                                                                               ║
║  1.2 Intelligence Gaps                                                        ║
║      • [Gap 1 - what is unknown]                                              ║
║      • [Gap 2 - what is unknown]                                              ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  2. TARGET PROFILE                                                            ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  2.1 Identity                                                                 ║
║      Primary Name:      [NAME]                                                ║
║      Known Aliases:     [LIST]                                                ║
║      Date of Birth:     [YYYY-MM-DD]                                          ║
║      Nationality:       [COUNTRY]                                             ║
║      Current Location:  [LOCATION]                                            ║
║                                                                               ║
║  2.2 Digital Footprint                                                        ║
║      Primary Email:     [EMAIL]                                               ║
║      Secondary Emails:  [LIST]                                                ║
║      Phone Numbers:     [LIST]                                                ║
║      Social Media:      [PLATFORM: HANDLE]                                    ║
║      Websites:          [LIST]                                                ║
║      IP Addresses:      [LIST]                                                ║
║                                                                               ║
║  2.3 Network Associations                                                     ║
║      [Relationship diagram or table]                                          ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  3. FINDINGS ANALYSIS                                                         ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  3.1 Finding 1: [TITLE]                                                       ║
║      Severity:          [CRITICAL/HIGH/MEDIUM/LOW]                            ║
║      Confidence:        [X%]                                                  ║
║      Sources:           [NUMBER]                                              ║
║      Description:       [Detailed description]                                ║
║      Evidence:          [Summary of supporting data]                          ║
║      Implications:      [What this means]                                     ║
║                                                                               ║
║  3.2 Finding 2: [TITLE]                                                       ║
║      [Same structure as above]                                                ║
║                                                                               ║
║  [Continue for all findings...]                                               ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  4. SOURCE ASSESSMENT                                                         ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  4.1 Source Matrix                                                            ║
║      ┌────────────────────┬─────────────┬─────────────┬───────────────────┐  ║
║      │ Source             │ Reliability │ Date        │ Confidence Impact │  ║
║      ├────────────────────┼─────────────┼─────────────┼───────────────────┤  ║
║      │ [Source 1]         │ A/B/C/D/E/F │ [Date]      │ [High/Med/Low]    │  ║
║      │ [Source 2]         │ A/B/C/D/E/F │ [Date]      │ [High/Med/Low]    │  ║
║      └────────────────────┴─────────────┴─────────────┴───────────────────┘  ║
║                                                                               ║
║  4.2 Source Reliability Grading                                               ║
║      A - Completely reliable     D - Not usually reliable                     ║
║      B - Usually reliable        E - Unreliable                               ║
║      C - Fairly reliable         F - Reliability cannot be judged             ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  5. INTELLIGENCE GAPS                                                         ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  5.1 Known Unknowns                                                           ║
║      • [What information is missing and why it matters]                       ║
║      • [Additional gaps...]                                                   ║
║                                                                               ║
║  5.2 Recommended Collection                                                   ║
║      • [What sources/methods could fill gaps]                                 ║
║      • [Additional recommendations...]                                        ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  6. APPENDICES                                                                ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Appendix A: Raw Data Extracts                                                ║
║  Appendix B: Technical Indicators (IOCs)                                      ║
║  Appendix C: Timeline of Events                                               ║
║  Appendix D: Entity Relationship Diagram                                      ║
║  Appendix E: Source Material References                                       ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3. Simple Summary (8th Grade Reading Level)

### Purpose
Accessible summary for non-technical stakeholders and public communication.

### Template
```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                         INVESTIGATION SUMMARY                                 ║
║                   What We Found and What It Means                             ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  WHAT WE WERE ASKED TO DO                                                     ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Organization/Name] asked us to find information about [subject].            ║
║  They wanted to know [what the investigation was supposed to answer].         ║
║                                                                               ║
║  WHAT WE DID                                                                  ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  We looked at public information from:                                        ║
║  • [Source 1 - explained simply, e.g., "websites and social media"]           ║
║  • [Source 2 - explained simply, e.g., "public records like court documents"] ║
║  • [Source 3 - explained simply]                                              ║
║                                                                               ║
║  We did not:                                                                  ║
║  • Hack into any private systems                                              ║
║  • Access any password-protected information                                  ║
║  • Break any laws or rules                                                    ║
║                                                                               ║
║  WHAT WE FOUND                                                                ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  We found [number] important things:                                          ║
║                                                                               ║
║  1. [Finding 1 in plain language]                                             ║
║     What this means: [Why it matters]                                         ║
║     How sure we are: [Confidence level in simple terms]                       ║
║                                                                               ║
║  2. [Finding 2 in plain language]                                             ║
║     What this means: [Why it matters]                                         ║
║     How sure we are: [Confidence level in simple terms]                       ║
║                                                                               ║
║  3. [Finding 3 in plain language]                                             ║
║     What this means: [Why it matters]                                         ║
║     How sure we are: [Confidence level in simple terms]                       ║
║                                                                               ║
║  HOW RISKY THIS IS                                                            ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Overall risk level: [LOW/MEDIUM/HIGH/CRITICAL explained]                     ║
║                                                                               ║
║  This means: [What the risk level actually means in practical terms]          ║
║                                                                               ║
║  Main risks we identified:                                                    ║
║  [ ] Financial risk        [ ] Reputation risk                                ║
║  [ ] Safety risk           [ ] Legal risk                                     ║
║  [ ] Privacy risk          [ ] Other: [explain]                               ║
║                                                                               ║
║  WHAT SHOULD HAPPEN NEXT                                                      ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  We recommend:                                                                ║
║                                                                               ║
║  1. [Action 1 in plain language - why it should be done]                      ║
║  2. [Action 2 in plain language - why it should be done]                      ║
║  3. [Action 3 in plain language - why it should be done]                      ║
║                                                                               ║
║  WHAT WE DON'T KNOW                                                           ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  There are still some things we don't know:                                   ║
║  • [Gap 1 - explained simply]                                                 ║
║  • [Gap 2 - explained simply]                                                 ║
║                                                                               ║
║  We could find out more by:                                                   ║
║  • [Method 1]                                                                 ║
║  • [Method 2]                                                                 ║
║                                                                               ║
║  QUESTIONS?                                                                   ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  If you have questions about this report, contact:                            ║
║  [Name], [Title]                                                              ║
║  Email: [email]                                                               ║
║  Phone: [phone]                                                               ║
║                                                                               ║
║  Report date: [Date]                                                          ║
║  Report by: [Analyst name]                                                    ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

### Writing Guidelines
- Use short sentences (average 15 words)
- Avoid jargon; explain technical terms when used
- Use active voice
- Include concrete examples
- Flesch-Kincaid Grade Level target: 8.0 or below

---

## 4. Legal Brief (Evidence-Focused)

### Purpose
Report formatted for legal proceedings with proper evidence chain.

### Template
```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                         LEGAL INTELLIGENCE BRIEF                              ║
║                    For Attorney Work Product / Litigation                     ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  ATTORNEY WORK PRODUCT - CONFIDENTIAL                                         ║
║  Prepared at the direction of counsel for litigation purposes                 ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  MATTER INFORMATION                                                           ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Matter Name:        [CASE NAME]                                              ║
║  Matter Number:      [NUMBER]                                                 ║
║  Client:             [CLIENT NAME]                                            ║
║  Responsible Attorney:[ATTORNEY NAME]                                         ║
║  Prepared By:        [INVESTIGATOR NAME]                                      ║
║  Date Prepared:      [DATE]                                                   ║
║  Version:            [VERSION NUMBER]                                         ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  EVIDENCE PRESERVATION CERTIFICATION                                          ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  I certify that:                                                              ║
║  □ All data was collected in a forensically sound manner                      ║
║  □ Chain of custody has been maintained for all physical/digital evidence     ║
║  □ Screenshots/timestamps were captured at time of collection                 ║
║  □ All sources are publicly available and legally obtained                    ║
║  □ No private systems were accessed without authorization                     ║
║  □ All applicable laws and regulations were followed                          ║
║                                                                               ║
║  Signature: _________________________ Date: _______________                   ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  EXECUTIVE SUMMARY FOR LEGAL TEAM                                             ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [2-3 paragraphs summarizing findings relevant to legal strategy]             ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  FINDINGS OF FACT                                                             ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Finding 1: [FACTUAL STATEMENT]                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Factual Statement:                                                           ║
║  [Neutral, objective statement of fact]                                       ║
║                                                                               ║
║  Supporting Evidence:                                                         ║
║  • Exhibit 1: [Description] - [Hash/MD5 if applicable]                        ║
║  • Exhibit 2: [Description] - [Hash/MD5 if applicable]                        ║
║                                                                               ║
║  Source Information:                                                          ║
║  URL: [Exact URL]                                                             ║
║  Date Accessed: [Timestamp]                                                   ║
║  Screenshot: [File reference]                                                 ║
║  Archive Link: [Wayback Machine or similar]                                   ║
║                                                                               ║
║  Authentication Notes:                                                        ║
║  [How the evidence was verified]                                              ║
║                                                                               ║
║  Relevance to Matter:                                                         ║
║  [How this fact relates to the legal case]                                    ║
║                                                                               ║
║  [Repeat for each finding...]                                                 ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  EVIDENCE INVENTORY                                                           ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  ┌──────────┬──────────────────┬───────────────┬───────────────┬───────────┐ ║
║  │ Exhibit  │ Description      │ Date Collected│ Hash (SHA256) │ Location  │ ║
║  ├──────────┼──────────────────┼───────────────┼───────────────┼───────────┤ ║
║  │ 001      │ [Description]    │ [Date]        │ [Hash]        │ [Path]    │ ║
║  │ 002      │ [Description]    │ [Date]        │ [Hash]        │ [Path]    │ ║
║  └──────────┴──────────────────┴───────────────┴───────────────┴───────────┘ ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  WITNESS/ENTITY INFORMATION                                                   ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Entity 1: [NAME]                                                             ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Full Name:              [Full legal name]                                    ║
║  Known Aliases:          [List]                                               ║
║  Date of Birth:          [DOB]                                                ║
║  Address:                [Current/previous addresses]                         ║
║  Contact Information:    [Phone/email]                                        ║
║  Employer:               [Current/previous]                                   ║
║  Position:               [Title]                                              ║
║  Professional Licenses:  [If applicable]                                      n║  Court Records:          [Relevant filings]                                   ║
║  Regulatory Actions:     [Sanctions, etc.]                                    ║
║  Criminal History:       [If applicable and relevant]                         ║
║                                                                               ║
║  [Repeat for each entity...]                                                  ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  TIMELINE OF RELEVANT EVENTS                                                  ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  ┌─────────────┬────────────────────────────────────────────────────────────┐ ║
║  │ Date        │ Event                                                      │ ║
║  ├─────────────┼────────────────────────────────────────────────────────────┤ ║
║  │ YYYY-MM-DD  │ [Event description with legal significance]                │ ║
║  │ YYYY-MM-DD  │ [Event description with legal significance]                │ ║
║  └─────────────┴────────────────────────────────────────────────────────────┘ ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  ANALYSIS AND CONCLUSIONS                                                     ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Investigator's analysis of findings and their implications for the case]    ║
║                                                                               ║
║  Limitations:                                                                 ║
║  • [What could not be determined]                                             ║
║  • [Information gaps]                                                         ║
║  • [Assumptions made]                                                         ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  METHODOLOGY STATEMENT                                                        ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Detailed description of investigation methods for potential testimony]      ║
║                                                                               ║
║  Qualifications of Investigator:                                              ║
║  [Credentials and experience]                                                 ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  CERTIFICATION                                                                ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  I hereby certify that:                                                       ║
║                                                                               ║
║  1. The information contained in this report is true and accurate to the     ║
║     best of my knowledge and belief;                                          ║
║  2. All information was obtained legally and ethically;                       ║
║  3. I have not omitted any material facts;                                    ║
║  4. I am competent to perform this investigation.                             ║
║                                                                               ║
║  _______________________________      _______________                         ║
║  [Investigator Signature]             [Date]                                  ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

---

## 5. Journalist Report (Source-Heavy)

### Purpose
Report formatted for media publication with extensive source attribution.

### Template
```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                        INVESTIGATIVE REPORT                                   ║
║                    For Editorial / Publication                                ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  WORKING TITLE: [Headline suggestion]                                         ║
║  BYLINE: [Reporter names]                                                     ║
║  DATE: [Date]                                                                 ║
║  PUBLICATION: [Outlet]                                                        ║
║  CLASSIFICATION: [Draft/Ready for Review/Ready for Publication]               ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  EDITOR'S SUMMARY                                                             ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [2-3 paragraph summary for editors - what's the story, why it matters]       ║
║                                                                               ║
║  News Value:                                                                  ║
║  □ Timeliness        □ Impact           □ Prominence                          ║
║  □ Proximity         □ Conflict         □ Human Interest                      ║
║  □ Novelty           □ Relevance        □ [Other]                             ║
║                                                                               ║
║  Legal Review Required: □ Yes □ No    Libel Risk: □ Low □ Med □ High         ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  LEAD / NUT GRAF                                                              ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Strong opening paragraph - the "lead"]                                      ║
║                                                                               ║
║  ["Nut graph" - paragraph explaining why this story matters]                  ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  BODY OF REPORT                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Section 1: Background]                                                      ║
║  [Context and history]                                                        ║
║                                                                               ║
║  Source: [Name, title, how they know this] [Contact info for verification]    ║
║  Quote: "[Exact quote]"                                                       ║
║  Verification: [How this was confirmed]                                       ║
║                                                                               ║
║  ───                                                                          ║
║                                                                               ║
║  [Section 2: Main Finding]                                                    ║
║  [Key revelation]                                                             ║
║                                                                               ║
║  Document/Source: [Description]                                               ║
║  Where obtained: [How the information was acquired]                           ║
║  Authentication: [How the source/document was verified]                       ║
║                                                                               ║
║  ───                                                                          ║
║                                                                               ║
║  [Section 3: Supporting Evidence]                                             ║
║  [Additional details]                                                         ║
║                                                                               ║
║  Data Point 1: [Fact]                                                         ║
║  Source: [Attribution]                                                        ║
║  URL/Document: [Reference]                                                    ║
║                                                                               ║
║  Data Point 2: [Fact]                                                         ║
║  Source: [Attribution]                                                        ║
║  URL/Document: [Reference]                                                    ║
║                                                                               ║
║  ───                                                                          ║
║                                                                               ║
║  [Section 4: Contradictions/Alternative Views]                                ║
║  [What the other side says]                                                   ║
║                                                                               ║
║  Subject Response:                                                            ║
║  □ Request for comment sent: [Date]                                           ║
║  □ Response received: [Date]                                                  ║
║  □ Response: [Quote or summary]                                               ║
║  □ No response by deadline                                                    ║
║  □ Declined to comment                                                        ║
║                                                                               ║
║  Alternative Explanation:                                                     ║
║  [Other plausible interpretations of the evidence]                            ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SOURCE LIST                                                                  ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Named Sources:                                                               ║
║  1. [Name], [Title], [Organization]                                           ║
║     Why credible: [Explanation]                                               ║
║     Contact: [Info for verification]                                          ║
║     What they provided: [Summary]                                             ║
║                                                                               ║
║  Anonymous Sources:                                                           ║
║  Source A: [Description of position/expertise - enough to establish cred]    ║
║     Why anonymous: [Safety/job security/etc.]                                 ║
║     Corroboration: [What other sources/docs confirm this]                     ║
║     What they provided: [Summary]                                             ║
║     Editor who knows identity: [Name]                                         ║
║                                                                               ║
║  Documentary Sources:                                                         ║
║  1. [Document type], [Date], [Origin]                                         ║
║     Obtained via: [How acquired]                                              ║
║     Authenticated by: [Expert/process]                                        ║
║     Key information: [What it proves]                                         ║
║                                                                               ║
║  Data Sources:                                                                ║
║  1. [Database/dataset name]                                                   ║
║     Provider: [Source]                                                        ║
║     Query date: [When accessed]                                               ║
║     Analysis performed: [What was done with data]                             ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  FACT CHECK                                                                   ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Every factual claim in this report has been verified by:                     ║
║                                                                               ║
║  □ Primary documentation                                                      ║
║  □ Multiple independent sources                                               ║
║  □ Subject confirmation                                                       ║
║  □ Expert analysis                                                            ║
║                                                                               ║
║  Claims needing further verification:                                         ║
║  • [Claim] - Status: [Being verified/Unable to verify]                        ║
║                                                                               ║
║  Corrections from subject:                                                    ║
║  • [Original claim] → [Correction] - [Reason]                                 ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  FAIRNESS CHECKLIST                                                           ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  □ Subject given opportunity to respond                                       ║
║  □ All sides of dispute represented                                           ║
║  □ Context provided for quotes/actions                                        ║
║  □ Private individuals' privacy respected                                     ║
║  □ No undue emphasis on race/gender/religion unless relevant                  ║
║  □ Headline accurate and supported by text                                    ║
║  □ Photos used with permission or fair use justified                          ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  LEGAL REVIEW NOTES                                                           ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Libel concerns:                                                              ║
║  [Any potentially risky claims and their supporting evidence]                 ║
║                                                                               ║
║  Privacy concerns:                                                            ║
║  [Any privacy issues and justification for publication]                       ║
║                                                                               ║
║  Copyright concerns:                                                          ║
║  [Any third-party content and clearance status]                               ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  PUBLICATION NOTES                                                            ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Recommended publication date: [Date]                                         ║
║  Embargo: □ None □ Until [Date/Time]                                          ║
║  Exclusivity: □ None □ [Outlet] has exclusive for [period]                    ║
║  Follow-up stories: [Ideas for related coverage]                              ║
║  Interactive elements: [Charts, maps, etc. needed]                            ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  REPORTER CERTIFICATION                                                       ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  I certify that this report is accurate, fair, and complete to the best of    ║
║  my knowledge and ability. All sources have been properly vetted and all      ║
║  quotes are accurate and in context.                                          ║
║                                                                               ║
║  _______________________________      _______________                         ║
║  [Reporter Signature]                 [Date]                                  ║
║                                                                               ║
║  Approved by Editor: _________________________ Date: _______________          ║
║                                                                               ║
║  Approved by Legal: _________________________ Date: _______________          ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

---

## 6. Security Assessment (Risk-Focused)

### Purpose
Security-focused report for threat assessment and protective intelligence.

### Template
```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                      SECURITY ASSESSMENT REPORT                               ║
║                    Protective Intelligence Analysis                           ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  CLASSIFICATION: [LEVEL]              NEED TO KNOW: [LIST]                    ║
║  REPORT ID: [ID]                      VERSION: [X.Y]                          ║
║  DATE: [YYYY-MM-DD]                   EXPIRY: [YYYY-MM-DD]                    ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  EXECUTIVE THREAT SUMMARY                                                     ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  THREAT LEVEL: [CRITICAL/HIGH/MEDIUM/LOW/MINIMAL]                             ║
║                                                                               ║
║  THREAT VECTOR:                                                               ║
║  [ ] Physical         [ ] Cyber           [ ] Financial                       ║
║  [ ] Reputational     [ ] Legal           [ ] Operational                     ║
║                                                                               ║
║  IMMINENCE: [Immediate/Near-term/Long-term/Not currently active]              ║
║                                                                               ║
║  THREAT ACTORS IDENTIFIED: [NUMBER]                                           ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  THREAT ACTOR PROFILES                                                          ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Actor 1: [NAME/IDENTIFIER]                                                   ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Classification:       [Individual/Group/Organization/State-sponsored]        ║
║  Capability:           [High/Medium/Low - with explanation]                   ║
║  Intent:               [Hostile/Neutral/Unknown]                              ║
║  Opportunity:          [High/Medium/Low - access to target]                   ║
║  Risk Score:           [Calculated: Capability × Intent × Opportunity]        ║
║                                                                               ║
║  Known TTPs (Tactics, Techniques, Procedures):                                ║
║  • [TTP 1]                                                                    ║
║  • [TTP 2]                                                                    ║
║                                                                               ║
║  Indicators of Compromise (IOCs):                                             ║
║  • [IOC 1]                                                                    ║
║  • [IOC 2]                                                                    ║
║                                                                               ║
║  Associated Infrastructure:                                                   ║
║  • [Domains, IPs, etc.]                                                       ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  VULNERABILITY ASSESSMENT                                                     ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Target Surface Analysis:                                                     ║
║                                                                               ║
║  ┌─────────────────────┬───────────┬───────────┬───────────────────────────┐  ║
║  │ Attack Vector       │ Exposure  │ Severity  │ Mitigation Status         │  ║
║  ├─────────────────────┼───────────┼───────────┼───────────────────────────┤  ║
║  │ Physical location   │ High      │ Critical  │ □ Mitigated □ Partial    │  ║
║  │ Digital footprint   │ High      │ High      │ □ Mitigated □ Partial    │  ║
║  │ Social engineering  │ Medium    │ High      │ □ Mitigated □ Partial    │  ║
║  │ Network access      │ Low       │ Medium    │ ☑ Mitigated              │  ║
║  │ Supply chain        │ Medium    │ Medium    │ □ Mitigated □ Partial    │  ║
║  └─────────────────────┴───────────┴───────────┴───────────────────────────┘  ║
║                                                                               ║
║  Critical Vulnerabilities:                                                    ║
║  1. [Vulnerability] - [Impact] - [CVSS-style score]                           ║
║  2. [Vulnerability] - [Impact] - [CVSS-style score]                           ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  THREAT SCENARIO ANALYSIS                                                     ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Scenario 1: [TITLE]                                                          ║
║  Likelihood:     [Probability assessment]                                     ║
║  Impact:         [Severity of outcome]                                        ║
║  Risk Level:     [Combined rating]                                            ║
║  Prerequisites:  [What would need to happen]                                  ║
║  Indicators:     [Warning signs to monitor]                                   ║
║  Timeline:       [When this could occur]                                      ║
║                                                                               ║
║  [Repeat for each scenario...]                                                ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  INTELLIGENCE GAPS                                                            ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Critical Information Needs (CINs):                                           ║
║  1. [What information is missing] - [Why it matters for security]             ║
║  2. [What information is missing] - [Why it matters for security]             ║
║                                                                               ║
║  Collection Requirements:                                                     ║
║  • [What needs to be monitored]                                               ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  PROTECTIVE RECOMMENDATIONS                                                   ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  IMMEDIATE (24-48 hours):                                                     ║
║  1. [Action] - [Rationale] - [Resource requirements]                          ║
║  2. [Action] - [Rationale] - [Resource requirements]                          ║
║                                                                               ║
║  SHORT-TERM (1-2 weeks):                                                      ║
║  1. [Action] - [Rationale] - [Resource requirements]                          ║
║  2. [Action] - [Rationale] - [Resource requirements]                          ║
║                                                                               ║
║  LONG-TERM (1-3 months):                                                      ║
║  1. [Action] - [Rationale] - [Resource requirements]                          ║
║  2. [Action] - [Rationale] - [Resource requirements]                          ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  MONITORING REQUIREMENTS                                                      ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Indicators to Watch:                                                         ║
║  • [Indicator 1] - [What it would signify]                                    ║
║  • [Indicator 2] - [What it would signify]                                    ║
║                                                                               ║
║  Monitoring Frequency: [Continuous/Daily/Weekly/Monthly]                      ║
║                                                                               ║
║  Next Review Date: [Date]                                                     ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  DISTRIBUTION LIST                                                            ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [ ] Security Team Lead                                                       ║
║  [ ] Executive Protection                                                     ║
║  [ ] Legal Counsel                                                            ║
║  [ ] Communications/PR                                                        ║
║  [ ] IT Security                                                              ║
║  [ ] Other: [Specify]                                                         ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  PREPARED BY                                                                  ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Analyst: [Name]                                                              ║
║  Certification: [Relevant credentials]                                        ║
║  Organization: [Entity]                                                       ║
║  Contact: [Secure contact method]                                             ║
║                                                                               ║
║  Reviewed by: [Name] [Date]                                                   ║
║  Approved by: [Name] [Date]                                                   ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

---

## 7. Investigation Dossier (Book Format)

### Purpose
Comprehensive case file documenting entire investigation from start to finish.

### Template
```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                        INVESTIGATION DOSSIER                                  ║
║                     Complete Case Documentation                               ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  DOSSIER INFORMATION                                                          ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Dossier ID:          [UNIQUE ID]                                             ║
║  Case Name:           [NAME]                                                  ║
║  Classification:      [LEVEL]                                                 ║
║  Date Opened:         [YYYY-MM-DD]                                            ║
║  Date Closed:         [YYYY-MM-DD or "Ongoing"]                               ║
║  Lead Analyst:        [NAME]                                                  ║
║  Contributing Analysts: [LIST]                                                ║
║  Client/Requestor:    [NAME]                                                  ║
║  Total Pages:         [COUNT]                                                 ║
║  Total Exhibits:      [COUNT]                                                 ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  TABLE OF CONTENTS                                                            ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Section 1:    Case Background .................................... Page 003 ║
║  Section 2:    Investigative Plan .................................. Page 008 ║
║  Section 3:    Chronology of Investigation ......................... Page 012 ║
║  Section 4:    Subject Profiles .................................... Page 025 ║
║  Section 5:    Entity Relationships ................................ Page 045 ║
║  Section 6:    Findings and Analysis ............................... Page 060 ║
║  Section 7:    Evidence Inventory .................................. Page 095 ║
║  Section 8:    Source Documentation ................................ Page 110 ║
║  Section 9:    Intelligence Gaps ................................... Page 125 ║
║  Section 10:   Recommendations ..................................... Page 130 ║
║  Section 11:   Appendices .......................................... Page 135 ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 1: CASE BACKGROUND                                                   ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  1.1 Origins of Investigation                                                 ║
║      [How the case started - who requested, why, initial scope]               ║
║                                                                               ║
║  1.2 Initial Intelligence Requirements                                        ║
║      [Original questions to be answered]                                      ║
║      • [Question 1]                                                           ║
║      • [Question 2]                                                           ║
║                                                                               ║
║  1.3 Scope and Limitations                                                    ║
║      [What was included and excluded, legal/ethical constraints]              ║
║                                                                               ║
║  1.4 Resources Allocated                                                      ║
║      [Time, budget, tools, personnel assigned]                                ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 2: INVESTIGATIVE PLAN                                                ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  2.1 Methodology                                                              ║
║      [Overall approach and framework used]                                    ║
║                                                                               ║
║  2.2 Data Collection Plan                                                     ║
║      [What sources were targeted, in what order]                              ║
║                                                                               ║
║  2.3 Analysis Framework                                                       ║
║      [How collected data would be analyzed]                                   ║
║                                                                               ║
║  2.4 Quality Assurance                                                        ║
║      [Verification and validation procedures]                                 ║
║                                                                               ║
║  2.5 Timeline                                                                 ║
║      [Original schedule and milestones]                                       ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 3: CHRONOLOGY OF INVESTIGATION                                       ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Day-by-day or week-by-week account of investigation activities]             ║
║                                                                               ║
║  Day 1 - [Date]                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Activities: [What was done]                                                  ║
║  Results:    [What was found]                                                 ║
║  Decisions:  [Any changes to plan based on findings]                          ║
║  Exhibits:   [References to evidence collected]                               ║
║                                                                               ║
║  [Continue for each day/phase...]                                             ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 4: SUBJECT PROFILES                                                  ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Complete profiles of all subjects/entities investigated]                    ║
║                                                                               ║
║  Profile 1: [NAME]                                                            ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Overview:                                                                    ║
║  [Summary description]                                                        ║
║                                                                               ║
║  Identity Information:                                                        ║
║  • Full legal name: [NAME]                                                    ║
║  • Aliases: [LIST]                                                            ║
║  • DOB: [DATE]                                                                ║
║  • POB: [LOCATION]                                                            ║
║  • Nationality: [COUNTRY/IES]                                                 ║
║  • Current residence: [ADDRESS]                                               ║
║                                                                               ║
║  Digital Presence:                                                            ║
║  [Comprehensive list of all online accounts, websites, etc.]                  ║
║                                                                               ║
║  Professional History:                                                        ║
║  [Timeline of employment, positions, business associations]                   ║
║                                                                               ║
║  Financial Profile:                                                           ║
║  [Assets, liabilities, companies owned, etc.]                                 ║
║                                                                               ║
║  Network:                                                                     ║
║  [Known associates, family members, connections]                              ║
║                                                                               ║
║  Timeline of Life Events:                                                     ║
║  [Major milestones - education, marriage, moves, etc.]                        ║
║                                                                               ║
║  [Continue for each subject...]                                               ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 5: ENTITY RELATIONSHIPS                                              ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Relationship mapping between all entities]                                  ║
║                                                                               ║
║  5.1 Relationship Matrix                                                      ║
║      [Table showing connections between entities]                             ║
║                                                                               ║
║  5.2 Network Diagrams                                                         ║
║      [Visual representations of relationships]                                ║
║                                                                               ║
║  5.3 Relationship Details                                                     ║
║      [Deep dive into specific important connections]                          ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 6: FINDINGS AND ANALYSIS                                             ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Comprehensive findings organized by topic]                                  ║
║                                                                               ║
║  Finding 1: [TITLE]                                                           ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Classification: [CRITICAL/HIGH/MEDIUM/LOW]                                   ║
║  Confidence: [X%]                                                             ║
║  Finding: [Detailed description]                                              ║
║  Evidence: [What supports this finding]                                       ║
║  Analysis: [Investigator's interpretation]                                    ║
║  Implications: [What this means]                                              ║
║  Related Findings: [Cross-references]                                         ║
║                                                                               ║
║  [Continue for all findings...]                                               ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 7: EVIDENCE INVENTORY                                                ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Complete catalog of all evidence collected]                                 ║
║                                                                               ║
║  ┌────────┬──────────────────┬───────────┬──────────┬──────────┬──────────┐  ║
║  │Exhibit │ Description      │ Type      │ Date     │ Location │ Hash     │  ║
║  ├────────┼──────────────────┼───────────┼──────────┼──────────┼──────────┤  ║
║  │ 001    │ [Description]    │ Screenshot│ [Date]   │ [Path]   │ [Hash]   │  ║
║  │ 002    │ [Description]    │ Document  │ [Date]   │ [Path]   │ [Hash]   │  ║
║  └────────┴──────────────────┴───────────┴──────────┴──────────┴──────────┘  ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 8: SOURCE DOCUMENTATION                                              ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [Detailed information about all sources used]                                ║
║                                                                               ║
║  Source 1: [NAME/IDENTIFIER]                                                  ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║  Type: [Platform/database/public record/etc.]                                 ║
║  URL/Location: [Where found]                                                  ║
║  Date Accessed: [Timestamp]                                                   ║
║  Reliability: [A/B/C/D/E/F]                                                   ║
║  Information Obtained: [What this source provided]                            ║
║  Verification: [How confirmed]                                                ║
║  Limitations: [Any caveats]                                                   ║
║                                                                               ║
║  [Continue for all sources...]                                                ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 9: INTELLIGENCE GAPS                                                 ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [What could not be determined]                                               ║
║                                                                               ║
║  Gap 1: [DESCRIPTION]                                                         ║
║  Why it matters: [Significance]                                               ║
║  Why it couldn't be filled: [Obstacles]                                       ║
║  How it could be filled: [Recommendations]                                    ║
║                                                                               ║
║  [Continue for all gaps...]                                                   ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 10: RECOMMENDATIONS                                                  ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  [All recommendations from investigation]                                     ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  SECTION 11: APPENDICES                                                       ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  Appendix A: Raw Data                                                         ║
║  Appendix B: Search Logs                                                      ║
║  Appendix C: Communication Records                                            ║
║  Appendix D: Tool Outputs                                                     ║
║  Appendix E: Legal Review Documents                                           ║
║  Appendix F: Glossary                                                         ║
║  Appendix G: Bibliography                                                     ║
║                                                                               ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  CERTIFICATION                                                                ║
║  ───────────────────────────────────────────────────────────────────────────  ║
║                                                                               ║
║  I hereby certify that this dossier represents a complete and accurate        ║
║  record of the investigation conducted. All findings are supported by         ║
║  documented evidence, and all sources have been properly attributed.          ║
║                                                                               ║
║  _______________________________      _______________                         ║
║  Lead Analyst Signature               Date                                      ║
║                                                                               ║
║  Reviewed and Approved:                                                       ║
║  _______________________________      _______________                         ║
║  Supervisor Signature                 Date                                      ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

---

*Version: 2.0 | Templates: 7 | Last Updated: 2026-02-27*

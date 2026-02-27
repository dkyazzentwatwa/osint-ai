# Person Investigation Wizard

A guided workflow for investigating individuals safely and effectively.

---

## Wizard Overview

**Purpose:** Guide users through ethical person investigation  
**Use Cases:** Verifying online contacts, background checks, finding lost contacts  
**Duration:** 20-30 minutes  
**Complexity:** Beginner to Intermediate  
**Output:** Investigation report with findings and confidence levels  

### Important Notice

```
⚠️ PRIVACY & ETHICS NOTICE

Before starting, please confirm:

1. You have a legitimate purpose for this investigation
2. You will use findings legally and ethically
3. You understand privacy laws in your jurisdiction
4. You will secure any personal information discovered

For employment/tenant screening:
  • You may need written consent
  • FCRA compliance may be required
  • Disclosure requirements may apply

This tool is for finding PUBLIC information only.

Continue? (yes/no)
```

---

## Step 1: Identity Confirmation

### Question 1a: Subject Information

**System asks:**
```
Step 1 of 5: Identity Confirmation

Who are you investigating?

Required information:
  Full name: _
  
Highly recommended:
  Current or last known location: _
  Approximate age or birth year: _

Optional (helps narrow results):
  Email address: _
  Phone number: _
  Known nicknames/aliases: _
  Occupation or employer: _
```

**Expected User Input:**
- Name (required)
- Location, age (strongly recommended)
- Additional identifiers (optional)

---

### Question 1b: Investigation Purpose

**System asks:**
```
What is the purpose of this investigation?

  1. Online dating verification
  2. Professional networking
  3. Reconnecting with lost contact
  4. Tenant/roommate screening
  5. Business due diligence
  6. Personal safety concern
  7. Other (please specify)

Purpose (number or describe): _
```

**Expected User Input:**
- Number 1-7
- Or brief description

**Purpose Determines Approach:**

| Purpose | Focus Areas | Special Considerations |
|---------|-------------|----------------------|
| Dating verification | Identity, photos, consistency | Scam detection priority |
| Professional | Career history, credentials | LinkedIn focus |
| Reconnecting | Contact info, current location | Privacy respect |
| Tenant screening | Financial, criminal, stability | FCRA compliance |
| Business | Professional reputation, affiliations | Corporate context |
| Safety | Red flags, concerning behavior | Immediate risk assessment |

---

### Question 1c: Initial Verification

**System Performs:**
```
Verifying identity information...

Searching for: "John Smith" in Boston, MA

Results:
  ⚠️ Multiple matches found (23 people)

Top matches by likelihood:
  1. John Smith, 34, Jamaica Plain, Teacher
  2. John Smith, 29, Back Bay, Marketing Manager
  3. John Smith, 41, Cambridge, Consultant
  4. John Smith, 52, Somerville, Attorney
  [20 additional...]
```

**System asks:**
```
I found multiple people with this name.

Can you provide more details to narrow it down?

  • Approximate age?
  • Neighborhood or specific area?
  • Occupation?
  • Any other identifying information?

Additional details: _
```

**Expected User Input:**
- More specific details
- Or indicate which match from list

---

## Step 2: Platform Discovery Flow

### System Performs:

```
Step 2 of 5: Discovering Online Presence

[Searching... ████████░░] 80%

Checking platforms:
  ✓ LinkedIn
  ✓ Facebook
  ✓ Twitter/X
  ⏳ Instagram
  ○ TikTok
  ○ GitHub
  ○ Other professional networks
```

### Professional Presence

**LinkedIn Results:**
```
💼 Professional Profile Found

Name: John Smith
Location: Boston, MA
Current: Marketing Manager at TechCorp
Previous: Senior Analyst at DataInc (2019-2023)
Education: Boston University, MBA
Connections: 487

Profile:
  ✓ Complete work history
  ✓ Education verified
  ✓ Active posting (last post: 3 days ago)
  ✓ Recommendations: 12 received
  
Red flags: None identified
```

**Other Professional:**
```
🏢 Additional Professional Presence

Found on:
  ✓ Company website team page
  ✓ Industry conference speaker list (2023)
  ✓ Professional association directory

Not found:
  • GitHub (not a developer)
  • Research publications
  • Patent filings
```

---

### Social Media Discovery

**Facebook:**
```
📘 Facebook Profile

Status: ✓ Profile found
Visibility: Friends only (limited public info)
Joined: 2010 (14 years ago)
Friends: ~350

Public information:
  • Profile photo visible
  • Cover photo visible
  • Basic info (hometown, current city)
  • Public posts: 2 in last year

Assessment: Normal personal use
```

**Twitter/X:**
```
🐦 Twitter/X Profile (@jsmith_boston)

Status: ✓ Active account
Created: March 2015
Followers: 234 | Following: 198
Tweets: 1,247

Recent activity:
  • Regular posting (3-5 tweets/week)
  • Topics: Marketing, Boston sports, local news
  • Engagement: Normal levels

Assessment: Genuine personal account
```

**Instagram:**
```
📷 Instagram Profile (@john.smith.boston)

Status: ✓ Private account
Posts: 89
Followers: 156 | Following: 203
Bio: "Marketing pro | Boston | Dog dad"

Assessment:
  • Private profile (respect privacy)
  • Reasonable follower ratio
  • Consistent with other profiles
```

---

### Platform Summary

```
📊 Platform Discovery Summary

Found: 5 profiles
Not found: 3 platforms checked

✓ Strong presence:
  • LinkedIn (professional, active)
  • Twitter (personal, active)
  • Facebook (personal, private)

✓ Moderate presence:
  • Instagram (personal, private)
  • Company website (professional)

✗ No presence found:
  • TikTok
  • GitHub
  • YouTube

Overall: Normal digital footprint for age/profession
```

---

### Question 2: Profile Review

**System asks:**
```
Do these profiles match the person you're investigating?

  1. Yes, this is definitely them
  2. Probably, but I'm not certain
  3. Unsure, need more information
  4. No, this is someone else

Your assessment: _
```

**Expected User Input:**
- 1-4 selection
- Optional: why unsure

---

## Step 3: Cross-Reference Guidance

### System Performs:

```
Step 3 of 5: Cross-Referencing Information

Verifying consistency across platforms...

[Analyzing... ████████░░] 80%
```

### Consistency Check

```
🔍 Cross-Platform Consistency Analysis

IDENTITY:
  ✓ Name consistent across all platforms
  ✓ Photo matches across LinkedIn/Facebook/Twitter
  ✓ Location consistent (Boston area)

EMPLOYMENT:
  ✓ Current job matches (TechCorp)
  ✓ Previous employer matches (DataInc)
  ✓ Job title progression logical

TIMELINE:
  ✓ LinkedIn: Started at TechCorp March 2023
  ✓ Twitter: Announced new job March 2023
  ✓ Company site: Listed since March 2023

EDUCATION:
  ✓ Boston University MBA confirmed
  ✓ Graduation year consistent (2019)
  ✓ No conflicting institutions found

CONNECTIONS:
  ✓ LinkedIn connections include DataInc colleagues
  ✓ Twitter follows Boston marketing professionals
  ✓ Facebook friends include family members

Assessment: HIGHLY CONSISTENT
```

---

### Red Flag Check

```
⚠️ Potential Concerns Review

Automated checks for:

Social Engineering Indicators:
  ✓ No sudden account creation spikes
  ✓ Account ages appropriate (2010-2015)
  ✓ No duplicate/similar accounts found

Identity Consistency:
  ✓ No name variations that don't match
  ✓ Photos appear genuine (not stock)
  ✓ No AI-generated image indicators

Behavioral Patterns:
  ✓ Posting history is organic
  ✓ Engagement patterns normal
  ✓ No copy-paste content detected

Financial Scam Indicators:
  ✓ No cryptocurrency promotion
  ✓ No investment opportunities posted
  ✓ No suspicious link patterns

Result: NO RED FLAGS DETECTED
```

---

### Question 3: Additional Verification

**System asks:**
```
Cross-reference complete. Information is consistent.

Would you like me to:

  1. Check for any public records
  2. Search for news or media mentions
  3. Build a timeline of their activity
  4. Proceed to final report
  5. All of the above

Select (1-5): _
```

**Expected User Input:**
- 1-5 selection

---

## Step 4: Timeline Building Assistance

### System Performs (if selected):

```
Step 4 of 5: Building Activity Timeline

[Compiling... ██████░░░░] 60%

Analyzing chronological data from all sources...
```

### Timeline Output

```
📅 Activity Timeline: John Smith

2010
  June: Facebook account created
  
2015
  March: Twitter account created
  
2017
  July: Started at DataInc (per LinkedIn)
  
2019
  May: Graduated BU with MBA
  August: Promoted to Senior Analyst
  
2021
  March: Moved to current Boston address
  
2023
  March: Started at TechCorp (Marketing Manager)
  March: Tweeted about new job
  
2024
  January: Spoke at Boston Marketing Conference
  Ongoing: Regular LinkedIn activity

GAPS IDENTIFIED:
  • 2011-2014: Limited online activity (pre-career, normal)
  • No concerning gaps detected

CONSISTENCY: ✓ Timeline is coherent and believable
```

---

### Life Events Correlation

```
🎯 Key Life Events Detected

Career:
  ✓ 2017: Entry-level position at DataInc
  ✓ 2019: Promotion + MBA completion
  ✓ 2023: Career move to TechCorp
  Pattern: Steady professional growth

Location:
  ✓ Consistently Boston area since 2017
  ✓ 2021: Address change (likely apartment)
  Pattern: Stable geographically

Education:
  ✓ 2019: MBA from Boston University
  ✓ Part-time while working (verified)
  Pattern: Professional development focus

Social:
  ✓ 2010-2015: Personal social media adoption
  ✓ 2023: Increased professional presence
  Pattern: Normal social media evolution
```

---

### Question 4: Timeline Review

**System asks:**
```
Timeline analysis complete.

Key findings:
  • Consistent career progression
  • Stable location history
  • No unexplained gaps
  • Normal social media evolution

Does this timeline match what you know
or have been told about this person? (yes/no/partially)
```

**Expected User Input:**
- yes / no / partially
- Optional: discrepancies noted

---

## Step 5: Final Report Generation

### System Compiles:

```
Step 5 of 5: Generating Investigation Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PERSON INVESTIGATION REPORT
Subject: John Smith
Date: January 15, 2024
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Executive Summary

```
📋 EXECUTIVE SUMMARY

Investigation Confidence: HIGH (87%)

VERDICT: IDENTITY VERIFIED ✓

This investigation found a real person with a consistent,
verifiable online presence. Identity claims appear genuine.

Risk Assessment: LOW RISK
  ✓ Consistent identity across platforms
  ✓ Verifiable employment history
  ✓ Normal digital footprint
  ✓ No red flags detected
  ✓ Timeline is coherent

Recommendation: Proceed with normal caution appropriate
for the context (dating, business, etc.)
```

---

### Detailed Findings

```
📊 DETAILED FINDINGS

IDENTITY VERIFICATION:
  Status: ✓ CONFIRMED
  Confidence: 87%
  
  Evidence:
    • Real person verified through multiple sources
    • Name consistent across all platforms
    • Photos match and appear genuine
    • Location claims verified

ONLINE PRESENCE:
  Platforms found: 5
  Active profiles: 4
  
  LinkedIn: ✓ Active, professional
  Facebook: ✓ Private, personal use
  Twitter: ✓ Active, personal/professional mix
  Instagram: ✓ Private, limited activity
  Company site: ✓ Listed as employee

EMPLOYMENT VERIFICATION:
  Current: TechCorp - Marketing Manager ✓
  Previous: DataInc - Senior Analyst ✓
  Education: Boston University MBA ✓
  
  All claims verified through:
    • LinkedIn history
    • Company website
    • Professional announcements

CONSISTENCY CHECK:
  Cross-platform: ✓ Consistent
  Timeline: ✓ Coherent
  Red flags: None detected
```

---

### Risk Factors

```
⚠️ RISK ASSESSMENT

Overall Risk Level: LOW (2/10)

Positive Indicators:
  + Long social media history (10+ years)
  + Consistent professional growth
  + Verifiable employment
  + Stable location
  + Genuine photos
  + Organic posting patterns

Negative Indicators:
  • None identified

Neutral Factors:
  • Private Instagram (privacy preference, not a concern)
  • Limited public Facebook (privacy conscious)
```

---

### Recommendations

```
💡 RECOMMENDATIONS

Based on Investigation Purpose: [Dating Verification]

Immediate Actions:
  ✓ Identity is verified - person is real
  ✓ Photos are genuine
  ✓ Background checks out

Next Steps:
  • Video call before meeting in person
  • Meet in public place first
  • Tell a friend your plans
  • Trust your instincts in person

Ongoing Cautions:
  • Continue to verify unusual claims
  • Never send money to someone you haven't met
  • Watch for pressure tactics
  • Red flags can appear later - stay alert

This investigation confirms identity but cannot predict
future behavior. Continue normal safety practices.
```

---

## Report Export

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REPORT OPTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  1. Save full report
     → Complete investigation record
     → All findings and sources

  2. Export summary only
     → 1-page overview
     → Key findings only

  3. Export verification card
     → Quick-reference format
     → Share with trusted contact

  4. Start new investigation
     → Clear and begin fresh

  5. Exit

Select option (1-5): _
```

---

## Wizard Command

### Activation

```
/wizard person

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
👤 Person Investigation Wizard
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

I'll help you investigate an individual through
publicly available information.

Duration: 20-30 minutes

⚠️ IMPORTANT ETHICAL REQUIREMENTS:

Before starting, please confirm:
  ✓ You have a legitimate purpose
  ✓ You will use findings legally
  ✓ You will protect personal information
  ✓ You understand privacy laws apply

This wizard will:
  1. Confirm the person's identity
  2. Find their online presence
  3. Cross-reference for consistency
  4. Build an activity timeline
  5. Provide a risk assessment

Perfect for:
  • Online dating verification
  • Professional background checks
  • Reconnecting with lost contacts
  • General identity verification

Ready to start? (yes/no)
```

---

## Alternative Outcome Examples

### Suspicious Profile Example

```
Investigation Confidence: MEDIUM (45%)

VERDICT: REQUIRES ADDITIONAL VERIFICATION ⚠️

Concerns:
  ⚠️ Recently created accounts (all within 6 months)
  ⚠️ Limited connection history
  ⚠️ Photos appear on other profiles
  ⚠️ Employment unverifiable
  
Recommendation:
  Request video call verification.
  Ask specific questions about claims.
  Do not meet in person without further verification.
```

### Potential Catfish Example

```
Investigation Confidence: LOW (15%)

VERDICT: LIKELY FAKE IDENTITY 🚨

Red Flags:
  🚨 Photos are stock images
  🚨 No verifiable employment
  🚨 No digital footprint before 3 months ago
  🚨 Inconsistent information
  🚨 Multiple similar fake profiles found

Recommendation:
  DO NOT PROCEED.
  Cease communication immediately.
  Report to platform.
```

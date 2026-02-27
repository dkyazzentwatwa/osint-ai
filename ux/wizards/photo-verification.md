# Photo Verification Wizard

A guided 5-step workflow for verifying the authenticity of images.

---

## Wizard Overview

**Purpose:** Guide users through photo verification step-by-step  
**Use Cases:** Dating safety, social media verification, evidence validation  
**Duration:** 5-10 minutes  
**Complexity:** Beginner-friendly  
**Output:** Verification verdict with confidence level  

### When to Use

- Someone sent you a photo online
- You're unsure if a profile picture is real
- You need to verify evidence
- Checking if an image has been manipulated

---

## Step 1: Image Collection & Basic Info

### Question 1a: Image Source

**System asks:**
```
Step 1 of 5: Where did you get this photo?

  1. Someone sent it to me (dating app, social media, email)
  2. I found it online (website, search result)
  3. It was given as evidence or documentation
  4. I'm not sure where it came from

Type the number (1-4):
```

**Expected User Input:**
- Number 1-4
- Optional: Brief explanation

**Analysis Based on Answer:**

| Answer | Risk Level | Next Focus |
|--------|------------|------------|
| 1 (Sent directly) | Higher | Check for stock/catfish photos |
| 2 (Found online) | Medium | Verify original source |
| 3 (Evidence) | Context-dependent | Check for tampering |
| 4 (Unknown) | Higher | All checks important |

---

### Question 1b: Claimed Context

**System asks:**
```
What does the person claim about this photo?

For example:
  • "This is me at the beach last week"
  • "Here's my driver's license"
  • "This is my house"
  • "I'm a model, this is my portfolio"

What you're told about this photo: _
```

**Expected User Input:**
- Free text description
- Can be vague if unknown

**Analysis Procedure:**
```
System notes claims to verify:
  ✓ Date/time claims
  ✓ Location claims
  ✓ Identity claims
  ✓ Ownership claims
  
These become verification targets for later steps.
```

---

### Question 1c: Initial Observations

**System asks:**
```
Before we analyze, does anything look odd to you?

  • Does the person look "too perfect"?
  • Do colors or lighting look unnatural?
  • Are shadows inconsistent?
  • Does background look fake or blurry?
  • Anything else suspicious?

Your observations (or "no, looks normal"): _
```

**Expected User Input:**
- Specific concerns
- "Looks normal" / "No concerns"

---

## Step 2: Metadata Analysis

### System Performs:

```
Step 2 of 5: Checking photo metadata...

[Analyzing... ████████░░] 80%

This step looks for:
  • Camera information (make, model)
  • Date and time photo was taken
  • GPS location data
  • Editing software used
  • Original dimensions
```

### Results Presentation

**Case A: Metadata Found**
```
📊 Metadata Results

Camera: iPhone 13 Pro
Date taken: January 15, 2024, 2:34 PM
Location: Miami Beach, Florida ✓
Dimensions: 3024 x 4032 pixels
Software: No editing detected

What this tells us:
  ✓ Real camera, not AI-generated
  ✓ Recent photo (if claim matches)
  ✓ Location data present (you can verify)
  ⚠️ Location data could be privacy risk
```

**Case B: No Metadata**
```
📊 Metadata Results

⚠️ No metadata found

This could mean:
  • Photo was sent through messaging app (common)
  • Posted to social media (strips metadata)
  • Intentionally removed
  • Screenshot of another image
  • Edited with metadata-stripping tool

This is neutral - neither good nor bad.
Most photos shared online lose metadata.
```

**Case C: Suspicious Metadata**
```
🚨 Suspicious Metadata Found

Software: Photoshop 2023 (edited 3 days ago)
Original date: 2019 (not recent as claimed)
Multiple compression artifacts

⚠️ Concerns:
  • Photo was edited recently
  • Claimed timeframe doesn't match
  • May be composite/image manipulation

Recommendation: Ask for unedited original.
```

---

### Question 2: User Response to Metadata

**System asks:**
```
Does this metadata match what you were told about the photo?

For example, if they said "this is me yesterday in New York"
but metadata shows "January 2023 in Los Angeles" - that's a
major red flag.

Does everything match? (yes/no/unsure)
```

**Expected User Input:**
- yes / no / unsure
- Optional explanation

**Next Steps Based on Answer:**

| Answer | Action |
|--------|--------|
| Yes | Proceed to Step 3 |
| No | Flag as high concern, continue for evidence |
| Unsure | Note uncertainty, proceed to gather more info |

---

## Step 3: Reverse Image Search

### System Performs:

```
Step 3 of 5: Searching for this image online...

[Searching... ██████░░░░] 60%

I'm checking:
  • Google Images
  • TinEye
  • Bing Visual Search
  • Yandex Images
  
Looking for:
  • Same image elsewhere
  • Similar images
  • Original source
  • How long it's been online
```

### Results Presentation

**Case A: Unique Image**
```
🔍 Reverse Image Search Results

Good news! This image appears to be unique.

Searched: Google, TinEye, Bing, Yandex
Matches found: 0

What this means:
  ✓ Not a stock photo
  ✓ Not widely circulated online
  ✓ Likely original to the person
  
⚠️ Note: Someone could still be impersonating
          the real person in the photo.
```

**Case B: Found Elsewhere**
```
⚠️ Image Found Elsewhere Online

Matches found: 12

Sources:
  1. Shutterstock (stock photo)
  2. Facebook profile - Maria G. (2018)
  3. Instagram - @maria_model
  4. Dating site - "Sarah, 28"
  5. Dating site - "Jessica, 32"
  6-12. Other sites...

🚨 RED FLAGS:
  • This is a STOCK PHOTO
  • Being used by multiple fake profiles
  • The person you're talking to is likely
    NOT the person in the photo

Recommendation: Stop communication. This is
likely a catfish/scammer.
```

**Case C: Stolen Photo**
```
⚠️ Photo Appears on Multiple Profiles

This exact photo found on:
  • LinkedIn - John Smith (real estate agent)
  • Facebook - John Smith (public profile)
  • Dating app - "Mike, 35, entrepreneur"

Analysis:
  • Original appears to be LinkedIn professional photo
  • Being used on dating app by someone else
  • Likely catfish using stolen identity

⚠️ The person in the photo is likely real, but
   the person sending it to you is someone else
   pretending to be them.
```

---

### Question 3: User Action

**System asks:**
```
Based on these results, what would you like to do?

If image is UNIQUE:
  1. Continue to deeper analysis
  2. Ask them for a verification photo

If image is FOUND ELSEWHERE:
  1. Confront them about it
  2. Report and block them
  3. Continue investigation anyway

What would you like to do? (1, 2, or 3)
```

---

## Step 4: Technical Analysis

### System Performs:

```
Step 4 of 5: Technical authenticity checks...

[Analyzing... ████████░░] 80%

Checking for:
  • AI-generated image artifacts
  • Digital manipulation signs
  • Lighting consistency
  • Shadow analysis
  • Compression patterns
```

### Analysis Results

**AI-Generated Image Detection:**
```
🤖 AI Generation Check

Result: POSSIBLY AI-GENERATED

Indicators found:
  • Unnatural skin texture patterns
  • Inconsistent hair details
  • Background artifacts
  • Eye reflection irregularities
  • Strange ear/limb proportions

Confidence: 78% AI-generated

🚨 This photo was likely created by AI, not a real photo.
   The person may not exist at all.

Ask for: Video call or unfiltered photo with specific pose.
```

**Digital Manipulation Detection:**
```
✂️ Photo Editing Check

Result: EVIDENCE OF EDITING

Found:
  • Clone stamp artifacts (left side)
  • Resizing artifacts around subject
  • Blending edges suggest compositing
  • Color balance inconsistency

Likely edited areas:
  • Background may be swapped
  • Subject possibly added to scene
  • Skin smoothing/filter applied

⚠️ Photo has been manipulated. Ask for original.
```

**Authentic Photo Result:**
```
✓ Technical Analysis

Result: APPEARS AUTHENTIC

No manipulation detected:
  ✓ Natural skin texture
  ✓ Consistent lighting throughout
  ✓ Shadows match light source
  ✓ No clone stamp artifacts
  ✓ Compression patterns normal
  ✓ EXIF data consistent with image

Photo appears to be genuine and unmanipulated.
```

---

### Question 4: Verification Request

**System asks:**
```
Based on the technical analysis, here's a recommended
verification test you can request:

SUGGESTED: Ask them to send a photo doing a specific
          action that's hard to fake.

Examples:
  • "Can you send a photo holding a spoon on your head?"
  • "Please send a photo with today's newspaper"
  • "Can you take a photo giving a thumbs up?"

Why this works:
  • AI can't generate specific, unusual poses well
  • Stock photos won't match your request
  • Shows they can actually take photos right now

Would you like me to suggest a specific request? (yes/no)
```

---

## Step 5: Verification Conclusion

### System Compiles Results:

```
Step 5 of 5: Final Verification Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHOTO VERIFICATION RESULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Source: [User provided]
Claimed context: [User provided]

VERDICT: [CALCULATED BELOW]

Confidence Level: [High/Medium/Low]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Verdict Categories

**VERDICT: LIKELY GENUINE ✓**
```
✓ VERDICT: LIKELY GENUINE

Confidence: HIGH (85%)

Supporting evidence:
  ✓ Metadata consistent with claims
  ✓ Image not found elsewhere (unique)
  ✓ No manipulation detected
  ✓ Technical analysis passed

Minor concerns:
  • None identified

Recommendation:
  Photo appears legitimate. Basic verification complete.
  For high-stakes situations, still recommend video call
  before meeting in person.
```

**VERDICT: SUSPICIOUS - REQUIRES FURTHER VERIFICATION ⚠️**
```
⚠️ VERDICT: SUSPICIOUS

Confidence: MEDIUM (60%)

Concerns identified:
  ⚠️ No metadata (stripped or screenshot)
  ⚠️ Image found on 2 other profiles
  ⚠️ Minor editing detected (filters/smoothing)

Positive indicators:
  ✓ Not a known stock photo
  ✓ Not AI-generated

Recommendation:
  Ask for verification photo with specific pose.
  Request video call before any in-person meeting.
  Proceed with caution.
```

**VERDICT: LIKELY FAKE - HIGH RISK 🚨**
```
🚨 VERDICT: LIKELY FAKE

Confidence: HIGH (92%)

Major red flags:
  🚨 STOCK PHOTO identified
  🚨 Used on multiple fake profiles
  🚨 Metadata shows heavy editing
  🚨 Claims don't match image data

Recommendation:
  DO NOT TRUST THIS PERSON.
  
  This is likely:
    • Romance scammer
    • Catfish
    • Identity thief
  
  Actions to take:
    1. Stop all communication
    2. Report to platform
    3. Block the user
    4. If money was requested, report to authorities
```

**VERDICT: AI-GENERATED - PERSON MAY NOT EXIST 🤖**
```
🤖 VERDICT: AI-GENERATED IMAGE

Confidence: HIGH (88%)

Detection evidence:
  🤖 AI generation artifacts detected
  🤖 No real camera metadata
  🤖 Perfect but unnatural features
  🤖 Background inconsistencies

Recommendation:
  The person you're communicating with likely does NOT exist.
  This is a completely fabricated identity.

Immediate actions:
    1. Cease all communication NOW
    2. Do NOT send money or personal info
    3. Report as fake account
    4. Block and delete contact
```

---

## Final Recommendations

### Based on Verdict

```
Follow-up recommendations based on your results:

For GENUINE photos:
  ✓ Photo appears real
  ✓ Still verify with video call
  ✓ Meet in public place first
  ✓ Tell someone where you're going

For SUSPICIOUS photos:
  ⚠️ Request verification photo
  ⚠️ Ask specific questions about photo context
  ⚠️ Video call before meeting
  ⚠️ Trust your instincts

For FAKE photos:
  🚨 Stop communication immediately
  🚨 Report and block
  🚨 Never send money
  🚨 Report to authorities if fraud attempted
```

### Report Generation

```
Would you like me to generate a verification report?

  /report photo-verification
    → Saves results for your records
    → Includes all findings
    → Can be shared (with caution)

  /share-results
    → Send to trusted friend for second opinion
    → Generate shareable summary

  /start-over
    → Analyze another photo
```

---

## Wizard Command

### Activation

```
/wizard photo

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📷 Photo Verification Wizard
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

I'll guide you through verifying if a photo is authentic.
This takes about 5-10 minutes.

Great for:
  • Online dating safety
  • Verifying someone's identity
  • Checking if images are real
  • Spotting AI-generated photos

What you'll need:
  • The photo (upload or URL)
  • Any claims made about it

Privacy note:
  Photos are analyzed but not stored permanently.
  You can delete results anytime.

Ready to start? (yes/no)
```

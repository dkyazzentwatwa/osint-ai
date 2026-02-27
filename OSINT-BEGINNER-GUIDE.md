# OSINT Investigator Skill - Beginner's Guide

A simple guide to investigating people, domains, and organizations using natural language and slash commands.

---

## What This Skill Does

The OSINT (Open Source Intelligence) Investigator helps you find publicly available information about:
- **People** (names, usernames, emails)
- **Domains/Websites** (security, exposed files, subdomains)
- **Organizations** (company info, digital footprint)

**Important**: This tool only searches **publicly available information**. It does not hack, bypass authentication, or access private data.

---

## How to Use It: Two Ways

### Method 1: Natural Language (Just Talk!)

Simply type what you want to investigate in plain English. The skill will automatically recognize what you're looking for.

#### Examples:

| What You Type | What Happens |
|---------------|--------------|
| "Find information about John Smith" | Starts person reconnaissance |
| "Search for example.com" | Runs domain reconnaissance |
| "Look up username @johnsmith on social media" | Searches username across platforms |
| "What can you find on sarah@email.com?" | Investigates email address |
| "Check security of mysite.com" | Runs security-focused dorking |

**Tip**: Be specific! The more details you provide (location, company, etc.), the better the results.

---

### Method 2: Slash Commands (Power User Mode)

Use these commands for precise control over your investigation.

#### Core Commands

| Command | What It Does | Example |
|---------|--------------|---------|
| `/recon [target]` | Full investigation on a person, domain, or username | `/recon elonmusk` |
| `/dork [domain]` | Find exposed files, admin panels, APIs | `/dork example.com` |
| `/pivot [data]` | Follow a lead (email, username, phone) | `/pivot john@email.com` |
| `/timeline [subject]` | Build chronological history | `/timeline Tesla` |
| `/entity [name]` | Track what we know about someone | `/entity John Smith` |
| `/report` | Generate a full investigation report | `/report` |
| `/simple-report` | Generate easy-to-understand report | `/simple-report` |
| `/full [target]` | Run EVERYTHING at once (comprehensive) | `/full example.com` |

#### Specialized Commands

| Command | When to Use |
|---------|-------------|
| `/analyze-metadata` | Paste EXIF data, email headers, or HTTP headers for analysis |
| `/verif-photo` | Verify if an image is authentic (guides you through steps) |
| `/sock-opsec` | Get privacy/safety tips for your investigation |

---

## The "Easy Button": `/full` Command

New to OSINT? Just want everything at once? Use the `/full` command:

```
User: /full example.com

Agent: Running comprehensive investigation...
[Does recon → dorking → pivot analysis → timeline → entity map → reports]

Results: 2 reports generated
1. Technical Report (for experts)
2. Simple Report (easy to understand)
```

**When to use `/full`:**
- Starting a new investigation
- Doing due diligence on a company
- You want everything without typing multiple commands

---

## Two Types of Reports

### Technical Report (`/report`)
For security professionals, analysts, or when you need detailed citations.
- Executive Summary
- Technical findings with confidence ratings
- Entity relationship maps
- Source lists
- Professional formatting

### Simple Report (`/simple-report`)
For clients, managers, or anyone who needs plain English.
- Written at 8th-grade reading level
- No jargon (translates technical terms)
- "What This Means" and "What To Do" sections
- Easy-to-understand explanations

**Example Translation:**
- Technical: "Subdomain enumeration revealed exposed API endpoints"
- Simple: "We found hidden website addresses that let anyone access the company's database without proper security checks. This is like leaving a back door unlocked."

---

## Step-by-Step Investigation Workflow

### Scenario 1: Investigating a Person

```
Step 1: /recon "John Doe"
        ↓
Step 2: Review findings - look for usernames, emails, other names
        ↓
Step 3: /pivot "johndoe123" (found username)
        ↓
Step 4: /pivot "john.doe@company.com" (found email)
        ↓
Step 5: /timeline "John Doe" (build history)
        ↓
Step 6: /report (generate final report)
```

### Scenario 2: Checking Website Security

```
Step 1: /dork example.com
        ↓
Step 2: Review exposed files, admin panels, APIs found
        ↓
Step 3: /pivot "admin.example.com" (found subdomain)
        ↓
Step 4: /report (document vulnerabilities)
```

### Scenario 3: Verifying Someone's Identity

```
Step 1: /recon "username" 
        ↓
Step 2: Check if same username appears on multiple platforms
        ↓
Step 3: Look for photos - use /verif-photo for guidance
        ↓
Step 4: Cross-reference information across sources
        ↓
Step 5: /report with confidence ratings
```

### Scenario 4: The "Full Sweep" (Easiest Method)

```
Step 1: /full "example.com"
        ↓
Step 2: Wait 3-5 minutes while all tools run automatically
        ↓
Step 3: Review both reports generated:
        - Technical report (detailed)
        - Simple report (easy to understand)
        ↓
Step 4: Ask follow-up questions about specific findings
```

---

## Understanding Confidence Ratings

Every finding is marked with a confidence level:

| Icon | Level | Meaning |
|------|-------|---------|
| 🟢 | **HIGH** | Verified from official/authoritative source |
| 🟡 | **MEDIUM** | Multiple sources agree but not 100% confirmed |
| 🔴 | **LOW** | Single source or unverified |
| ⚪ | **SPECULATIVE** | Analyst guess based on patterns |

**Rule of Thumb**: Only trust 🟢 and 🟡 for important decisions. Use 🔴 as leads to investigate further.

---

## Passive Mode: Always Listening

Even without commands, the skill recognizes:
- Email addresses (`name@domain.com`)
- Domain names (`example.com`)
- Usernames (`@username`)
- IP addresses (`192.168.1.1`)
- Phone numbers

When detected, it will suggest next steps like "Want me to `/dork` this domain?"

---

## Quick Reference: Natural Language Patterns

| You Say | Skill Interprets As |
|---------|---------------------|
| "Search for..." | General web search |
| "Find info on..." | Person/entity recon |
| "Check security of..." | Domain dorking |
| "Look up username..." | Username recon |
| "What do we know about..." | Entity map query |
| "Follow up on..." | Pivot search |
| "Create a report" | Generate INTSUM |

---

## Safety & Ethics Reminders

✅ **DO**:
- Investigate public figures, companies, your own footprint
- Use for journalism, research, security audits
- Verify information across multiple sources
- Respect people's privacy

❌ **DON'T**:
- Use for harassment, stalking, or doxing
- Attempt to access private accounts
- Trust single sources without verification
- Share personal information irresponsibly

---

## Example Session (Step-by-Step)

```
You: Find information about techtiff.ai

Assistant: [Runs reconnaissance]
🟢 Found: Website at https://techtiff.ai
🟡 Found: Subdomain flow-club.techtiff.ai
🟢 Found: Associated with Tiffany Kyazze (founder)
🟢 Found: Uses Circle.so platform

You: /dork techtiff.ai

Assistant: [Checks for exposed files/admin panels]
✅ No sensitive files exposed
✅ No admin panels found publicly
🟢 Privacy policy at /privacy

You: Who is Tiffany Kyazze?

Assistant: [Runs person recon]
🟢 Founder of TechTiff
🟢 Co-founder of AI Flow Club with David Kyazze-Ntwatwa
🟡 Likely spouse/partner (shared surname)
...

You: /simple-report

Assistant: [Generates easy-to-understand report]
=== SIMPLE SUMMARY ===
THE BOTTOM LINE:
TechTiff is an AI education company run by Tiffany Kyazze 
and her partner David. They teach people how to use AI tools.

WHAT WE FOUND:
• Website: techtiff.ai (active and secure)
• 800+ member community called "AI Flow Club"
• Tiffany has 250,000+ followers on social media
• No security problems found on the website
...
```

## Example Session (The Easy Way)

```
You: /full techtiff.ai

Assistant: Running comprehensive investigation...
Phase 1: Reconnaissance ✓
Phase 2: Security Check ✓
Phase 3: Timeline Build ✓
Phase 4: Report Generation ✓

DONE! Generated 2 reports:
1. techtiff-ai-technical-report.md (for experts)
2. techtiff-ai-simple-report.md (easy version)

Key Finding: Clean security posture, active business.
```

---

## Tips for Better Results

1. **Start Broad, Then Narrow**: Begin with `/recon`, then use `/pivot` on interesting findings
2. **Follow the Breadcrumbs**: One piece of data leads to another
3. **Check Confidence Ratings**: Don't treat speculation as fact
4. **Generate Reports**: Use `/report` to document findings
5. **Stay Safe**: Use `/sock-opsec` if investigating sensitive topics

---

## Need Help?

- Type `/help` for command list
- Ask "What can this skill do?"
- Request examples: "Show me an example investigation"

---

**Remember**: OSINT is about connecting dots that are already public. Be ethical, be thorough, be skeptical.

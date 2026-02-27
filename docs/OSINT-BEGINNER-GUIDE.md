# OSINT Investigator Skill - Beginner's Guide v2.0

A comprehensive guide to investigating people, domains, and organizations using natural language and slash commands. **Now with 40+ commands, wizards, templates, and accessibility features!**

---

## What This Skill Does

The OSINT (Open Source Intelligence) Investigator helps you find publicly available information about:
- **People** (names, usernames, emails, social profiles)
- **Domains/Websites** (security, exposed files, subdomains, history)
- **Organizations** (company info, digital footprint, key personnel)
- **Documents** (metadata, EXIF data, hidden content)
- **Breaches** (compromised credentials, data leaks)

**Important**: This tool only searches **publicly available information**. It does not hack, bypass authentication, or access private data.

---

## 🎯 Three Ways to Use It

### Method 1: Natural Language (Easiest)

Simply type what you want in plain English. The skill recognizes:
- Email addresses (`name@domain.com`)
- Domain names (`example.com`)
- Usernames (`@username`)
- IP addresses (`192.168.1.1`)
- Phone numbers

**Examples:**

| What You Type | What Happens |
|---------------|--------------|
| "Find information about John Smith" | Starts person reconnaissance |
| "Search for example.com" | Runs domain reconnaissance |
| "Look up username @johnsmith on social media" | Searches username across platforms |
| "What can you find on sarah@email.com?" | Investigates email address |
| "Check security of mysite.com" | Runs security-focused dorking |

**Tip**: Be specific! The more details you provide (location, company, etc.), the better the results.

---

### Method 2: The "Easy Button" (`/full`)

**New in v2.0!** Just want everything at once?

```
User: /full example.com

Agent: Running comprehensive investigation...
Phase 1: Reconnaissance ✓
Phase 2: Security Analysis ✓
Phase 3: Timeline Build ✓
Phase 4: Entity Mapping ✓
Phase 5: Dual Report Generation ✓

DONE! Generated 2 reports:
1. Technical Report (detailed, for experts)
2. Simple Report (easy to understand)
```

**When to use `/full`:**
- Starting a new investigation
- Doing due diligence on a company
- You want everything without typing multiple commands
- You want both technical and simple reports automatically

---

### Method 3: Guided Wizards (Most Helpful)

**New in v2.0!** Step-by-step guidance for specific tasks:

```
/wizard person
# Guides you through investigating a person

/wizard domain
# Step-by-step domain security audit

/wizard photo
# Verify if a photo is authentic
```

**Perfect for:**
- First-time users
- Seniors (65+)
- Anyone who wants hand-holding
- Complex investigations

---

## 📚 Complete Command Reference

### 🚀 Core Investigation Commands

| Command | What It Does | Example |
|---------|--------------|---------|
| `/full [target]` | **Run EVERYTHING at once** (easiest) | `/full example.com` |
| `/recon [target]` | Full reconnaissance pass | `/recon elonmusk` |
| `/dork [domain]` | Find exposed files, admin panels, APIs | `/dork example.com` |
| `/pivot [data]` | Follow a lead (email, username, phone) | `/pivot john@email.com` |
| `/timeline [subject]` | Build chronological history | `/timeline Tesla` |
| `/entity [name]` | Track what we know about someone | `/entity John Smith` |

### 📊 Report Commands

| Command | What It Does | Best For |
|---------|--------------|----------|
| `/report` | Technical intelligence summary | Security professionals |
| `/simple-report` | Plain-language (8th grade level) | Clients, managers, seniors |
| `/report brief` | One-page executive summary | C-suite, quick overview |
| `/report json` | Structured data export | Integration with other tools |
| `/report csv` | Spreadsheet format | Data analysis |
| `/report legal` | Evidence-focused format | Legal proceedings |
| `/report journalist` | Source-citation heavy | Journalism |

### 🧠 Intelligence & Analysis

| Command | What It Does |
|---------|--------------|
| `/risk-assessment` | Generate risk profile (1-100 score) |
| `/risk-trend` | Show how risk changes over time |
| `/history [url]` | View historical website snapshots |
| `/what-changed [url]` | Compare old vs new versions |
| `/deleted-content [url]` | Find content that was removed |
| `/breach-check [email]` | Check if email was in data breaches |
| `/leak-search [term]` | Search paste sites for leaks |

### 📈 Visualization Commands

| Command | What You'll See |
|---------|-----------------|
| `/visualize entities` | ASCII relationship graph |
| `/visualize timeline` | Gantt-style timeline chart |
| `/visualize risk` | Risk heatmap matrix |
| `/visualize network` | Connection topology map |
| `/dashboard` | Interactive investigation overview |
| `/stats` | Investigation statistics |

### 🔍 Specialized Analysis

| Command | Use Case |
|---------|----------|
| `/analyze-doc` | Extract metadata from documents |
| `/compare-docs [f1] [f2]` | Find differences between files |
| `/redaction-check [pdf]` | Verify PDF redactions are secure |
| `/analyze-email` | Analyze email headers |
| `/analyze-metadata` | Analyze EXIF, HTTP headers |
| `/verif-photo` | Verify photo authenticity |

### 💾 Session Management

| Command | Purpose |
|---------|---------|
| `/save [name]` | Save current investigation |
| `/load [name]` | Load previous investigation |
| `/list-saves` | Show all saved investigations |
| `/compare [s1] [s2]` | Compare two sessions |
| `/status` | Check operation status |
| `/cancel` | Stop current operation |

**Auto-Save**: Investigations auto-save every 5 minutes!

### 🎯 UX & Accessibility

| Command | Purpose |
|---------|---------|
| `/mode beginner` | Simple mode with explanations |
| `/mode expert` | Advanced mode, minimal hand-holding |
| `/wizard person` | Guided person investigation |
| `/wizard domain` | Guided domain audit |
| `/wizard photo` | Guided photo verification |
| `/template list` | Show investigation templates |
| `/template run [name]` | Run a template |
| `/tutorial` | First-time user tutorial |
| `/glossary` | Look up OSINT terms |
| `/accessibility` | Accessibility options |
| `/explain [finding]` | Explain a specific finding |

### ✅ Quality Assurance

| Command | Purpose |
|---------|---------|
| `/qa-check` | Run quality assurance on investigation |
| `/coverage` | Show what you've checked |
| `/gaps` | Identify missing investigation areas |
| `/verify-sources` | Check if sources are still valid |

### 🔒 Safety

| Command | Purpose |
|---------|---------|
| `/sock-opsec` | Get privacy/safety tips |
| `/entity [name]` | Check what we know |

---

## 🎓 Understanding Modes

### Beginner Mode (`/mode beginner`)
- Simple explanations for everything
- Auto-suggests next steps
- Patient, no-jargon language
- Extra guidance and examples
- **Best for**: First-time users, seniors, non-technical users

### Expert Mode (`/mode expert`)
- Raw output, minimal explanation
- Assumes you know OSINT
- Technical terminology
- Faster execution
- **Best for**: Security professionals, researchers

**Switch anytime:** `/mode beginner` or `/mode expert`

---

## 📋 Investigation Templates

**New in v2.0!** Pre-built workflows for common tasks:

### Available Templates

| Template | Use Case | Command |
|----------|----------|---------|
| Due Diligence | Investigate a company before investment | `/template run due-diligence` |
| Background Check | Vet a person (HR, dating, etc.) | `/template run background-check` |
| Security Audit | Check website/domain security | `/template run security-audit` |

### What Templates Do

1. **Due Diligence** (7 steps):
   - Company reconnaissance
   - Key personnel identification
   - Domain security audit
   - Financial records search
   - News/sentiment analysis
   - Risk assessment
   - Executive report generation

2. **Background Check** (6 steps):
   - Identity verification
   - Social media screening
   - Credential verification
   - Timeline construction
   - Risk scoring
   - Final report

3. **Security Audit** (5 steps):
   - Domain reconnaissance
   - Dork execution
   - Vulnerability assessment
   - Risk scoring
   - Remediation recommendations

---

## 🔍 Understanding Confidence Ratings

Every finding is marked with a confidence level:

| Icon | Level | Meaning | Trust It? |
|------|-------|---------|-----------|
| 🟢 | **HIGH** | Verified from official/authoritative source | ✅ Yes |
| 🟡 | **MEDIUM** | Multiple sources agree but not 100% confirmed | ✅ Probably |
| 🔴 | **LOW** | Single source, unverified | ⚠️ Investigate more |
| ⚪ | **SPECULATIVE** | Analyst guess based on patterns | ❌ Don't act on it |

**Rule of Thumb**: Only trust 🟢 and 🟡 for important decisions. Use 🔴 as leads to investigate further. Never act on ⚪ alone.

---

## 🧩 Risk Scoring Explained

**New in v2.0!** Automated risk assessment (0-100 scale):

| Score | Level | Meaning |
|-------|-------|---------|
| 0-25 | **LOW** | Minimal concerns |
| 26-50 | **MODERATE** | Some issues, monitor |
| 51-75 | **HIGH** | Significant problems |
| 76-100 | **CRITICAL** | Immediate action needed |

**Risk Categories:**
- **Security**: Exposed credentials, admin panels, vulnerabilities
- **Privacy**: Personal info leaks, location exposure
- **Reputation**: Negative news, controversial associations
- **Legal**: Regulatory issues, compliance problems

---

## 📖 Glossary of Common Terms

**Quick reference** (use `/glossary` anytime for full list):

| Term | Simple Definition |
|------|-------------------|
| **API** | A way for computer programs to talk to each other |
| **Dork** | Special search query to find hidden information |
| **EXIF Data** | Hidden info in photos (location, camera, time) |
| **Metadata** | Hidden data about files, emails, or documents |
| **OSINT** | Finding public information to answer questions |
| **Pivot** | Using one clue to find more information |
| **Reconnaissance** | Researching and gathering information |
| **Subdomain** | A smaller section of a website (like blog.example.com) |

---

## 🛡️ Safety & Ethics

### ✅ DO:
- Investigate public figures and companies
- Check your own digital footprint
- Use for journalism, research, security audits
- Verify information across multiple sources
- Respect people's privacy
- Report illegal activity to authorities

### ❌ DON'T:
- Use for harassment, stalking, or doxing
- Attempt to access private accounts
- Trust single sources without verification
- Share personal information irresponsibly
- Use findings to harm others
- Ignore legal and ethical boundaries

**Remember**: This is research, not surveillance.

---

## 💡 Pro Tips for Better Results

1. **Start with `/full`** - Gets you everything quickly
2. **Use `/wizard` when stuck** - Guides you step-by-step
3. **Check confidence ratings** - Don't trust 🔴 or ⚪ alone
4. **Save your work** - Use `/save` frequently
5. **Review coverage** - `/coverage` shows what you might have missed
6. **Generate reports** - Document everything with `/report`
7. **Stay safe** - Use `/sock-opsec` for sensitive investigations
8. **Ask for help** - `/explain` clarifies any finding

---

## 📱 Example Sessions

### Example 1: The Complete Beginner

```
You: /mode beginner

Agent: Switched to beginner mode. I'll explain everything simply!

You: /wizard person

Agent: Welcome to the Person Investigation Wizard!
Step 1/6: What's the person's full name?
You: John Smith

Agent: Step 2/6: Do you know their location? (optional)
You: New York

[Wizard continues...]

Agent: Investigation complete! Summary:
🟢 Found LinkedIn profile
🟡 Found Twitter account
🟢 Found company affiliation

Would you like me to generate a simple report? (yes/no)
You: yes

Agent: [Generates plain-English report]
```

### Example 2: The Power User

```
You: /full examplecorp.com

Agent: [Runs complete investigation - 5 minutes]

PHASE 1: Recon ✓ (12 entities found)
PHASE 2: Security ✓ (3 low-risk issues)
PHASE 3: Timeline ✓ (24 dated events)
PHASE 4: Reports ✓ (Generated 2 reports)

Risk Score: 15/100 (LOW)

Reports saved:
- examplecorp-technical-report.md
- examplecorp-simple-report.md

You: /visualize network

Agent: [Shows ASCII relationship graph]

You: /save examplecorp-investigation

Agent: Saved!
```

### Example 3: Security Audit

```
You: /template run security-audit

Agent: Running Security Audit Template...
Step 1/5: Domain reconnaissance
🟢 Domain: example.com
🟢 IP: 192.168.1.1
🟡 Subdomains: 5 found

Step 2/5: Security dorking
🔴 Exposed: /api/v1/docs
🔴 Exposed: staging.example.com
🟢 Clean: No leaked credentials

Step 3/5: Vulnerability assessment
Risk: 65/100 (HIGH)

Step 4/5: Risk scoring
Category breakdown:
- Security: HIGH (exposed APIs)
- Privacy: LOW
- Legal: MODERATE

Step 5/5: Remediation recommendations
1. IMMEDIATE: Secure /api/v1/docs
2. HIGH: Add auth to staging subdomain
3. MEDIUM: Review API documentation

Report generated: security-audit-report.md
```

---

## 🆘 Getting Help

**Anytime you need help:**

1. **Command list**: Type `/help`
2. **Explain something**: `/explain [finding]`
3. **Term definitions**: `/glossary`
4. **Tutorial**: `/tutorial`
5. **What's this skill do?** Ask "What can this skill do?"

---

## 🎯 Quick Start Checklist

**First time using the skill?**

- [ ] Try `/tutorial` for a walkthrough
- [ ] Run `/full example.com` to see everything
- [ ] Try `/wizard person` for guided help
- [ ] Check `/glossary` if you see unfamiliar terms
- [ ] Use `/simple-report` for easy-to-read results
- [ ] Save your work with `/save my-first-investigation`

---

## 🔮 What's New in v2.0?

If you used v1.0, here's what's new:

- ✅ **40+ commands** (was 10)
- ✅ **Investigation templates** - Pre-built workflows
- ✅ **Guided wizards** - Step-by-step assistance
- ✅ **Risk scoring** - Automated risk assessment
- ✅ **Session management** - Save/load investigations
- ✅ **Visualization** - ASCII graphs and dashboards
- ✅ **Quality assurance** - Check your investigation coverage
- ✅ **Accessibility** - Senior-friendly features
- ✅ **Export formats** - JSON, CSV, IOC
- ✅ **Professional playbooks** - For journalists, HR, security

---

**Ready to start investigating? Try `/full [target]` or `/wizard person` now!**

**Remember**: Be ethical, be thorough, be skeptical. Happy investigating! 🔍

# Feedback Mechanisms

Clear, actionable feedback keeps users informed and confident in their investigations.

---

## Confirmation Prompt Design

### When to Request Confirmation

**Always Confirm:**
- Operations affecting >50 results
- Deleting saved investigations
- Modifying template configurations
- Exporting sensitive data
- Sharing findings externally
- Running resource-intensive scans

**Never Confirm (Beginner Override):**
- Simple /recon queries
- Status checks
- Help requests
- Viewing results
- Navigation commands

### Confirmation Prompt Format

**Standard Confirmation:**
```
⚠️ Confirm Action

You are about to: Delete investigation "Project Alpha"

This will:
  ✗ Remove all 23 saved findings
  ✗ Delete associated timeline
  ✗ Cannot be undone

Type 'yes' to proceed, or 'no' to cancel.
```

**Destructive Action Confirmation:**
```
🛑 DESTRUCTIVE ACTION

You are about to: Purge all search history

WARNING:
  • This deletes 156 saved investigations
  • This removes 2,340 stored findings
  • This action is PERMANENT
  
To confirm, type: PURGE
To cancel, type: no
```

### Confirmation Shortcuts

| Mode | Confirm | Cancel |
|------|---------|--------|
| Beginner | Type "yes" | Type "no" or press Escape |
| Intermediate | "y" or "yes" | "n" or "no" |
| Expert | "y" | "n" or Ctrl+C |

---

## Error Message Templates

### Error Message Structure

```
Every error includes:
  1. Error icon/indicator
  2. Plain language description
  3. What went wrong (technical, optional)
  4. How to fix it
  5. Prevention tip
  6. Alternative approach
```

### Error Templates by Type

**Input Error (Invalid Command):**
```
❌ I Didn't Understand

You typed: "rekcon example.com"

I think you meant: /recon example.com

Try:
  /recon example.com
  
Or see all commands: /help
```

**Input Error (Missing Parameter):**
```
❌ Missing Information

The /recon command needs a target to search for.

You typed: /recon

Try:
  /recon example.com     (domain)
  /recon johndoe@email.com  (email)
  /recon "John Smith"    (person)

What would you like to investigate?
```

**Network Error:**
```
⚠️ Connection Issue

I couldn't reach the search service.

This might be:
  • Temporary network problem
  • Service maintenance
  • Rate limit reached

What to do:
  1. Wait 30 seconds and try again
  2. Check your internet connection
  3. Try a different search: /dork "query"

Error code: NET_TIMEOUT (try again in 0:42)
```

**Rate Limit Error:**
```
⏱️ Slow Down

You've made many requests quickly.

To be fair to everyone, I need to pause briefly.

Retry available in: 45 seconds

While waiting:
  • Review your findings: /results
  • Check status: /status
  • Read tips: /help efficiency

Your progress is saved automatically.
```

**Permission Error:**
```
🚫 Access Denied

I can't access that resource.

Possible reasons:
  • Content requires login
  • Site blocks automated access
  • Geographic restriction

Alternatives:
  • Try manual search with: /dork "query" site:example.com
  • Check if site has public API
  • Search for cached version: /cache URL

Error code: ACCESS_DENIED
```

**Data Error (No Results):**
```
ℹ️ No Results Found

I searched for "xyzabc123456" but found nothing.

This could mean:
  • The information doesn't exist online
  • Different spelling or format
  • Too new to be indexed

Try:
  • Check your spelling
  • Try name variations
  • Use broader terms
  • Search in specific time range

Need help? /help search-tips
```

**System Error:**
```
🔧 Unexpected Problem

Something went wrong on my end.

Error ID: ERR_20240115_143052

What happened:
  An unexpected error occurred while processing your request.

What to do:
  1. Try your command again
  2. If it fails again, try: /reset
  3. Still broken? Report: /feedback

Your data is safe. Nothing was lost.
```

### Error Recovery Flow

```
Automatic recovery attempts:
  1. Retry failed request (1 time, immediate)
  2. Try alternative source
  3. Degrade to simpler query
  4. Offer manual workarounds
  
User-initiated recovery:
  /retry    - Try the operation again
  /skip     - Skip this step, continue
  /alt      - Show alternative approaches
  /debug    - Show technical details
```

---

## Success Indicators

### Success Message Format

```
✅ Operation Complete: [Brief description]

Results:
  • [Key finding 1]
  • [Key finding 2]
  • [Key finding 3]

Next steps:
  [Suggested action 1] | [Suggested action 2] | /help
```

### Success Variations

**Simple Success:**
```
✅ Found 5 email addresses for example.com

Next: /emails | /verify | /export
```

**Detailed Success:**
```
✅ Investigation Complete

Domain: example.com
Duration: 2 minutes 34 seconds
Sources checked: 12
Findings: 23 items

Highlights:
  📧 5 email addresses discovered
  🌐 8 subdomains mapped
  📄 3 leaked documents found
  ⚠️ 1 security issue identified

View: /summary | /details | /timeline
Export: /export pdf | /export csv
```

**Partial Success (with issues):**
```
✅ Investigation Complete (with notes)

Found: 15 items for example.com

⚠️ Some sources unavailable:
  • LinkedIn (rate limited)
  • Twitter API (maintenance)

Results may be incomplete.
Retry unavailable sources? /retry-failed
```

### Success Celebrations (Beginner Mode)

```
Small achievements:
  "🎉 First investigation complete!"
  "📊 10 findings collected!"
  "🔍 First custom dork created!"
  "⏱️ Speed milestone: 5 searches in 1 minute!"
```

---

## Warning Formats

### Warning Levels

| Level | Icon | Use Case | Action Required |
|-------|------|----------|-----------------|
| Info | ℹ️ | FYI only | None |
| Low | ⚠️ | Minor issue | Optional |
| Medium | ⚠️ | Caution advised | Review suggested |
| High | 🚨 | Significant risk | Acknowledge |
| Critical | 🛑 | Severe risk | Confirm to proceed |

### Warning Templates

**Low Priority (Informational):**
```
ℹ️ Note: Results limited to last 5 years
Use /daterange to search older content.
```

**Medium Priority (Caution):**
```
⚠️ Caution: Multiple similar names found

I found 3 "John Smith" profiles in Boston.
Please verify which person you're investigating.

/verify-identity for help
```

**High Priority (Risk):**
```
🚨 Privacy Warning

This search may reveal:
  • Personal addresses
  • Family member information
  • Private contact details

This data is sensitive. Handle responsibly.

Type 'acknowledge' to continue, or 'cancel' to stop.
```

**Critical Priority (Danger):**
```
🛑 CRITICAL WARNING

You're about to investigate a protected individual:
  • Law enforcement officer
  • Government official
  • Judge or prosecutor

This may:
  • Violate platform terms of service
  • Be illegal in your jurisdiction
  • Result in account suspension

You are responsible for compliance with all laws.

Type 'I understand the risks' to proceed.
Type 'cancel' to abort.
```

### Warning Persistence

```
Warnings remain visible:
  • High/Critical: Until acknowledged
  • Medium: For 30 seconds or until dismissed
  • Low: For 10 seconds, auto-dismiss

Warning history available via: /warnings
```

---

## Suggestion Presentation

### When to Suggest

```
Proactive suggestions:
  • After 3 similar commands
  • When operation completes
  • When user appears stuck
  • After error recovery
  • When new feature available

Reactive suggestions (on request):
  • /hint or /suggest commands
  • Help menu browsing
  • Tutorial progression
```

### Suggestion Format

**Contextual Suggestion:**
```
💡 Tip: You keep searching for email addresses.

Try this dork to find them faster:
  /dork "@example.com" filetype:pdf

Save time? /save-dork email-finder
```

**Next Step Suggestion:**
```
💡 Based on your findings, you might want to:

1. Cross-reference these emails: /verify emails
2. Check for password leaks: /leak-check
3. Map the domain infrastructure: /map-domain

Or ask me what any of these mean.
```

**Efficiency Suggestion:**
```
💡 Time-saver tip:

Instead of 3 separate searches, try:
  /template run full-background-check

This runs all steps automatically.
```

### Suggestion Preferences

```
User controls:
  /suggestions on  - Enable proactive tips
  /suggestions off - Tips on request only
  /suggestions max - Maximum helpfulness
  
System respects:
  • Declined suggestions (won't repeat)
  • Frequently used paths (prioritize)
  • Time of day (fewer interruptions)
```

---

## Help Request Handling

### Help Command Responses

**Generic Help:**
```
OSINT Investigator Help
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Quick Start:
  /tutorial         - First-time walkthrough
  /recon [target]   - Start investigating
  /template list    - See investigation templates

Common Tasks:
  /help search      - How to search effectively
  /help photos      - Verify image authenticity
  /help domains     - Audit website security
  /help people      - Background check guide

Learning:
  /glossary         - Term definitions
  /commands         - Full command list
  /examples         - Usage examples

Support:
  /status           - Check system status
  /feedback         - Report issues or ideas

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What would you like help with? (Type a topic)
```

**Contextual Help:**
```
User just got an error → /help

"It looks like you had trouble. Here's help for that:

Your error: Missing parameter

Fix:
  The /recon command needs a target.
  
  Try:
    /recon example.com
    /recon johndoe@email.com
    /recon "John Smith"

Full guide: /help recon"
```

### Help Escalation

```
Level 1: Automated response
  • Pattern matching
  • Command suggestions
  • Quick fixes

Level 2: Guided assistance
  • Step-by-step walkthrough
  • Interactive tutorial
  • Video demonstration

Level 3: Human support
  • /feedback with priority flag
  • Community forum link
  • Documentation reference
```

### Help Effectiveness Tracking

```
System tracks:
  • Help requests per session
  • Resolution success rate
  • Time to resolution
  • User satisfaction (implicit)

Adapts by:
  • Improving common help topics
  • Proactive help for frequent issues
  • Simplifying complex workflows
```

---

## Feedback Collection

### In-App Feedback

```
Command: /feedback

"How can we improve?"

1. Report a problem
2. Suggest a feature
3. Share praise
4. Get help

Or type your feedback now...
```

**Problem Report:**
```
"What went wrong?"

1. Command didn't work
2. Results were wrong
3. System was slow
4. Confusing interface
5. Something else

Your selection helps us fix it faster.
```

### Implicit Feedback

```
Tracked signals:
  • Command retry rate
  • Help request frequency
  • Session duration
  • Feature usage patterns
  • Error recovery success

Used for:
  • Priority bug fixes
  • UX improvements
  • Feature development
```

---

## Message Formatting Standards

### Icon Usage

| Icon | Meaning | Usage |
|------|---------|-------|
| ✅ | Success | Operation complete |
| ❌ | Error | Operation failed |
| ⚠️ | Warning | Caution needed |
| ℹ️ | Info | Additional context |
| 💡 | Tip | Helpful suggestion |
| 🔄 | Loading | In progress |
| ✓ | Complete | Step done |
| ○ | Pending | Step waiting |
| 🛑 | Blocked | Cannot proceed |
| 📊 | Results | Data available |

### Text Formatting

```
Emphasis:
  **bold** - Important actions, buttons
  _italic_ - Technical terms being defined
  `code` - Commands to type, file names

Structure:
  ━━━ - Section dividers
  • - List items
  1. - Numbered steps
  [text] - Buttons/actions
```

# Progress Tracking System

The progress system keeps users informed during long-running operations without overwhelming them.

---

## Progress Indicator Formats

### Visual Progress Bar

**For operations > 10 seconds:**
```
Investigating target... [████████░░░░░░░░░░░░] 40%

Current: Searching Twitter profiles
Sources checked: 4 of 12
Time elapsed: 1:23
Estimated remaining: 2:05
```

**Compact version (Expert mode):**
```
[████████░░] 40% | 4/12 src | 1:23 elapsed | ~2:05 left
```

**Minimal version (Expert + silent flag):**
```
40%...
```

### Spinner States

**For indeterminate operations:**
```
⏳ Analyzing image metadata...
🔄 Searching for similar images...
📡 Connecting to data sources...
🔍 Scanning public records...
📊 Compiling results...
```

### Stage-Based Progress

**For multi-phase operations:**
```
Domain Audit Progress:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ Phase 1: Domain registration lookup
✓ Phase 2: DNS record enumeration
✓ Phase 3: Subdomain discovery
⏳ Phase 4: Technology fingerprinting
○ Phase 5: Security header analysis
○ Phase 6: Vulnerability assessment
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Overall: 50% complete | Step 4 of 6
```

---

## Time Estimation Logic

### Estimation Algorithm

```python
Estimation Formula:
  remaining_time = (elapsed_time / percent_complete) - elapsed_time
  
Adjustments:
  - First 10%: Use historical averages
  - 10-50%: Linear projection
  - 50-90%: Weighted average (current rate 70%, historical 30%)
  - 90-100%: Pad by 20% (final steps often slower)

Uncertainty Display:
  - <30% complete: Show range (e.g., "2-5 minutes")
  - 30-70% complete: Show estimate (e.g., "~3 minutes")
  - >70% complete: Show countdown (e.g., "About 45 seconds")
```

### Time Display by Confidence

| Confidence | Display Format | Example |
|------------|----------------|---------|
| Low (<30%) | Range | "2-5 minutes remaining" |
| Medium (30-70%) | Estimate | "About 3 minutes left" |
| High (>70%) | Precise | "45 seconds remaining" |
| Unknown | Indefinite | "Still working..." |

### Historical Learning

```
System tracks:
  - Average time per source type
  - Time per result count
  - Network latency patterns
  - User's typical target complexity

Improves estimates by:
  - Comparing to similar past operations
  - Adjusting for current network conditions
  - Learning user's typical use patterns
```

---

## Search Counter Display

### Counter Types

**Source Counter:**
```
Checking data sources:
  ✓ Google (1,240 results scanned)
  ✓ Bing (890 results scanned)
  ✓ Twitter (12 profiles found)
  ⏳ LinkedIn (searching...)
  ○ Facebook (queued)
  ○ Instagram (queued)

Sources: 3 of 6 complete | Items found: 42
```

**Result Counter:**
```
Findings so far:
  📧 Emails: 5 found
  📱 Phone numbers: 2 found
  🏠 Addresses: 1 found
  👤 Social profiles: 8 found
  🌐 Websites: 3 found
  📄 Documents: 12 found

Total: 31 items discovered
```

### Counter Display Modes

**Beginner Mode:**
```
I've searched 3 websites and found:
  • 5 email addresses
  • 2 phone numbers
  
I'm now checking LinkedIn...
```

**Intermediate Mode:**
```
Progress: 3/6 sources | 31 items found
Current: LinkedIn search
Rate: ~10 items/minute
```

**Expert Mode:**
```
[3/6] G:1240 B:890 T:12 | L:⏳ F:○ I:○ | TTL:31 | R:~10/m
```

---

## Completion Percentage

### Percentage Calculation

```
Calculation Methods:

1. Source-based (default):
   percent = (completed_sources / total_sources) × 100

2. Time-based (for unknown scope):
   percent = min((elapsed / estimated_total) × 100, 99)

3. Phase-based (for workflows):
   percent = (completed_phases / total_phases) × 100
   
4. Item-based (for result enumeration):
   percent = (processed_items / estimated_total_items) × 100
```

### Percentage Display Rules

```
Never show:
  - 0% (show "Starting..." instead)
  - 100% until fully complete
  - Decreasing percentages
  - Impossible speeds (e.g., 50% to 90% in 1 second)

Always show:
  - Progress at least every 5 seconds
  - Activity indicator during stalls
  - Explanation if paused >10 seconds
```

### Completion Animation

```
At 100%:
  Beginner: "✓ All done! I found 23 items for you."
  Intermediate: "✓ Complete: 23 findings in 2:34"
  Expert: "✓ 23 finds | 2:34 | /view /export"

With results ready:
  [Animation: Progress bar fills green]
  [Sound cue: Optional soft chime]
  [Auto-advance to results after 2 seconds]
```

---

## Multi-Phase Operation Tracking

### Phase Definition

```
Each phase has:
  - Name (short, descriptive)
  - Estimated duration
  - Dependencies (what must complete first)
  - Skippable (can user skip?)
  - Critical (must succeed?)
```

### Phase Display

**Active Investigation:**
```
Background Check: John Smith

Phase 1: Identity Verification
  ✓ Validate full name
  ✓ Check for name variations
  ✓ Confirm approximate age/location
  
Phase 2: Digital Presence
  ⏳ Search social media platforms
  ⏳ Find professional profiles
  ○ Check public records
  
Phase 3: Credential Verification
  ○ Verify employment claims
  ○ Check education history
  ○ Validate certifications

Overall: 40% | Phase 2 of 3 | Est: 4 min remaining

[/skip-phase] [/pause] [/cancel]
```

### Phase Skipping

```
When user enters: /skip-phase

"Skip current phase (Digital Presence)?
 
 ⚠ Skipping means:
   • Social media findings will be incomplete
   • Some connections may be missed
   • You can re-run this phase later with /phase 2
   
 Type 'yes' to skip, or 'no' to continue waiting."
```

### Dependency Handling

```
If Phase 3 depends on Phase 2:
  Phase 2 fails → Offer to:
    a) Retry Phase 2
    b) Skip with limited Phase 3
    c) Stop investigation

Display:
  "Phase 2 had some issues. Phase 3 may be incomplete.
   Continue anyway? (yes/no/retry)"
```

---

## Background Operation Handling

### Background Mode Activation

```
Commands that support background mode:
  /recon [target] --background
  /audit [domain] --background
  /template run [name] --background

When to suggest background mode:
  - Operation estimated > 5 minutes
  - User starts new command while one runs
  - User explicitly requests it
```

### Background Progress Display

```
[Status Bar - always visible]
🔄 2 background tasks running | /status for details

[Detailed Status - via /status command]
Background Operations:

1. Domain Audit: example.com [████████░░] 80%
   Started: 10:23 AM | Est complete: 10:31 AM
   
2. Person Search: "Jane Smith" [████░░░░░░] 40%
   Started: 10:25 AM | Est complete: 10:35 AM

Commands: /foreground 1 | /cancel 2 | /results 1
```

### Completion Notifications

```
When background task completes:

Visual notification:
  "✓ Domain audit complete! 23 findings ready."
  [Button: View Results] [Button: Dismiss]

Auto-save:
  Results automatically saved to /history
  Available via /results [task-id] at any time

Notification settings:
  /notify on  - Enable completion alerts
  /notify off - Disable alerts (check /status manually)
```

### Foreground Switching

```
Command: /foreground [task-id]

Brings background task to foreground:
  - Shows full progress display
  - Allows real-time interaction
  - Pauses other background tasks if resource-limited

Example:
  /foreground 1
  
  "Domain audit now in foreground.
   [████████░░] 80% complete..."
```

---

## Status Command

### /status Implementation

```
Default /status output:

System Status
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Active Operations: 2
  1. Recon: example.com [██████░░░░] 60% ~2min
  2. Dork: "CEO email" [██░░░░░░░░] 20% ~5min

Recent Completions (last hour):
  ✓ Photo verification (10:15 AM)
  ✓ WHOIS lookup (10:08 AM)
  ✓ Social search (9:45 AM)

System Health:
  ✓ All data sources online
  ✓ Rate limits: Normal
  ✓ Response time: 1.2s average

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
/history | /cancel | /results
```

### Status by Context

```
When operations running:
  /status → Shows operation details
  
When no operations:
  /status → Shows recent history + system health
  
When errors occurred:
  /status → Highlights failed operations
```

---

## Progress Display Best Practices

### Timing Rules

```
Show progress indicator when:
  - Operation > 2 seconds: Brief spinner
  - Operation > 10 seconds: Progress bar
  - Operation > 60 seconds: Detailed breakdown
  - Operation > 5 minutes: Background mode option

Update frequency:
  - <10 seconds: Every second
  - 10-60 seconds: Every 5 seconds
  - >60 seconds: Every 10 seconds + activity proof
```

### Activity Proof

```
During long operations, periodically show:
  "Still working... found 3 more items in last 10 seconds"
  "Still working... checked 200 more results"
  "Still working... currently analyzing metadata"

Prevents user thinking system is frozen.
```

### Cancellation Handling

```
When user enters: /cancel

"Cancel current operation?
 
 Progress will be lost:
   • 3 sources completed
   • 12 items found so far
   
 Type 'yes' to cancel, 'no' to continue, 
 or 'background' to run in background."
```

---

## Mobile Progress Display

### Mobile Optimizations

```
Compact format for small screens:
  ▓▓▓▓▓░░░░░ 50%
  3 of 6 complete
  ~2 min left

Swipe actions:
  - Swipe up: More details
  - Swipe down: Minimize to status bar
  - Tap: Pause/resume

Status bar integration:
  Always show: 🔄 2 tasks | /status
```

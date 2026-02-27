# Complexity Levels System

The OSINT Investigator Skill uses a three-tier progressive disclosure system that adapts to user expertise.

---

## Mode Overview

| Mode | Target User | Detail Level | Automation | Explanation |
|------|-------------|--------------|------------|-------------|
| Beginner | First-time users, seniors | Minimal output | High | Every step explained |
| Intermediate | Occasional users | Moderate detail | Medium | On-demand explanation |
| Expert | Daily practitioners | Full detail | Low | Reference only |

---

## Beginner Mode

### Specifications

- **Output Length**: Maximum 3 sentences per finding
- **Technical Terms**: Always defined on first use
- **Command Suggestions**: Proactively offered
- **Error Handling**: Gentle with full recovery guidance
- **Examples**: Every command includes usage example
- **Confirmations**: Always confirm before destructive actions

### Beginner Mode Behaviors

```
When user enters: /recon example.com

Beginner output:
  "I found 3 social media profiles for example.com.
   [1] Twitter: @example
   [2] LinkedIn: Example Inc
   [3] Facebook: Example Company
   
   Would you like me to investigate any of these profiles?
   (Say 'investigate 1' to look at Twitter, or 'skip' to continue)"
```

### Auto-Explanation Triggers (Beginner)

- First use of any command
- Commands with more than 2 parameters
- Error conditions
- When operation exceeds 30 seconds
- Any finding with risk score > 7

---

## Intermediate Mode

### Specifications

- **Output Length**: Up to 1 paragraph per finding
- **Technical Terms**: Defined on request with `/explain [term]`
- **Command Suggestions**: Offered when relevant
- **Error Handling**: Concise with recovery steps
- **Examples**: Available via `/help [command]`
- **Confirmations**: For operations affecting > 10 results

### Intermediate Mode Behaviors

```
When user enters: /recon example.com

Intermediate output:
  "Found 3 social profiles for example.com:
   - Twitter (@example): 50K followers, active since 2010
   - LinkedIn (Example Inc): 500 employees listed
   - Facebook (Example Company): 12K likes
   
   Use /pivot 1 to investigate Twitter, or /timeline to see activity patterns."
```

---

## Expert Mode

### Specifications

- **Output Length**: Raw data, structured output
- **Technical Terms**: None defined automatically
- **Command Suggestions**: Only on `/hint`
- **Error Handling**: Error codes with minimal text
- **Examples**: None displayed
- **Confirmations**: Disabled (use `/confirm` flag for safety)

### Expert Mode Behaviors

```
When user enters: /recon example.com

Expert output:
  [PROFILES_FOUND:3]
  [0] TWITTER: @example | F:50K | S:2010 | R:0.82
  [1] LINKEDIN: Example Inc | E:500 | R:0.91
  [2] FACEBOOK: Example Company | L:12K | R:0.76
```

---

## Mode Switching Logic

### Automatic Mode Detection

```
User indicators for BEGINNER mode:
  - First session with skill
  - Using `/tutorial` command
  - Typing full sentences as commands
  - Hesitant typing patterns (>5s between keystrokes)
  - Repeated similar commands
  - Explicit age indication (65+)
  - Using accessibility features

User indicators for EXPERT mode:
  - Rapid command sequences
  - Using advanced flags
  - Referencing documentation
  - Custom dork creation
  - Pipeline construction
  - Explicit certification mention
```

### Manual Mode Selection

```
Command: /mode [level]

/mode beginner    # Switch to beginner mode
/mode intermediate # Switch to intermediate mode
/mode expert      # Switch to expert mode
/mode auto        # Return to auto-detection
```

### Mode Persistence

- Selected mode persists for session duration
- Mode preference stored in user profile
- Can be overridden per-command with flags
- `/mode reset` returns to default

---

## Auto-Suggestion System

### Complexity Level Recommendation

```
When user performance indicates mode mismatch:

  IF user in EXPERT mode BUT:
    - Using `/help` frequently (>3 times in 5 minutes)
    - Commands failing with syntax errors
    - Requesting explanations
  
  THEN suggest: "Consider switching to Intermediate mode? /mode intermediate"

  IF user in BEGINNER mode BUT:
    - Using advanced commands correctly
    - Declining explanations
    - Rapid successful command sequences
  
  THEN suggest: "You're moving fast! Try Expert mode? /mode expert"
```

### Smart Mode Prompts

| Situation | Suggested Action |
|-----------|------------------|
| First login | Start in Beginner, offer `/tutorial` |
| After 10 successful commands | Suggest Intermediate mode |
| After creating custom dorks | Suggest Expert mode |
| Multiple errors in sequence | Offer Beginner mode |
| Using `/glossary` frequently | Stay in Beginner mode |

---

## Contextual Help System

### Help Triggering

```
Automatic help appears when:
  1. Unknown command entered
  2. Command syntax invalid
  3. Command takes >60 seconds
  4. User types "help" or "?"
  5. Finding with confidence < 50%

Manual help via: /help or /help [command]
```

### Help Display by Mode

**Beginner Mode:**
```
"I didn't understand 'rekcon'. Did you mean:
   • /recon - Find information about a target
   • /dork - Search with advanced queries
   
   Type the number or full command to proceed.
   Or type '/tutorial' for a guided walkthrough."
```

**Intermediate Mode:**
```
"Unknown command: 'rekcon'
Did you mean: /recon ?
Use /help for command list."
```

**Expert Mode:**
```
ERR_UNKNOWN_COMMAND: rekcon
SUGGEST: /recon
```

### Proactive Suggestions

```
After 3 consecutive /recon commands on same target:
  "Tip: Try /pivot to discover connected accounts"

After finding email without domain:
  "Tip: Use /recon [domain] to find associated website"

After timeline with gaps:
  "Tip: Use /dork with date ranges to fill timeline gaps"
```

---

## Mode Comparison Table

| Feature | Beginner | Intermediate | Expert |
|---------|----------|--------------|--------|
| Output verbosity | High | Medium | Minimal |
| Auto-explanation | Always | On request | Never |
| Command confirmation | All | Major ops | None |
| Error detail | Full guide | Steps only | Code only |
| Progress indicators | Visual + text | Text | None |
| Example inclusion | Always | On demand | Never |
| Glossary links | Auto | Manual | None |
| Tutorial offers | Frequent | Rare | Never |
| Large text default | Yes | No | No |
| Simplified aliases | Yes | Optional | No |

---

## Implementation Notes

### Mode Configuration Storage

```json
{
  "user_mode": "beginner",
  "auto_detect": true,
  "explanations_enabled": true,
  "large_text": false,
  "session_count": 5,
  "proficiency_score": 23
}
```

### Mode Detection Algorithm

```
Proficiency Score Calculation:
  +5 points: Successful command execution
  +10 points: Advanced command usage
  +15 points: Custom dork creation
  -5 points: Help request
  -10 points: Syntax error
  -3 points: Timeout/cancellation

Mode thresholds:
  Beginner: 0-25 points
  Intermediate: 26-75 points
  Expert: 76+ points
```

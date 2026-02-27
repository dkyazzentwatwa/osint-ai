# Senior-Friendly Features

Accessibility features designed specifically for users aged 65 and older.

---

## Large Text Mode

### Activation

```
Command: /large-text

When activated:
  ✓ All text increases by 40%
  ✓ Line spacing increases
  ✓ Paragraphs break more frequently
  ✓ Icons scale with text
  ✓ Button sizes increase

Toggle off: /large-text off
```

### Large Text Display

**Normal Mode:**
```
Found 5 email addresses for example.com
- admin@example.com
- support@example.com
```

**Large Text Mode:**
```
━━━━━━━━━━━━━━━━━━━━

SEARCH RESULTS

I found 5 email addresses 
for example.com:

1. admin@example.com
2. support@example.com

━━━━━━━━━━━━━━━━━━━━
```

### Font Specifications

```
Large Text Settings:
  Base font size: 18pt (normal: 12pt)
  Heading size: 24pt (normal: 16pt)
  Line height: 1.8 (normal: 1.4)
  Paragraph spacing: 1.5em (normal: 1em)
  Max line length: 60 characters
```

---

## High Contrast Mode

### Activation

```
Command: /accessibility

Menu:
  [ ] High Contrast Mode
  [ ] Large Text Mode
  [ ] Screen Reader Mode
  [ ] Simplified Commands
  
Toggle: Click or type number
```

### Contrast Options

**High Contrast (Black/White):**
```
██████████████████████████████████
██                              ██
██  SEARCH RESULTS              ██
██                              ██
██  Found: 5 items              ██
██                              ██
██████████████████████████████████
```

**Dark Mode High Contrast:**
```
Background: #000000 (pure black)
Text: #FFFFFF (pure white)
Accents: #FFFF00 (yellow)
Warnings: #FF0000 (red)
Success: #00FF00 (green)
```

**Light Mode High Contrast:**
```
Background: #FFFFFF (pure white)
Text: #000000 (pure black)
Accents: #0000FF (blue)
Warnings: #CC0000 (dark red)
Success: #006600 (dark green)
```

### Visual Indicators

```
Color-blind friendly indicators:
  ✓ Success: Checkmark + "✓" + GREEN
  ✗ Error: X mark + "✗" + RED
  ⚠ Warning: Triangle + "⚠" + YELLOW
  ℹ Info: Circle + "ℹ" + BLUE
  
Always use BOTH color AND symbol
```

---

## Screen Reader Optimization

### ARIA Labels

```
Every interactive element includes:
  • Descriptive labels
  • Role definitions
  • State announcements
  • Context information

Example button:
  <button aria-label="Run domain security audit for example.com"
          role="button"
          aria-pressed="false">
    Run Audit
  </button>
```

### Structured Output

**Before (Screen reader unfriendly):**
```
Found 5. admin@example.com support@example.com ...
```

**After (Screen reader optimized):**
```
"Search complete. Found 5 email addresses. 
 First: admin at example dot com. 
 Second: support at example dot com. 
 Say 'next' to hear more, or 'stop' to end."
```

### Navigation Shortcuts

```
Screen Reader Commands:
  
  R - Read current section
  N - Next result/item
  P - Previous result/item
  S - Stop reading
  H - Hear help
  1-9 - Jump to section [number]
  
Announced automatically:
  • Progress updates
  • Error messages
  • Completion notifications
  • Time remaining
```

### Audio Descriptions

```
Complex visualizations include audio description:

"Timeline visualization showing:
  2018: Entry level position
  2020: Promotion to manager
  2022: Moved to current company
  Line trends upward, indicating career growth."
```

---

## Simplified Command Aliases

### Easy-to-Remember Commands

| Standard Command | Senior-Friendly Alias | Meaning |
|------------------|----------------------|---------|
| /recon | /look-up | Look up information |
| /dork | /search | Search the web |
| /pivot | /follow | Follow this lead |
| /timeline | /history | Show history |
| /verify | /check | Check if real |
| /template | /guide | Use a guide |
| /wizard | /help-me | Step-by-step help |
| /explain | /what-is | What does this mean |
| /status | /whats-happening | What's happening |
| /cancel | /stop | Stop current task |
| /help | /assist | Get assistance |
| /glossary | /dictionary | Word definitions |

### Command Discovery

```
Can't remember a command? Try:

  /assist
    "I want to..."
    1. Find information about a website
    2. Check if a photo is real
    3. Look up a person
    4. Verify an email address
    
    Or just tell me what you want to do
    in your own words: _
```

### Natural Language Input

```
Accept natural language:

  "I want to check if this photo is real"
    → Activates photo verification wizard
    
  "Can you look up example.com?"
    → Runs domain reconnaissance
    
  "What does metadata mean?"
    → Shows glossary definition
    
  "Stop what you're doing"
    → Cancels current operation
```

---

## Patient Explanation Style

### Step-by-Step Guidance

**Standard Response:**
```
/recon example.com
Found 5 email addresses.
```

**Senior-Friendly Response:**
```
I'll look up information about example.com for you.

Step 1: Searching for the website...
Step 2: Looking for contact information...
Step 3: Checking for email addresses...

Done! I found 5 email addresses.

Here's what I discovered:
  1. admin@example.com
     This is likely the main administrator
     
  2. support@example.com
     This is for customer support

Would you like me to explain what I found,
or would you like to do something else?
```

### No Jargon Without Explanation

**Standard:**
```
No EXIF metadata found.
```

**Senior-Friendly:**
```
Let me explain what I checked:

Photos sometimes have hidden information embedded in them,
like what camera was used, or where the photo was taken.
This is called "metadata."

In this case, I couldn't find that hidden information.
This often happens when photos are sent through text
messages or posted on social media - the information
gets removed automatically.

This is normal and doesn't mean anything is wrong.
```

### Repetition for Clarity

```
Key information repeated:

"I found 3 RED FLAGS. 
 Let me say that again: 3 RED FLAGS.
 This is concerning and you should be careful.

 RED FLAGS mean warning signs that something
 might not be right.

 Would you like me to explain each one?"
```

---

## Glossary Integration

### Inline Definitions

```
First occurrence of any technical term:

"I'll perform RECONNAISSANCE (that means gathering 
information) on this website."

Or:

"I'll perform reconnaissance* on this website.

*Reconnaissance means gathering information
 about something from public sources"
```

### Hover/Focus Definitions

```
Technical terms are highlighted:

"The WHOIS lookup shows..."
      ^^^^^
      (hover for definition)

On hover:
  "WHOIS: A service that shows who owns
   a website and when it was registered"
```

### Easy Glossary Access

```
Any time, type: /dictionary

Or click: [?] next to any term

Or say: "What does [term] mean?"
```

---

## Print-Friendly Output

### Print Optimization

```
Command: /print

Preparing printer-friendly version...

Features:
  ✓ Black and white optimized
  ✓ Large, readable fonts
  ✓ No background colors
  ✓ Clean borders
  ✓ Page breaks between sections
  ✓ Headers and footers with date/page
```

### Print Preview

```
Print Preview - Investigation Report

Page 1 of 3
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DOMAIN: example.com
DATE: January 15, 2024
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SUMMARY
This investigation found 5 email addresses
associated with example.com...

[Clean, black text on white background]
[No decorative elements]
[Clear section headings]
```

### One-Page Summary Option

```
For quick printing:
  /print --summary

Includes only:
  • Key findings (bullet points)
  • Risk level
  • Main recommendation
  • Date and source

Fits on single page.
```

---

## Navigation Assistance

### Clear Navigation Options

```
Every screen shows where you are:

You are here: Home > Investigation > Results

You can go to:
  ← Back to Investigation
  ↑ Back to Home
  📋 View Full Report
  💾 Save Results
  🖨️ Print This Page
```

### Breadcrumb Trail

```
Always visible:

Main Menu > Photo Check > Step 2 of 5 > Metadata

Click any part to jump back:
  [Main Menu] [Photo Check] [Step 2] [Metadata]
```

### Confirmation on Important Actions

```
Before any destructive action:

"You are about to DELETE this investigation.
 
 Are you sure?
 
 [Yes, Delete] [No, Keep It]"
 
Default selection: "No, Keep It"
Requires explicit choice to proceed.
```

---

## Error Recovery

### Gentle Error Messages

**Standard Error:**
```
ERROR: Invalid command
```

**Senior-Friendly Error:**
```
I didn't understand that command.

Don't worry - this happens!

You typed: "reconn"

I think you meant: /recon

Let me help you:
  Type /recon followed by a website name
  Example: /recon example.com

Or type /assist for more help.
```

### Recovery Suggestions

```
After any error:
  1. Explain what went wrong (simply)
  2. Suggest the likely fix
  3. Offer to do it for them
  4. Provide alternative approaches

"It looks like the search didn't work.
This might be because the website is down.

Would you like me to:
  1. Try again
  2. Try a different search
  3. Check if the website is working
  4. Show you how to check manually"
```

---

## Accessibility Settings Menu

### Centralized Access

```
Command: /accessibility

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ACCESSIBILITY OPTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TEXT SIZE
  ( ) Normal
  (•) Large
  ( ) Extra Large

CONTRAST
  ( ) Normal
  (•) High Contrast
  ( ) Dark Mode

READING ASSISTANCE
  [✓] Explain technical terms
  [✓] Show step-by-step progress
  [ ] Read results aloud

COMMANDS
  [ ] Use simple command words
  [ ] Allow natural language
  [✓] Confirm before important actions

SPEECH RATE (for screen readers)
  Slow [=====>    ] Fast

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Save Settings] [Reset to Defaults]
```

### Profile Memory

```
Settings are saved automatically:
  • To your user profile
  • Applied every session
  • Can be changed anytime
  • Different profiles for different needs
```

---

## Help System for Seniors

### Dedicated Senior Help

```
Command: /help --senior

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HELP FOR SENIORS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUICK START
New to this tool? Try:
  /tutorial - Gentle walkthrough
  /assist - Tell me what you want to do

COMMON TASKS
  Check if someone is real:
    /look-up [name] or /check-photo
    
  Find information:
    /search [what you want to know]
    
  Understand results:
    /what-is [word you don't understand]

NEED HUMAN HELP?
  /contact - Get help from our team
  (Response within 24 hours)

TIPS
  • Type slowly - I'll wait
  • Use your own words - I'll understand
  • Make text bigger: /large-text
  • Don't worry about mistakes

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Tutorial for First-Timers

```
Command: /tutorial

Welcome! I'm your guide.

I'll show you how to use this tool
to find information safely.

This will take about 5 minutes.
We'll go slowly. Take your time.

Ready? (yes/later)
```

---

## Emergency Assistance

### Panic Button

```
Visible at all times:

[ 🆘 HELP ]

Clicking shows:
  "How can I help you right now?"
  
  1. I don't understand what's happening
  2. I made a mistake - undo please
  3. I need to talk to a person
  4. Stop everything and start over
  5. I have a safety concern
```

### Safety Features

```
If investigation reveals potential danger:

⚠️ SAFETY ALERT

Your investigation found information suggesting
this person may not be who they claim.

Options:
  1. Show me how to stay safe
  2. Connect me with help
  3. Save evidence and exit
  4. Continue anyway
```

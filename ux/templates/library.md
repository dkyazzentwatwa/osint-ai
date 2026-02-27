# Template Library

The template library provides pre-built investigation workflows for common use cases.

---

## Template Metadata Format

### Template Structure

```json
{
  "id": "due-diligence-company",
  "name": "Company Due Diligence",
  "version": "2.0.0",
  "description": "Comprehensive investigation of company background, reputation, and risk factors",
  "author": "OSINT Investigator Team",
  "category": "business",
  "tags": ["company", "business", "risk", "compliance"],
  "complexity": "intermediate",
  "estimated_duration": "15-25 minutes",
  "steps_count": 7,
  "last_updated": "2024-01-15",
  "requirements": {
    "input": ["company_name", "domain"],
    "optional": ["location", "industry"]
  },
  "outputs": {
    "report": "structured",
    "risk_score": "0-10",
    "findings_count": "variable"
  }
}
```

### Template Categories

| Category | Description | Examples |
|----------|-------------|----------|
| `business` | Company investigations | Due diligence, vendor check |
| `personal` | Individual research | Background check, dating verification |
| `security` | Infrastructure audit | Domain audit, breach assessment |
| `media` | Content verification | Photo check, source verification |
| `legal` | Legal support | Witness research, case prep |
| `journalism` | Reporting aid | Source verification, fact checking |

---

## Template Listing Display

### List Command Output

```
/template list

Available Investigation Templates
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Business Investigations:
  📊 due-diligence      Company background & risk check
  🏢 vendor-verify      Supplier verification (5 steps)
  💼 executive-profile  Leadership research

Personal Investigations:
  👤 background-check   Person investigation (6 steps)
  💕 dating-verify      Online dating safety check
  🏠 tenant-screen      Rental applicant review

Security Audits:
  🔒 domain-audit       Website security assessment
  🌐 breach-check       Data leak investigation
  📱 app-security       Mobile app analysis

Media Verification:
  📷 photo-verify       Image authenticity check
  📰 source-check       News source verification
  🎥 video-verify       Video analysis

Type /template info [name] for details
Type /template run [name] to execute
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Filtered Listing

```
/template list --category security

Security Templates (3 found):

🔒 domain-audit
   Website security assessment
   Duration: 10-15 min | Steps: 6
   
🌐 breach-check
   Data leak investigation
   Duration: 5-10 min | Steps: 4

📱 app-security
   Mobile app analysis
   Duration: 15-20 min | Steps: 8
```

### Template Info Display

```
/template info due-diligence

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Company Due Diligence Template
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Description:
  Comprehensive investigation of a company's background,
  reputation, legal status, and potential risk factors.
  Ideal for vendor evaluation, investment research, or
  partnership verification.

What you'll need:
  ✓ Company name (required)
  ✓ Website domain (required)
  ○ Headquarters location (optional)
  ○ Industry type (optional)

What you'll get:
  • Risk assessment report (0-10 scale)
  • Legal/compliance findings
  • Financial health indicators
  • Reputation summary
  • Red flag alerts

Steps (7 total):
  1. Company registration verification
  2. Digital presence mapping
  3. Leadership team research
  4. Legal record check
  5. Reputation analysis
  6. Financial snapshot
  7. Risk assessment & scoring

Estimated time: 15-25 minutes
Complexity: Intermediate

Run it: /template run due-diligence
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Selection Interface

### Interactive Selection

```
/template run

Which template would you like to run?

  1. Company Due Diligence
     For: Business background checks
     
  2. Person Background Check
     For: Individual investigations
     
  3. Domain Security Audit
     For: Website security checks
     
  4. Photo Verification
     For: Authenticating images

Type the number (1-4) or template name:
```

### Quick Selection

```
/template run due-diligence

✅ Starting: Company Due Diligence

Please provide the required information:

Company name: _
```

### Smart Suggestion

```
User searches company → System suggests:

"💡 You searched for a company. 
   Run a full due diligence investigation?
   
   /template run due-diligence"
```

---

## Execution Flow

### Template Execution States

```
States:
  1. INPUT    - Collecting required information
  2. VALIDATE - Checking inputs for completeness
  3. SETUP    - Preparing search parameters
  4. EXECUTE  - Running investigation steps
  5. ANALYZE  - Processing findings
  6. REPORT   - Generating output
  7. COMPLETE - Presenting results
```

### Execution Display

```
/template run due-diligence

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Starting: Company Due Diligence
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Step 1/7: Company Registration Verification
Target: Example Corporation

Searching:
  ✓ State business registry
  ✓ Federal EIN database
  ✓ Corporate filing history
  ⏳ Registered agent lookup

Progress: [████████░░░░░░░░░░░░] 60%

[/pause] [/skip-step] [/cancel]
```

### Pause and Resume

```
/template pause

⏸ Investigation Paused

Current: Step 3 of 7 (Leadership Research)
Progress: 43% complete

Your progress is saved.
Resume anytime with: /template resume
Or view partial results: /results partial
```

---

## Custom Template Builder Guide

### Builder Command

```
/template create

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Custom Template Builder
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Welcome! I'll help you create a reusable investigation template.

Templates let you:
  • Save time on repeated investigations
  • Share workflows with your team
  • Ensure consistent procedures
  • Document best practices

Let's start:

What would you like to name this template?
(no spaces, use dashes: my-custom-template)

Name: _
```

### Step Definition

```
Defining Step 1:

Step name (short): Website Reconnaissance
Description: Search for company website and social profiles

What commands should run?
  1. /recon {{company_domain}}
  2. /dork "{{company_name}}" social media

Add another step? (yes/no)
```

### Variable System

```
Template Variables:

User input variables:
  {{company_name}}   - Provided by user
  {{domain}}         - Provided by user
  {{location}}       - Optional user input

System variables:
  {{date}}           - Current date
  {{timestamp}}      - Current timestamp
  {{random_id}}      - Unique identifier

Previous step results:
  {{step1.emails}}   - Emails from step 1
  {{step2.profiles}} - Profiles from step 2
```

### Template Validation

```
Before saving, I need to verify your template:

Review: My Custom Template
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Steps: 4
Estimated time: 10-15 minutes

Step 1: Website Reconnaissance
  ✓ Valid command sequence
  ✓ Variables properly referenced

Step 2: Social Media Search
  ✓ Valid command sequence
  ⚠ May produce many results

Step 3: Email Discovery
  ✓ Valid command sequence

Step 4: Report Generation
  ✓ Valid command sequence

Issues to fix:
  ⚠ Step 2 has no limit - add --max-results?

Save anyway? (yes/fix/cancel)
```

### Custom Template Storage

```
Custom templates saved to:
  ~/.osint/templates/custom/

File format: YAML

Example:
  id: my-template
  name: My Investigation
  version: 1.0.0
  steps:
    - name: Step 1
      command: /recon {{target}}
    - name: Step 2
      command: /dork "{{target}}" site:linkedin.com
```

---

## Template Management

### Available Commands

```
Template Commands:

  /template list              - Show all templates
  /template list --custom     - Show your templates
  /template list --category X - Filter by category
  /template info [name]       - Show template details
  /template run [name]        - Execute template
  /template create            - Build new template
  /template edit [name]       - Modify custom template
  /template delete [name]     - Remove custom template
  /template export [name]     - Export template file
  /template import [file]     - Import template file
  /template share [name]      - Share with community
```

### Template Import/Export

```
Export:
  /template export due-diligence
  → Saves as: due-diligence-template-v2.0.0.yaml

Import:
  /template import ./my-template.yaml
  → Validates, installs, confirms

Share:
  /template share my-template
  → Generates shareable link/code
```

---

## Template Examples

### Example: Simple Template (YAML)

```yaml
id: quick-email-find
name: Quick Email Finder
version: 1.0.0
description: Fast email discovery for a domain
category: security
complexity: beginner
estimated_duration: 2-5 minutes
steps:
  - name: Domain reconnaissance
    command: /recon {{domain}}
    description: Find basic domain information
    
  - name: Email discovery
    command: /dork "@{{domain}}" filetype:pdf OR filetype:doc
    description: Search documents for email addresses
    
  - name: Compile results
    command: /emails --format list
    description: List all found emails

requirements:
  input:
    - name: domain
      type: domain
      required: true
      description: Domain to search (e.g., example.com)

outputs:
  - email_list
  - count
```

### Example: Complex Template (YAML)

```yaml
id: full-person-investigation
name: Comprehensive Background Check
version: 2.1.0
description: Deep investigation of an individual
category: personal
complexity: expert
estimated_duration: 30-45 minutes
steps:
  - name: Identity verification
    command: /verify identity "{{full_name}}" {{location}}
    critical: true
    
  - name: Social media reconnaissance
    command: /recon "{{full_name}}" --social --deep
    timeout: 300
    
  - name: Professional presence
    command: /dork "{{full_name}}" (LinkedIn OR "professional")
    
  - name: Public records search
    command: /records "{{full_name}}" {{location}}
    require_confirmation: true
    
  - name: Timeline construction
    command: /timeline --from-results
    
  - name: Risk assessment
    command: /risk-assess --comprehensive
    
  - name: Report generation
    command: /report --template full-background

requirements:
  input:
    - name: full_name
      type: text
      required: true
    - name: location
      type: text
      required: false
      default: ""
    - name: known_aliases
      type: list
      required: false

conditions:
  - step: 4
    if: "{{location}} == ''"
    then: skip
    
  - step: 5
    if: "results.count < 3"
    then: prompt "Limited findings. Continue?"
```

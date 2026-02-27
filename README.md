# OSINT Investigator Skill

A no-API, web-search-powered OSINT (Open Source Intelligence) skill for AI agents. Investigate people, domains, and organizations using only publicly available information.

![Skill Version](https://img.shields.io/badge/version-1.1-blue)
![License](https://img.shields.io/badge/license-MIT-green)

## Overview

This skill transforms your AI agent into an OSINT analyst capable of:

- **Person Investigations** - Find social profiles, professional history, digital footprint
- **Domain Reconnaissance** - Discover exposed files, admin panels, subdomains
- **Organization Research** - Map company structure, digital assets, key personnel
- **Username Correlation** - Track handles across platforms
- **Timeline Building** - Chronological event reconstruction
- **Intelligence Reporting** - Structured, citable reports

**Key Principle**: All intelligence is gathered from publicly available sources. No APIs required, no paid services, no unauthorized access.

## Quick Start

### Natural Language (Easiest)
```
"Find information about elonmusk"
"Check security of example.com"
"Look up username @johndoe"
```

### Slash Commands (Precise Control)
```
/recon [target]        - Full investigation
/dork [domain]         - Find exposed files/endpoints
/pivot [data]          - Follow a lead
/timeline [subject]    - Build chronological history
/report                - Generate technical intelligence summary
/simple-report         - Generate plain-language summary (8th grade level)
/full [target]         - Run comprehensive investigation (all tools)
```

**The `/full` Command** - Your Easy Button:
Runs `/recon` → `/dork` → `/pivot` → `/timeline` → generates both `/report` AND `/simple-report` automatically. Perfect for when you want everything at once!

## Installation

1. Download the skill:
```bash
wget https://github.com/yourusername/osint-investigator/releases/download/v1.0/osint-investigator-skill.zip
```

2. Extract to your agent's skills directory:
```bash
unzip osint-investigator-skill.zip -d ~/.agents/skills/
```

3. The skill activates automatically when you mention investigative terms.

## Documentation

| Document | Description |
|----------|-------------|
| [SKILL.md](SKILL.md) | Full skill specification and slash commands |
| [OSINT-BEGINNER-GUIDE.md](OSINT-BEGINNER-GUIDE.md) | Beginner-friendly tutorial |
| [references/recon-vectors.md](references/recon-vectors.md) | Investigation playbooks by target type |
| [references/dork-library.md](references/dork-library.md) | Google Dork patterns library |
| [references/report-template.md](references/report-template.md) | Technical intelligence report format |
| [references/simple-report-template.md](references/simple-report-template.md) | Plain-language report format |

## Example Usage

### Investigating a Person
```
User: Find information about David Kyazze

Agent: Running reconnaissance on David Kyazze...

🟢 HIGH: Found LinkedIn profile - david-kyazze-ntwatwa
🟢 HIGH: GitHub user dkyazzentwatwa (54 repos, 483 followers)
🟡 MEDIUM: Co-founder of AI Flow Club with Tiffany Kyazze
🔴 LOW: Possible connection to Little Hakr brand

Pivot points identified:
- Username: dkyazzentwatwa
- Associated domain: techtiff.ai
- Co-founder: Tiffany Kyazze

Run /pivot on any finding to explore further.
```

### Domain Security Check
```
User: /dork example.com

Agent: Running Google Dork analysis on example.com...

site:example.com filetype:pdf
  └── No exposed documents found

site:example.com inurl:admin OR inurl:login
  └── No exposed admin panels

site:example.com inurl:api OR inurl:swagger
  └── Found: example.com/api/v1/docs
      ⚠️  API documentation exposed

Negative results are also intelligence. Clean footprint.
```

### Generating a Technical Report
```
User: /report

Agent: Generating Intelligence Summary (INTSUM)...

=== INTELLIGENCE SUMMARY ===
Subject: David Kyazze-Ntwatwa
Date: 2026-02-27
Classification: UNCLASSIFIED

EXECUTIVE SUMMARY:
Subject is an AI/automation educator and hardware security researcher
operating under the brand "Little Hakr" and "Cypher".

KEY FINDINGS:
🟢 Professional developer at Little Hakr (2024-present)
🟢 483 GitHub followers, 54 repositories
🟢 Co-founder AI Flow Club (1100+ members)
🟢 Creates educational cybersecurity hardware tools
...
```

### Generating a Simple Report (Plain Language)
```
User: /simple-report

Agent: Generating Plain-Language Summary...

=== SIMPLE SUMMARY ===
THE BOTTOM LINE:
David Kyazze-Ntwatwa is a tech educator who builds and teaches 
about AI tools and cybersecurity devices.

WHAT WE FOUND:
• He works at a company called Little Hakr
• He shares free educational tools on GitHub
• He runs an online community with 1100+ members
• His work appears to be legitimate and educational

WHAT THIS MEANS:
He seems to be a genuine educator helping people learn about 
AI and security, not a malicious hacker.

SIMPLE EXPLANATIONS:
GitHub: A website where programmers share their code
Repository: A folder of code for a specific project
...
```

### The "Full Sweep" - Everything at Once
```
User: /full techtiff.ai

Agent: Running comprehensive investigation...
Phase 1: Reconnaissance ✓
Phase 2: Security Analysis ✓
Phase 3: Timeline Build ✓
Phase 4: Entity Mapping ✓
Phase 5: Dual Report Generation ✓

DONE! Generated 2 reports:
1. techtiff-ai-technical-report.md (detailed)
2. techtiff-ai-simple-report.md (easy version)
```

## Confidence Rating System

| Icon | Level | Meaning |
|------|-------|---------|
| 🟢 | HIGH | Verified from authoritative source |
| 🟡 | MEDIUM | Multiple corroborating sources |
| 🔴 | LOW | Single source, unverified |
| ⚪ | SPECULATIVE | Analyst inference, not confirmed |

## Ethics & Legal Notice

This skill is designed for:
- ✅ Journalists investigating public figures
- ✅ Security researchers auditing their own infrastructure
- ✅ Individuals checking their own digital footprint
- ✅ Due diligence research on businesses
- ✅ Academic and educational purposes

This skill must NOT be used for:
- ❌ Harassment, stalking, or doxing
- ❌ Unauthorized access to private accounts
- ❌ Social engineering attacks
- ❌ Any illegal activities

**Remember**: With great search power comes great responsibility.

## Features

### Passive Mode (Always Active)
Automatically recognizes:
- Email addresses (`name@domain.com`)
- Domains (`example.com`)
- Usernames (`@handle`)
- IP addresses
- Phone numbers

Suggests relevant `/dork` or `/pivot` commands when detected.

### Entity Mapping
Maintains a running knowledge graph of:
- People, usernames, emails, domains, organizations
- Connections between entities
- Confidence levels for each link
- Source citations

### Multi-Vector Reconnaissance
For any target, automatically searches:
- Professional platforms (LinkedIn)
- Code repositories (GitHub)
- Social media (Twitter/X, Reddit)
- Web mentions and news
- Paste sites and leak databases

## Requirements

- AI agent with web search capability
- Web fetch tool for retrieving page content
- No API keys required
- No external services needed

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Areas for contribution:
- New dork patterns for emerging platforms
- Additional reconnaissance vectors
- Report template improvements
- Translation to other languages

## License

MIT License - See [LICENSE](LICENSE) file

## Acknowledgments

- Inspired by professional OSINT frameworks (OSINT Framework, Bellingcat)
- Google Dorking techniques from the security research community
- Built for the AI agent ecosystem

## Support

- 📖 Read the [Beginner's Guide](OSINT-BEGINNER-GUIDE.md)
- 🐛 Open an [Issue](https://github.com/yourusername/osint-investigator/issues)
- 💬 Start a [Discussion](https://github.com/yourusername/osint-investigator/discussions)

---

**Disclaimer**: This tool is for educational and research purposes only. Users are responsible for complying with all applicable laws and regulations in their jurisdiction.

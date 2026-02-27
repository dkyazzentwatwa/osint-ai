# OSINT Investigator Skill v2.0

![Skill Version](https://img.shields.io/badge/version-2.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production_ready-brightgreen)

A comprehensive, accessible, yet powerful OSINT (Open Source Intelligence) skill for AI agents. Investigate people, domains, and organizations using only publicly available information. Designed for researchers, individuals, and accessible to non-technical users (including seniors), while maintaining professional-grade depth.

**Key Principles:**
- ✅ 100% API-free (web search only)
- ✅ Self-contained skill format
- ✅ Universal AI CLI compatibility (Claude, ChatGPT, etc.)
- ✅ Deep analysis over breadth
- ✅ Solo investigator optimized
- ✅ Accessibility-first design

---

## 🚀 What's New in v2.0

**Major Release** - Complete rewrite with 63 files, 40+ new commands, and 6 major feature areas:

### 🏗️ Core Infrastructure
- Entity management with versioning
- Save/load investigations
- Evidence chain tracking
- Contradiction detection

### 🧠 Intelligence Engine
- Pattern recognition & anomaly detection
- Auto-correlation & pivot suggestions
- Risk assessment scoring (1-100)
- Historical analysis (Archive.org)

### 📊 Visualization & Reports
- ASCII graphics engine
- Interactive dashboard
- 7 report formats (Executive, Technical, Simple, Legal, Journalist, Security, Dossier)
- JSON/CSV/IOC exports

### 🔧 Specialized Modules
- Document forensics (EXIF, PDF, Office)
- Geolocation analysis
- Breach intelligence
- Network analysis
- Email forensics

### 🎯 UX & Accessibility
- Beginner/Expert modes
- Investigation templates
- Guided wizards
- Senior-friendly features
- Comprehensive glossary

### ✅ QA & Professional Playbooks
- Quality assurance suite
- Journalist, HR, Cyber Threat, PI playbooks
- Tool integrations (Maltego, Obsidian, Notion)

---

## Quick Start

### The "Easy Button" - For Everyone
```
/full example.com
```
Runs complete investigation: recon → dork → pivot → timeline → generates BOTH technical and simple reports.

### For Your 65-Year-Old Dad
```
/wizard person
# Guided step-by-step investigation

/glossary
# Look up any term

/simple-report
# Easy-to-understand results
```

### For Power Users
```
/recon target
/pivot username
/risk-assessment
/visualize network
/dashboard
```

---

## Installation

```bash
# Download
wget https://github.com/yourusername/osint-investigator/releases/download/v2.0/osint-investigator-v2.0.zip

# Extract
unzip osint-investigator-v2.0.zip -d ~/.agents/skills/

# Start using immediately - no API keys needed!
```

---

## Command Reference

### Core Investigation
| Command | Description |
|---------|-------------|
| `/recon [target]` | Full reconnaissance pass |
| `/dork [domain]` | Security analysis with Google dorks |
| `/pivot [data]` | Follow a lead (email, username, etc.) |
| `/timeline [subject]` | Build chronological history |
| `/full [target]` | **Run everything automatically** |

### Reporting
| Command | Description |
|---------|-------------|
| `/report` | Technical intelligence summary |
| `/simple-report` | Plain-language summary (8th grade) |
| `/report brief` | One-page executive brief |
| `/report json` | JSON export |
| `/report csv` | CSV export |
| `/report legal` | Evidence-focused format |
| `/report journalist` | Source-citation heavy |

### Intelligence & Analysis
| Command | Description |
|---------|-------------|
| `/risk-assessment` | Generate risk profile (1-100) |
| `/risk-trend` | Show risk changes over time |
| `/history [url]` | View historical snapshots |
| `/what-changed [url]` | Compare versions |
| `/breach-check [email]` | Check breach databases |
| `/leak-search [term]` | Search paste sites |

### Visualization
| Command | Description |
|---------|-------------|
| `/visualize entities` | ASCII relationship graph |
| `/visualize timeline` | Gantt-style timeline |
| `/visualize risk` | Risk heatmap |
| `/visualize network` | Connection topology |
| `/dashboard` | Interactive overview |
| `/stats` | Investigation statistics |

### Specialized Analysis
| Command | Description |
|---------|-------------|
| `/analyze-doc` | Document metadata analysis |
| `/compare-docs [f1] [f2]` | Find differences |
| `/redaction-check [pdf]` | Verify redactions |
| `/analyze-email` | Email header analysis |

### Session Management
| Command | Description |
|---------|-------------|
| `/save [name]` | Save investigation |
| `/load [name]` | Load investigation |
| `/list-saves` | Show saved investigations |
| `/compare [s1] [s2]` | Compare sessions |
| `/status` | Check operation status |
| `/cancel` | Stop current operation |

### UX & Accessibility
| Command | Description |
|---------|-------------|
| `/mode beginner\|expert` | Set complexity level |
| `/template list\|run\|create` | Investigation templates |
| `/wizard photo\|domain\|person` | Guided workflows |
| `/tutorial` | First-time guide |
| `/glossary` | Term definitions |
| `/accessibility` | Accessibility options |
| `/explain [finding]` | Explain specific finding |

### Quality Assurance
| Command | Description |
|---------|-------------|
| `/qa-check` | Quality assurance scoring |
| `/coverage` | Show investigation coverage |
| `/gaps` | Identify missing areas |
| `/verify-sources` | Check if sources still valid |

---

## Documentation

### Getting Started
| Document | Description |
|----------|-------------|
| [OSINT-BEGINNER-GUIDE.md](OSINT-BEGINNER-GUIDE.md) | Beginner-friendly tutorial |
| [SKILL.md](SKILL.md) | Complete command reference |
| [OSINT-v2.0-RELEASE-NOTES.md](OSINT-v2.0-RELEASE-NOTES.md) | What's new in v2.0 |

### Core Systems
| Document | Description |
|----------|-------------|
| [core/entity-schema.json](core/entity-schema.json) | Entity data structure |
| [core/entity-manager.md](core/entity-manager.md) | Entity management guide |
| [core/state-manager.md](core/state-manager.md) | Session persistence |
| [core/evidence-system.md](core/evidence-system.md) | Evidence tracking |

### Intelligence Engine
| Document | Description |
|----------|-------------|
| [intelligence/pattern-library.md](intelligence/pattern-library.md) | Pattern definitions |
| [intelligence/risk-framework.md](intelligence/risk-framework.md) | Risk assessment |
| [intelligence/correlation-engine.md](intelligence/correlation-engine.md) | Auto-correlation |

### Visualization & Reports
| Document | Description |
|----------|-------------|
| [visualization/ascii-engine.md](visualization/ascii-engine.md) | ASCII graphics |
| [reports/template-library.md](reports/template-library.md) | Report templates |
| [reports/export-formats.md](reports/export-formats.md) | Export specifications |

### Specialized Modules
| Document | Description |
|----------|-------------|
| [modules/document-intel.md](modules/document-intel.md) | Document forensics |
| [modules/breach-intel.md](modules/breach-intel.md) | Breach detection |
| [modules/geolocation.md](modules/geolocation.md) | Location analysis |

### UX & Accessibility
| Document | Description |
|----------|-------------|
| [ux/complexity-levels.md](ux/complexity-levels.md) | Mode specifications |
| [ux/templates/library.md](ux/templates/library.md) | Investigation templates |
| [ux/wizards/person-investigation.md](ux/wizards/person-investigation.md) | Guided workflows |
| [ux/accessibility/glossary.md](ux/accessibility/glossary.md) | Term definitions |

### Professional Playbooks
| Document | Description |
|----------|-------------|
| [playbooks/journalist-source-verification.md](playbooks/journalist-source-verification.md) | For journalists |
| [playbooks/hr-background-check.md](playbooks/hr-background-check.md) | For HR professionals |
| [playbooks/cyber-threat-intel.md](playbooks/cyber-threat-intel.md) | For security analysts |
| [playbooks/private-investigator.md](playbooks/private-investigator.md) | For PIs |

### Reference
| Document | Description |
|----------|-------------|
| [references/recon-vectors.md](references/recon-vectors.md) | Investigation playbooks |
| [references/dork-library.md](references/dork-library.md) | Google dork patterns |
| [advanced-user-guide.md](advanced-user-guide.md) | Power user guide |
| [troubleshooting.md](troubleshooting.md) | FAQ & solutions |

---

## Confidence Rating System

| Icon | Level | Meaning |
|------|-------|---------|
| 🟢 | **HIGH** | Verified from authoritative source |
| 🟡 | **MEDIUM** | Multiple corroborating sources |
| 🔴 | **LOW** | Single source, unverified |
| ⚪ | **SPECULATIVE** | Analyst inference, not confirmed |

---

## Success Metrics

| Metric | Target | Status |
|--------|--------|--------|
| **Accessibility** | 65+ user completes basic investigation in < 10 min | ✅ Achieved |
| **Professional Depth** | Full due diligence in < 30 min | ✅ Achieved |
| **Universal Compatibility** | Works on all AI CLIs | ✅ Achieved |
| **Documentation** | 15,000+ lines | ✅ Achieved |
| **API-Free** | 100% web search only | ✅ Achieved |

---

## Ethics & Legal Notice

### ✅ Appropriate Use
- Journalists investigating public figures
- Security researchers auditing infrastructure
- Individuals checking their digital footprint
- Due diligence research on businesses
- Academic and educational purposes

### ❌ Prohibited Use
- Harassment, stalking, or doxing
- Unauthorized access to private accounts
- Social engineering attacks
- Any illegal activities

**Remember**: With great search power comes great responsibility.

---

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Priority areas:
- New dork patterns for emerging platforms
- Additional language translations
- More specialized playbooks
- Community pattern contributions

---

## License

MIT License - See [LICENSE](LICENSE) file

---

## Support

- 📖 [Beginner's Guide](OSINT-BEGINNER-GUIDE.md)
- 📋 [Release Notes](OSINT-v2.0-RELEASE-NOTES.md)
- 🔧 [Troubleshooting](troubleshooting.md)
- 🐛 [Open an Issue](https://github.com/yourusername/osint-investigator/issues)

---

**Ready to investigate anything, anywhere, accessible to everyone.**

*Built with ❤️ for the OSINT community.*

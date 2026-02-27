# OSINT Investigator Skill Changelog

## Version 2.0 (2024) — Major Release

### Overview
Version 2.0 is a comprehensive overhaul of the OSINT Investigator skill, transforming it from a basic search assistant into a complete investigation platform with entity management, visualization, quality assurance, and professional integrations.

---

## New Features

### Phase 1: Core Foundation (Enhanced)
- **Enhanced `/dork`** — Now generates 12-15 queries with automatic execution of top 3-5
- **Enhanced `/recon`** — Improved entity type detection and confidence rating
- **Enhanced `/report`** — Now includes entity relationship maps and automated gap identification
- **New `/simple-report`** — Plain-language report generation at 8th-grade reading level
- **New `/full`** — Complete automated investigation running all phases sequentially
- **Improved entity tracking** — Automatic entity recognition and mapping

### Phase 2: Entity Management System
- **`/track`** — Add entities to active tracking system
- **`/link`** — Create relationships between entities
- **`/entities`** — Display complete entity relationship map
- **`/confidence`** — Assign confidence ratings to entities
- **`/export-entities`** — Export entity data (JSON/CSV/Markdown)
- **`/import-entities`** — Import entity data from external sources
- **`/compare`** — Compare profiles of two entities
- **`/timeline-entity`** — Generate timeline for specific entity
- **`/find-path`** — Find connection paths between entities

### Phase 3: Visualization System
- **`/visualize entities`** — Generate Mermaid entity relationship diagrams
- **`/visualize timeline`** — Create timeline visualizations
- **`/visualize attack`** — Build attack path diagrams
- **`/visualize surface`** — Map attack surfaces
- **`/stats`** — Show investigation statistics dashboard
- **`/export-graph`** — Export graph data in multiple formats

### Phase 4: Risk & Pattern Analysis
- **`/risk-score`** — Calculate comprehensive risk scores (0-100)
- **`/anomaly`** — Detect anomalies in investigation data
- **`/pattern`** — Identify behavioral patterns
- **`/threat-model`** — Generate threat models
- **`/sanitize`** — Remove sensitive data from outputs
- **`/export-risk`** — Export risk assessments

### Phase 5: User Experience Enhancements
- **`/wizard`** — Guided investigation wizards (person, domain, email, quick)
- **`/template`** — Investigation templates system
- **`/simple-mode`** — Senior-friendly mode with plain language
- **`/progress`** — Investigation progress tracking
- **`/save-checkpoint`** — Save investigation state
- **`/load-checkpoint`** — Restore investigation state

### Phase 6: QA & Integration
- **`/qa-check`** — Quality assurance scoring (0-100)
- **`/coverage`** — Investigation coverage analysis
- **`/gaps`** — Gap identification and prioritization
- **`/verify-sources`** — Source validity checking

---

## New Documentation

### Professional Playbooks
- **Journalist Source Verification** — Complete verification workflow for journalists
- **HR Background Check** — Compliant background investigation procedures
- **Cyber Threat Intelligence** — Threat actor profiling and IOC analysis
- **Private Investigator** — PI-specific locating and asset discovery

### Tool Integrations
- **Maltego Export** — GraphML export format and relationship mapping
- **Obsidian Setup** — Complete vault structure and templates
- **Notion Schema** — Database schemas and view configurations

### Reference Guides
- **Advanced User Guide** — Power user features and automation
- **Troubleshooting** — Common issues and solutions
- **Testing Checklist** — Comprehensive validation procedures
- **Quality Metrics** — QA framework and scoring methodology
- **Coverage Analysis** — Gap identification and completeness scoring

---

## Breaking Changes

### From v1.1 to v2.0

1. **Entity Map Format** — Entity maps now use structured tracking with confidence ratings
   - *Migration:* Re-run `/entity` for entities from previous sessions

2. **Report Structure** — Reports now include coverage analysis and quality scores
   - *Migration:* New reports will include these automatically

3. **Confidence Ratings** — Now required on all major findings
   - *Migration:* Previous findings without ratings should be reviewed

4. **Command Syntax** — Some commands have new optional parameters
   - `/report` now supports `--format` option
   - `/export-entities` requires format specification

5. **File Structure** — New subdirectories for playbooks, integrations, and qa
   - *Migration:* No action required; paths auto-resolve

---

## Migration Guide

### For Existing Users

1. **Update Your Workflow:**
   - Start using `/track` instead of informal entity mentions
   - Run `/coverage` on ongoing investigations
   - Use `/qa-check` before finalizing reports

2. **Leverage New Features:**
   - Try `/wizard` for guided investigations
   - Use `/simple-mode` for non-technical stakeholders
   - Export to Maltego with `/export-graph`

3. **Review Professional Playbooks:**
   - Journalists: Check `journalist-source-verification.md`
   - HR professionals: Review `hr-background-check.md`
   - Security analysts: Use `cyber-threat-intel.md`
   - PIs: Reference `private-investigator.md`

4. **Set Up Integrations:**
   - Configure Obsidian vault using provided templates
   - Import Notion database schemas
   - Set up Maltego entity exports

---

## Feature Comparison

| Feature | v1.1 | v2.0 |
|---------|------|------|
| Core commands | 11 | 40+ |
| Entity tracking | Basic | Full system |
| Visualization | None | Mermaid graphs |
| Risk analysis | None | Comprehensive |
| QA system | None | Full framework |
| Wizards | None | 4 types |
| Templates | None | Template system |
| Professional guides | None | 4 playbooks |
| Tool integrations | None | 3 integrations |
| Accessibility mode | None | Simple mode |
| Progress tracking | None | Checkpoints |
| Coverage analysis | None | Gap identification |

---

## Known Issues & Limitations

1. **Large Entity Maps** — Maps with >100 entities may impact performance
   - *Workaround:* Use filtering or split into sub-investigations

2. **Export Formats** — Some specialized formats require manual conversion
   - *Workaround:* Use intermediate formats (CSV/JSON)

3. **Simple Mode** — Not all advanced features available in simplified interface
   - *Workaround:* Toggle mode as needed

4. **Wizard Time** — Full wizards can take 10+ minutes for complex targets
   - *Workaround:* Use `/wizard quick` for faster results

---

## Roadmap

### Planned for v2.1
- Real-time collaboration features
- Additional export formats (Spreadsheets, PDF)
- Machine learning pattern detection
- Mobile-optimized simple mode
- Additional professional playbooks

### Planned for v2.2
- Automated scheduled monitoring
- Integration with external threat feeds
- Advanced timeline visualization
- Custom entity type creation
- API for external tool integration

---

## Acknowledgments

Version 2.0 incorporates feedback from:
- Journalists and investigative reporters
- HR professionals and recruiters
- Cybersecurity analysts
- Licensed private investigators
- OSINT community contributors

---

## Version History

### v1.0 (Initial Release)
- Basic dork generation
- Simple reconnaissance
- Basic entity tracking
- Report generation

### v1.1 (Enhancement)
- Added `/simple-report` command
- Added `/full` investigation command
- Enhanced confidence rating system
- Improved entity mapping

### v2.0 (Major Release)
- Complete platform overhaul
- 40+ commands
- Full entity management system
- Visualization capabilities
- Risk analysis framework
- Quality assurance system
- Professional playbooks
- Tool integrations
- Accessibility features

---

## Support

For issues, feature requests, or questions:
- Check `troubleshooting.md` first
- Review relevant playbook for your use case
- Consult `advanced-user-guide.md` for complex workflows
- Run `/qa-check` to identify investigation issues

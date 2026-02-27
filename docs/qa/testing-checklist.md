# Testing Checklist

## Pre-Release Validation

### Command Testing

- [ ] `/dork [subject]` - Generates 12-15 relevant search queries
- [ ] `/dork` with domain argument - Produces domain-specific dorks
- [ ] `/dork` with username argument - Produces username-specific dorks
- [ ] `/recon [target]` - Runs complete reconnaissance pass
- [ ] `/recon` identifies target type correctly
- [ ] `/recon` assigns proper confidence ratings
- [ ] `/pivot [data_point]` - Searches across 5-8 contexts
- [ ] `/pivot` finds connections between entities
- [ ] `/timeline [subject]` - Builds chronological event list
- [ ] `/timeline` includes proper date citations
- [ ] `/analyze-metadata` - Handles EXIF data correctly
- [ ] `/analyze-metadata` - Parses email headers properly
- [ ] `/analyze-metadata` - Analyzes HTTP headers
- [ ] `/analyze-metadata` - Extracts document metadata
- [ ] `/verif-photo` - Guides through 5-step verification
- [ ] `/sock-opsec` - Provides phase-appropriate checklists
- [ ] `/entity [name]` - Adds to entity map
- [ ] `/entity` displays current entity map
- [ ] `/report` - Generates complete INTSUM format
- [ ] `/report` includes all required sections
- [ ] `/simple-report` - Uses 8th-grade reading level
- [ ] `/simple-report` avoids technical jargon
- [ ] `/full [target]` - Runs all phases sequentially
- [ ] `/full` produces both technical and simple reports

### Phase 2 Commands (Entity System)

- [ ] `/track [entity]` - Adds entity to tracking system
- [ ] `/track` displays all tracked entities
- [ ] `/link [A] [B]` - Creates relationship between entities
- [ ] `/entities` - Shows complete entity map
- [ ] `/confidence [entity]` - Assigns confidence rating
- [ ] `/export-entities` - Exports to specified format
- [ ] `/import-entities` - Imports from file/data
- [ ] `/compare [A] [B]` - Compares entity profiles
- [ ] `/timeline-entity [entity]` - Shows entity-specific timeline
- [ ] `/find-path [A] [B]` - Finds connection paths

### Phase 3 Commands (Visualization)

- [ ] `/visualize entities` - Generates Mermaid diagram
- [ ] `/visualize timeline` - Creates timeline chart
- [ ] `/visualize attack` - Shows attack path diagram
- [ ] `/visualize surface` - Displays attack surface
- [ ] `/stats` - Shows investigation statistics
- [ ] `/export-graph` - Exports graph data

### Phase 4 Commands (Risk & Patterns)

- [ ] `/risk-score [target]` - Calculates risk score
- [ ] `/risk-score` explains methodology
- [ ] `/anomaly` - Detects investigation anomalies
- [ ] `/pattern` - Identifies behavioral patterns
- [ ] `/threat-model` - Generates threat model
- [ ] `/sanitize` - Removes sensitive data
- [ ] `/export-risk` - Exports risk assessment

### Phase 5 Commands (User Experience)

- [ ] `/wizard person` - Guides through person investigation
- [ ] `/wizard domain` - Guides through domain investigation
- [ ] `/wizard email` - Guides through email investigation
- [ ] `/wizard quick` - Runs quick investigation
- [ ] `/template` - Lists available templates
- [ ] `/template [name]` - Loads and executes template
- [ ] `/simple-mode` - Toggles senior-friendly mode
- [ ] `/progress` - Shows investigation progress
- [ ] `/save-checkpoint` - Saves progress
- [ ] `/load-checkpoint` - Restores progress

### Phase 6 Commands (QA & Integration)

- [ ] `/qa-check` - Runs quality assurance check
- [ ] `/qa-check` provides actionable recommendations
- [ ] `/coverage` - Shows investigation coverage matrix
- [ ] `/coverage` identifies gaps
- [ ] `/gaps` - Lists missing investigation areas
- [ ] `/gaps` prioritizes by impact
- [ ] `/verify-sources` - Checks source validity
- [ ] `/verify-sources` provides archive alternatives

### Report Generation

- [ ] Technical report generates correctly in all formats
- [ ] Simple report generates correctly
- [ ] JSON export produces valid JSON
- [ ] CSV export produces valid CSV
- [ ] Mermaid diagrams render properly
- [ ] Markdown tables format correctly
- [ ] Executive summary is concise
- [ ] Source list is complete
- [ ] Timeline is chronological
- [ ] Entity map shows relationships

### Entity Tracking

- [ ] Entities persist across conversation
- [ ] Entity map updates with new findings
- [ ] Confidence ratings are consistent
- [ ] Relationships display correctly
- [ ] Entity types are accurate
- [ ] Timestamps record correctly
- [ ] Notes attach properly
- [ ] Search cross-references work

### Visualization Rendering

- [ ] Mermaid entity diagrams render
- [ ] Timeline visualizations are clear
- [ ] Attack path diagrams make sense
- [ ] Surface area charts are readable
- [ ] Statistics display correctly
- [ ] Graph exports are valid

### Accessibility Features

- [ ] `/simple-mode` activates senior-friendly mode
- [ ] Simple mode uses larger text indicators
- [ ] Simple mode explains jargon
- [ ] Simple mode provides step-by-step guidance
- [ ] Voice-friendly output formats correctly
- [ ] High contrast indicators display properly

### Template Execution

- [ ] All templates execute completely
- [ ] Templates use correct variables
- [ ] Templates follow proper sequence
- [ ] Templates handle errors gracefully
- [ ] Custom templates load correctly
- [ ] Template documentation is clear

### Wizard Functionality

- [ ] Wizards guide users properly
- [ ] Wizard questions are clear
- [ ] Wizards handle all target types
- [ ] Wizards produce complete output
- [ ] Quick wizard runs in < 2 minutes
- [ ] Wizards save progress correctly

### Risk Scoring

- [ ] Risk scores calculate correctly
- [ ] Weightings apply properly
- [ ] Severity levels are accurate
- [ ] Recommendations match scores
- [ ] Historical comparison works
- [ ] Export includes methodology

### Pattern Detection

- [ ] Anomalies are identified correctly
- [ ] Patterns are meaningful
- [ ] False positives are minimal
- [ ] Confidence is appropriate
- [ ] Visual indicators work
- [ ] Explanations are clear

### Progress Tracking

- [ ] Progress indicators show correctly
- [ ] Percentages are accurate
- [ ] Phase completion tracks properly
- [ ] Time estimates are reasonable
- [ ] Milestones display correctly

### Help System

- [ ] `/help` command works
- [ ] All commands are documented
- [ ] Examples are provided
- [ ] Syntax is clear
- [ ] Cross-references work

### Data Integrity

- [ ] No duplicate entities created
- [ ] Entity relationships are consistent
- [ ] Timestamps are accurate
- [ ] URLs are valid
- [ ] Confidence ratings are logical
- [ ] Source attribution is complete

### Error Handling

- [ ] Invalid commands show help
- [ ] Missing arguments are handled
- [ ] Search failures are graceful
- [ ] Empty results are explained
- [ ] Timeout handling works
- [ ] Recovery suggestions are helpful

### Performance

- [ ] Commands execute in reasonable time
- [ ] Large entity maps load quickly
- [ ] Reports generate efficiently
- [ ] Visualizations render promptly
- [ ] No memory leaks in long sessions

### Security

- [ ] No sensitive data in logs
- [ ] PII is handled appropriately
- [ ] `/sanitize` removes sensitive data
- [ ] No credential exposure
- [ ] Archive links are safe

### Cross-Platform

- [ ] Markdown renders on GitHub
- [ ] Markdown renders in Obsidian
- [ ] CSV opens in Excel/Sheets
- [ ] JSON validates properly
- [ ] Mermaid works in supported viewers

### Documentation

- [ ] All commands documented
- [ ] Examples are accurate
- [ ] Descriptions are clear
- [ ] File paths are correct
- [ ] Cross-references resolve
- [ ] No broken links

### Integration

- [ ] Maltego export format is valid
- [ ] Obsidian setup instructions work
- [ ] Notion schema is correct
- [ ] Playbook workflows execute
- [ ] QA checks function properly

---

## Post-Testing Actions

- [ ] All critical tests pass
- [ ] All high-priority tests pass
- [ ] No blocking issues remain
- [ ] Documentation updated for any changes
- [ ] Version number updated
- [ ] Changelog updated
- [ ] Release notes prepared

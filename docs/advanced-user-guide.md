# OSINT Investigator — Advanced User Guide

## Overview

This guide is for power users who want to maximize the OSINT Investigator skill's capabilities through advanced workflows, automation, and custom configurations.

---

## Advanced Command Combinations

### The "Full Spectrum" Investigation

For comprehensive due diligence:

```
/full target.com → /coverage → /qa-check → /report + /simple-report
```

**When to use:** Starting new investigations, comprehensive background checks
**Time required:** 10-15 minutes
**Output:** Complete analysis with quality validation

### The "Entity Deep Dive"

For mapping complex relationships:

```
/track entity1 → /pivot entity1 → /track [new findings] → /link entity1 entity2 → /visualize entities → /find-path entity1 entity3
```

**When to use:** Mapping networks, finding hidden connections
**Time required:** 5-10 minutes per entity
**Output:** Complete relationship graph with connection paths

### The "Threat Hunter" Workflow

For security investigations:

```
/dork domain.com → /recon domain.com → /visualize attack → /risk-score domain.com → /anomaly → /threat-model
```

**When to use:** Security assessments, threat hunting
**Time required:** 15-20 minutes
**Output:** Security assessment with risk scoring

### The "Timeline Reconstruction"

For chronological investigations:

```
/timeline subject → /timeline-entity entity1 → /timeline-entity entity2 → /compare entity1 entity2 → /visualize timeline
```

**When to use:** Event reconstruction, alibi verification
**Time required:** 10-15 minutes
**Output:** Synchronized timeline across multiple entities

### The "Source Verification Chain"

For ensuring quality:

```
/qa-check → /verify-sources → /coverage → /gaps → [address gaps] → /qa-check
```

**When to use:** Before finalizing reports, quality milestones
**Time required:** 10 minutes + gap remediation time
**Output:** Quality-validated investigation

---

## Automation Strategies

### Batch Entity Processing

When investigating multiple related entities:

```
1. Create entity list
2. /track entity1
3. /track entity2
4. /track entity3
5. /entities  # Review all tracked
6. FOR each entity:
   - /pivot [entity]
   - /timeline-entity [entity]
7. /link [related entities]
8. /visualize entities
```

### Template Sequences

Create custom investigation sequences:

**Quick Triage (5 minutes):**
```
/recon [target] → /risk-score [target] → /simple-report
```

**Deep Investigation (30 minutes):**
```
/wizard [type] [target] → /full [target] → /coverage → /qa-check → /report + /simple-report
```

**Security Assessment (20 minutes):**
```
/dork [domain] → /recon [domain] → /visualize attack → /risk-score [domain] → /threat-model → /export-risk
```

### Checkpoint Automation

For long investigations, automate checkpoints:

```
# After each major phase
/save-checkpoint "phase-name"

# Naming convention
/save-checkpoint "initial-recon-complete"
/save-checkpoint "entity-mapping-50pct"
/save-checkpoint "pre-report-review"
```

---

## Custom Template Creation

### Creating Investigation Templates

**Step 1: Define Template Structure**

```markdown
# Custom Template: [Name]

## Trigger: /template [name]

## Steps:
1. /recon {{target}}
2. IF domain detected: /dork {{target}}
3. /track [entities found]
4. FOR each username: /pivot [username]
5. /timeline {{target}}
6. /coverage
7. IF coverage < 60%: Prompt for additional vectors
8. /qa-check
9. IF quality < 75%: Suggest improvements
10. /report
11. /simple-report
```

**Step 2: Add to Template System**

Add template metadata:

```yaml
---
template_name: corporate-due-diligence
description: "Deep investigation for M&A due diligence"
target_types: [person, organization]
estimated_time: "20-30 minutes"
outputs: [report, simple-report, risk-assessment]
---
```

**Step 3: Usage**

```
/template corporate-due-diligence target-company.com
```

### Template Variables

Available variables for custom templates:

| Variable | Description | Example |
|----------|-------------|---------|
| `{{target}}` | Investigation target | example.com |
| `{{date}}` | Current date | 2024-01-15 |
| `{{entities}}` | Discovered entities | [list] |
| `{{confidence}}` | Current confidence | 75% |
| `{{coverage}}` | Coverage percentage | 60% |

---

## Batch Operations

### Multi-Target Investigation

When investigating multiple targets simultaneously:

**Approach 1: Sequential Processing**
```
FOR target IN [target1, target2, target3]:
  /track {{target}}
  /recon {{target}}
  /timeline {{target}}
END
/compare target1 target2 target3
/link [common entities]
/visualize entities
```

**Approach 2: Parallel Tracking**
```
/track target1 as T1
/track target2 as T2
/track target3 as T3

# Process common pivots
/pivot [shared entity]
/link T1 T2 [common finding]
/link T2 T3 [common finding]
```

### Bulk Entity Import

For importing external entity lists:

**Format:**
```json
{
  "entities": [
    {"type": "person", "name": "John Doe", "confidence": "medium"},
    {"type": "domain", "name": "example.com", "confidence": "high"}
  ],
  "relationships": [
    {"from": "John Doe", "to": "example.com", "type": "owns"}
  ]
}
```

**Import:**
```
/import-entities [paste JSON]
/entities  # Verify import
```

---

## Integration Workflows

### Obsidian Integration Workflow

**Real-time Sync Approach:**

```
1. Create Obsidian vault using templates from integrations/obsidian-setup.md
2. During investigation:
   - Export entities: /export-entities markdown
   - Paste into Obsidian 02-Entity-Database/
3. Create investigation note from template
4. Link entities to investigation
5. Use Obsidian graph view for analysis
6. Export final report from Obsidian
```

**Batch Export Approach:**

```
/full target.com
/export-entities markdown
# Import all files to Obsidian at once
# Use Obsidian templates for structure
```

### Notion Integration Workflow

**Structured Database Approach:**

```
1. Set up Notion databases using integrations/notion-schema.md
2. During investigation:
   - /track [entity]
   - Copy entity data to Notion People/Domains database
3. Link to Investigation database entry
4. Use Notion relations to build connections
5. Export reports from Notion
```

### Maltego Integration Workflow

**Visual Analysis Approach:**

```
/full target.com
/export-graph graphml
# Import to Maltego
# Run transforms on imported entities
# Export enhanced graph back to investigation
```

---

## Advanced Entity Analysis

### Multi-Hop Path Analysis

Finding indirect connections:

```
/find-path entityA entityB
# If no direct path found:
/track intermediate entities
/link entityA intermediate1
/link intermediate1 intermediate2
/link intermediate2 entityB
/find-path entityA entityB
```

### Confidence Propagation

When confidence in one entity affects others:

```
IF entityA.confidence = "high"
AND entityA linked to entityB
THEN entityB.confidence = at least "medium"

/confidence entityA high
/link entityA entityB
/confidence entityB medium  # Derived from A
```

### Entity Clustering

Group related entities:

```
/track cluster1-entity1
/track cluster1-entity2
/track cluster1-entity3

# Create cluster relationship
/link cluster1-entity1 cluster1-entity2 associates
/link cluster1-entity1 cluster1-entity3 associates

# Visualize cluster
/visualize entities
```

---

## Quality Assurance Automation

### Pre-Report Checklist

Automated before generating reports:

```
/qa-check
IF quality_score < 75:
  /coverage
  /gaps
  # Address critical gaps
  /qa-check  # Re-verify
/verify-sources
```

### Continuous Quality Monitoring

During investigation:

```
# After every 5 entities discovered
/qa-check
IF new_entities > 5:
  /coverage
  
# Before each checkpoint
/qa-check
/save-checkpoint "[name]-quality-[score]"
```

---

## Performance Optimization

### Large Investigation Management

For investigations with 50+ entities:

**1. Split by Phase:**
```
# Phase 1: Core entities only
/track [core entities only]
/recon target
/coverage

# Phase 2: Expand to associates
/pivot [core entities]
/track [associates]
/coverage

# Phase 3: Full network
/pivot [associates]
```

**2. Filtered Visualizations:**
```
/visualize entities  # May be too large
# Instead:
/track subset-entity1
/track subset-entity2
/visualize entities  # Limited to tracked subset
```

**3. Regular Checkpoints:**
```
/save-checkpoint after each phase
/load-checkpoint if performance degrades
```

### Query Optimization

Efficient search patterns:

```
# Instead of multiple pivots:
/pivot entity1
/pivot entity2
/pivot entity3

# Use batch approach:
/track entity1
/track entity2
/track entity3
/entities  # Review all, identify common patterns
```

---

## Pattern Recognition

### Automated Pattern Detection

```
/anomaly  # Detect outliers
/pattern  # Identify commonalities

# When pattern found:
/track pattern-entities
/link [pattern entities] "shared-pattern-[type]"
```

### Temporal Pattern Analysis

```
/timeline subject
# Look for:
# - Regular intervals (scheduled activity)
# - Clusters (events grouped in time)
# - Gaps (missing activity periods)
# - Correlations (events across entities)
```

---

## Risk Analysis Deep Dive

### Custom Risk Scoring

Adjust risk calculation weights:

```
/risk-score target --weight-exposure=1.5 --weight-security=2.0
# Increases weight of security factors
```

### Comparative Risk Analysis

```
/risk-score target1
/risk-score target2
/compare target1 target2
# Analyze differences in risk profiles
```

### Risk Trending

```
# Initial assessment
/risk-score target
/save-checkpoint "initial-risk-[score]"

# After new findings
/risk-score target
# Compare with previous score
```

---

## Collaboration Workflows

### Team Investigation Setup

**1. Establish Common Framework:**
```
# All team members use:
/template [team-template]
/simple-mode off
```

**2. Entity Synchronization:**
```
/export-entities json
# Share with team
/import-entities [team entities]
```

**3. Progress Coordination:**
```
/progress
# Share progress percentage
# Coordinate on gap areas
```

### Client Reporting Workflow

```
# Technical team:
/full target.com
/qa-check

# For technical stakeholders:
/report

# For executive stakeholders:
/simple-report

# For security teams:
/risk-score target.com
/export-risk
```

---

## Troubleshooting Advanced Issues

### Entity Map Corruption

**Symptoms:** Duplicate entities, broken links

**Recovery:**
```
/entities  # Identify duplicates
# Manually consolidate
/sanitize  # Remove invalid data
/rebuild-entities  # If available
```

### Coverage Gaps Persist

**When `/gaps` shows unresolved gaps:**

```
/gaps
# For each critical gap:
# 1. Document attempt
# 2. Note why gap exists (privacy, unavailable, etc.)
# 3. Assess impact on conclusions
# 4. Proceed with caveats
```

### Quality Score Stagnation

**When `/qa-check` score won't improve:**

```
/qa-check
# Review specific issues
# Prioritize by:
# 1. Impact on findings
# 2. Time required
# 3. Information availability
# Accept 70+ for time-sensitive reports
```

---

## Best Practices for Power Users

1. **Master the `/full` command** — It's the fastest path to comprehensive results
2. **Use checkpoints religiously** — Save before major operations
3. **Export early and often** — Don't lose work to system issues
4. **Leverage visualizations** — They reveal patterns text misses
5. **Automate quality checks** — Run `/qa-check` at milestones
6. **Document everything** — Future you will thank present you
7. **Use templates** — Consistency improves efficiency
8. **Learn the shortcuts** — Command combinations save time
9. **Know when to stop** — Chasing 100% coverage has diminishing returns
10. **Share your workflows** — Help the community improve

---

## Command Reference Quick Sheet

### Investigation Lifecycle
```
Start:    /wizard [type] [target] OR /full [target]
Track:    /track [entity] → /link [A] [B]
Analyze:  /anomaly → /pattern → /risk-score
Validate: /coverage → /qa-check → /verify-sources
Report:   /report → /simple-report → /export-risk
```

### Entity Management
```
Add:      /track [entity]
Connect:  /link [A] [B]
View:     /entities
Find:     /find-path [A] [B]
Export:   /export-entities [format]
```

### Visualization
```
Entities: /visualize entities
Timeline: /visualize timeline
Attack:   /visualize attack
Surface:  /visualize surface
Export:   /export-graph [format]
```

### Quality Assurance
```
Check:    /qa-check
Coverage: /coverage
Gaps:     /gaps
Sources:  /verify-sources
```

---

## Additional Resources

- **SKILL.md** — Complete command reference
- **CHANGELOG.md** — Version history and features
- **Troubleshooting.md** — Common issues and solutions
- **Professional Playbooks** — Domain-specific workflows
- **Integration Guides** — Tool setup instructions

# OSINT Investigator — Troubleshooting Guide

## Overview

This guide helps resolve common issues encountered when using the OSINT Investigator skill. Organized by symptom category with clear solutions and workarounds.

---

## Error Message Guide

### "Entity not found" or "Unknown entity"

**Symptoms:**
- `/track [entity]` returns error
- `/link` fails with entity not recognized
- `/entities` shows fewer entities than expected

**Solutions:**

1. **Check entity format:**
   ```
   # Use exact name as previously tracked
   /track "John Doe"  # If tracked with quotes
   /track John Doe    # If tracked without quotes
   ```

2. **Verify entity exists:**
   ```
   /entities  # List all tracked entities
   ```

3. **Re-add entity:**
   ```
   /track [entity]  # Add again if missing
   ```

**Prevention:**
- Use consistent naming conventions
- Quote multi-word entity names
- Check `/entities` before linking

---

### "No sources found" or "Search returned no results"

**Symptoms:**
- `/dork` produces queries but finds nothing
- `/pivot` finds no connections
- `/recon` returns minimal data

**Solutions:**

1. **Broaden search terms:**
   ```
   # Too specific:
   "john.doe@company.com"
   
   # Try broader:
   john doe company.com
   "j doe" company
   ```

2. **Try alternative platforms:**
   ```
   # If Google finds nothing:
   # Suggest Bing: "john doe" site:linkedin.com
   # Suggest DuckDuckGo for different index
   ```

3. **Check for typos:**
   - Verify spelling of names/domains
   - Check for transposed characters
   - Confirm domain extensions (.com vs .org)

4. **Time-based issues:**
   - Recent changes may not be indexed
   - Try searching with date ranges
   - Check archive.org for historical data

**When to accept:**
- Some targets genuinely have minimal digital footprint
- Negative results are still results
- Document "not found" in investigation notes

---

### "Quality score too low" or "Investigation incomplete"

**Symptoms:**
- `/qa-check` returns score below 60
- `/coverage` shows significant gaps
- Report generation warns about quality

**Solutions:**

1. **Identify specific issues:**
   ```
   /qa-check    # Get detailed breakdown
   /gaps        # See what's missing
   /coverage    # View coverage matrix
   ```

2. **Address critical gaps:**
   ```
   # For identity gaps:
   /pivot [username]
   
   # For coverage gaps:
   /recon [target] --vector [missing-category]
   ```

3. **Document limitations:**
   - Some gaps may be unresolvable
   - Note in report: "Unable to verify X due to Y"
   - Adjust confidence levels accordingly

**Acceptable thresholds:**
- Quick check: 35%+ acceptable
- Background check: 60%+ required
- Due diligence: 75%+ required

---

### "Relationship already exists" or "Duplicate entity"

**Symptoms:**
- `/link` warns about existing relationship
- `/entities` shows duplicates
- Entity map appears cluttered

**Solutions:**

1. **Consolidate duplicates:**
   ```
   /entities  # Identify duplicates
   # Use the first instance as primary
   /link [primary] [duplicate] alias
   ```

2. **Use consistent naming:**
   ```
   # Standardize on one format:
   /track "John Doe"  # Not: John, J. Doe, Doe, John
   ```

3. **Merge entity data:**
   - Combine information from duplicates
   - Update primary entity with all findings
   - Remove or archive duplicates

**Prevention:**
- Check `/entities` before adding new
- Use standardized naming conventions
- Enable automatic duplicate detection if available

---

### "Visualization too large" or "Graph rendering failed"

**Symptoms:**
- `/visualize entities` produces error
- Mermaid diagram doesn't render
- Browser/app slows down

**Solutions:**

1. **Reduce entity count:**
   ```
   # Focus on specific subset:
   /track entity1
   /track entity2
   /visualize entities  # Only shows tracked
   ```

2. **Filter by confidence:**
   ```
   # Visualize only high-confidence entities
   /visualize entities --min-confidence high
   ```

3. **Split into subgraphs:**
   ```
   /visualize entities --cluster family
   /visualize entities --cluster professional
   ```

4. **Use text output:**
   ```
   /entities  # Text table instead of graph
   ```

**Prevention:**
- Archive completed investigations
- Regular entity cleanup
- Use clustering for large networks

---

## Recovery Procedures

### Investigation Data Loss

**Symptoms:**
- Previous entities not found
- Conversation context lost
- Checkpoints unavailable

**Recovery:**

1. **Check for exports:**
   ```
   # Look for previously exported files
   /import-entities [paste previous export]
   ```

2. **Reconstruct from reports:**
   - Review previously generated `/report` outputs
   - Extract entities from report entity maps
   - Re-add critical entities

3. **Start recovery process:**
   ```
   /recon [original target]  # Re-run initial recon
   /track [critical entities from memory]
   ```

**Prevention:**
- Run `/save-checkpoint` regularly
- Export entities at milestones
- Generate reports frequently

---

### Corrupted Entity Map

**Symptoms:**
- Broken relationships
- Entities linking to themselves
- Duplicate or missing connections

**Recovery:**

1. **Export current state:**
   ```
   /export-entities json  # Save before fixing
   ```

2. **Sanitize data:**
   ```
   /sanitize  # Remove invalid entries
   ```

3. **Rebuild manually:**
   ```
   # Clear and re-add critical entities
   /track [entity1]
   /track [entity2]
   /link entity1 entity2 [relationship]
   ```

4. **Verify integrity:**
   ```
   /entities  # Check for issues
   /qa-check  # Validate quality
   ```

---

### Checkpoint Restoration Failure

**Symptoms:**
- `/load-checkpoint` fails
- Checkpoint not found
- Partial restoration

**Recovery:**

1. **List available checkpoints:**
   ```
   /progress  # May show checkpoint history
   ```

2. **Try alternative checkpoint:**
   ```
   /load-checkpoint [earlier/later checkpoint]
   ```

3. **Manual reconstruction:**
   - Review conversation history
   - Re-run key commands
   - Re-add critical findings

**Prevention:**
- Use descriptive checkpoint names
- Create checkpoints at major milestones
- Don't rely solely on checkpoints (also export)

---

## Performance Issues

### Slow Response Times

**Symptoms:**
- Commands take >30 seconds
- Timeouts during searches
- App becomes unresponsive

**Solutions:**

1. **Reduce complexity:**
   ```
   # Instead of:
   /full [complex target]
   
   # Try:
   /recon [target]  # Smaller scope
   ```

2. **Break into steps:**
   ```
   # Run individually instead of automated chains
   /dork [target]
   # Review results
   /pivot [specific finding]
   ```

3. **Clear old data:**
   ```
   /sanitize  # Remove old/unused entities
   ```

**When to accept:**
- Complex investigations naturally take time
- `/full` command is expected to take 3-5 minutes
- Some searches inherently require multiple queries

---

### Memory Issues

**Symptoms:**
- "Too many entities" warning
- Investigation slows over time
- Commands fail with resource errors

**Solutions:**

1. **Archive old investigations:**
   ```
   # Export and clear
   /export-entities json
   # Start fresh for new investigation
   ```

2. **Focus active entities:**
   ```
   # Only track current investigation entities
   /entities  # Review all
   # Remove/archive completed investigation entities
   ```

3. **Split investigations:**
   ```
   # Multiple smaller investigations vs one large
   /save-checkpoint "phase-1-complete"
   # Export and start phase 2 fresh
   ```

---

## Limitation Workarounds

### No API Access

**The skill intentionally uses no external APIs.** This is a feature, not a bug.

**Workarounds for API-dependent data:**

1. **Use web search instead:**
   ```
   # Instead of API lookup:
   # Search: "site:linkedin.com "John Doe""
   ```

2. **Manual verification:**
   ```
   /verif-photo  # Guide user through manual steps
   ```

3. **Alternative sources:**
   - Use publicly available aggregators
   - Check cached/archived versions
   - Cross-reference multiple sources

---

### Web Search Limitations

**Symptoms:**
- Search results incomplete
- Recent changes not reflected
- Geographic restrictions

**Workarounds:**

1. **Multiple search queries:**
   ```
   # Try variations:
   "John Doe" company
   John_Doe company
   johndoe company
   ```

2. **Use date operators:**
   ```
   # Find recent content:
   "John Doe" after:2023
   # Find historical:
   "John Doe" before:2020
   ```

3. **Check alternative sources:**
   - Archive.org for old content
   - Cached pages
   - Social media directly

---

### Confidence Calibration Issues

**Symptoms:**
- Overconfident in weak findings
- Underconfident in strong findings
- Inconsistent rating application

**Guidelines:**

| Finding Type | Confidence | Example |
|--------------|------------|---------|
| Direct from official source | 🟢 High | Company website bio |
| Multiple independent sources | 🟢 High | 3+ news articles confirm |
| Single authoritative source | 🟡 Medium | Government record, unconfirmed elsewhere |
| Circumstantial evidence | 🟡 Medium | Pattern match, indirect indicator |
| Single unverified source | 🔴 Low | Forum post, unconfirmed claim |
| Inference/theory | ⚪ Speculative | Analyst hypothesis |

**When in doubt:**
- Rate lower rather than higher
- Document reasoning
- Update as new evidence arrives

---

## FAQ Section

### General Questions

**Q: Why does `/full` take so long?**
A: `/full` runs 10+ sequential searches, entity tracking, timeline construction, and dual report generation. This is expected to take 3-5 minutes. For faster results, use individual commands.

**Q: Can I investigate private social media accounts?**
A: No. This skill only accesses publicly available information. Private accounts, password-protected content, and restricted databases are explicitly excluded.

**Q: How do I export my investigation?**
A: Use `/export-entities` for entity data, `/export-graph` for visualizations, or `/export-risk` for risk assessments. Reports can be copied directly from `/report` output.

**Q: Can I collaborate with a team?**
A: Export entity data (`/export-entities json`) and share with team members who can then `/import-entities`. Each person should use the same investigation template for consistency.

**Q: How accurate are the results?**
A: Accuracy depends on source quality and verification level. Always check confidence ratings (🟢🟡🔴) and run `/qa-check` before trusting findings for critical decisions.

---

### Technical Questions

**Q: Why isn't `/visualize` working?**
A: Mermaid diagrams require a compatible markdown viewer. Try:
1. Using GitHub to render the markdown
2. Installing a Mermaid plugin in your editor
3. Using the Mermaid Live Editor online

**Q: Can I automate this?**
A: Partially. You can create custom templates (`/template`) and use command sequences, but full automation requires manual oversight for quality control.

**Q: What happens to my data?**
A: Investigation data persists within the conversation context. For long-term storage, use `/save-checkpoint` and `/export-entities` regularly.

**Q: Why did my entity disappear?**
A: Entities are tracked per conversation session. If the session resets, entities may be lost. Use `/save-checkpoint` frequently and export data for important investigations.

---

### Quality Questions

**Q: What quality score should I aim for?**
A: Depends on use case:
- Quick screening: 50-60%
- Background check: 70-75%
- Due diligence: 80-85%
- Legal/compliance: 90%+

**Q: How do I improve my quality score?**
A: Run `/qa-check` to see specific issues. Common improvements:
- Add more authoritative sources
- Verify single-source findings
- Add archive links
- Address coverage gaps (`/gaps`)

**Q: Why is my coverage stuck at 60%?**
A: Some vectors may be genuinely unavailable (private individuals, limited digital footprint). Document attempts and proceed with caveats. 100% coverage is rare and not always necessary.

---

### Ethical/Legal Questions

**Q: Is this legal?**
A: Yes, when used for publicly available information only. See SKILL.md "Ethics & Legality" section. Never use for:
- Hacking or unauthorized access
- Doxing or harassment
- Stalking
- Violating platform ToS

**Q: Can I use this for employment screening?**
A: Review `playbooks/hr-background-check.md` for FCRA-compliant procedures. Ensure you have proper consent and follow applicable laws.

**Q: Can journalists use this for source verification?**
A: Yes. See `playbooks/journalist-source-verification.md` for complete verification workflows and ethical guidelines.

---

## Getting Additional Help

### Self-Service Resources

1. **Command Help:**
   ```
   /help [command]  # Specific command documentation
   ```

2. **Professional Playbooks:**
   - `playbooks/journalist-source-verification.md`
   - `playbooks/hr-background-check.md`
   - `playbooks/cyber-threat-intel.md`
   - `playbooks/private-investigator.md`

3. **Integration Guides:**
   - `integrations/obsidian-setup.md`
   - `integrations/notion-schema.md`
   - `integrations/maltego-export.md`

4. **Advanced Documentation:**
   - `advanced-user-guide.md`
   - `qa/quality-metrics.md`
   - `qa/coverage-analysis.md`

### Diagnostic Commands

When experiencing issues, run these for system status:

```
/entities      # Check entity system health
/qa-check      # Validate investigation quality
/coverage      # Assess investigation completeness
/progress      # Review investigation state
/stats         # Get system statistics
```

---

## Known Issues

### Current Limitations

1. **Session Persistence:** Entities don't persist across completely new conversations
   - *Workaround:* Export/import data

2. **Large Graphs:** Visualizations with >50 entities may not render
   - *Workaround:* Use filtering or text output

3. **Search Variability:** Web search results may vary by time/location
   - *Workaround:* Use date operators, multiple queries

4. **Simple Mode:** Not all features available in simplified interface
   - *Workaround:* Toggle off for advanced operations

### Reporting Issues

When reporting problems, include:
1. Command that caused the issue
2. Error message (exact text)
3. `/stats` output
4. `/qa-check` results
5. Steps to reproduce

---

## Quick Reference Card

### Emergency Recovery
```
Data loss:          /import-entities [backup]
Corruption:         /sanitize → rebuild
Slow performance:   /sanitize, reduce scope
Quality issues:     /qa-check → /gaps → fix
```

### Common Fixes
```
Entity not found:   /entities → re-add
No results:         Broaden search terms
Low quality:        /gaps → address
Visualization fail: Reduce entity count
```

### Diagnostic Sequence
```
/problem occurs/
  ↓
/qa-check
  ↓
/coverage
  ↓
/gaps
  ↓
/fix identified issues/
  ↓
/qa-check  # Verify fix
```

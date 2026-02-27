# Contradiction Detection System

## Overview

The Contradiction Detection System automatically identifies conflicting information within an investigation, flags inconsistencies, and provides rules for confidence adjustment based on resolution outcomes.

## Types of Contradictions

### 1. Value Contradictions

When the same entity attribute has different values across sources.

**Example:**
```
Entity: John Doe
Source A: Date of Birth = 1980-01-15
Source B: Date of Birth = 1985-03-22
```

### 2. Temporal Contradictions

When timeline events cannot coexist chronologically.

**Example:**
```
Event A: Started job at Company X in 2018
Event B: Graduated university in 2019
Event C: Claims 5 years experience at Company X in 2020
```

### 3. Existence Contradictions

When one source claims something exists and another claims it doesn't.

**Example:**
```
Source A: Domain example.com registered to John Doe
Source B: John Doe claims no domain ownership
```

### 4. Relationship Contradictions

When entity relationships conflict.

**Example:**
```
Relationship A: Person A EMPLOYS Person B
Relationship B: Person B owns Company that COMPETES_WITH Person A's company
```

### 5. Location Contradictions

When an entity cannot be in multiple places simultaneously.

**Example:**
```
Post A: User posts from New York at 2024-01-15 14:00 UTC
Post B: User posts from Tokyo at 2024-01-15 14:30 UTC
```

## Contradiction Detection Algorithm

```javascript
function detectContradictions(investigation) {
    const contradictions = [];
    
    // Check each entity for internal contradictions
    investigation.entities.forEach(entity => {
        const entityContradictions = checkEntityContradictions(entity);
        contradictions.push(...entityContradictions);
    });
    
    // Check for cross-entity contradictions
    const crossContradictions = checkCrossEntityContradictions(investigation.entities);
    contradictions.push(...crossContradictions);
    
    // Check relationship contradictions
    const relationshipContradictions = checkRelationshipContradictions(
        investigation.entities, 
        investigation.relationships
    );
    contradictions.push(...relationshipContradictions);
    
    // Check temporal contradictions
    const temporalContradictions = checkTemporalContradictions(investigation);
    contradictions.push(...temporalContradictions);
    
    return contradictions.map(c => ({
        ...c,
        id: generateUUID(),
        detected_at: now(),
        status: "OPEN"
    }));
}

function checkEntityContradictions(entity) {
    const contradictions = [];
    const evidenceByAttribute = {};
    
    // Group evidence by attribute
    entity.evidence.forEach(ev => {
        const attr = extractAttribute(ev.content);
        if (attr) {
            if (!evidenceByAttribute[attr.name]) {
                evidenceByAttribute[attr.name] = [];
            }
            evidenceByAttribute[attr.name].push({
                evidence: ev,
                value: attr.value
            });
        }
    });
    
    // Check for conflicting values
    Object.entries(evidenceByAttribute).forEach(([attrName, items]) => {
        const uniqueValues = [...new Set(items.map(i => normalizeValue(i.value)))];
        
        if (uniqueValues.length > 1) {
            contradictions.push({
                type: "VALUE_CONTRADICTION",
                severity: calculateSeverity(items),
                entity_id: entity.id,
                entity_type: entity.type,
                attribute: attrName,
                conflicting_values: uniqueValues.map(v => ({
                    value: v,
                    sources: items
                        .filter(i => normalizeValue(i.value) === v)
                        .map(i => ({
                            evidence_id: i.evidence.id,
                            source: i.evidence.source,
                            reliability: i.evidence.source_reliability
                        }))
                })),
                recommendation: generateRecommendation(items)
            });
        }
    });
    
    return contradictions;
}

function checkCrossEntityContradictions(entities) {
    const contradictions = [];
    
    // Check for duplicate entities with conflicting info
    for (let i = 0; i < entities.length; i++) {
        for (let j = i + 1; j < entities.length; j++) {
            const e1 = entities[i];
            const e2 = entities[j];
            
            // Check if entities might be the same
            if (mightBeSameEntity(e1, e2)) {
                const conflicts = findAttributeConflicts(e1, e2);
                
                if (conflicts.length > 0) {
                    contradictions.push({
                        type: "ENTITY_IDENTITY_CONTRADICTION",
                        severity: "HIGH",
                        entity_ids: [e1.id, e2.id],
                        entities: [
                            { id: e1.id, value: e1.value, type: e1.type },
                            { id: e2.id, value: e2.value, type: e2.type }
                        ],
                        conflicting_attributes: conflicts,
                        recommendation: "Investigate if these are the same entity or resolve attribute conflicts"
                    });
                }
            }
        }
    }
    
    return contradictions;
}

function checkRelationshipContradictions(entities, relationships) {
    const contradictions = [];
    
    // Check for mutually exclusive relationships
    const mutualExclusives = {
        "PARENT_OF": ["CHILD_OF", "SIBLING_OF"],
        "EMPLOYMENT": ["COMPETES_WITH"],
        "OWNERSHIP": ["REGISTRATION"]  // Person can't own and register as different entities
    };
    
    relationships.forEach((rel, idx) => {
        const conflictingTypes = mutualExclusives[rel.type] || [];
        
        const conflicts = relationships.filter((other, otherIdx) => {
            if (idx === otherIdx) return false;
            
            return conflictingTypes.includes(other.type) &&
                   ((rel.source_id === other.source_id && rel.target_id === other.target_id) ||
                    (rel.source_id === other.target_id && rel.target_id === other.source_id));
        });
        
        if (conflicts.length > 0) {
            contradictions.push({
                type: "RELATIONSHIP_CONTRADICTION",
                severity: "HIGH",
                relationship_id: rel.id,
                relationship_type: rel.type,
                conflicting_relationships: conflicts.map(c => c.id),
                entities_involved: [rel.source_id, rel.target_id],
                recommendation: "Verify relationship accuracy - relationships are mutually exclusive"
            });
        }
    });
    
    return contradictions;
}

function checkTemporalContradictions(investigation) {
    const contradictions = [];
    const events = extractTemporalEvents(investigation);
    
    // Sort events by claimed time
    events.sort((a, b) => new Date(a.timestamp) - new Date(b.timestamp));
    
    // Check for impossible sequences
    for (let i = 0; i < events.length; i++) {
        for (let j = i + 1; j < events.length; j++) {
            const e1 = events[i];
            const e2 = events[j];
            
            // Check if events involve the same entity
            if (e1.entity_id === e2.entity_id) {
                const impossible = isImpossibleSequence(e1, e2);
                
                if (impossible) {
                    contradictions.push({
                        type: "TEMPORAL_CONTRADICTION",
                        severity: "HIGH",
                        entity_id: e1.entity_id,
                        event_1: {
                            description: e1.description,
                            claimed_time: e1.timestamp,
                            evidence_id: e1.evidence_id
                        },
                        event_2: {
                            description: e2.description,
                            claimed_time: e2.timestamp,
                            evidence_id: e2.evidence_id
                        },
                        reason: impossible.reason,
                        recommendation: "Verify timestamps - at least one event is incorrectly dated"
                    });
                }
            }
        }
    }
    
    return contradictions;
}

function mightBeSameEntity(e1, e2) {
    // Check for matching aliases
    if (e1.aliases.some(a => e2.aliases.includes(a) || e2.value === a)) {
        return true;
    }
    
    // Check for similar values
    const similarity = calculateSimilarity(e1.value, e2.value);
    if (similarity > 0.8) return true;
    
    // Check for shared attributes
    const sharedAttrs = findSharedAttributes(e1, e2);
    if (sharedAttrs.length > 0) return true;
    
    return false;
}

function findAttributeConflicts(e1, e2) {
    const conflicts = [];
    
    // Compare all attributes
    Object.entries(e1.attributes).forEach(([key, value1]) => {
        const value2 = e2.attributes[key];
        if (value2 && normalizeValue(value1) !== normalizeValue(value2)) {
            conflicts.push({
                attribute: key,
                entity_1_value: value1,
                entity_2_value: value2
            });
        }
    });
    
    return conflicts;
}

function calculateSeverity(conflictingItems) {
    // Severity based on source reliability differences
    const reliabilities = conflictingItems.map(i => i.evidence.source_reliability);
    const hasReliableSource = reliabilities.some(r => ['A', 'B'].includes(r));
    const hasUnreliableSource = reliabilities.some(r => ['D', 'E'].includes(r));
    
    if (hasReliableSource && hasUnreliableSource) {
        return "LOW"; // Easy to resolve - trust reliable source
    } else if (reliabilities.every(r => r === reliabilities[0])) {
        return "HIGH"; // Same reliability sources disagree
    }
    
    return "MEDIUM";
}

function generateRecommendation(items) {
    const reliabilities = items.map(i => i.evidence.source_reliability);
    const bestReliability = Math.min(...reliabilities.map(r => 
        ({A:1, B:2, C:3, D:4, E:5, F:3})[r]
    ));
    
    const bestSource = items.find(i => 
        ({A:1, B:2, C:3, D:4, E:5, F:3})[i.evidence.source_reliability] === bestReliability
    );
    
    return `Prefer value from ${bestSource.evidence.source} (reliability: ${bestSource.evidence.source_reliability})`;
}
```

## Contradiction Resolution Framework

```javascript
const resolutionStrategies = {
    // Strategy 1: Prefer higher reliability source
    preferReliability: (contradiction) => {
        const { conflicting_values } = contradiction;
        
        // Score each value's sources
        const scored = conflicting_values.map(cv => ({
            value: cv.value,
            score: cv.sources.reduce((sum, s) => {
                const weights = { A: 1.0, B: 0.8, C: 0.6, D: 0.4, E: 0.2, F: 0.5 };
                return sum + (weights[s.reliability] || 0.5);
            }, 0)
        }));
        
        const best = scored.sort((a, b) => b.score - a.score)[0];
        
        return {
            resolution: "PREFER_RELIABILITY",
            selected_value: best.value,
            confidence_adjustment: 0.8,
            reasoning: `Selected based on higher cumulative source reliability (score: ${best.score.toFixed(2)})`
        };
    },
    
    // Strategy 2: Prefer more recent evidence
    preferRecency: (contradiction, investigation) => {
        const { conflicting_values } = contradiction;
        
        const withDates = conflicting_values.map(cv => ({
            value: cv.value,
            latest: Math.max(...cv.sources.map(s => 
                new Date(getEvidenceTimestamp(s.evidence_id, investigation)).getTime()
            ))
        }));
        
        const mostRecent = withDates.sort((a, b) => b.latest - a.latest)[0];
        
        return {
            resolution: "PREFER_RECENCY",
            selected_value: mostRecent.value,
            confidence_adjustment: 0.7,
            reasoning: "Selected most recent evidence as more likely to be current"
        };
    },
    
    // Strategy 3: Require additional verification
    requireVerification: (contradiction) => {
        return {
            resolution: "REQUIRES_VERIFICATION",
            selected_value: null,
            confidence_adjustment: 0.0,
            reasoning: "Conflicting sources have equal reliability - requires third-party verification",
            action_required: "Find additional corroborating source"
        };
    },
    
    // Strategy 4: Both values valid (different contexts)
    acceptBoth: (contradiction, context) => {
        return {
            resolution: "ACCEPT_BOTH",
            selected_value: null,
            confidence_adjustment: 1.0,
            reasoning: "Values may be valid in different contexts (e.g., maiden name vs married name)",
            note: "Store both values with context annotations"
        };
    },
    
    // Strategy 5: Flag as uncertain
    flagUncertain: (contradiction) => {
        return {
            resolution: "FLAG_UNCERTAIN",
            selected_value: null,
            confidence_adjustment: 0.0,
            reasoning: "Unable to resolve - mark entity confidence as DISPUTED",
            action_required: "Manual review required"
        };
    }
};

function resolveContradiction(contradiction, strategy = "auto") {
    let resolution;
    
    if (strategy === "auto") {
        // Auto-select strategy based on contradiction type
        switch (contradiction.type) {
            case "VALUE_CONTRADICTION":
                resolution = resolutionStrategies.preferReliability(contradiction);
                break;
            case "TEMPORAL_CONTRADICTION":
                resolution = resolutionStrategies.flagUncertain(contradiction);
                break;
            case "ENTITY_IDENTITY_CONTRADICTION":
                resolution = resolutionStrategies.requireVerification(contradiction);
                break;
            default:
                resolution = resolutionStrategies.flagUncertain(contradiction);
        }
    } else {
        resolution = resolutionStrategies[strategy](contradiction);
    }
    
    return {
        ...contradiction,
        resolution: resolution,
        resolved_at: now(),
        status: resolution.resolution === "REQUIRES_VERIFICATION" ? "PENDING" : "RESOLVED"
    };
}
```

## Confidence Adjustment Rules

```javascript
const confidenceAdjustmentRules = {
    // When contradiction is resolved in favor of an entity
    resolvedFavorably: (entity, resolution) => {
        const adjustments = {
            PREFER_RELIABILITY: 0.0,    // No penalty - used reliable source
            PREFER_RECENCY: -0.1,       // Small penalty for recency-based selection
            ACCEPT_BOTH: 0.0,           // No penalty - both may be valid
            REQUIRES_VERIFICATION: -0.3, // Significant penalty - unverified
            FLAG_UNCERTAIN: -0.5        // Major penalty - unresolved
        };
        
        return adjustments[resolution.resolution] || -0.2;
    },
    
    // When entity has unresolved contradictions
    unresolvedContradiction: (entity, contradictions) => {
        const severityWeights = { HIGH: 0.4, MEDIUM: 0.2, LOW: 0.1 };
        
        const totalPenalty = contradictions.reduce((sum, c) => {
            return sum + (severityWeights[c.severity] || 0.2);
        }, 0);
        
        return -Math.min(totalPenalty, 0.8); // Cap at 0.8 reduction
    },
    
    // When contradiction is found and resolved during evidence addition
    evidenceContradictionResolved: (originalConfidence, resolution) => {
        switch (resolution.resolution) {
            case "PREFER_RELIABILITY":
                return originalConfidence; // Unchanged
            case "PREFER_RECENCY":
                return originalConfidence * 0.9;
            case "REQUIRES_VERIFICATION":
                return originalConfidence * 0.5;
            case "FLAG_UNCERTAIN":
                return "DISPUTED";
            default:
                return originalConfidence * 0.8;
        }
    }
};

function adjustConfidence(entity, contradictions) {
    let currentConfidence = entity.confidence;
    let confidenceScore = confidenceLevelToScore(currentConfidence);
    
    // Separate resolved and unresolved
    const resolved = contradictions.filter(c => c.status === "RESOLVED");
    const unresolved = contradictions.filter(c => c.status === "OPEN" || c.status === "PENDING");
    
    // Apply resolved adjustments
    resolved.forEach(c => {
        const adjustment = confidenceAdjustmentRules.resolvedFavorably(entity, c.resolution);
        confidenceScore += adjustment;
    });
    
    // Apply unresolved penalties
    if (unresolved.length > 0) {
        const penalty = confidenceAdjustmentRules.unresolvedContradiction(entity, unresolved);
        confidenceScore += penalty;
    }
    
    // Ensure within bounds
    confidenceScore = Math.max(0, Math.min(1.5, confidenceScore));
    
    // Convert back to level
    return scoreToConfidenceLevel(confidenceScore);
}

function confidenceLevelToScore(level) {
    const scores = {
        "CERTAIN": 1.2,
        "HIGH": 0.9,
        "MEDIUM": 0.6,
        "LOW": 0.3,
        "UNCERTAIN": 0.1,
        "DISPUTED": 0.0
    };
    return scores[level] || 0.5;
}

function scoreToConfidenceLevel(score) {
    if (score >= 1.0) return "CERTAIN";
    if (score >= 0.7) return "HIGH";
    if (score >= 0.4) return "MEDIUM";
    if (score >= 0.2) return "LOW";
    if (score > 0) return "UNCERTAIN";
    return "DISPUTED";
}
```

## Example Contradictions and Resolutions

### Example 1: Simple Value Contradiction

```javascript
// Contradiction Detected:
{
    type: "VALUE_CONTRADICTION",
    entity_id: "person-001",
    attribute: "date_of_birth",
    conflicting_values: [
        {
            value: "1980-01-15",
            sources: [
                { source: "Public records", reliability: "A" }
            ]
        },
        {
            value: "1985-03-22",
            sources: [
                { source: "Social media profile", reliability: "C" }
            ]
        }
    ],
    severity: "LOW"
}

// Resolution:
{
    resolution: "PREFER_RELIABILITY",
    selected_value: "1980-01-15",
    confidence_adjustment: 0.8,
    reasoning: "Public records (reliability A) preferred over social media (reliability C)"
}

// Result: Entity confidence remains HIGH
```

### Example 2: Unresolvable Contradiction

```javascript
// Contradiction Detected:
{
    type: "VALUE_CONTRADICTION",
    entity_id: "company-001",
    attribute: "registration_number",
    conflicting_values: [
        {
            value: "12345678",
            sources: [
                { source: "Registry A", reliability: "A" }
            ]
        },
        {
            value: "87654321",
            sources: [
                { source: "Registry B", reliability: "A" }
            ]
        }
    ],
    severity: "HIGH"
}

// Resolution:
{
    resolution: "REQUIRES_VERIFICATION",
    selected_value: null,
    confidence_adjustment: 0.0,
    reasoning: "Both sources have equal reliability (A) - cannot auto-resolve",
    action_required: "Contact company directly or verify against original incorporation documents"
}

// Result: Entity confidence reduced to DISPUTED until resolved
```

### Example 3: Temporal Contradiction

```javascript
// Contradiction Detected:
{
    type: "TEMPORAL_CONTRADICTION",
    entity_id: "person-002",
    event_1: {
        description: "Started at Company X",
        claimed_time: "2018-06-01",
        evidence_id: "ev-010"
    },
    event_2: {
        description: "Graduated from University",
        claimed_time: "2019-05-15",
        evidence_id: "ev-011"
    },
    reason: "Claims 5 years experience at Company X in 2020 post, but graduated in 2019"
}

// Resolution Options:
// Option A: Start date is incorrect (maybe internship)
// Option B: Graduation date is incorrect (maybe earlier graduation)
// Option C: Experience claim is exaggerated

// Selected Resolution:
{
    resolution: "FLAG_UNCERTAIN",
    selected_value: null,
    confidence_adjustment: -0.5,
    reasoning: "Timeline inconsistency requires manual investigation",
    notes: "Possible explanations: (1) Started as intern before graduation, (2) Previous graduation not recorded, (3) Inflated experience claim"
}

// Result: Entity marked with temporal flag, confidence reduced to MEDIUM
```

### Example 4: Entity Identity Contradiction

```javascript
// Contradiction Detected:
{
    type: "ENTITY_IDENTITY_CONTRADICTION",
    entities: [
        { id: "email-001", value: "john.doe@example.com", type: "EMAIL" },
        { id: "email-002", value: "john.doe@company.com", type: "EMAIL" }
    ],
    conflicting_attributes: [
        {
            attribute: "associated_person",
            entity_1_value: "John Doe (Developer)",
            entity_2_value: "John Doe (Marketing)"
        }
    ]
}

// Resolution:
{
    resolution: "ACCEPT_BOTH",
    selected_value: null,
    confidence_adjustment: 0.0,
    reasoning: "Same person with multiple emails for different roles/employers",
    note: "Create relationship between emails: ALIAS_OF or ASSOCIATED_WITH"
}

// Result: Both entities kept, relationship added, confidence unchanged
```

### Example 5: Location Contradiction

```javascript
// Contradiction Detected:
{
    type: "LOCATION_CONTRADICTION",
    entity_id: "person-003",
    location_1: {
        location: "New York, NY",
        timestamp: "2024-01-15T14:00:00Z",
        source: "Twitter post with geotag"
    },
    location_2: {
        location: "Tokyo, Japan",
        timestamp: "2024-01-15T14:30:00Z",
        source: "Instagram post with geotag"
    },
    impossibility: "Cannot travel NYC to Tokyo in 30 minutes"
}

// Resolution:
{
    resolution: "PREFER_RELIABILITY",
    selected_value: "New York, NY",
    confidence_adjustment: 0.0,
    reasoning: "Twitter geotag (device GPS) considered more reliable than Instagram location tag (user-selected)",
    note: "Tokyo post may be: (1) Scheduled post, (2) VPN/proxy location, (3) Previously taken photo"
}

// Result: NYC location accepted, Tokyo flagged as potentially inaccurate
```

## Command Interface

```javascript
// Detect contradictions in current investigation
/detect-contradictions [--entity <id>] [--severity <level>]

// Show all detected contradictions
/list-contradictions [--status <open|resolved|pending>]

// Show details of specific contradiction
/show-contradiction <id>

// Resolve a contradiction
/resolve-contradiction <id> --strategy <strategy> [--value <selected>]

// Available strategies:
// - prefer-reliability: Choose value from most reliable source(s)
// - prefer-recency: Choose most recent evidence
// - accept-both: Keep both values (valid in different contexts)
// - flag-uncertain: Mark as disputed pending verification
// - custom: Manual resolution with explanation

// Examples:
/resolve-contradiction cnt-001 --strategy prefer-reliability
/resolve-contradiction cnt-002 --strategy custom --value "1980-01-15" --reason "Birth certificate found"

// Ignore a contradiction (mark as false positive)
/ignore-contradiction <id> --reason <explanation>

// Recheck after new evidence
/recheck-contradictions

// Export contradictions report
/export-contradictions [filename]
```

## Contradiction Report Format

```javascript
function generateContradictionReport(investigation) {
    const contradictions = detectContradictions(investigation);
    
    return {
        generated_at: now(),
        investigation_id: investigation.id,
        summary: {
            total_contradictions: contradictions.length,
            by_type: countBy(contradictions, 'type'),
            by_severity: countBy(contradictions, 'severity'),
            by_status: countBy(contradictions, 'status'),
            open_count: contradictions.filter(c => c.status === "OPEN").length
        },
        open_contradictions: contradictions.filter(c => c.status === "OPEN"),
        recently_resolved: contradictions.filter(c => 
            c.status === "RESOLVED" && 
            hoursSince(c.resolved_at) < 24
        ),
        recommendations: generateContradictionRecommendations(contradictions)
    };
}

function generateContradictionRecommendations(contradictions) {
    const open = contradictions.filter(c => c.status === "OPEN");
    const recs = [];
    
    if (open.length === 0) {
        recs.push("No open contradictions - investigation consistency is good");
    } else {
        const highSeverity = open.filter(c => c.severity === "HIGH");
        if (highSeverity.length > 0) {
            recs.push(`Address ${highSeverity.length} high-severity contradictions before finalizing report`);
        }
        
        const unresolvable = open.filter(c => c.severity === "HIGH" && 
            c.conflicting_values?.every(v => v.sources.every(s => s.reliability === 'A'))
        );
        if (unresolvable.length > 0) {
            recs.push(`${unresolvable.length} contradictions have conflicting A-grade sources - requires primary source verification`);
        }
    }
    
    return recs;
}
```

## Visual Output Example

```
Contradiction Report for: Phishing Domain Investigation
═══════════════════════════════════════════════════════════════

Summary:
  Total Contradictions: 5
  By Severity: HIGH: 1 | MEDIUM: 2 | LOW: 2
  By Status: OPEN: 3 | RESOLVED: 2 | PENDING: 0

OPEN CONTRADICTIONS:
─────────────────────────────────────────────────────────────────

[CRITICAL] #cnt-001 - Value Contradiction (HIGH)
Entity: person-001 (John Doe)
Attribute: Date of Birth
Conflict:
  Value A: 1980-01-15 (Source: Public Records - Reliability: A)
  Value B: 1985-03-22 (Source: Social Media - Reliability: C)
Recommendation: Prefer 1980-01-15 from Public Records
Action: /resolve-contradiction cnt-001 --strategy prefer-reliability

[WARNING] #cnt-003 - Temporal Contradiction (HIGH)
Entity: person-002
Issue: Timeline impossibility detected
  Event: Started at Company X (2018-06-01)
  Event: Graduated University (2019-05-15)
  Claim: 5 years experience in 2020
Recommendation: Manual review required - possible explanations:
  - Started as intern before graduation
  - Earlier graduation not recorded
  - Inflated experience claim
Action: /show-contradiction cnt-003 for details

─────────────────────────────────────────────────────────────────

RECOMMENDATIONS:
• Address 1 high-severity contradiction before finalizing report
• 1 contradiction has conflicting A-grade sources - requires verification
• Consider reviewing temporal consistency of all person entities

Run /detect-contradictions --entity person-001 to recheck after resolution
```

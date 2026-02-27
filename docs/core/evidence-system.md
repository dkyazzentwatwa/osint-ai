# Evidence System Specification

## Overview

The Evidence System provides a structured framework for tracking, verifying, and weighting information gathered during OSINT investigations. Every piece of data must have associated evidence with clear provenance and confidence assessment.

## Evidence Types

### [DIRECT] Evidence

Direct evidence comes from firsthand observation or official records. This is the strongest evidence type.

**Characteristics:**
- Primary source material
- Directly observed or recorded
- Minimal interpretation required
- High confidence when source is reliable

**Examples:**
- WHOIS registration records
- SSL certificate data
- Official company registration documents
- Direct screenshots of a profile
- Raw API responses
- Email headers

**Confidence Weight:** 1.0 (baseline)

```javascript
{
    "id": "ev-001",
    "type": "DIRECT",
    "source": "WHOIS lookup for example.com",
    "source_url": "https://whois.icann.org/...",
    "source_reliability": "A",
    "content": "Registrar: NameCheap, Inc. | Created: 2020-01-15",
    "captured_at": "2024-01-15T10:30:00Z"
}
```

### [INFERRED] Evidence

Inferred evidence is derived through logical deduction from direct evidence. Requires explicit reasoning chain.

**Characteristics:**
- Derived from direct evidence
- Requires logical inference
- Documented reasoning required
- Confidence depends on inference strength

**Examples:**
- Inferring email from domain (admin@domain.com)
- Deducing location from IP geolocation
- Inferring real name from username patterns
- Deducing employment from LinkedIn connections

**Confidence Weight:** 0.6 - 0.8 (depending on inference strength)

```javascript
{
    "id": "ev-002",
    "type": "INFERRED",
    "source": "Logical deduction from domain",
    "source_reliability": "N/A",
    "content": "Email admin@example.com inferred from domain ownership",
    "captured_at": "2024-01-15T10:35:00Z",
    "inference_basis": ["ev-001"],
    "reasoning": "Standard admin email pattern for owned domains"
}
```

### [CORROBORATED] Evidence

Corroborated evidence is confirmed by multiple independent sources.

**Characteristics:**
- Multiple independent sources
- Sources don't share common origin
- Consistent information across sources
- Higher confidence than single source

**Examples:**
- Email found on multiple social platforms
- Phone number in both WHOIS and business directory
- Name matching across LinkedIn and company registry
- Location confirmed by IP and social posts

**Confidence Weight:** 1.2 (multiplier applied to base)

```javascript
{
    "id": "ev-003",
    "type": "CORROBORATED",
    "source": "Cross-platform verification",
    "source_reliability": "A",
    "content": "john@example.com found on LinkedIn and GitHub profiles",
    "captured_at": "2024-01-15T11:00:00Z",
    "corroborated_by": ["ev-004", "ev-005"],
    "corroboration_sources": [
        { "source": "LinkedIn profile", "evidence_id": "ev-004" },
        { "source": "GitHub profile", "evidence_id": "ev-005" }
    ]
}
```

### [CONFLICTING] Evidence

Conflicting evidence contradicts other known information and requires resolution.

**Characteristics:**
- Directly contradicts existing data
- Requires investigation
- Reduces confidence in disputed claims
- Must be resolved or documented as contradiction

**Examples:**
- Different registration dates in different WHOIS sources
- Name spelled differently across sources
- Location claims that can't coexist
- Age discrepancies

**Confidence Weight:** 0.0 (flags for review)

```javascript
{
    "id": "ev-006",
    "type": "CONFLICTING",
    "source": "Secondary WHOIS provider",
    "source_reliability": "B",
    "content": "Domain created 2019-12-01 (conflicts with 2020-01-15)",
    "captured_at": "2024-01-15T11:30:00Z",
    "conflicts_with": ["ev-001"],
    "conflict_resolution": "PENDING",
    "notes": "Requires verification against registry directly"
}
```

### [HEARSAY] Evidence

Hearsay evidence is secondhand information from unverified sources.

**Characteristics:**
- Not directly observed
- Passed through intermediaries
- Lower reliability
- Requires verification

**Examples:**
- Information from forum posts
- Claims in blog comments
- Unverified social media mentions
- Third-party reports without primary sources

**Confidence Weight:** 0.3

```javascript
{
    "id": "ev-007",
    "type": "HEARSAY",
    "source": "Forum post on reddit.com/r/example",
    "source_reliability": "E",
    "content": "User claims John Doe works at Example Corp",
    "captured_at": "2024-01-15T12:00:00Z",
    "verification_status": "UNVERIFIED",
    "notes": "Requires confirmation from official source"
}
```

## Source Reliability Scale

| Grade | Description | Examples |
|-------|-------------|----------|
| **A** | Completely Reliable | Official registries, primary sources, direct observation |
| **B** | Usually Reliable | Established news, corporate websites, verified social media |
| **C** | Fairly Reliable | Blogs with track record, industry publications |
| **D** | Not Usually Reliable | Anonymous forums, unverified claims |
| **E** | Unreliable | Known disinformation sources, highly biased sources |
| **F** | Cannot Be Judged | Insufficient information to assess |

## Evidence Strength Matrix

Calculate evidence strength by combining evidence type and source reliability:

```javascript
const evidenceTypeWeights = {
    DIRECT: 1.0,
    INFERRED: 0.6,
    CORROBORATED: 1.2,
    CONFLICTING: 0.0,
    HEARSAY: 0.3
};

const sourceReliabilityWeights = {
    A: 1.0,
    B: 0.8,
    C: 0.6,
    D: 0.4,
    E: 0.2,
    F: 0.5  // Unknown reliability = moderate weight
};

function calculateEvidenceStrength(evidence) {
    const typeWeight = evidenceTypeWeights[evidence.type];
    const reliabilityWeight = sourceReliabilityWeights[evidence.source_reliability];
    
    // Base calculation
    let strength = typeWeight * reliabilityWeight;
    
    // Adjustments
    if (evidence.type === "CORROBORATED" && evidence.corroborated_by) {
        // Bonus for multiple corroborating sources
        const corroborationBonus = Math.min(
            evidence.corroborated_by.length * 0.1, 
            0.3
        );
        strength += corroborationBonus;
    }
    
    if (evidence.content_hash) {
        // Bonus for content integrity verification
        strength += 0.05;
    }
    
    if (evidence.archived_copy) {
        // Bonus for archived snapshot
        strength += 0.05;
    }
    
    return Math.min(strength, 1.5); // Cap at 1.5
}
```

## Evidence Chain Tracking

Document how conclusions are reached through linked evidence:

```javascript
function createEvidenceChain(conclusionEntityId, steps) {
    const chain = {
        id: generateUUID(),
        conclusion_entity_id: conclusionEntityId,
        steps: steps.map((step, index) => ({
            step_number: index + 1,
            entity_id: step.entityId,
            relationship_id: step.relationshipId,
            evidence_id: step.evidenceId,
            reasoning: step.reasoning,
            confidence_at_step: calculateStepConfidence(steps.slice(0, index + 1))
        })),
        overall_confidence: calculateChainConfidence(steps),
        created_at: now()
    };
    
    return chain;
}

function calculateStepConfidence(stepsSoFar) {
    // Confidence compounds with each step
    return stepsSoFar.reduce((acc, step) => {
        const evidence = getEvidence(step.evidenceId);
        const stepStrength = calculateEvidenceStrength(evidence);
        return acc * stepStrength;
    }, 1.0);
}

// Example: Chain showing how we determined a person's employer
const employmentChain = createEvidenceChain("person-001", [
    {
        entityId: "email-001",
        relationshipId: "rel-001",
        evidenceId: "ev-003",
        reasoning: "Email found on LinkedIn profile, corroborated by GitHub"
    },
    {
        entityId: "linkedin-001",
        relationshipId: "rel-002",
        evidenceId: "ev-008",
        reasoning: "LinkedIn profile lists current employment at Example Corp"
    },
    {
        entityId: "employer-001",
        evidenceId: "ev-009",
        reasoning: "Company website lists employee in team directory"
    }
]);
```

## Uncertainty Quantification

Express confidence levels with explicit uncertainty bounds:

```javascript
function quantifyUncertainty(entity) {
    const evidenceStrengths = entity.evidence.map(calculateEvidenceStrength);
    
    if (evidenceStrengths.length === 0) {
        return {
            confidence: "UNCERTAIN",
            confidence_score: 0.0,
            uncertainty_range: [0.0, 0.1],
            basis: "No evidence"
        };
    }
    
    // Strongest single evidence
    const maxStrength = Math.max(...evidenceStrengths);
    
    // Combined strength (with diminishing returns)
    const combinedStrength = evidenceStrengths.reduce((acc, strength) => {
        return acc + (strength * (1 - acc) * 0.5);
    }, 0);
    
    // Calculate uncertainty range
    const variance = calculateVariance(evidenceStrengths);
    const uncertaintyMargin = Math.sqrt(variance);
    
    const confidenceScore = Math.max(maxStrength, combinedStrength);
    const lowerBound = Math.max(0, confidenceScore - uncertaintyMargin);
    const upperBound = Math.min(1.5, confidenceScore + uncertaintyMargin);
    
    // Map to confidence level
    const confidence = scoreToConfidenceLevel(confidenceScore);
    
    return {
        confidence: confidence,
        confidence_score: confidenceScore,
        uncertainty_range: [lowerBound, upperBound],
        evidence_count: evidenceStrengths.length,
        strongest_evidence: maxStrength,
        combined_strength: combinedStrength,
        variance: variance,
        basis: generateConfidenceBasis(entity.evidence)
    };
}

function scoreToConfidenceLevel(score) {
    if (score >= 1.2) return "CERTAIN";
    if (score >= 0.8) return "HIGH";
    if (score >= 0.5) return "MEDIUM";
    if (score >= 0.3) return "LOW";
    return "UNCERTAIN";
}

function generateConfidenceBasis(evidence) {
    const types = [...new Set(evidence.map(e => e.type))];
    const directCount = evidence.filter(e => e.type === "DIRECT").length;
    const corroboratedCount = evidence.filter(e => e.type === "CORROBORATED").length;
    
    let basis = `Based on ${evidence.length} evidence item(s)`;
    if (directCount > 0) basis += `, including ${directCount} direct`;
    if (corroboratedCount > 0) basis += `, ${corroboratedCount} corroborated`;
    
    return basis;
}
```

## Evidence Verification Workflow

```javascript
const verificationWorkflow = {
    // Step 1: Capture evidence
    capture: (source, content, options) => {
        const evidence = {
            id: generateUUID(),
            type: options.type || "DIRECT",
            source: source,
            source_url: options.url,
            source_reliability: assessReliability(source),
            content: content,
            content_hash: hashContent(content),
            captured_at: now(),
            archived_copy: options.archive ? archiveContent(source, content) : null
        };
        
        return evidence;
    },
    
    // Step 2: Assess initial reliability
    assessReliability: (source) => {
        // Check against known reliable sources
        if (isOfficialRegistry(source)) return "A";
        if (isEstablishedPublication(source)) return "B";
        if (isKnownBlog(source)) return "C";
        if (isAnonymousForum(source)) return "D";
        if (isKnownUnreliable(source)) return "E";
        return "F";
    },
    
    // Step 3: Cross-reference with existing evidence
    crossReference: (newEvidence, existingEvidence) => {
        const matches = [];
        const conflicts = [];
        
        existingEvidence.forEach(existing => {
            const similarity = compareEvidence(newEvidence, existing);
            
            if (similarity > 0.9) {
                matches.push({ evidence: existing, similarity });
            } else if (similarity < 0.5 && sameSubject(newEvidence, existing)) {
                conflicts.push({ evidence: existing, similarity });
            }
        });
        
        // If corroborating matches found, upgrade type
        if (matches.length > 0 && newEvidence.type === "DIRECT") {
            newEvidence.type = "CORROBORATED";
            newEvidence.corroborated_by = matches.map(m => m.evidence.id);
        }
        
        // If conflicts found, flag for review
        if (conflicts.length > 0) {
            newEvidence.type = "CONFLICTING";
            newEvidence.conflicts_with = conflicts.map(c => c.evidence.id);
            newEvidence.conflict_resolution = "PENDING";
        }
        
        return { evidence: newEvidence, matches, conflicts };
    },
    
    // Step 4: Calculate final strength
    finalize: (evidence) => {
        evidence.strength = calculateEvidenceStrength(evidence);
        evidence.verified = evidence.strength > 0.7;
        return evidence;
    }
};
```

## Evidence Archiving

Preserve evidence at capture time to prevent loss or tampering:

```javascript
function archiveEvidence(sourceUrl, content) {
    const timestamp = now();
    const sanitizedName = sanitizeFilename(sourceUrl);
    const archivePath = `./archives/${sanitizedName}_${timestamp}.html`;
    
    // If URL, attempt to save full page
    if (sourceUrl.startsWith('http')) {
        const archived = archiveWebPage(sourceUrl, archivePath);
        return archived ? archivePath : null;
    }
    
    // Otherwise, save content to file
    writeFile(archivePath, content);
    return archivePath;
}

function verifyArchiveIntegrity(archivePath, originalHash) {
    const archivedContent = readFile(archivePath);
    const archivedHash = hashContent(archivedContent);
    return archivedHash === originalHash;
}
```

## Command Interface

```javascript
// Add evidence to entity
/add-evidence <entity_id> <type> --source <desc> --content <data>

// Examples:
/add-evidence domain-001 DIRECT \\
    --source "WHOIS lookup" \\
    --content "Registrar: NameCheap" \\
    --url "https://whois.example.com" \\
    --archive

/add-evidence person-001 INFERRED \\
    --source "Email pattern analysis" \\
    --content "john.doe@example.com" \\
    --basis entity-002 \\
    --reasoning "Standard corporate email format"

// Show evidence for entity
/show-evidence <entity_id> [--details]

// Verify evidence integrity
/verify-evidence <evidence_id>

// Archive evidence
/archive-evidence <evidence_id> [url]

// Show evidence chain
/show-chain <entity_id>

// Compare evidence
/compare-evidence <id1> <id2>

// Flag conflicting evidence
/flag-conflict <evidence_id> --conflicts-with <other_id>

// Resolve conflict
/resolve-conflict <evidence_id> --resolution <verdict> [--notes <text>]
```

## Evidence Report Generation

```javascript
function generateEvidenceReport(entity) {
    const report = {
        entity_id: entity.id,
        entity_value: entity.value,
        overall_confidence: quantifyUncertainty(entity),
        evidence_summary: {
            total: entity.evidence.length,
            by_type: countBy(entity.evidence, 'type'),
            by_reliability: countBy(entity.evidence, 'source_reliability')
        },
        evidence_details: entity.evidence.map(ev => ({
            id: ev.id,
            type: ev.type,
            source: ev.source,
            reliability: ev.source_reliability,
            strength: calculateEvidenceStrength(ev),
            captured: ev.captured_at,
            verified: ev.verified || false,
            content_preview: ev.content.substring(0, 100) + "..."
        })),
        chains: getEvidenceChains(entity.id),
        recommendations: generateRecommendations(entity)
    };
    
    return report;
}

function generateRecommendations(entity) {
    const recs = [];
    const analysis = quantifyUncertainty(entity);
    
    if (analysis.confidence_score < 0.5) {
        recs.push("Collect additional direct evidence to improve confidence");
    }
    
    if (!entity.evidence.some(e => e.type === "DIRECT")) {
        recs.push("No direct evidence found - seek primary sources");
    }
    
    if (entity.evidence.some(e => e.type === "CONFLICTING")) {
        recs.push("Resolve conflicting evidence before relying on this entity");
    }
    
    const lowReliabilityCount = entity.evidence.filter(
        e => ['D', 'E', 'F'].includes(e.source_reliability)
    ).length;
    
    if (lowReliabilityCount > entity.evidence.length / 2) {
        recs.push("Majority of sources have low reliability - seek better sources");
    }
    
    return recs;
}
```

## Evidence Strength Visualization

```
Evidence Strength for: john@example.com
═══════════════════════════════════════════════════════════════

Overall Confidence: HIGH (0.85)
Uncertainty Range: [0.72, 0.98]

Evidence Items (4 total):
┌─────────────────────────────────────────────────────────────┐
│ [CORROBORATED] Email on social profiles                    │
│   Source: LinkedIn + GitHub (Reliability: A)               │
│   Strength: ████████████████████░░░░░ 1.25                 │
│   Corroborated by: 2 sources                               │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ [DIRECT] WHOIS record                                      │
│   Source: ICANN WHOIS (Reliability: A)                     │
│   Strength: ████████████████░░░░░░░░░ 1.00                 │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ [DIRECT] Company directory                                 │
│   Source: Example Corp website (Reliability: B)            │
│   Strength: ████████████░░░░░░░░░░░░░ 0.80                 │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ [INFERRED] Pattern match                                   │
│   Source: Email format analysis (Reliability: N/A)         │
│   Strength: ███████░░░░░░░░░░░░░░░░░░ 0.60                 │
│   ⚠️  Inference: Standard corporate format                 │
└─────────────────────────────────────────────────────────────┘

Recommendations:
• Evidence base is strong - confidence is well-supported
• Consider archiving corroborating sources for preservation
```

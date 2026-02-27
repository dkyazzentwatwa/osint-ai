# Entity Manager Specification

## Overview

The Entity Manager provides comprehensive CRUD operations for OSINT investigation entities, with deduplication, cross-referencing, and relationship mapping capabilities.

## Core Operations

### 1. Entity CRUD

#### Create Entity
```javascript
// Command Interface
/add-entity <type> <value> [options]

// Options:
--confidence <level>     # CERTAIN, HIGH, MEDIUM, LOW, UNCERTAIN
--source <description>   # Evidence source
--alias <name>          # Add alias
--attribute <k>=<v>     # Type-specific attribute
--note <text>           # Entity notes

// Examples:
/add-entity EMAIL john@example.com --confidence HIGH --source "WHOIS lookup"
/add-entity PERSON "John Doe" --alias "JD" --attribute dob=1980-01-01
```

**Internal Function:**
```javascript
function createEntity(type, value, options) {
    // 1. Normalize value based on type
    const normalizedValue = normalizeEntityValue(type, value);
    
    // 2. Check for duplicates
    const duplicate = findDuplicate(type, normalizedValue, options.aliases);
    if (duplicate) {
        return mergeOrCreateNew(duplicate, normalizedValue, options);
    }
    
    // 3. Create entity with UUID
    const entity = {
        id: generateUUID(),
        type: type,
        value: normalizedValue,
        display_name: options.display_name || normalizedValue,
        aliases: options.aliases || [],
        attributes: options.attributes || {},
        confidence: options.confidence || "MEDIUM",
        evidence: [createEvidence(options)],
        status: "ACTIVE",
        first_seen: now(),
        last_seen: now(),
        created_at: now(),
        modified_at: now(),
        version: 1,
        source_entities: options.source_entities || [],
        tags: options.tags || []
    };
    
    // 4. Add to investigation
    investigation.entities.push(entity);
    
    // 5. Auto-create relationships if source_entities provided
    if (options.source_entities) {
        options.source_entities.forEach(sourceId => {
            createRelationship(sourceId, entity.id, "REFERENCES", {
                evidence: entity.evidence
            });
        });
    }
    
    // 6. Log audit entry
    auditLog("ENTITY_CREATE", { entity_id: entity.id });
    
    return entity;
}
```

#### Read Entity
```javascript
// Command Interface
/show-entity <id_or_value>
/find-entities [filters]

// Filters:
--type <type>           # Filter by entity type
--confidence <level>    # Filter by confidence
--tag <tag>            # Filter by tag
--status <status>      # Filter by status
--connected-to <id>    # Find entities connected to specific entity
--has-evidence         # Only entities with evidence
--no-evidence          # Only entities without evidence

// Examples:
/show-entity john@example.com
/find-entities --type DOMAIN --confidence HIGH
/find-entities --connected-to <uuid> --type EMAIL
```

**Internal Function:**
```javascript
function getEntity(idOrValue) {
    // Try UUID first
    let entity = investigation.entities.find(e => e.id === idOrValue);
    
    // Try value match
    if (!entity) {
        entity = investigation.entities.find(e => 
            e.value === idOrValue || 
            e.aliases.includes(idOrValue)
        );
    }
    
    // Fuzzy match if still not found
    if (!entity) {
        entity = fuzzyFindEntity(idOrValue);
    }
    
    return entity;
}

function findEntities(filters) {
    return investigation.entities.filter(entity => {
        if (filters.type && entity.type !== filters.type) return false;
        if (filters.confidence && entity.confidence !== filters.confidence) return false;
        if (filters.tag && !entity.tags.includes(filters.tag)) return false;
        if (filters.status && entity.status !== filters.status) return false;
        if (filters.connected_to && !isConnected(entity.id, filters.connected_to)) return false;
        if (filters.has_evidence && entity.evidence.length === 0) return false;
        if (filters.no_evidence && entity.evidence.length > 0) return false;
        return true;
    });
}
```

#### Update Entity
```javascript
// Command Interface
/update-entity <id_or_value> [changes]

// Change Options:
--value <new_value>         # Update primary value
--add-alias <alias>         # Add alias
--remove-alias <alias>      # Remove alias
--attribute <k>=<v>         # Set attribute
--remove-attribute <k>      # Remove attribute
--confidence <level>        # Update confidence
--status <status>           # Update status
--note <text>               # Update notes
--add-tag <tag>             # Add tag
--remove-tag <tag>          # Remove tag

// Examples:
/update-entity john@example.com --confidence CERTAIN --add-alias "john.doe"
/update-entity <uuid> --status VERIFIED
```

**Internal Function:**
```javascript
function updateEntity(idOrValue, changes) {
    const entity = getEntity(idOrValue);
    if (!entity) throw new Error(`Entity not found: ${idOrValue}`);
    
    // Store previous version
    const previousVersion = {
        version: entity.version,
        modified_at: entity.modified_at,
        snapshot: deepClone(entity)
    };
    entity.previous_versions = entity.previous_versions || [];
    entity.previous_versions.push(previousVersion);
    
    // Track changes
    const changeLog = [];
    
    // Apply changes
    if (changes.value) {
        changeLog.push({ field: "value", old: entity.value, new: changes.value });
        entity.value = normalizeEntityValue(entity.type, changes.value);
    }
    
    if (changes.add_alias) {
        if (!entity.aliases.includes(changes.add_alias)) {
            entity.aliases.push(changes.add_alias);
            changeLog.push({ field: "aliases", action: "add", value: changes.add_alias });
        }
    }
    
    if (changes.remove_alias) {
        entity.aliases = entity.aliases.filter(a => a !== changes.remove_alias);
        changeLog.push({ field: "aliases", action: "remove", value: changes.remove_alias });
    }
    
    if (changes.attribute) {
        const [key, value] = changes.attribute.split('=');
        const oldValue = entity.attributes[key];
        entity.attributes[key] = value;
        changeLog.push({ field: `attributes.${key}`, old: oldValue, new: value });
    }
    
    if (changes.confidence) {
        changeLog.push({ field: "confidence", old: entity.confidence, new: changes.confidence });
        entity.confidence = changes.confidence;
    }
    
    if (changes.status) {
        changeLog.push({ field: "status", old: entity.status, new: changes.status });
        entity.status = changes.status;
    }
    
    // Update metadata
    entity.modified_at = now();
    entity.version++;
    
    // Add change entry to previous version
    previousVersion.changes = changeLog;
    
    auditLog("ENTITY_UPDATE", { entity_id: entity.id, changes: changeLog });
    
    return entity;
}
```

#### Delete Entity
```javascript
// Command Interface
/remove-entity <id_or_value> [--cascade]

// Examples:
/remove-entity john@example.com
/remove-entity <uuid> --cascade  # Also remove related relationships
```

**Internal Function:**
```javascript
function deleteEntity(idOrValue, cascade = false) {
    const entity = getEntity(idOrValue);
    if (!entity) throw new Error(`Entity not found: ${idOrValue}`);
    
    // Soft delete - mark as REMOVED
    entity.status = "REMOVED";
    entity.modified_at = now();
    
    // Handle relationships
    if (cascade) {
        // Remove all relationships involving this entity
        investigation.relationships = investigation.relationships.filter(r => {
            if (r.source_id === entity.id || r.target_id === entity.id) {
                auditLog("RELATIONSHIP_DELETE", { relationship_id: r.id, reason: "entity_cascade" });
                return false;
            }
            return true;
        });
    } else {
        // Mark relationships as involving removed entity
        investigation.relationships
            .filter(r => r.source_id === entity.id || r.target_id === entity.id)
            .forEach(r => r.verified = false);
    }
    
    auditLog("ENTITY_DELETE", { entity_id: entity.id, cascade });
    
    return { success: true, entity_id: entity.id };
}
```

### 2. Deduplication Logic

```javascript
function findDuplicate(type, value, aliases = []) {
    const candidates = investigation.entities.filter(e => 
        e.type === type && e.status !== "REMOVED"
    );
    
    for (const candidate of candidates) {
        const matchScore = calculateMatchScore(candidate, type, value, aliases);
        
        if (matchScore >= 0.95) {
            return { entity: candidate, score: matchScore, action: "MERGE" };
        } else if (matchScore >= 0.80) {
            return { entity: candidate, score: matchScore, action: "REVIEW" };
        }
    }
    
    return null;
}

function calculateMatchScore(candidate, type, value, aliases) {
    let score = 0;
    
    // Exact value match
    if (normalizeValue(candidate.value) === normalizeValue(value)) {
        score += 1.0;
    }
    
    // Alias match
    const allAliases = [...candidate.aliases, ...aliases];
    const normalizedAliases = allAliases.map(normalizeValue);
    const normalizedValue = normalizeValue(value);
    
    if (normalizedAliases.includes(normalizedValue)) {
        score += 0.9;
    }
    
    // Type-specific matching
    switch (type) {
        case "EMAIL":
            // Email local part match
            const [local1, domain1] = candidate.value.split('@');
            const [local2, domain2] = value.split('@');
            if (local1 === local2 && domain1 !== domain2) {
                score += 0.5; // Same username, different domain
            }
            break;
            
        case "DOMAIN":
            // Subdomain match
            if (value.endsWith(candidate.value) || candidate.value.endsWith(value)) {
                score += 0.7;
            }
            break;
            
        case "PERSON":
            // Name similarity
            const nameScore = calculateNameSimilarity(candidate.value, value);
            score += nameScore * 0.8;
            break;
            
        case "USERNAME":
            // Case-insensitive match
            if (candidate.value.toLowerCase() === value.toLowerCase()) {
                score += 1.0;
            }
            break;
    }
    
    return Math.min(score, 1.0);
}

function mergeOrCreateNew(duplicateCheck, value, options) {
    const { entity, score, action } = duplicateCheck;
    
    if (action === "MERGE") {
        // Add as alias if different
        if (entity.value !== value && !entity.aliases.includes(value)) {
            entity.aliases.push(value);
            entity.modified_at = now();
            auditLog("ENTITY_MERGE_ALIAS", { entity_id: entity.id, alias: value });
        }
        return entity;
    }
    
    if (action === "REVIEW") {
        // Prompt user or flag for review
        return {
            requires_review: true,
            existing: entity,
            proposed: { value, options },
            match_score: score
        };
    }
    
    return null;
}
```

### 3. Cross-Reference Engine

```javascript
function crossReferenceEntity(entityId) {
    const entity = getEntity(entityId);
    const findings = {
        direct_matches: [],
        potential_matches: [],
        related_entities: [],
        inconsistencies: []
    };
    
    // Check all entities for relationships
    investigation.entities.forEach(other => {
        if (other.id === entity.id) return;
        if (other.status === "REMOVED") return;
        
        // Check for shared attributes
        const sharedAttrs = findSharedAttributes(entity, other);
        if (sharedAttrs.length > 0) {
            findings.related_entities.push({
                entity: other,
                connection_type: "SHARED_ATTRIBUTE",
                details: sharedAttrs
            });
        }
        
        // Check for value similarities
        const similarity = calculateSimilarity(entity.value, other.value);
        if (similarity > 0.8) {
            findings.potential_matches.push({
                entity: other,
                similarity
            });
        }
        
        // Check for contradictions
        const contradictions = findContradictions(entity, other);
        if (contradictions.length > 0) {
            findings.inconsistencies.push(...contradictions);
        }
    });
    
    return findings;
}

function findSharedAttributes(entity1, entity2) {
    const shared = [];
    
    for (const [key, value1] of Object.entries(entity1.attributes)) {
        if (entity2.attributes[key] === value1) {
            shared.push({ attribute: key, value: value1 });
        }
    }
    
    // Check for shared aliases
    const sharedAliases = entity1.aliases.filter(a => 
        entity2.aliases.includes(a) || entity2.value === a
    );
    
    if (sharedAliases.length > 0) {
        shared.push({ attribute: "alias", values: sharedAliases });
    }
    
    return shared;
}

// Command Interface
/xref <id_or_value>
/xref john@example.com --depth 2
```

### 4. Relationship Mapping

```javascript
function createRelationship(sourceId, targetId, type, options = {}) {
    const source = getEntity(sourceId);
    const target = getEntity(targetId);
    
    if (!source || !target) {
        throw new Error("Source or target entity not found");
    }
    
    // Check for existing relationship
    const existing = investigation.relationships.find(r =>
        r.source_id === source.id &&
        r.target_id === target.id &&
        r.type === type
    );
    
    if (existing) {
        // Add evidence to existing relationship
        if (options.evidence) {
            existing.evidence.push(...options.evidence);
            existing.modified_at = now();
        }
        return existing;
    }
    
    const relationship = {
        id: generateUUID(),
        source_id: source.id,
        target_id: target.id,
        type: type,
        direction: options.direction || "DIRECTED",
        confidence: options.confidence || "MEDIUM",
        evidence: options.evidence || [],
        attributes: options.attributes || {},
        created_at: now(),
        modified_at: now(),
        verified: options.verified || false
    };
    
    investigation.relationships.push(relationship);
    auditLog("RELATIONSHIP_CREATE", { relationship_id: relationship.id });
    
    return relationship;
}

function getRelatedEntities(entityId, options = {}) {
    const relationships = investigation.relationships.filter(r =>
        r.source_id === entityId || r.target_id === entityId
    );
    
    return relationships.map(r => {
        const isSource = r.source_id === entityId;
        const otherId = isSource ? r.target_id : r.source_id;
        const other = getEntity(otherId);
        
        return {
            entity: other,
            relationship: r,
            direction: isSource ? "OUTGOING" : "INCOMING",
            connection_type: r.type
        };
    }).filter(r => options.type ? r.relationship.type === options.type : true);
}

// Command Interface
/add-relationship <source> <target> <type> [options]
/show-relationships <id_or_value>
/find-path <from> <to> [--max-hops <n>]

// Examples:
/add-relationship john@example.com example.com REGISTRATION --confidence HIGH
/show-relationships example.com --type HOSTING
/find-path john@example.com 192.168.1.1 --max-hops 3
```

### 5. Entity Queries

```javascript
// Graph traversal query
function findPath(fromId, toId, maxHops = 5) {
    const start = getEntity(fromId);
    const end = getEntity(toId);
    
    if (!start || !end) return null;
    
    // BFS for shortest path
    const queue = [{ entity: start, path: [], visited: new Set() }];
    
    while (queue.length > 0) {
        const { entity, path, visited } = queue.shift();
        
        if (entity.id === end.id) {
            return path;
        }
        
        if (path.length >= maxHops) continue;
        if (visited.has(entity.id)) continue;
        
        visited.add(entity.id);
        
        const related = getRelatedEntities(entity.id);
        for (const rel of related) {
            if (!visited.has(rel.entity.id)) {
                queue.push({
                    entity: rel.entity,
                    path: [...path, { from: entity, relationship: rel.relationship, to: rel.entity }],
                    visited: new Set(visited)
                });
            }
        }
    }
    
    return null; // No path found
}

// Complex query builder
function queryEntities(querySpec) {
    let results = [...investigation.entities];
    
    // Apply filters
    if (querySpec.types) {
        results = results.filter(e => querySpec.types.includes(e.type));
    }
    
    if (querySpec.confidence_min) {
        const levels = ["UNCERTAIN", "LOW", "MEDIUM", "HIGH", "CERTAIN"];
        const minIndex = levels.indexOf(querySpec.confidence_min);
        results = results.filter(e => levels.indexOf(e.confidence) >= minIndex);
    }
    
    if (querySpec.tags) {
        results = results.filter(e => 
            querySpec.tags.every(tag => e.tags.includes(tag))
        );
    }
    
    if (querySpec.attributes) {
        results = results.filter(e => {
            return Object.entries(querySpec.attributes).every(([key, value]) => {
                return e.attributes[key] === value;
            });
        });
    }
    
    if (querySpec.connected_to) {
        results = results.filter(e => isConnected(e.id, querySpec.connected_to));
    }
    
    if (querySpec.has_evidence_type) {
        results = results.filter(e => 
            e.evidence.some(ev => querySpec.has_evidence_type.includes(ev.type))
        );
    }
    
    // Sort
    if (querySpec.sort_by) {
        results.sort((a, b) => {
            const aVal = getNestedValue(a, querySpec.sort_by);
            const bVal = getNestedValue(b, querySpec.sort_by);
            return querySpec.sort_order === "desc" ? bVal - aVal : aVal - bVal;
        });
    }
    
    // Pagination
    if (querySpec.limit) {
        const offset = querySpec.offset || 0;
        results = results.slice(offset, offset + querySpec.limit);
    }
    
    return results;
}

// Command Interface
/query-entities [query_spec]

// Examples:
/query-entities --type DOMAIN --confidence-min HIGH
/query-entities --tag suspicious --has-evidence-type DIRECT
/query-entities --connected-to <uuid> --sort-by modified_at --sort-order desc
```

## Normalization Functions

```javascript
const normalizationRules = {
    EMAIL: (value) => value.toLowerCase().trim(),
    DOMAIN: (value) => value.toLowerCase().trim().replace(/^www\./, ''),
    USERNAME: (value) => value.toLowerCase().trim(),
    IP_ADDRESS: (value) => {
        // Normalize IPv6, validate IPv4
        return value.trim();
    },
    PHONE: (value) => {
        // Remove non-numeric, standardize format
        return value.replace(/\D/g, '');
    },
    PERSON: (value) => {
        // Title case, standardize spacing
        return value.trim().replace(/\s+/g, ' ')
            .split(' ')
            .map(w => w.charAt(0).toUpperCase() + w.slice(1).toLowerCase())
            .join(' ');
    },
    URL: (value) => {
        try {
            const url = new URL(value);
            return url.toString().toLowerCase();
        } catch {
            return value.toLowerCase().trim();
        }
    }
};

function normalizeEntityValue(type, value) {
    const rule = normalizationRules[type];
    return rule ? rule(value) : value.trim();
}
```

## Type-Specific Attributes

### PERSON Attributes
- `full_name`: Complete name
- `dob`: Date of birth (YYYY-MM-DD)
- `nationality`: Country code
- `occupation`: Job title/role
- `location`: Current location
- `gender`: M/F/NB
- `pob`: Place of birth

### DOMAIN Attributes
- `registrar`: Domain registrar
- `creation_date`: Registration date
- `expiration_date`: Expiry date
- `nameservers`: Array of NS records
- `status`: Array of domain status codes
- `dns_records`: Object with record types

### EMAIL Attributes
- `domain`: Extracted domain
- `local_part`: Username portion
- `provider`: Email service provider
- `disposable`: Boolean - is disposable email
- `breached`: Boolean - found in breaches

### IP_ADDRESS Attributes
- `version`: 4 or 6
- `asn`: Autonomous System Number
- `isp`: Internet Service Provider
- `location`: Geo-location object
- `reverse_dns`: PTR record
- `ports`: Array of open ports

### USERNAME Attributes
- `platforms`: Array of platforms where found
- `variations`: Array of similar usernames
- `avatar_url`: Profile image URL
- `bio`: Profile description
- `created_at`: Account creation date

### ORGANIZATION Attributes
- `legal_name`: Registered business name
- `registration_number`: Business ID
- `incorporation_date`: Date founded
- `jurisdiction`: Registration country/state
- `industry`: Business sector
- `employees`: Number of employees
- `revenue`: Annual revenue

### PHONE Attributes
- `country_code`: Country calling code
- `national_format`: Formatted number
- `carrier`: Mobile carrier
- `type`: MOBILE/LANDLINE/VOIP
- `valid`: Boolean - validation status
- `whatsapp`: Boolean - WhatsApp availability

### LOCATION Attributes
- `address`: Street address
- `city`: City name
- `region`: State/Province
- `country`: Country code
- `postal_code`: ZIP/Postal code
- `coordinates`: Lat/lng object
- `timezone`: Time zone

## Output Formats

```javascript
function formatEntityOutput(entity, format = "summary") {
    switch (format) {
        case "summary":
            return `[${entity.type}] ${entity.display_name} (${entity.confidence})`;
            
        case "detailed":
            return `
Entity: ${entity.display_name}
Type: ${entity.type}
ID: ${entity.id}
Value: ${entity.value}
Confidence: ${entity.confidence}
Status: ${entity.status}
Aliases: ${entity.aliases.join(', ') || 'None'}
Attributes:
${formatAttributes(entity.attributes)}
Evidence Count: ${entity.evidence.length}
Created: ${entity.created_at}
Modified: ${entity.modified_at}
Version: ${entity.version}
Notes: ${entity.notes || 'None'}
            `.trim();
            
        case "json":
            return JSON.stringify(entity, null, 2);
            
        case "table":
            // Return table row format
            return {
                Type: entity.type,
                Value: entity.value.substring(0, 50),
                Confidence: entity.confidence,
                Evidence: entity.evidence.length,
                Modified: entity.modified_at
            };
    }
}

function formatRelationshipOutput(rel, includeEntities = false) {
    const source = getEntity(rel.source_id);
    const target = getEntity(rel.target_id);
    
    let output = `${source.display_name} --[${rel.type}]--> ${target.display_name} (${rel.confidence})`;
    
    if (includeEntities) {
        output += `\n  Source: ${formatEntityOutput(source, "summary")}`;
        output += `\n  Target: ${formatEntityOutput(target, "summary")}`;
        output += `\n  Evidence: ${rel.evidence.length} items`;
    }
    
    return output;
}
```

## Command Summary

| Command | Description |
|---------|-------------|
| `/add-entity` | Create new entity |
| `/show-entity` | Display entity details |
| `/update-entity` | Modify existing entity |
| `/remove-entity` | Remove entity (soft delete) |
| `/find-entities` | Search/filter entities |
| `/add-relationship` | Create entity relationship |
| `/show-relationships` | List entity relationships |
| `/find-path` | Find connection path between entities |
| `/xref` | Cross-reference entity |
| `/query-entities` | Advanced entity query |
| `/merge-entities` | Merge duplicate entities |
| `/split-entity` | Split multi-value entity |

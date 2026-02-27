# Session Format Documentation

## Overview

This document defines the JSON structure for saved OSINT investigation sessions. All investigations are stored in this standardized format to ensure compatibility, portability, and future migration capabilities.

## File Format Versions

| Version | Status | Description |
|---------|--------|-------------|
| 1.0.0 | Legacy | Original OSINT skill format |
| 2.0.0 | Current | Extended schema with versioning, evidence chains, and audit logs |

## Top-Level Structure

```json
{
    "version": "2.0.0",
    "investigation_id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Example Investigation",
    "description": "Investigation into suspicious domain activity",
    "classification": "CONFIDENTIAL",
    "created_at": "2024-01-15T10:30:00Z",
    "modified_at": "2024-01-15T14:45:00Z",
    "version": 5,
    "version_history": [...],
    "entities": [...],
    "relationships": [...],
    "evidence_chains": [...],
    "audit_log": [...],
    "session_metadata": {...},
    "tags": ["domain", "phishing"]
}
```

## Field Specifications

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `version` | string | Schema version, must be "2.0.0" |
| `investigation_id` | UUID | Unique investigation identifier |
| `name` | string | Human-readable investigation name (max 256 chars) |
| `created_at` | ISO8601 | Investigation creation timestamp |
| `entities` | array | List of entities in the investigation |
| `relationships` | array | List of relationships between entities |

### Optional Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `description` | string | "" | Investigation summary (max 4000 chars) |
| `classification` | enum | "OPEN" | Security classification level |
| `modified_at` | ISO8601 | created_at | Last modification timestamp |
| `version` | integer | 1 | Investigation revision number |
| `version_history` | array | [] | History of version snapshots |
| `evidence_chains` | array | [] | Evidence reasoning chains |
| `audit_log` | array | [] | Action audit trail |
| `session_metadata` | object | {} | Session tracking data |
| `tags` | array | [] | Investigation tags |

## Entity Structure

```json
{
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "type": "DOMAIN",
    "value": "suspicious-example.com",
    "display_name": "suspicious-example.com",
    "aliases": ["www.suspicious-example.com"],
    "attributes": {
        "registrar": "NameCheap, Inc.",
        "creation_date": "2023-06-15",
        "expiration_date": "2024-06-15",
        "nameservers": ["ns1.example.com", "ns2.example.com"],
        "status": ["clientTransferProhibited"]
    },
    "confidence": "HIGH",
    "evidence": [
        {
            "id": "550e8400-e29b-41d4-a716-446655440002",
            "type": "DIRECT",
            "source": "WHOIS lookup via whois.com",
            "source_url": "https://whois.com/whois/suspicious-example.com",
            "source_reliability": "A",
            "content": "Domain Name: SUSPICIOUS-EXAMPLE.COM...",
            "content_hash": "a3f5c8d2...",
            "captured_at": "2024-01-15T10:35:00Z",
            "corroborated_by": []
        }
    ],
    "source_entities": ["550e8400-e29b-41d4-a716-446655440003"],
    "status": "ACTIVE",
    "notes": "Domain shows suspicious registration patterns",
    "first_seen": "2024-01-15T10:35:00Z",
    "last_seen": "2024-01-15T10:35:00Z",
    "created_at": "2024-01-15T10:35:00Z",
    "modified_at": "2024-01-15T10:35:00Z",
    "version": 1,
    "previous_versions": [],
    "tags": ["suspicious", "phishing"]
}
```

### Entity Field Details

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `id` | Yes | UUID | Unique entity identifier |
| `type` | Yes | enum | Entity type (PERSON, DOMAIN, EMAIL, etc.) |
| `value` | Yes | string | Primary normalized value |
| `display_name` | No | string | Human-readable display name |
| `aliases` | No | array | Alternative identifiers |
| `attributes` | No | object | Type-specific data |
| `confidence` | No | enum | Confidence level (default: MEDIUM) |
| `evidence` | No | array | Supporting evidence items |
| `source_entities` | No | array | UUIDs of parent entities |
| `status` | No | enum | Entity status (default: ACTIVE) |
| `notes` | No | string | Free-form notes |
| `first_seen` | No | ISO8601 | First observation timestamp |
| `last_seen` | No | ISO8601 | Most recent observation |
| `created_at` | Yes | ISO8601 | Creation timestamp |
| `modified_at` | No | ISO8601 | Last modification |
| `version` | No | integer | Revision number (default: 1) |
| `previous_versions` | No | array | Version history |
| `tags` | No | array | Entity tags |

## Relationship Structure

```json
{
    "id": "550e8400-e29b-41d4-a716-446655440004",
    "source_id": "550e8400-e29b-41d4-a716-446655440001",
    "target_id": "550e8400-e29b-41d4-a716-446655440005",
    "type": "REGISTRATION",
    "direction": "DIRECTED",
    "confidence": "HIGH",
    "evidence": [...],
    "attributes": {
        "start_date": "2023-06-15",
        "registrar": "NameCheap"
    },
    "created_at": "2024-01-15T10:35:00Z",
    "modified_at": "2024-01-15T10:35:00Z",
    "verified": true
}
```

### Relationship Field Details

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `id` | Yes | UUID | Unique relationship identifier |
| `source_id` | Yes | UUID | Source entity ID |
| `target_id` | Yes | UUID | Target entity ID |
| `type` | Yes | enum | Relationship type |
| `direction` | No | enum | DIRECTED or BIDIRECTIONAL |
| `confidence` | No | enum | Confidence level |
| `evidence` | No | array | Supporting evidence |
| `attributes` | No | object | Relationship metadata |
| `created_at` | Yes | ISO8601 | Creation timestamp |
| `modified_at` | No | ISO8601 | Last modification |
| `verified` | No | boolean | Manual verification status |

## Evidence Structure

```json
{
    "id": "550e8400-e29b-41d4-a716-446655440002",
    "type": "DIRECT",
    "source": "WHOIS lookup via whois.com",
    "source_url": "https://whois.com/whois/suspicious-example.com",
    "source_reliability": "A",
    "content": "Domain Name: SUSPICIOUS-EXAMPLE.COM...",
    "content_hash": "a3f5c8d2e9b1f4a7c6d8e2b5f9c3a7d1e4b8f2c5a9d3e7b1f4c8a2d6e9b3f5c7a1d4e8",
    "captured_at": "2024-01-15T10:35:00Z",
    "archived_copy": "./archives/suspicious-example.com_whois_20240115.html",
    "corroborated_by": ["550e8400-e29b-41d4-a716-446655440006"]
}
```

### Evidence Field Details

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `id` | Yes | UUID | Unique evidence identifier |
| `type` | Yes | enum | DIRECT, INFERRED, CORROBORATED, CONFLICTING, HEARSAY |
| `source` | Yes | string | Source description |
| `source_url` | No | URL | Source URL if applicable |
| `source_reliability` | No | enum | A-F reliability rating |
| `content` | No | string | Evidence content/excerpt |
| `content_hash` | No | string | SHA-256 hash for integrity |
| `captured_at` | Yes | ISO8601 | Capture timestamp |
| `archived_copy` | No | string | Path to archived snapshot |
| `corroborated_by` | No | array | Corroborating evidence IDs |

## Evidence Chain Structure

```json
{
    "id": "550e8400-e29b-41d4-a716-446655440007",
    "conclusion_entity_id": "550e8400-e29b-41d4-a716-446655440008",
    "steps": [
        {
            "step_number": 1,
            "entity_id": "550e8400-e29b-41d4-a716-446655440001",
            "relationship_id": "550e8400-e29b-41d4-a716-446655440004",
            "reasoning": "Domain registered to this email",
            "evidence_id": "550e8400-e29b-41d4-a716-446655440002"
        },
        {
            "step_number": 2,
            "entity_id": "550e8400-e29b-41d4-a716-446655440005",
            "relationship_id": "550e8400-e29b-41d4-a716-446655440009",
            "reasoning": "Email found on social media profile",
            "evidence_id": "550e8400-e29b-41d4-a716-446655440010"
        }
    ],
    "created_at": "2024-01-15T11:00:00Z"
}
```

## Audit Log Structure

```json
{
    "id": "550e8400-e29b-41d4-a716-446655440011",
    "timestamp": "2024-01-15T10:35:00Z",
    "action": "ENTITY_CREATE",
    "entity_id": "550e8400-e29b-41d4-a716-446655440001",
    "details": {
        "type": "DOMAIN",
        "value": "suspicious-example.com"
    }
}
```

### Audit Action Types

- `ENTITY_CREATE` - New entity added
- `ENTITY_UPDATE` - Entity modified
- `ENTITY_DELETE` - Entity removed
- `RELATIONSHIP_CREATE` - Relationship added
- `RELATIONSHIP_UPDATE` - Relationship modified
- `RELATIONSHIP_DELETE` - Relationship removed
- `EVIDENCE_ADD` - Evidence added to entity
- `SEARCH_EXECUTE` - Search performed
- `SESSION_SAVE` - Investigation saved
- `SESSION_LOAD` - Investigation loaded
- `EXPORT` - Investigation exported
- `IMPORT` - Investigation imported
- `MERGE` - Entities merged

## Session Metadata Structure

```json
{
    "session_metadata": {
        "start_time": "2024-01-15T10:30:00Z",
        "last_activity": "2024-01-15T14:45:00Z",
        "duration_seconds": 15300,
        "search_count": 12,
        "entity_add_count": 8,
        "entity_update_count": 3,
        "relationship_add_count": 7,
        "save_count": 5,
        "load_count": 1,
        "auto_save_enabled": true,
        "last_auto_save": "2024-01-15T14:40:00Z",
        "commands_executed": [
            {
                "command": "/search-domain suspicious-example.com",
                "timestamp": "2024-01-15T10:35:00Z",
                "duration_ms": 2340
            }
        ]
    }
}
```

## Complete Example Session

```json
{
    "version": "2.0.0",
    "investigation_id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Phishing Domain Investigation",
    "description": "Investigation into suspicious-example.com and associated infrastructure",
    "classification": "CONFIDENTIAL",
    "created_at": "2024-01-15T10:30:00Z",
    "modified_at": "2024-01-15T14:45:00Z",
    "version": 5,
    "version_history": [
        {
            "version": 1,
            "name": "Initial Discovery",
            "description": "First domain identified",
            "created_at": "2024-01-15T10:30:00Z",
            "entity_count": 1,
            "relationship_count": 0,
            "checksum": "a1b2c3d4..."
        },
        {
            "version": 2,
            "name": "WHOIS Analysis",
            "description": "Added registrar and contact information",
            "created_at": "2024-01-15T11:00:00Z",
            "entity_count": 3,
            "relationship_count": 2,
            "checksum": "e5f6g7h8..."
        }
    ],
    "entities": [
        {
            "id": "550e8400-e29b-41d4-a716-446655440001",
            "type": "DOMAIN",
            "value": "suspicious-example.com",
            "display_name": "suspicious-example.com",
            "aliases": ["www.suspicious-example.com"],
            "attributes": {
                "registrar": "NameCheap, Inc.",
                "creation_date": "2023-06-15",
                "expiration_date": "2024-06-15",
                "nameservers": ["ns1.example.com", "ns2.example.com"]
            },
            "confidence": "HIGH",
            "evidence": [
                {
                    "id": "550e8400-e29b-41d4-a716-446655440002",
                    "type": "DIRECT",
                    "source": "WHOIS lookup",
                    "source_reliability": "A",
                    "content": "Domain Name: SUSPICIOUS-EXAMPLE.COM...",
                    "captured_at": "2024-01-15T10:35:00Z"
                }
            ],
            "source_entities": [],
            "status": "ACTIVE",
            "notes": "Domain registered recently, short expiration",
            "first_seen": "2024-01-15T10:35:00Z",
            "last_seen": "2024-01-15T10:35:00Z",
            "created_at": "2024-01-15T10:35:00Z",
            "modified_at": "2024-01-15T10:35:00Z",
            "version": 1,
            "previous_versions": [],
            "tags": ["suspicious", "phishing"]
        },
        {
            "id": "550e8400-e29b-41d4-a716-446655440005",
            "type": "EMAIL",
            "value": "admin@suspicious-example.com",
            "display_name": "admin@suspicious-example.com",
            "aliases": [],
            "attributes": {
                "domain": "suspicious-example.com",
                "local_part": "admin"
            },
            "confidence": "MEDIUM",
            "evidence": [
                {
                    "id": "550e8400-e29b-41d4-a716-446655440006",
                    "type": "INFERRED",
                    "source": "Standard admin email pattern",
                    "source_reliability": "C",
                    "content": "Inferred admin email from domain",
                    "captured_at": "2024-01-15T10:40:00Z"
                }
            ],
            "source_entities": ["550e8400-e29b-41d4-a716-446655440001"],
            "status": "ACTIVE",
            "notes": "Inferred email - not confirmed",
            "first_seen": "2024-01-15T10:40:00Z",
            "last_seen": "2024-01-15T10:40:00Z",
            "created_at": "2024-01-15T10:40:00Z",
            "modified_at": "2024-01-15T10:40:00Z",
            "version": 1,
            "previous_versions": [],
            "tags": ["inferred"]
        }
    ],
    "relationships": [
        {
            "id": "550e8400-e29b-41d4-a716-446655440004",
            "source_id": "550e8400-e29b-41d4-a716-446655440001",
            "target_id": "550e8400-e29b-41d4-a716-446655440005",
            "type": "CONTAINS",
            "direction": "DIRECTED",
            "confidence": "HIGH",
            "evidence": [],
            "attributes": {},
            "created_at": "2024-01-15T10:40:00Z",
            "modified_at": "2024-01-15T10:40:00Z",
            "verified": false
        }
    ],
    "evidence_chains": [
        {
            "id": "550e8400-e29b-41d4-a716-446655440007",
            "conclusion_entity_id": "550e8400-e29b-41d4-a716-446655440005",
            "steps": [
                {
                    "step_number": 1,
                    "entity_id": "550e8400-e29b-41d4-a716-446655440001",
                    "relationship_id": "550e8400-e29b-41d4-a716-446655440004",
                    "reasoning": "Domain contains this email address",
                    "evidence_id": "550e8400-e29b-41d4-a716-446655440002"
                }
            ],
            "created_at": "2024-01-15T10:40:00Z"
        }
    ],
    "audit_log": [
        {
            "id": "550e8400-e29b-41d4-a716-446655440011",
            "timestamp": "2024-01-15T10:35:00Z",
            "action": "ENTITY_CREATE",
            "entity_id": "550e8400-e29b-41d4-a716-446655440001",
            "details": {
                "type": "DOMAIN",
                "value": "suspicious-example.com"
            }
        },
        {
            "id": "550e8400-e29b-41d4-a716-446655440012",
            "timestamp": "2024-01-15T10:40:00Z",
            "action": "ENTITY_CREATE",
            "entity_id": "550e8400-e29b-41d4-a716-446655440005",
            "details": {
                "type": "EMAIL",
                "value": "admin@suspicious-example.com"
            }
        },
        {
            "id": "550e8400-e29b-41d4-a716-446655440013",
            "timestamp": "2024-01-15T10:40:00Z",
            "action": "RELATIONSHIP_CREATE",
            "relationship_id": "550e8400-e29b-41d4-a716-446655440004",
            "details": {
                "type": "CONTAINS",
                "source": "suspicious-example.com",
                "target": "admin@suspicious-example.com"
            }
        }
    ],
    "session_metadata": {
        "start_time": "2024-01-15T10:30:00Z",
        "last_activity": "2024-01-15T14:45:00Z",
        "duration_seconds": 15300,
        "search_count": 5,
        "entity_add_count": 2,
        "entity_update_count": 0,
        "relationship_add_count": 1,
        "save_count": 5,
        "load_count": 1,
        "auto_save_enabled": true,
        "last_auto_save": "2024-01-15T14:40:00Z",
        "commands_executed": [
            {
                "command": "/search-domain suspicious-example.com",
                "timestamp": "2024-01-15T10:35:00Z",
                "duration_ms": 2340
            },
            {
                "command": "/add-entity DOMAIN suspicious-example.com",
                "timestamp": "2024-01-15T10:35:00Z",
                "duration_ms": 120
            }
        ]
    },
    "tags": ["phishing", "domain-analysis", "active"]
}
```

## Migration from v1.0.0 to v2.0.0

### Changes from v1.0.0

| Aspect | v1.0.0 | v2.0.0 |
|--------|--------|--------|
| Entity ID | String | UUID v4 |
| Timestamps | Unix epoch | ISO8601 |
| Evidence | Simple string | Structured object |
| Relationships | Implicit | Explicit with types |
| Versioning | None | Built-in |
| Audit Log | None | Comprehensive |
| Evidence Chains | None | Full tracking |
| Session Metadata | None | Complete tracking |

### Migration Script

```javascript
function migrateV1ToV2(v1Data) {
    const v2Data = {
        version: "2.0.0",
        investigation_id: generateUUID(),
        name: v1Data.name || "Migrated Investigation",
        description: v1Data.description || "",
        classification: "OPEN",
        created_at: new Date(v1Data.created_at * 1000).toISOString(),
        modified_at: new Date().toISOString(),
        version: 1,
        version_history: [],
        entities: v1Data.entities.map(migrateEntity),
        relationships: v1Data.relationships ? v1Data.relationships.map(migrateRelationship) : [],
        evidence_chains: [],
        audit_log: [{
            id: generateUUID(),
            timestamp: new Date().toISOString(),
            action: "IMPORT",
            details: { source_version: "1.0.0" }
        }],
        session_metadata: initializeSessionMetadata(),
        tags: v1Data.tags || []
    };
    
    return v2Data;
}

function migrateEntity(v1Entity) {
    return {
        id: v1Entity.id || generateUUID(),
        type: mapEntityType(v1Entity.type),
        value: v1Entity.value,
        display_name: v1Entity.display_name || v1Entity.value,
        aliases: v1Entity.aliases || [],
        attributes: v1Entity.metadata || {},
        confidence: mapConfidence(v1Entity.confidence),
        evidence: v1Entity.sources ? v1Entity.sources.map(migrateSource) : [],
        source_entities: [],
        status: "ACTIVE",
        notes: v1Entity.notes || "",
        first_seen: v1Entity.first_seen ? new Date(v1Entity.first_seen * 1000).toISOString() : null,
        last_seen: v1Entity.last_seen ? new Date(v1Entity.last_seen * 1000).toISOString() : null,
        created_at: new Date().toISOString(),
        modified_at: new Date().toISOString(),
        version: 1,
        previous_versions: [],
        tags: []
    };
}

function migrateSource(v1Source) {
    return {
        id: generateUUID(),
        type: "DIRECT",
        source: v1Source,
        source_reliability: "C",
        captured_at: new Date().toISOString(),
        corroborated_by: []
    };
}

function migrateRelationship(v1Rel) {
    return {
        id: generateUUID(),
        source_id: v1Rel.from,
        target_id: v1Rel.to,
        type: mapRelationshipType(v1Rel.type),
        direction: "DIRECTED",
        confidence: "MEDIUM",
        evidence: [],
        attributes: {},
        created_at: new Date().toISOString(),
        modified_at: new Date().toISOString(),
        verified: false
    };
}
```

## Validation

Use the provided JSON Schema (`entity-schema.json`) to validate session files:

```javascript
const Ajv = require('ajv');
const ajv = new Ajv();
const schema = require('./entity-schema.json');

function validateSession(sessionData) {
    const validate = ajv.compile(schema);
    const valid = validate(sessionData);
    
    if (!valid) {
        return {
            valid: false,
            errors: validate.errors
        };
    }
    
    return { valid: true };
}
```

## File Naming Convention

Saved session files should follow this naming convention:

```
<investigation_id>_<sanitized_name>_<timestamp>[_encrypted].json

Examples:
550e8400-e29b-41d4-a716-446655440000_phishing_domain_investigation_2024-01-15T10-30-00.json
550e8400-e29b-41d4-a716-446655440000_critical_findings_2024-01-15T14-45-00_encrypted.json
```

## Compression

Sessions may be compressed using deflate. When compressed, the structure is:

```json
{
    "compressed": true,
    "algorithm": "deflate",
    "original_schema_version": "2.0.0",
    "data": "<base64-encoded-compressed-data>"
}
```

## Encryption

Encrypted sessions follow this structure:

```json
{
    "encrypted": true,
    "algorithm": "AES-256-GCM",
    "salt": "<base64>",
    "iv": "<base64>",
    "ciphertext": "<base64>",
    "key_derivation": "PBKDF2-SHA256-100000",
    "metadata": {
        "investigation_name": "<encrypted-name-hint>",
        "created_at": "2024-01-15T10:30:00Z"
    }
}
```

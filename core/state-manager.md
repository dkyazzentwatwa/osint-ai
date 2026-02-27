# State Manager Specification

## Overview

The State Manager provides session persistence, auto-save functionality, and investigation versioning for OSINT investigations. All data is stored locally with optional encryption for sensitive investigations.

## Session Storage Location

```
~/.agents/skills/osint-investigator/sessions/
├── investigations/          # Saved investigation files
│   ├── <investigation_id>/
│   │   ├── current.json     # Current state
│   │   ├── versions/        # Version snapshots
│   │   │   ├── v1_2024-01-15T10-30-00.json
│   │   │   ├── v2_2024-01-15T11-45-00.json
│   │   │   └── ...
│   │   ├── auto-saves/      # Auto-save snapshots
│   │   │   └── auto_2024-01-15T12-00-00.json
│   │   └── metadata.json    # Investigation metadata
├── index.json               # Master index of all investigations
└── config.json              # State manager configuration
```

## Configuration

```javascript
// config.json
{
    "version": "2.0.0",
    "auto_save_enabled": true,
    "auto_save_interval_minutes": 5,
    "max_auto_saves": 10,
    "max_versions": 50,
    "default_encryption": false,
    "compression_enabled": true,
    "backup_count": 3
}
```

## Core Functions

### Initialize State Manager

```javascript
function initializeStateManager() {
    const basePath = "~/.agents/skills/osint-investigator/sessions";
    
    // Ensure directory structure exists
    ensureDirectory(`${basePath}/investigations`);
    
    // Load or create config
    const config = loadConfig();
    
    // Load investigation index
    const index = loadIndex();
    
    // Start auto-save timer if enabled
    if (config.auto_save_enabled) {
        startAutoSaveTimer(config.auto_save_interval_minutes);
    }
    
    return { config, index };
}
```

### Save Investigation

```javascript
// Command Interface
/save [name] [--encrypt] [--description <text>]

// Examples:
/save                          # Quick save with current name
/save "Phase 1 Complete"       # Named save point
/save --encrypt                # Save with encryption
/save "Critical Findings" --encrypt --description "Key evidence collected"
```

**Internal Implementation:**

```javascript
function saveInvestigation(options = {}) {
    const investigation = getCurrentInvestigation();
    
    // Generate save metadata
    const saveData = {
        investigation: investigation,
        metadata: {
            save_name: options.name || `Save ${generateTimestamp()}`,
            description: options.description || "",
            saved_at: now(),
            saved_by: getCurrentUser(),
            version: investigation.version,
            encryption: options.encrypt || false,
            checksum: null  // Will be computed
        }
    };
    
    // Calculate checksum before any transformation
    saveData.metadata.checksum = calculateChecksum(saveData.investigation);
    
    // Apply compression if enabled
    let outputData = saveData;
    if (config.compression_enabled) {
        outputData = compressData(saveData);
    }
    
    // Apply encryption if requested
    if (options.encrypt) {
        const password = promptForPassword("Enter encryption password:");
        outputData = encryptData(outputData, password);
    }
    
    // Determine save path
    const basePath = getInvestigationPath(investigation.id);
    const filename = options.name 
        ? `save_${sanitizeFilename(options.name)}_${timestamp()}.json`
        : `current.json`;
    const filepath = `${basePath}/${filename}`;
    
    // Write to file
    writeFile(filepath, JSON.stringify(outputData, null, 2));
    
    // Update index
    updateIndex(investigation.id, {
        name: investigation.name,
        last_saved: now(),
        save_count: increment(),
        encrypted: options.encrypt || false,
        size_bytes: getFileSize(filepath)
    });
    
    // Create version snapshot
    createVersionSnapshot(investigation);
    
    // Clean up old auto-saves
    pruneAutoSaves(investigation.id);
    
    auditLog("SESSION_SAVE", { 
        investigation_id: investigation.id,
        name: options.name,
        encrypted: options.encrypt || false
    });
    
    return {
        success: true,
        filepath: filepath,
        timestamp: now(),
        size: getFileSize(filepath)
    };
}

function createVersionSnapshot(investigation) {
    const versionsDir = `${getInvestigationPath(investigation.id)}/versions`;
    const versionFile = `${versionsDir}/v${investigation.version}_${timestamp()}.json`;
    
    // Save snapshot
    writeFile(versionFile, JSON.stringify(investigation, null, 2));
    
    // Prune old versions
    const versions = listVersions(investigation.id);
    if (versions.length > config.max_versions) {
        const toDelete = versions.slice(0, versions.length - config.max_versions);
        toDelete.forEach(v => deleteFile(v.path));
    }
}
```

### Load Investigation

```javascript
// Command Interface
/load [name_or_id] [--version <n>]

// Examples:
/load                          # List available investigations
/load "Phase 1 Complete"       # Load by name
/load <investigation_id>       # Load by ID
/load "Phase 1 Complete" --version 3  # Load specific version
```

**Internal Implementation:**

```javascript
function loadInvestigation(identifier, options = {}) {
    // Find investigation
    let investigationInfo;
    
    if (isUUID(identifier)) {
        investigationInfo = findById(identifier);
    } else {
        investigationInfo = findByName(identifier);
    }
    
    if (!investigationInfo) {
        throw new Error(`Investigation not found: ${identifier}`);
    }
    
    // Determine file to load
    let filepath;
    if (options.version) {
        filepath = getVersionPath(investigationInfo.id, options.version);
    } else {
        filepath = `${getInvestigationPath(investigationInfo.id)}/current.json`;
    }
    
    // Read file
    let data = JSON.parse(readFile(filepath));
    
    // Decrypt if encrypted
    if (data.metadata && data.metadata.encryption) {
        const password = promptForPassword("Enter decryption password:");
        data = decryptData(data, password);
    }
    
    // Decompress if compressed
    if (data.compressed) {
        data = decompressData(data);
    }
    
    // Verify checksum
    const computedChecksum = calculateChecksum(data.investigation);
    if (computedChecksum !== data.metadata.checksum) {
        throw new Error("Data integrity check failed - file may be corrupted");
    }
    
    // Set as current investigation
    setCurrentInvestigation(data.investigation);
    
    // Update session metadata
    currentInvestigation.session_metadata.last_loaded = now();
    currentInvestigation.session_metadata.load_count++;
    
    auditLog("SESSION_LOAD", { 
        investigation_id: investigationInfo.id,
        version: options.version || "current"
    });
    
    return {
        success: true,
        investigation: data.investigation,
        metadata: data.metadata
    };
}

function listInvestigations() {
    const index = loadIndex();
    
    return index.investigations.map(inv => ({
        id: inv.id,
        name: inv.name,
        created_at: inv.created_at,
        last_saved: inv.last_saved,
        entity_count: inv.entity_count || 0,
        relationship_count: inv.relationship_count || 0,
        encrypted: inv.encrypted || false,
        size_mb: (inv.size_bytes / 1024 / 1024).toFixed(2)
    }));
}
```

### Auto-Save Mechanism

```javascript
function startAutoSaveTimer(intervalMinutes) {
    const intervalMs = intervalMinutes * 60 * 1000;
    
    setInterval(() => {
        autoSave();
    }, intervalMs);
}

function autoSave() {
    const investigation = getCurrentInvestigation();
    
    // Only auto-save if there are changes
    if (!hasUnsavedChanges(investigation)) {
        return { saved: false, reason: "no_changes" };
    }
    
    const autoSaveDir = `${getInvestigationPath(investigation.id)}/auto-saves`;
    const filename = `auto_${timestamp()}.json`;
    const filepath = `${autoSaveDir}/${filename}`;
    
    const saveData = {
        investigation: investigation,
        metadata: {
            auto_save: true,
            saved_at: now(),
            checksum: calculateChecksum(investigation)
        }
    };
    
    writeFile(filepath, JSON.stringify(saveData, null, 2));
    
    // Update investigation metadata
    investigation.session_metadata.last_auto_save = now();
    
    // Notify (if UI available)
    notify("Auto-saved investigation");
    
    auditLog("AUTO_SAVE", { investigation_id: investigation.id });
    
    return { saved: true, filepath };
}

function pruneAutoSaves(investigationId) {
    const autoSaveDir = `${getInvestigationPath(investigationId)}/auto-saves`;
    const autoSaves = listFiles(autoSaveDir)
        .map(f => ({ ...f, time: parseTimestamp(f.name) }))
        .sort((a, b) => b.time - a.time);
    
    // Keep only the most recent N auto-saves
    const toDelete = autoSaves.slice(config.max_auto_saves);
    toDelete.forEach(f => deleteFile(f.path));
    
    return { kept: config.max_auto_saves, deleted: toDelete.length };
}

function hasUnsavedChanges(investigation) {
    const currentChecksum = calculateChecksum(investigation);
    const lastSavePath = `${getInvestigationPath(investigation.id)}/current.json`;
    
    if (!fileExists(lastSavePath)) {
        return true;
    }
    
    const lastSave = JSON.parse(readFile(lastSavePath));
    return currentChecksum !== lastSave.metadata.checksum;
}
```

### Session Metadata

```javascript
function initializeSessionMetadata() {
    return {
        start_time: now(),
        last_activity: now(),
        duration_seconds: 0,
        search_count: 0,
        entity_add_count: 0,
        entity_update_count: 0,
        relationship_add_count: 0,
        save_count: 0,
        load_count: 0,
        auto_save_enabled: config.auto_save_enabled,
        last_auto_save: null,
        commands_executed: []
    };
}

function updateSessionMetadata(action) {
    const metadata = currentInvestigation.session_metadata;
    
    metadata.last_activity = now();
    metadata.duration_seconds = calculateDuration(metadata.start_time, now());
    
    switch (action.type) {
        case "SEARCH":
            metadata.search_count++;
            break;
        case "ENTITY_CREATE":
            metadata.entity_add_count++;
            break;
        case "ENTITY_UPDATE":
            metadata.entity_update_count++;
            break;
        case "RELATIONSHIP_CREATE":
            metadata.relationship_add_count++;
            break;
        case "SAVE":
            metadata.save_count++;
            break;
        case "LOAD":
            metadata.load_count++;
            break;
    }
    
    metadata.commands_executed.push({
        command: action.command,
        timestamp: now(),
        duration_ms: action.duration
    });
}

function getSessionStats() {
    const m = currentInvestigation.session_metadata;
    const inv = currentInvestigation;
    
    return {
        session_duration: formatDuration(m.duration_seconds),
        entities: {
            total: inv.entities.length,
            by_type: countBy(inv.entities, 'type')
        },
        relationships: {
            total: inv.relationships.length,
            by_type: countBy(inv.relationships, 'type')
        },
        activity: {
            searches: m.search_count,
            entities_added: m.entity_add_count,
            entities_updated: m.entity_update_count,
            relationships_added: m.relationship_add_count
        },
        persistence: {
            saves: m.save_count,
            loads: m.load_count,
            last_auto_save: m.last_auto_save
        }
    };
}

// Command Interface
/session-stats
```

### Investigation Versioning

```javascript
function createSavePoint(name, description = "") {
    const investigation = getCurrentInvestigation();
    
    // Increment version
    investigation.version = (investigation.version || 1) + 1;
    
    // Create snapshot
    const snapshot = {
        version: investigation.version,
        name: name,
        description: description,
        created_at: now(),
        entity_count: investigation.entities.length,
        relationship_count: investigation.relationships.length,
        checksum: calculateChecksum(investigation)
    };
    
    // Save version
    const versionsDir = `${getInvestigationPath(investigation.id)}/versions`;
    const filename = `v${snapshot.version}_${timestamp()}.json`;
    writeFile(`${versionsDir}/${filename}`, JSON.stringify(investigation, null, 2));
    
    // Add to version history
    investigation.version_history = investigation.version_history || [];
    investigation.version_history.push(snapshot);
    
    auditLog("VERSION_CREATE", { 
        investigation_id: investigation.id,
        version: snapshot.version,
        name: name
    });
    
    return snapshot;
}

function listVersions(investigationId) {
    const versionsDir = `${getInvestigationPath(investigationId)}/versions`;
    const files = listFiles(versionsDir);
    
    return files.map(f => {
        const match = f.name.match(/v(\d+)_(.+?)\.json/);
        if (match) {
            return {
                version: parseInt(match[1]),
                timestamp: parseTimestamp(match[2]),
                path: f.path,
                size: f.size
            };
        }
    }).filter(Boolean).sort((a, b) => a.version - b.version);
}

function restoreVersion(investigationId, version) {
    const versionPath = getVersionPath(investigationId, version);
    
    if (!fileExists(versionPath)) {
        throw new Error(`Version ${version} not found`);
    }
    
    const data = JSON.parse(readFile(versionPath));
    
    // Verify checksum
    const computedChecksum = calculateChecksum(data);
    // ... validation
    
    // Set as current
    setCurrentInvestigation(data);
    
    auditLog("VERSION_RESTORE", { investigation_id: investigationId, version });
    
    return { success: true, version };
}

// Command Interface
/save-point <name> [--description <text>]
/list-versions
/restore-version <number>
/delete-version <number>
```

### Session Comparison

```javascript
function compareSessions(save1, save2) {
    const inv1 = loadForComparison(save1);
    const inv2 = loadForComparison(save2);
    
    const comparison = {
        metadata: {
            save1: { name: inv1.metadata.save_name, timestamp: inv1.metadata.saved_at },
            save2: { name: inv2.metadata.save_name, timestamp: inv2.metadata.saved_at }
        },
        entities: {
            added: [],
            removed: [],
            modified: [],
            unchanged: []
        },
        relationships: {
            added: [],
            removed: [],
            modified: []
        },
        statistics: {
            entity_change_count: 0,
            relationship_change_count: 0
        }
    };
    
    // Compare entities
    const entityMap1 = new Map(inv1.investigation.entities.map(e => [e.id, e]));
    const entityMap2 = new Map(inv2.investigation.entities.map(e => [e.id, e]));
    
    // Find added entities
    entityMap2.forEach((entity, id) => {
        if (!entityMap1.has(id)) {
            comparison.entities.added.push(entity);
        }
    });
    
    // Find removed entities
    entityMap1.forEach((entity, id) => {
        if (!entityMap2.has(id)) {
            comparison.entities.removed.push(entity);
        }
    });
    
    // Find modified entities
    entityMap1.forEach((entity1, id) => {
        const entity2 = entityMap2.get(id);
        if (entity2) {
            const changes = diffEntities(entity1, entity2);
            if (changes.length > 0) {
                comparison.entities.modified.push({
                    entity: entity2,
                    changes: changes
                });
            } else {
                comparison.entities.unchanged.push(entity1);
            }
        }
    });
    
    // Similar comparison for relationships...
    
    comparison.statistics.entity_change_count = 
        comparison.entities.added.length + 
        comparison.entities.removed.length + 
        comparison.entities.modified.length;
    
    return comparison;
}

function diffEntities(e1, e2) {
    const changes = [];
    const fields = ['value', 'display_name', 'confidence', 'status', 'aliases', 'attributes', 'notes'];
    
    fields.forEach(field => {
        const v1 = JSON.stringify(e1[field]);
        const v2 = JSON.stringify(e2[field]);
        if (v1 !== v2) {
            changes.push({
                field: field,
                old_value: e1[field],
                new_value: e2[field]
            });
        }
    });
    
    return changes;
}

// Command Interface
/compare [save1] [save2]

// Examples:
/compare                       # Compare current to last save
/compare "Phase 1" "Phase 2"   # Compare two named saves
/compare --current <save>      # Compare current to specific save
```

### Encryption System

```javascript
function encryptData(data, password) {
    // Derive key from password using PBKDF2
    const salt = generateRandomBytes(32);
    const key = deriveKey(password, salt, 100000);
    
    // Generate IV
    const iv = generateRandomBytes(16);
    
    // Encrypt
    const plaintext = JSON.stringify(data);
    const ciphertext = aesGcmEncrypt(plaintext, key, iv);
    
    // Return encrypted payload
    return {
        encrypted: true,
        algorithm: "AES-256-GCM",
        salt: base64Encode(salt),
        iv: base64Encode(iv),
        ciphertext: base64Encode(ciphertext),
        key_derivation: "PBKDF2-SHA256-100000"
    };
}

function decryptData(encryptedData, password) {
    if (!encryptedData.encrypted) {
        return encryptedData;
    }
    
    // Derive key
    const salt = base64Decode(encryptedData.salt);
    const key = deriveKey(password, salt, 100000);
    
    // Decrypt
    const iv = base64Decode(encryptedData.iv);
    const ciphertext = base64Decode(encryptedData.ciphertext);
    const plaintext = aesGcmDecrypt(ciphertext, key, iv);
    
    return JSON.parse(plaintext);
}
```

## Utility Functions

```javascript
function calculateChecksum(data) {
    const normalized = JSON.stringify(data, Object.keys(data).sort());
    return sha256(normalized);
}

function compressData(data) {
    return {
        compressed: true,
        algorithm: "deflate",
        data: deflate(JSON.stringify(data))
    };
}

function decompressData(data) {
    if (!data.compressed) return data;
    return JSON.parse(inflate(data.data));
}

function sanitizeFilename(name) {
    return name.replace(/[^a-zA-Z0-9_-]/g, '_').substring(0, 100);
}

function generateTimestamp() {
    return new Date().toISOString().replace(/[:.]/g, '-');
}

function formatDuration(seconds) {
    const hours = Math.floor(seconds / 3600);
    const minutes = Math.floor((seconds % 3600) / 60);
    const secs = seconds % 60;
    
    if (hours > 0) {
        return `${hours}h ${minutes}m ${secs}s`;
    } else if (minutes > 0) {
        return `${minutes}m ${secs}s`;
    }
    return `${secs}s`;
}
```

## Command Summary

| Command | Description |
|---------|-------------|
| `/save` | Save current investigation |
| `/load` | Load saved investigation |
| `/list-saves` | List all saved investigations |
| `/save-point` | Create named save point/version |
| `/list-versions` | List investigation versions |
| `/restore-version` | Restore to specific version |
| `/compare` | Compare two sessions |
| `/session-stats` | Show session statistics |
| `/auto-save` | Toggle auto-save on/off |
| `/export` | Export investigation to file |
| `/import` | Import investigation from file |
| `/backup` | Create backup copy |
| `/cleanup` | Remove old auto-saves |

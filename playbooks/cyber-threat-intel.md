# Cyber Threat Intelligence Playbook

## Overview

This playbook provides security analysts with systematic procedures for investigating threat actors, analyzing indicators of compromise (IOCs), and producing actionable intelligence for defensive operations.

---

## Threat Actor Profiling

### Actor Attribution Framework

**Attribution Levels:**

| Level | Confidence | Evidence Required |
|-------|------------|-------------------|
| **Strategic** | 90%+ | Multiple independent sources, technical artifacts, geopolitical alignment |
| **Tactical** | 70-89% | Strong technical indicators, TTP matches, infrastructure overlap |
| **Suspected** | 50-69% | Some technical indicators, behavioral similarities |
| **Unattributed** | <50% | Insufficient evidence for attribution |

### Actor Profile Components

**Identity & Attribution:**
- Known aliases/handles
- Associated groups/affiliations
- Nation-state attribution (if applicable)
- Financial motivation indicators
- Hacktivist ideology markers

**Technical Capabilities:**
- Malware arsenal
- Infrastructure preferences
- Exploit usage patterns
- Tool development sophistication
- Operational security level

**TTPs (Tactics, Techniques, Procedures):**
- Initial access methods
- Persistence mechanisms
- Lateral movement patterns
- Data staging techniques
- Exfiltration methods

**Target Preferences:**
- Industry verticals
- Geographic focus
- Organization size/type
- Asset types of interest
- Campaign timing patterns

### Actor Research Process

```
PHASE 1: Initial Identification
├── Identify incident/IOCs
├── Search threat intel databases
├── Review industry reports
└── Check government advisories

PHASE 2: Technical Analysis
├── Reverse engineer malware samples
├── Analyze C2 infrastructure
├── Map TTPs to MITRE ATT&CK
└── Correlate with known actors

PHASE 3: Infrastructure Mapping
├── Enumerate related domains/IPs
├── Identify hosting patterns
├── Track registration patterns
└── Map to bulletproof hosters

PHASE 4: Attribution Assessment
├── Compare with known actor profiles
├── Assess confidence level
├── Identify knowledge gaps
└── Document uncertainty

PHASE 5: Profile Documentation
├── Create actor profile card
├── Update TTP mappings
├── Share with CTI community
└── Set monitoring alerts
```

---

## IOC Identification

### IOC Types & Collection

**Network Indicators:**
```
IPv4 Addresses:
- Known malicious IPs
- C2 servers
- Scanning sources
- Phishing infrastructure

Domains:
- Malicious domains
- DGA domains
- Typosquats
- Compromised legitimate sites

URLs:
- Malware download URLs
- Phishing pages
- Exploit kit gates
- C2 channels
```

**Host Indicators:**
```
File Hashes:
- MD5 (legacy compatibility)
- SHA1 (transitional)
- SHA256 (preferred)
- SSDEEP (fuzzy matching)

Filenames:
- Known malware names
- System file impersonations
- Random/generated names
- Legitimate file abuse

Registry Keys:
- Persistence locations
- Configuration storage
- Anti-analysis checks
- System modifications
```

**Behavioral Indicators:**
```
Process Activity:
- Injection techniques
- Process hollowing
- Process doppelgänging
- Parent-child relationships

Network Behavior:
- Beaconing patterns
- Data exfiltration signatures
- DGA traffic
- Protocol abuse
```

### IOC Analysis Workflow

**Step 1: Initial Assessment**
- Verify IOC validity (false positive check)
- Determine IOC type and context
- Assess reliability of source
- Check for known good hash/domain lists

**Step 2: Enrichment**
```
FOR each IOC:
  - Query threat intel platforms
  - Check reputation databases
  - Analyze historical activity
  - Identify related IOCs
  - Determine first/last seen
  - Map to campaigns/actors
END
```

**Step 3: Contextualization**
- What attack stage does this indicate?
- Which TTPs from MITRE ATT&CK?
- What defensive actions are appropriate?
- What is confidence level?

**Step 4: Operationalization**
- Convert to detection rules
- Create hunting queries
- Generate block lists
- Update threat feeds

### IOC Quality Scoring

| Factor | Weight | Assessment |
|--------|--------|------------|
| Source Reliability | 25% | Known good source? History of accuracy? |
| False Positive Rate | 25% | Known FP issues? Context-dependent? |
| Age/Recency | 20% | Still active? Historical only? |
| Specificity | 15% | Specific to threat or too broad? |
| Actionability | 15% | Can we detect/block on this? |

**Quality Rating:**
- **High (90-100):** Block/detect immediately
- **Medium (70-89):** Monitor and investigate
- **Low (50-69):** Context only, not actionable
- **Unusable (<50):** Discard or archive

---

## Attack Pattern Analysis

### Campaign Tracking

**Campaign Attributes:**
```
IDENTIFICATION:
- Campaign name/identifier
- First observed date
- Attribution (if any)
- Associated actors

TARGETING:
- Target industries
- Target geographies
- Target organization size
- Delivery vectors used

TIMELINE:
- Campaign duration
- Active periods
- Major milestones
- Associated incidents
```

### Kill Chain Mapping

**Map to Lockheed Martin Cyber Kill Chain:**

1. **Reconnaissance**
   - What was observed?
   - OSINT indicators?
   - Target research evidence?

2. **Weaponization**
   - Malware family used
   - Exploit/CVE leveraged
   - Delivery document type

3. **Delivery**
   - Phishing email analysis
   - Watering hole details
   - Supply chain vector
   - Direct access method

4. **Exploitation**
   - Vulnerability exploited
   - Exploit kit used
   - Zero-day or known?

5. **Installation**
   - Malware installed
   - Persistence mechanism
   - Privilege escalation

6. **Command & Control**
   - C2 infrastructure
   - Communication protocol
   - DGA details

7. **Actions on Objectives**
   - Data exfiltrated
   - Ransomware deployed
   - Destructive actions
   - Long-term access maintained

### TTP Mapping to MITRE ATT&CK

**Technique Identification:**
```
FOR each observed behavior:
  - Match to MITRE technique ID
  - Identify sub-technique (if applicable)
  - Note data source for detection
  - Map to defensive controls
END
```

**Example Mapping:**
| Observed Behavior | ATT&CK ID | Technique Name | Detection Method |
|-------------------|-----------|----------------|------------------|
| PowerShell download cradle | T1059.001 | PowerShell | Script block logging |
| Registry run key persistence | T1547.001 | Registry Run Keys | Registry monitoring |
| LSASS memory access | T1003.001 | LSASS Memory | Credential dumping detection |

---

## Attribution Methodology

### Technical Indicators

**Code Analysis:**
- Compiler fingerprints
- PDB path strings
- Code signing certificates
- Coding style/features
- Shared libraries/frameworks

**Infrastructure Analysis:**
- IP address ranges
- Domain registration patterns
- SSL certificate details
- Hosting provider preferences
- Bulletproof hosting usage

**TTP Analysis:**
- Attack sequencing
- Tool preferences
- Operational hours (timezone analysis)
- Error patterns
- Anti-analysis techniques

### Contextual Indicators

**Target Selection:**
- Industry alignment
- Geopolitical relevance
- Timing relative to events
- Victim significance

**Campaign Objectives:**
- Espionage indicators
- Financial motivation
- Destructive intent
- Hacktivist messaging

### Confidence Assessment

**Scoring Criteria:**

| Factor | Strong Attribution | Weak Attribution |
|--------|-------------------|------------------|
| Technical overlap | Multiple unique indicators | Generic/common techniques |
| Infrastructure | Dedicated/dedicated | Shared/compromised |
| TTPs | Distinctive patterns | Commodity techniques |
| Targeting | Consistent with actor | Opportunistic |
| Timing | Geopolitical alignment | Random distribution |

**Documentation Requirements:**
- List all supporting evidence
- Note contradictory indicators
- Identify intelligence gaps
- State confidence level clearly
- Provide alternative hypotheses

---

## Intelligence Reporting

### Report Types

**1. Tactical Intelligence (Immediate)**
- IOC lists
- Detection rules
- Mitigation steps
- Time-sensitive updates

**2. Operational Intelligence (Near-term)**
- Campaign analysis
- TTP updates
- Infrastructure changes
- Targeting shifts

**3. Strategic Intelligence (Long-term)**
- Actor profiles
- Trend analysis
- Capability assessments
- Attribution reports

### Intelligence Requirements (IRs)

**Developing IRs:**
```
1. PRIORITY
   - Critical: Immediate threat to operations
   - High: Significant threat potential
   - Medium: Moderate concern
   - Low: Background monitoring

2. QUESTION
   - Specific, answerable question
   - Time-bound
   - Measurable

3. INFORMATION NEEDS
   - What data sources?
 - What analysis required?
   - Who are consumers?

4. SUCCESS CRITERIA
   - What constitutes an answer?
   - Threshold for action?
```

**Example IRs:**
- "Which APT groups are targeting our industry in the next 6 months?"
- "What TTPs are ransomware groups using for initial access?"
- "Which vulnerabilities are being actively exploited in the wild?"

### Report Structure

**Executive Summary:**
- Key findings (3-5 bullets)
- Threat level assessment
- Recommended actions
- Confidence statement

**Technical Details:**
- IOCs (structured format)
- TTP descriptions
- Infrastructure details
- Malware analysis summary

**Analysis:**
- Attribution assessment
- Campaign context
- Trend implications
- Predictive assessment

**Appendices:**
- Full IOC lists
- Detection rules
- MITRE mappings
- Source documentation

### Confidence & Bias Statements

**Required Disclaimers:**
```
ATTRIBUTION CONFIDENCE: [High/Medium/Low]
- Supporting evidence: [Summary]
- Alternative hypotheses: [If any]
- Intelligence gaps: [Unknowns]

SOURCES:
- Primary sources: [List]
- Reliability assessment: [Evaluation]
- Potential bias: [Disclosure]

LIMITATIONS:
- Data gaps
- Analytical uncertainties
- Collection limitations
```

---

## Sharing Guidelines

### Classification Levels

**Public/White:**
- IOCs for broad protection
- General TTP descriptions
- Unclassified threat bulletins
- Vendor-agnostic guidance

**Confidential/Green:**
- Customer-specific IOCs
- Proprietary detection methods
- Attribution assessments
- Actor profiles

**Restricted/Amber:**
- Law enforcement sensitive
- Active investigation details
- Source protection required
- Victim-identifying information

**Secret/Red:**
- Sources and methods
- Active operations
- National security information
- Human intelligence details

### Sharing Communities

**ISACs (Information Sharing and Analysis Centers):**
- FS-ISAC (financial services)
- H-ISAC (healthcare)
- MS-ISAC (state/local government)
- Sector-specific communities

**Sharing Platforms:**
- MISP (Malware Information Sharing Platform)
- TAXII (Threat intelligence exchange)
- STIX (Structured Threat Information Expression)
- Commercial TIP platforms

**Trust Groups:**
- Industry-specific circles
- Regional sharing groups
- Government partnerships
- Vendor intelligence networks

### Responsible Disclosure

**Principles:**
1. Share what protects, not what harms
2. Protect sources and methods
3. Consider operational security
4. Respect legal boundaries
5. Enable defensive action

**Check Before Sharing:**
- [ ] Does this enable defense?
- [ ] Does this reveal sources?
- [ ] Is this properly classified?
- [ ] Are victims protected?
- [ ] Is attribution certain enough?

---

## Tools & Resources

### Threat Intelligence Platforms

**Commercial:**
- Mandiant Advantage
- Recorded Future
- ThreatConnect
- Anomali
- Microsoft Defender TI

**Open Source:**
- MISP
- OpenCTI
- Yeti
- MineMeld (legacy)

### Reputation Databases

- VirusTotal
- AbuseIPDB
- AlienVault OTX
- URLhaus
- MalwareBazaar

### Analysis Tools

**Malware Analysis:**
- Cuckoo Sandbox
- Any.Run
- Joe Sandbox
- Hybrid Analysis

**Infrastructure:**
- PassiveTotal/RiskIQ
- Shodan
- Censys
- SecurityTrails

**Investigation:**
- Maltego
- SpiderFoot
- theHarvester
- OSINT Framework

### Frameworks & Standards

- MITRE ATT&CK
- STIX/TAXII
- Cyber Kill Chain
- Diamond Model of Intrusion Analysis
- VERIS Framework

---

## Best Practices

1. **Source Evaluation** - Always assess source reliability
2. **Confidence Calibration** - Avoid overstating certainty
3. **Bias Awareness** - Recognize analyst cognitive biases
4. **Collaboration** - Share with trusted partners
5. **Continuous Learning** - Update TTPs and IOCs regularly
6. **Operational Security** - Protect your investigation methods
7. **Legal Compliance** - Follow applicable laws and regulations
8. **Ethical Boundaries** - Intelligence for defense, not offense

# Social Graph Module

> **Module ID:** SOCIAL-GRAPH-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Relationship Mapping

---

## 1. Overview

Social graph mapping creates visual representations of relationships and interactions between entities. This module covers graph construction, connection analysis, and visualization techniques.

---

## 2. Graph Construction Methodology

### 2.1 Entity Definition

**Primary Entities (Nodes):**
```
Persons:
- Real names
- Usernames/handles
- Email addresses
- Phone numbers

Organizations:
- Companies
- Groups
- Institutions
- Events

Content:
- Posts
- Documents
- Locations
- Topics
```

**Entity Attributes:**
```json
{
  "id": "unique_identifier",
  "type": "person|organization|content",
  "label": "Display name",
  "properties": {
    "platform": "twitter|linkedin|facebook",
    "verified": true|false,
    "created_date": "2024-01-15",
    "followers": 1500,
    "location": "City, Country"
  }
}
```

### 2.2 Relationship Definition

**Relationship Types (Edges):**
```
Social:
- follows
- friends
- blocks
- mentions

Professional:
- works_with
- reports_to
- employed_by
- collaborated

Content:
- authored
- commented_on
- shared
- liked

Personal:
- related_to
- married_to
- knows
- located_in
```

### 2.3 Graph Building Workflow

```
Step 1: Scope Definition
  └─→ Define investigation boundaries
  └─→ Identify seed entity(ies)
  └─→ Set depth limit (how many hops)

Step 2: Data Collection
  └─→ Extract entities from sources
  └─→ Identify relationships
  └─→ Record interaction data

Step 3: Data Normalization
  └─→ Deduplicate entities
  └─→ Resolve aliases
  └─→ Standardize formats

Step 4: Graph Construction
  └─→ Create nodes for entities
  └─→ Create edges for relationships
  └─→ Assign weights and attributes

Step 5: Validation
  └─→ Check for missing connections
  └─→ Verify entity resolution
  └─→ Assess coverage completeness
```

### 2.4 Data Sources by Platform

**Twitter/X:**
```
Nodes: Users
Edges: follows, mentions, replies, retweets
Data: Profile info, tweets, engagement metrics
```

**LinkedIn:**
```
Nodes: People, Companies, Schools
Edges: connections, employed_by, studied_at, endorsed
Data: Job history, education, skills
```

**Facebook:**
```
Nodes: Profiles, Pages, Groups
Edges: friends, member_of, likes, tags
Data: Limited by privacy settings
```

**Email:**
```
Nodes: Email addresses
Edges: sent_to, cc, bcc, replied_to
Data: Headers, threads, frequency
```

---

## 3. Connection Strength Scoring

### 3.1 Scoring Factors

**Frequency:**
```
Score = log(interaction_count + 1)

1 interaction    → 0.3
10 interactions  → 1.0
100 interactions → 2.0
1000 interactions→ 3.0
```

**Recency:**
```
Time decay factor:
  Weight = 1 / (days_since_interaction + 1)^0.5

Recent: 1 day    → 0.71
Medium: 30 days  → 0.18
Old: 365 days    → 0.05
```

**Interaction Type Weights:**
```
Direct message:    1.0 (strongest)
In-person mention: 0.9
Comment/Reply:     0.7
Share/Retweet:     0.5
Like/Favorite:     0.3
Follow/Connect:    0.2
View:              0.1 (weakest)
```

### 3.2 Composite Scoring Formula

```
Connection Strength = Σ (interaction_weight × recency_factor)

Normalized to 0-1 scale:
  0.0-0.2: Weak connection
  0.2-0.4: Casual acquaintance
  0.4-0.6: Regular contact
  0.6-0.8: Close relationship
  0.8-1.0: Very close/constant contact
```

### 3.3 Scoring Examples

**Example 1: Colleagues:**
```
Interactions:
- 50 work emails (weight: 0.8 each)
- 20 LinkedIn messages (weight: 1.0 each)
- All within 30 days (recency: 0.18)

Calculation:
  (50 × 0.8 + 20 × 1.0) × 0.18 = 10.8
  Normalized: 0.72 (Close relationship)
```

**Example 2: Distant Contact:**
```
Interactions:
- 3 LinkedIn endorsements (weight: 0.3 each)
- 1 comment 6 months ago (weight: 0.7, recency: 0.08)

Calculation:
  (3 × 0.3 × 0.04) + (1 × 0.7 × 0.08) = 0.092
  Normalized: 0.09 (Weak connection)
```

---

## 4. Path Finding Between Entities

### 4.1 Shortest Path Algorithms

**Breadth-First Search (BFS):**
```
Use case: Unweighted graphs
Guarantee: Finds shortest path in terms of hops
Process: Explore all neighbors at current depth before moving deeper
```

**Dijkstra's Algorithm:**
```
Use case: Weighted graphs with positive weights
Guarantee: Finds path with minimum total weight
Process: Greedy expansion of lowest-cost path
```

**A* Search:**
```
Use case: Large graphs with heuristic available
Advantage: Faster than Dijkstra with good heuristic
Heuristic: Estimated distance to target (e.g., geographic distance)
```

### 4.2 Path Interpretation

**Path Length:**
```
Direct connection: 1 hop
Friend of friend: 2 hops
Distant acquaintance: 3+ hops

Small world principle: Most people connected within 6 hops
```

**Path Strength:**
```
Path strength = minimum edge weight along path

Strong path: All connections are strong
Weak path: At least one weak connection
Broken path: Connection no longer exists
```

**Multiple Paths:**
```
More paths = stronger relationship
Diverse paths = resilient connection
Shared paths = common intermediaries
```

### 4.3 Example Path Analysis

```
Target Path: Alice → Bob

Path 1: Alice → Carol → Bob
  - Alice-Carol: 0.9 (strong)
  - Carol-Bob: 0.8 (strong)
  - Path strength: min(0.9, 0.8) = 0.8
  - Total hops: 2

Path 2: Alice → Dave → Eve → Bob
  - Alice-Dave: 0.6
  - Dave-Eve: 0.7
  - Eve-Bob: 0.5
  - Path strength: min(0.6, 0.7, 0.5) = 0.5
  - Total hops: 3

Conclusion: Path 1 is shorter and stronger
```

---

## 5. Shared Connection Analysis

### 5.1 Common Neighbors

**Calculation:**
```
Common_Neighbors(A, B) = Neighbors(A) ∩ Neighbors(B)

Example:
  Neighbors(Alice) = {Bob, Carol, Dave, Eve}
  Neighbors(Frank) = {Carol, Eve, Grace, Henry}
  
  Common = {Carol, Eve}
  Count = 2
```

**Interpretation:**
```
High common neighbors: Likely to know each other
Low common neighbors: Periphery of network
Zero common neighbors: Different social circles
```

### 5.2 Shared Affiliations

**Organization Overlap:**
```
Same employer
Same school/university
Same groups/organizations
Same locations
Same interests/hashtags
```

**Analysis:**
```
More shared affiliations = stronger connection likelihood
Shared affiliations can explain how strangers met
Multiple shared contexts = deep relationship
```

### 5.3 Triangle Closure

**Principle:**
```
If A knows B, and B knows C, 
then A is likely to know C (especially in closed networks)

Formula: 
  Clustering Coefficient = (closed triangles) / (open triangles)
```

**Predictive Value:**
```
High clustering: Recommend connection with high confidence
Low clustering: Connection would bridge communities
```

---

## 6. Unusual Pattern Detection

### 6.1 Anomaly Types

**Structural Anomalies:**
```
Isolates: Nodes with no connections
Hubs: Nodes with unusually high degree
Bridges: Only connection between groups
Outliers: Distant from main network
```

**Temporal Anomalies:**
```
Rapid connection formation
Sudden activity spikes
Coordinated behavior
Unusual interaction timing
```

**Behavioral Anomalies:**
```
Cross-community connections
Connection to known bad actors
Unusual interaction patterns
Inconsistent with stated role
```

### 6.2 Detection Methods

**Statistical Outliers:**
```
Z-score calculation:
  z = (value - mean) / standard_deviation

|z| > 2: Unusual
|z| > 3: Very unusual (investigate)
```

**Graph-Based Detection:**
```
Betweenness: Unusually high = potential bridge
Closeness: Unusually low = peripheral
Degree: Unusually high = potential hub
Clustering: Unusually low = potential anomaly
```

### 6.3 Pattern Interpretation

**Potential Concerns:**
```
- New connection to competitor
- Connection to suspended/banned accounts
- Rapid follower growth (inauthentic)
- Connections across conflicting groups
- Unusual geographic spread
```

**Benign Explanations:**
```
- Public figure (high degree expected)
- Community organizer (bridge role expected)
- New to platform (few connections expected)
- Professional networker (high connectivity)
```

---

## 7. Visualization Preparation

### 7.1 Node Representation

**Size Encoding:**
```
By degree (number of connections)
By importance (PageRank, betweenness)
By activity level
By follower count
```

**Color Encoding:**
```
By community membership
By entity type (person/org/content)
By platform
By sentiment
By verification status
```

**Shape Encoding:**
```
Circles: Persons
Squares: Organizations
Diamonds: Content
Triangles: Locations
```

### 7.2 Edge Representation

**Thickness:**
```
Proportional to connection strength
Log scale for wide dynamic range
```

**Color:**
```
By relationship type
By recency (fade old connections)
By platform
```

**Style:**
```
Solid: Strong connections
Dashed: Weak connections
Dotted: Historical/inactive
Arrow: Directed relationships
```

### 7.3 Layout Strategies

**Force-Directed:**
```
Best for: Social networks, community detection
Advantages: Natural clustering, intuitive
Tools: Gephi, Cytoscape, D3.js
```

**Hierarchical:**
```
Best for: Organizations, reporting structures
Advantages: Clear authority levels
Tools: Graphviz, yFiles
```

**Geographic:**
```
Best for: Location-based analysis
Advantages: Spatial patterns visible
Tools: Leaflet, Mapbox, Gephi GeoLayout
```

**Circular:**
```
Best for: Small networks, radial patterns
Advantages: Symmetric, compact
Tools: Most graph libraries
```

### 7.4 Export Formats

**GEXF (Gephi):**
```xml
<gexf>
  <graph mode="static">
    <nodes>
      <node id="1" label="Alice"/>
    </nodes>
    <edges>
      <edge source="1" target="2" weight="5"/>
    </edges>
  </graph>
</gexf>
```

**GraphML:**
```xml
<graphml>
  <key id="weight" for="edge"/>
  <graph>
    <node id="n1"/>
    <edge source="n1" target="n2">
      <data key="weight">5</data>
    </edge>
  </graph>
</graphml>
```

**Cytoscape JSON:**
```json
{
  "elements": {
    "nodes": [{"data": {"id": "a", "name": "Alice"}}],
    "edges": [{"data": {"source": "a", "target": "b", "weight": 5}}]
  }
}
```

---

## 8. Confidence Ratings

| Graph Element | Confidence | Notes |
|--------------|-----------|-------|
| Node existence | HIGH | Directly observed |
| Edge existence | HIGH | Interaction confirmed |
| Edge weight | MEDIUM | Calculated estimation |
| Path existence | HIGH | Graph traversal |
| Path strength | MEDIUM | Minimum edge weight |
| Community membership | MEDIUM | Algorithm output |
| Relationship type | LOW | Interpretation |

---

## 9. Limitations

### 9.1 Data Limitations

1. **Incomplete visibility** - Private interactions hidden
2. **Historical gaps** - Deleted content lost
3. **Multi-platform fragmentation** - Single platform bias
4. **Fake accounts** - Pollute graph structure
5. **Temporal snapshots** - Static view of dynamic system

### 9.2 Interpretation Limitations

1. **Correlation ≠ causation** - Connection doesn't imply relationship
2. **Missing context** - Don't know nature of connection
3. **Weight subjectivity** - Scoring requires assumptions
4. **Community ambiguity** - Overlapping memberships
5. **Dynamic relationships** - Graph is point-in-time

---

## 10. Tools Reference

| Tool | Type | Best For |
|------|------|----------|
| Gephi | Desktop | Large network analysis |
| Cytoscape | Desktop | Bio/social networks |
| NodeXL | Excel | Office environment |
| D3.js | JavaScript | Web visualizations |
| NetworkX | Python | Analysis scripting |
| igraph | R/Python | Statistical analysis |

---

*Social Graph Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*

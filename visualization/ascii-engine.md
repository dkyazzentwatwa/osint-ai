# ASCII Engine v2.0

Text-based graphics system for OSINT visualization.

---

## 1. ASCII Art Generation Rules

### Character Set
```
Box Drawing:
  ─ ━ │ ┃ ┄ ┅ ┆ ┇ ┈ ┉ ┊ ┋
  ┌ ┍ ┎ ┏ ┐ ┑ ┒ ┓
  └ ┕ ┖ ┗ ┘ ┙ ┚ ┛
  ├ ┝ ┞ ┟ ┠ ┡ ┢ ┣ ┤ ┥ ┦ ┧ ┨ ┩ ┪ ┫
  ┬ ┭ ┮ ┯ ┰ ┱ ┲ ┳ ┴ ┵ ┶ ┷ ┸ ┹ ┺ ┻
  ┼ ┽ ┾ ┿ ╀ ╁ ╂ ╃ ╄ ╅ ╆ ╇ ╈ ╉ ╊ ╋
  ═ ║ ╒ ╓ ╔ ╕ ╖ ╗ ╘ ╙ ╚ ╛ ╜ ╝ ╞ ╟
  ╠ ╡ ╢ ╣ ╤ ╥ ╦ ╧ ╨ ╩ ╪ ╫ ╬

Block Elements:
  █ ▓ ▒ ░ ▀ ▄ ▌ ▐ ▖ ▗ ▘ ▙ ▚ ▛ ▜ ▝ ▞ ▟

Arrows:
  ← ↑ → ↓ ↔ ↕ ↖ ↗ ↘ ↙ ⇐ ⇑ ⇒ ⇓ ⇔ ⇕
  ⬅ ⬆ ➡ ⬇ ⬈ ⬉ ⬊ ⬋ ⬌ ⬍

Special:
  ● ○ ◐ ◑ ◒ ◓ ◔ ◕ ◖ ◗ ◦ ◯
  ★ ☆ ✓ ✗ ✘ ✓ ✔ ✕ ✖ ✗ ✘
  ⚠ ⚡ ⚪ ⚫ ⏳ ⏸ ⏹ ⏺
```

### Generation Principles

1. **Width Constraint**: Maximum 78 characters (with padding)
2. **Height Constraint**: Maximum 40 lines per visualization
3. **Whitespace**: Always include 1-space padding inside boxes
4. **Alignment**: Center-align headers, left-align content
5. **Consistency**: Same character style throughout document

---

## 2. Box Drawing Characters Guide

### Standard Boxes

**Single Line Box**:
```
┌────────────────────────────────────────┐
│ Header                                 │
├────────────────────────────────────────┤
│ Content goes here                      │
└────────────────────────────────────────┘
```

**Double Line Box**:
```
╔════════════════════════════════════════╗
║ CRITICAL FINDING                       ║
╠════════════════════════════════════════╣
║ Requires immediate attention           ║
╚════════════════════════════════════════╝
```

**Rounded Corners** (when supported):
```
╭────────────────────────────────────────╮
│ Modern style header                    │
╰────────────────────────────────────────╯
```

**Nested Boxes**:
```
┌────────────────────────────────────────┐
│ Parent Container                       │
│ ┌──────────────────────────────────┐   │
│ │ Nested: Child Container          │   │
│ └──────────────────────────────────┘   │
└────────────────────────────────────────┘
```

### Border Styles by Severity

| Severity | Left Border | Right Border | Header |
|----------|-------------|--------------|--------|
| CRITICAL | ║           | ║            | ════   |
| HIGH     | █           | █            | ────   |
| MEDIUM   | ▌           | ▐            | ────   |
| LOW      | │           │              | ────   |
| INFO     | ┃           ┃              | ━━━━   |

---

## 3. Entity Relationship Graph Templates

### Simple Hierarchy
```
                    ┌─────────────┐
                    │   TARGET    │
                    │  John Doe   │
                    └──────┬──────┘
                           │
           ┌───────────────┼───────────────┐
           │               │               │
           ▼               ▼               ▼
    ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
    │   EMAIL     │ │   DOMAIN    │ │   PHONE     │
    │j@doe.com    │ │ doe.com     │ │ +1-555-...  │
    └──────┬──────┘ └──────┬──────┘ └──────┬──────┘
           │               │               │
           ▼               ▼               ▼
    ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
    │  SOCIAL     │ │   SERVER    │ │   OWNER     │
    │  LinkedIn   │ │ 1.2.3.4     │ │ Jane Doe    │
    └─────────────┘ └─────────────┘ └─────────────┘
```

### Complex Network
```
                        ┌──────────┐
                   ┌────┤  TARGET  ├────┐
                   │    │ Entity A │    │
                   │    └────┬─────┘    │
                   │         │          │
              ┌────┴───┐ ┌───┴───┐ ┌────┴───┐
              │Entity B│ │Entity C│ │Entity D│
              └───┬────┘ └───┬───┘ └───┬────┘
                  │          │          │
        ┌─────────┴───┐  ┌───┴────┐   ┌┴───────┐
        │             │  │        │   │        │
   ┌────┴───┐   ┌─────┴──┴┐  ┌───┴───┐     ┌───┴────┐
   │Entity E│   │Entity F │  │Entity G│     │Entity H│
   └────────┘   └─────────┘  └────────┘     └────────┘
```

### Ownership Chain
```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│ Company A│────▶│ Company B│────▶│ Company C│────▶│  TARGET  │
│ (Delaware│ owns│ (Cayman) │ owns│  (BVI)   │ owns│  Entity  │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
  100%             75%              51%              Asset
```

### Association Web
```
                         TARGET
                           ●
                          /|\
                         / | \
                        /  |  \
                       ○   ●   ○
                      /    |    \
                     ○   ┌─┴─┐   ○
                        /  |  \
                       ○   ○   ○

Legend: ● = Direct link  ○ = Indirect link  │ = Connection
```

---

## 4. Network Topology ASCII Maps

### Simple Network
```
    Internet
        │
        ▼
   ┌─────────┐
   │  Router │
   │ 1.1.1.1 │
   └────┬────┘
        │
   ┌────┴────┐
   │         │
   ▼         ▼
┌─────┐   ┌─────┐
│Web  │   │Mail │
│Srv  │   │Srv  │
└─────┘   └─────┘
```

### Multi-Subnet Topology
```
                        INTERNET
                           │
                    ┌──────┴──────┐
                    │   Firewall  │
                    │  203.0.113.1│
                    └──────┬──────┘
                           │
            ┌──────────────┼──────────────┐
            │              │              │
            ▼              ▼              ▼
      ┌──────────┐   ┌──────────┐   ┌──────────┐
      │ DMZ      │   │ Internal │   │ Mgmt     │
      │10.0.1.0/24│   │10.0.2.0/24│   │10.0.3.0/24│
      └────┬─────┘   └────┬─────┘   └────┬─────┘
           │              │              │
     ┌─────┴─────┐   ┌────┴────┐    ┌────┴────┐
     │           │   │         │    │         │
┌────┴───┐ ┌─────┴──┐│┌────┴───┐    │         │
│Web 10.1│ │DNS 10.2│││DB 10.21│    │         │
└────────┘ └────────┘│└────────┘    │         │
                     │              │         │
                     │  ┌─────┐     │ ┌─────┐ │
                     └──┤Work │     └─┤Jump │ │
                        │ 10.5│       │10.31│ │
                        └─────┘       └─────┘ │
```

### Geographic Distribution
```
        ┌─────────────────────────────────────────────────────┐
        │                    ASIA-PACIFIC                     │
        │  ┌─────┐      ┌─────┐      ┌─────┐                 │
        │  │Tokyo│◄────►│Seoul│◄────►│Sydney│                │
        │  └─────┘      └─────┘      └─────┘                 │
        └────────────┬───────────────────────────────────────┘
                     │
    ┌────────────────┼────────────────┐
    │   EUROPE       │    AMERICAS    │
    │  ┌─────┐       │   ┌─────┐      │
    │  │London│◄─────┼──►│NYC  │      │
    │  └──┬──┘       │   └──┬──┘      │
    │     │          │      │         │
    │  ┌──┴──┐       │   ┌──┴──┐      │
    │  │Frank│◄──────┼──►│LA   │      │
    │  └─────┘       │   └─────┘      │
    └────────────────┴────────────────┘
```

---

## 5. Timeline Gantt Visualization

### Investigation Timeline
```
Investigation: Project PHOENIX
═══════════════════════════════════════════════════════════════════════════

ACTIVITY                    WEEK 1    WEEK 2    WEEK 3    WEEK 4    WEEK 5
───────────────────────────────────────────────────────────────────────────
Initial OSINT               [████]    
Domain Enumeration                    [████]
Social Media Analysis                 [████]    [████]
Email Discovery                                         [████]
Network Mapping                                         [████]    [████]
Report Preparation                                                [████]
───────────────────────────────────────────────────────────────────────────
MILESTONES
  ▼ Day 3: Target identified
          ▼ Day 8: First pivot point discovered
                    ▼ Day 15: Critical finding
                              ▼ Day 22: Network mapped
                                        ▼ Day 30: Final report
```

### Event Timeline
```
CHRONOLOGY: Target Activity
═══════════════════════════════════════════════════════════════════════════

2024-01-15 09:23 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
                 │ Account created on Platform A                        ┃
                 │ Source: platform-a.com/user/12345                    ┃
2024-01-15 14:56 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓   ┃
                 │ First post published                                ┃   ┃
                 │ Content: "Starting new chapter..."                  ┃   ┃
2024-01-18 11:34 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓   ┃   ┃
                 │ Email registration confirmed                     ┃   ┃   ┃
                 │ Email: target@example.com                        ┃   ┃   ┃
2024-01-22 08:45 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓   ┃   ┃   ┃
                 │ Connected to Subject B                        ┃   ┃   ┃   ┃
                 │ Relationship: Former colleague                ┃   ┃   ┃   ┃
2024-01-25 16:12 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓   ┃   ┃   ┃   ┃
                 │ Location check-in: New York City                 ┃   ┃   ┃   ┃   ┃
                 │ Geo: 40.7128, -74.0060                           ┃   ┃   ┃   ┃   ┃
2024-01-28 09:00 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓   ┃   ┃   ┃   ┃   ┃
                 │ Domain registered: target-corp.com                ┃   ┃   ┃   ┃   ┃
                 │ Registrar: GoDaddy                                ┃   ┃   ┃   ┃   ┃
                                                          ▼   ▼   ▼   ▼   ▼   ▼
```

### Compact Timeline
```
TIMELINE (Last 30 Days)
─────────────────────────────────────────────────────────────────────────────
    ████    ████        ████    ████████        ████    ████
────┴──────┴────┴────────┴────┴────────┴────────┴────┴───┴──────▶
   Day 5   Day 10      Day 15  Day 20          Day 25  Day 30

    ████ = High activity    ████ = Medium activity    · = Low activity
```

---

## 6. Risk Heatmap Matrix

### Standard 5x5 Risk Matrix
```
                    PROBABILITY
           Rare  Unlikely Possible Likely  Certain
            (1)     (2)      (3)     (4)      (5)
         ┌───────┬───────┬───────┬───────┬───────┐
Catastrophic│       │       │       │       │       │
    (5)    │  M    │  M    │  H    │  C    │  C    │
         ├───────┼───────┼───────┼───────┼───────┤
  Major    │       │       │       │       │       │
    (4)    │  L    │  M    │  M    │  H    │  C    │
         ├───────┼───────┼───────┼───────┼───────┤
 Moderate  │       │       │       │       │       │
    (3)    │  L    │  L    │  M    │  M    │  H    │
I M P A C T├───────┼───────┼───────┼───────┼───────┤
   Minor    │       │       │       │       │       │
    (2)    │  L    │  L    │  L    │  M    │  M    │
         ├───────┼───────┼───────┼───────┼───────┤
Negligible │       │       │       │       │       │
    (1)    │  L    │  L    │  L    │  L    │  M    │
         └───────┴───────┴───────┴───────┴───────┘

Legend: L = LOW (Green)  M = MEDIUM (Yellow)  H = HIGH (Orange)  C = CRITICAL (Red)
```

### Detailed Heatmap with Findings
```
RISK HEATMAP: Investigation PHOENIX
═══════════════════════════════════════════════════════════════════════════════

                         LIKELIHOOD
            Low (1)    Medium (2)   High (3)   Critical (4)
         ┌───────────┬───────────┬───────────┬───────────┐
CRITICAL │           │           │           │ ☠️ ☠️ ☠️  │  ▲
Impact 4 │           │           │   ☠️      │ ☠️ ☠️     │  │
         │           │           │ Identity  │ Financial │  I
         ├───────────┼───────────┼───────────┼───────────┤  M
HIGH     │           │   ⚠️      │ ☠️ ☠️     │ ☠️        │  P
Impact 3 │           │  Reput.   │  Loc.     │  Threat   │  A
         ├───────────┼───────────┼───────────┼───────────┤  C
MEDIUM   │   ℹ️      │   ⚠️      │   ⚠️      │           │  T
Impact 2 │  Data Leak│  Social   │  Network  │           │
         ├───────────┼───────────┼───────────┼───────────┤
LOW      │   ℹ️ ℹ️   │   ℹ️      │           │           │
Impact 1 │  Exposure │  Metadata │           │           │
         └───────────┴───────────┴───────────┴───────────┘

CURRENT RISK SCORE: 73/100 [████████░░░░░░░░░░░░] HIGH

Priority Actions:
  ☠️  CRITICAL: Immediate protective measures required
  ⚠️  HIGH: Address within 24 hours
  ℹ️  MEDIUM: Monitor and document
```

### Compact Mini-Heatmap
```
RISK MATRIX
───────────
  12345
1 ░░░░░  L=░░░ M=▒▒▒ H=███ C=▓▓▓
2 ░░░░▒
3 ░░▒▒▓  Current findings: 2H 3M 1L
4 ░▒▓▓▓
5 ▒▓▓▓▓
```

---

## 7. Geographic Coordinate Plotting

### World Map (ASCII)
```
                          WORLD MAP OVERVIEW
    ═══════════════════════════════════════════════════════════════════

         ┌───────────────────────────────────────────────────────┐
        ╱                                                         ╲
       ╱    🌐 ASIA                                  🌐 N. AMERICA  ╲
      ╱    ● Beijing 40°N,116°E                     ● NYC 40°N,74°W   ╲
     │     ● Tokyo 35°N,139°E                       ● LA 34°N,118°W   │
     │     ● Seoul 37°N,126°E                       ● Toronto 43°N,79°W│
     │                                                               │
🌐 EUROPE│                                    🌐 S. AMERICA          │
● London 51°N,0°W                         ● São Paulo 23°S,46°W      │
● Paris 48°N,2°E                          ● Buenos Aires 34°S,58°W   │
● Berlin 52°N,13°E                                                   │
     │                                                               │
     │                         🌐 AFRICA                             │
     │                    ● Cairo 30°N,31°E                          │
     │                    ● Lagos 6°N,3°E                            │
     │                    ● Johannesburg 26°S,28°E                   │
      ╲                                                              ╱
       ╲                  🌐 AUSTRALIA                              ╱
        ╲            ● Sydney 33°S,151°E                           ╱
         ╲           ● Melbourne 37°S,144°E                       ╱
          └───────────────────────────────────────────────────────┘

LEGEND: ● = Target Location  🌐 = Region Hub
```

### Regional Zoom: Europe
```
                         EUROPE - TARGET LOCATIONS
    ═══════════════════════════════════════════════════════════════════

                              ┌─────────────┐
                              │   REYKJAVIK │
                              └─────────────┘
         ┌─────────────┐           │           ┌─────────────┐
         │   DUBLIN    │           │           │   OSLO      │
         │  ● Target   │───────────┼───────────│             │
         └─────────────┘           │           └──────┬──────┘
                                    │                  │
    ┌─────────────┐         ┌──────┴──────┐          ┌┴────────────┐
    │   LONDON    │         │   STOCKHOLM │          │   HELSINKI  │
    │  ● Subject A│◄───────►│             │◄─────────│             │
    └──────┬──────┘         └─────────────┘          └─────────────┘
           │
           │              ┌─────────────┐           ┌─────────────┐
           │              │   BERLIN    │◄─────────►│   WARSAW    │
           │              │  ● Server   │           │             │
           │              └──────┬──────┘           └─────────────┘
           │                     │
           │              ┌──────┴──────┐           ┌─────────────┐
           └─────────────►│   PARIS     │◄──────────│   PRAGUE    │
                          │             │           │             │
                          └──────┬──────┘           └─────────────┘
                                 │
                          ┌──────┴──────┐           ┌─────────────┐
                          │   MADRID    │           │   ROME      │
                          │  ● Shell Co │           │             │
                          └─────────────┘           └─────────────┘

    Distances from London:
    • Paris: 344 km  |  Berlin: 932 km  |  Madrid: 1,265 km
```

### City-Level Map
```
                         MANHATTAN - ACTIVITY ZONES
    ═══════════════════════════════════════════════════════════════════

                              N
                              │
                              ▼

                   ┌─────────────────────────┐
                   │      CENTRAL PARK       │
                   │                         │
                   │    ▲ Hotel check-in     │
                   │    40.7681, -73.9819    │
                   └────────────┬────────────┘
                                │
                   ┌────────────┴────────────┐
                   │        MIDTOWN          │
                   │  ● Office location      │
                   │  40.7589, -73.9851      │
                   │                         │
                   └────────────┬────────────┘
                                │
    ┌───────────────────────────┼───────────────────────────┐
    │                           ▼                           │
    │                     ▲ Restaurant                      │
    │                     40.7282, -73.7942                 │
    │                                                     ┌─┴───────┐
    │                                                     │ FINANCIAL│
    │              ● Meeting point                         │ DISTRICT │
    │              40.7074, -74.0113                       │          │
    │                                                     └─────────┘
    └───────────────────────────────────────────────────────┘

    Scale: 1 km per 10 characters
    Legend: ● = Confirmed  ▲ = Suspected  ☐ = Monitored
```

### IP Geolocation Plot
```
                    NETWORK ORIGIN GEOLOCATION
    ═══════════════════════════════════════════════════════════════════

    IP: 203.0.113.45
    ┌─────────────────────────────────────────────────────────────────┐
    │ Coordinates: 35.6762°N, 139.6503°E                            │
    │ Accuracy: City level (±5km)                                     │
    │ ISP: Example Telecom Japan                                      │
    │                                                                 │
    │                    JAPAN                                        │
    │              ┌─────────────────┐                                │
    │              │                 │                                │
    │              │      ●          │  ◄── Target (Tokyo)            │
    │              │                 │                                │
    │              │    Sapporo      │                                │
    │              │       ●         │                                │
    │              │                 │                                │
    │              │  Nagoya ●       │                                │
    │              │                 │                                │
    │              │     Osaka ●     │                                │
    │              │                 │                                │
    │              │       Fukuoka ● │                                │
    │              └─────────────────┘                                │
    │                    ◄── Target appears here                     │
    └─────────────────────────────────────────────────────────────────┘
```

---

## 8. Generation Functions (Pseudocode)

### Box Generator
```python
def generate_box(title, content, width=70, style="single"):
    """
    Generates ASCII box with content
    """
    chars = {
        "single": ("┌", "┐", "└", "┘", "─", "│", "├", "┤"),
        "double": ("╔", "╗", "╚", "╝", "═", "║", "╠", "╣"),
        "rounded": ("╭", "╮", "╰", "╯", "─", "│", "├", "┤")
    }
    
    # Implementation generates border + content + border
    # with proper padding and line wrapping
```

### Tree Generator
```python
def generate_tree(root, children, max_depth=3):
    """
    Generates hierarchical tree structure
    """
    # Uses: ├ ┤ ┌ ┐ └ ┘ │ ─ characters
    # Handles multi-level nesting
    # Returns formatted string
```

### Bar Chart Generator
```python
def generate_bars(data, width=50, label_width=15):
    """
    Generates horizontal bar chart
    
    Input: [("Label", value, max_value), ...]
    Output: Formatted bar chart string
    """
    # Uses: █ ▓ ▒ ░ characters for bars
    # Aligns labels and values
```

### Table Generator
```python
def generate_table(headers, rows, col_widths=None):
    """
    Generates ASCII table
    """
    # Auto-calculates column widths
    # Handles text wrapping
    # Returns formatted table with borders
```

---

## 9. Screen Reader Accessibility

### Text Alternatives

Every visualization MUST include:

1. **Alt Text Header**: Description of what the visualization shows
2. **Data Summary**: Plain text summary of key data points
3. **Structured Data**: Table format for complex data

**Example**:
```
[ASCII GRAPH: Risk Matrix showing 4 findings]

Alt: Heatmap displaying 4 risk findings across impact and probability
axes. Two critical risks in upper right quadrant.

Data Summary:
- Finding A: Critical risk (Financial fraud, 95% confidence)
- Finding B: High risk (Identity theft, 80% confidence)  
- Finding C: Medium risk (Data exposure, 60% confidence)
- Finding D: Low risk (Metadata leak, 40% confidence)

Structured Table:
| Finding | Impact | Likelihood | Risk Level |
|---------|--------|------------|------------|
| A       | 5      | 4          | CRITICAL   |
| B       | 4      | 4          | HIGH       |
| C       | 3      | 2          | MEDIUM     |
| D       | 2      | 1          | LOW        |
```

### Best Practices

- Avoid graphics-only communication
- Use consistent patterns (viewers learn the visual language)
- Include numeric values alongside visual representations
- Provide sorting/filtering options for large datasets
- Support keyboard navigation in interactive dashboards

---

## 10. Terminal Color Codes

### ANSI Escape Sequences

```
Reset:     \033[0m
Bold:      \033[1m
Dim:       \033[2m
Underline: \033[4m

Colors:
  Black:   \033[30m  \033[40m (bg)
  Red:     \033[31m  \033[41m (bg)
  Green:   \033[32m  \033[42m (bg)
  Yellow:  \033[33m  \033[43m (bg)
  Blue:    \033[34m  \033[44m (bg)
  Magenta: \033[35m  \033[45m (bg)
  Cyan:    \033[36m  \033[46m (bg)
  White:   \033[37m  \033[47m (bg)

Bright variants: \033[9Xm (90-97 foreground, 100-107 background)
```

### Risk Color Mapping

| Level | Foreground | Background | Example |
|-------|------------|------------|---------|
| CRITICAL | Red (31) | Red (41) | `[████]` |
| HIGH | Yellow (33) | Yellow (43) | `[████]` |
| MEDIUM | Cyan (36) | Cyan (46) | `[████]` |
| LOW | Green (32) | Green (42) | `[░░░░]` |
| INFO | White (37) | Default | `[····]` |

---

*Version: 2.0 | Last Updated: 2026-02-27*

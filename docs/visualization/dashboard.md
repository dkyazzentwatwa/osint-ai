# Dashboard v2.0

Interactive dashboard system for OSINT investigations.

---

## 1. Dashboard Layout Specification

### Standard Layout (80x24 Terminal)
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[LOGO] OSINT INVESTIGATOR v2.0           [CASE: PHOENIX]    [● LIVE]  14:32 │
├─────────────────────────────────────────────────────────────────────────────┤
│┌──────────────┐┌──────────────┐┌──────────────┐┌──────────────────────────┐│
││  ENTITIES    ││   SOURCES    ││   FINDINGS   ││      RISK LEVEL          ││
││     47       ││     23       ││     34       ││      HIGH (73/100)       ││
││   [████]     ││   [██░░]     ││   [████]     ││   [██████████████░░░░]   ││
││  +5 today    ││  +2 today    ││  +3 today    ││      ↑ +12 pts           ││
│└──────────────┘└──────────────┘└──────────────┘└──────────────────────────┘│
│┌────────────────────────────────────┐┌─────────────────────────────────────┐│
││      RECENT ACTIVITY FEED          ││        QUICK ACTIONS                ││
││  14:32 ● New entity discovered     ││                                     ││
││  13:15 ✓ Source verified          ││  [1] Add Entity    [5] Run Query    ││
││  11:47 ⚠ Risk score updated       ││  [2] View Graph    [6] Export Data  ││
││  09:23 → Daily scan completed     ││  [3] Timeline      [7] Settings     ││
││  08:45 ● New connection found     ││  [4] Reports       [8] Help         ││
││                                    ││                                     ││
││  [M]ore activity...               ││  [Q]uit  [R]efresh  [S]earch        ││
│└────────────────────────────────────┘└─────────────────────────────────────┘│
│┌───────────────────────────────────────────────────────────────────────────┐│
││  STATUS: ████████████████████████████████░░░░ 78% | Phase: DEEP DIVE     ││
││  Next Milestone: Network Mapping (ETA: 3 days)                           ││
│└───────────────────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

### Extended Layout (80x40 Terminal)
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[LOGO] OSINT INVESTIGATOR v2.0           [CASE: PHOENIX]    [● LIVE]  14:32 │
├─────────────────────────────────────────────────────────────────────────────┤
│┌──────────────┐┌──────────────┐┌──────────────┐┌──────────────────────────┐│
││  ENTITIES    ││   SOURCES    ││   FINDINGS   ││      RISK LEVEL          ││
││     47       ││     23       ││     34       ││      HIGH (73/100)       ││
││   [████]     ││   [██░░]     ││   [████]     ││   [██████████████░░░░]   ││
││  +5 today    ││  +2 today    ││  +3 today    ││      ↑ +12 pts           ││
│└──────────────┘└──────────────┘└──────────────┘└──────────────────────────┘│
│┌─────────────────────────────┐┌────────────────────────────────────────────┐│
││      ACTIVITY TIMELINE      ││         ENTITY DISTRIBUTION                ││
││                             ││                                            ││
││  Week 1  Week 2  Week 3     ││  Individuals     ████████████████████ 18  ││
││   ████    ████    ██░░      ││  Organizations   ██████████████░░░░░░ 12  ││
││   ████    ████    ██░░      ││  Domains         ██████████░░░░░░░░░░  8  ││
││   ████    ████    ██░░      ││  Locations       ████████░░░░░░░░░░░░  6  ││
││                             ││  Devices         ██████░░░░░░░░░░░░░░  3  ││
││  ████ = High activity       ││                                            ││
││                             ││  Total: 47 entities across 5 categories     ││
│└─────────────────────────────┘└────────────────────────────────────────────┘│
│┌────────────────────────────────────┐┌─────────────────────────────────────┐│
││      RECENT ACTIVITY FEED          ││        QUICK ACTIONS                ││
││  14:32 ● New entity discovered     ││                                     ││
││  13:15 ✓ Source verified          ││  [1] Add Entity    [5] Run Query    ││
││  11:47 ⚠ Risk score updated       ││  [2] View Graph    [6] Export Data  ││
││  09:23 → Daily scan completed     ││  [3] Timeline      [7] Settings     ││
││  08:45 ● New connection found     ││  [4] Reports       [8] Help         ││
││  07:30 ✓ Source archived          ││                                     ││
││                                    ││  [Q]uit  [R]efresh  [S]earch        ││
││  [M]ore activity...               ││                                     ││
│└────────────────────────────────────┘└─────────────────────────────────────┘│
│┌───────────────────────────────────────────────────────────────────────────┐│
││  ALERTS: ⚠️ 2 warnings  |  ℹ️ 5 notifications  |  ✓ System operational    ││
│├───────────────────────────────────────────────────────────────────────────┤│
││  STATUS: ████████████████████████████████░░░░ 78% | Phase: DEEP DIVE     ││
││  Next Milestone: Network Mapping (ETA: 3 days)                           ││
│└───────────────────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

### Compact Layout (80x12 Terminal)
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[OSINT] PHOENIX | 47E 23S 34F | RISK: HIGH | 14:32 | [?]Help [Q]uit        │
├─────────────────────────────────────────────────────────────────────────────┤
│Last: 14:32 ●Entity | 13:15 ✓Verified | 11:47 ⚠Risk↑ | 09:23 →Scan done   │
│┌──────────┐┌──────────┐┌──────────┐┌──────────┐┌──────────┐┌──────────────┐│
││[1]Add    ││[2]Graph  ││[3]Timeline││[4]Reports││[5]Query  ││[6]Export     ││
│└──────────┘└──────────┘└──────────┘└──────────┘└──────────┘└──────────────┘│
│Status: ████████████████████████████████░░░░ 78% | Phase: DEEP DIVE (3d ETA)│
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Real-Time Statistics Display

### Live Metrics Panel
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         LIVE INVESTIGATION METRICS                           │
│                    Last Update: 14:32:18 UTC (2s ago)                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  PRIMARY METRICS                    TREND (24H)                              │
│  ═══════════════════════════════════════════════════════════════════════    │
│                                                                              │
│  ┌──────────┐  Total Entities: 47    ↑ +5 (12%)  ████████████████████      │
│  │   47     │  Active Entities: 42   ↑ +3 (8%)   ████████████████░░░░      │
│  │  ████    │  Archived: 5           → 0 (0%)    ████████░░░░░░░░░░░░      │
│  └──────────┘                                                               │
│                                                                              │
│  ┌──────────┐  Total Sources: 23     ↑ +2 (10%)  ██████████████████░░       │
│  │   23     │  Verified: 18          ↑ +2 (13%)  █████████████████░░░       │
│  │  ██░░    │  Pending: 3            → 0 (0%)    ████░░░░░░░░░░░░░░░░       │
│  └──────────┘  Unreliable: 2         → 0 (0%)    ██░░░░░░░░░░░░░░░░░░       │
│                                                                              │
│  ┌──────────┐  Total Findings: 34    ↑ +3 (10%)  ████████████████████      │
│  │   34     │  Critical: 5           ↑ +1 (25%)  ████████████████░░░░      │
│  │  ████    │  High: 12              ↑ +2 (20%)  ██████████████████░░      │
│  └──────────┘  Medium: 14            → 0 (0%)    ████████████████████      │
│                Low: 3                → 0 (0%)    ██████░░░░░░░░░░░░░░       │
│                                                                              │
│  ┌──────────┐  Connections: 89       ↑ +8 (10%)  ████████████████████      │
│  │   89     │  Direct: 34            ↑ +3 (10%)  ██████████████████░░       │
│  │  ████    │  Indirect: 55          ↑ +5 (11%)  ████████████████████      │
│  └──────────┘                                                               │
│                                                                              │
│  REFRESH RATE: 5 seconds | Data Source: Live DB | Latency: 12ms             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Confidence Metrics
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      CONFIDENCE ANALYSIS DASHBOARD                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  OVERALL CONFIDENCE: 67% [████████████████████████░░░░░░░░░░░░░░░░░░░░]     │
│                                                                              │
│  BY CATEGORY:                                                                │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │ Identity Data     ████████████████████████████████████████░░░░ 78%  │    │
│  │ Contact Info      ██████████████████████████████████░░░░░░░░░░ 65%  │    │
│  │ Location Data     ████████████████████████████████░░░░░░░░░░░░ 62%  │    │
│  │ Financial Data    ████████████████████████░░░░░░░░░░░░░░░░░░░░ 48%  │    │
│  │ Network Data      ███████████████████████████████████████░░░░░ 75%  │    │
│  │ Social Graph      ██████████████████████████████████░░░░░░░░░░ 65%  │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  CONFIDENCE DISTRIBUTION:                                                    │
│  95-100%: 12 entities ████████████████████████ 24%                          │
│  80-94%:  8 entities  ██████████░░░░░░░░░░░░░░ 16%                          │
│  60-79%:  7 entities  █████████░░░░░░░░░░░░░░░ 14%                          │
│  40-59%:  6 entities  ████████░░░░░░░░░░░░░░░░ 12%                          │
│  <40%:    14 entities ██████████████████░░░░░░ 28% (needs verification)     │
│                                                                              │
│  LOW CONFIDENCE ALERTS (5):                                                  │
│  • 3 entities lacking secondary confirmation                                │
│  • 2 entities with conflicting information                                  │
│  • 9 entities derived from single sources                                   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Source Health Monitor
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        SOURCE HEALTH MONITOR                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  SOURCE RELIABILITY INDEX: 72/100 [████████████████████████████░░░░░░░░]    │
│                                                                              │
│  ACTIVE SOURCES (23):                                                        │
│  ┌────────────────┬────────┬───────────┬─────────────┬────────────────────┐ │
│  │ Source         │ Status │ Reliability│ Last Check │ Health             │ │
│  ├────────────────┼────────┼───────────┼─────────────┼────────────────────┤ │
│  │ LinkedIn       │ ✓ LIVE │ A (95%)   │ 2 min ago  │ ████████████████░░ │ │
│  │ Twitter/X      │ ✓ LIVE │ B (82%)   │ 5 min ago  │ ██████████████░░░░ │ │
│  │ Facebook       │ ✓ LIVE │ B (78%)   │ 3 min ago  │ █████████████░░░░░ │ │
│  │ Instagram      │ ✓ LIVE │ C (65%)   │ 1 min ago  │ ███████████░░░░░░░ │ │
│  │ WHOIS DB       │ ✓ LIVE │ A (90%)   │ 10 min ago │ ███████████████░░░ │ │
│  │ Court Records  │ ✓ LIVE │ A (88%)   │ 1 hour ago │ ██████████████░░░░ │ │
│  │ News Archives  │ ✓ LIVE │ B (75%)   │ 30 min ago │ ███████████░░░░░░░ │ │
│  │ Business Reg.  │ ⚠ SLOW │ B (80%)   │ 15 min ago │ ████████████░░░░░░ │ │
│  │ Property DB    │ ✓ LIVE │ B (72%)   │ 45 min ago │ ███████████░░░░░░░ │ │
│  │ Dark Web Index │ ✓ LIVE │ D (45%)   │ 2 hours ago│ ███████░░░░░░░░░░░ │ │
│  └────────────────┴────────┴───────────┴─────────────┴────────────────────┘ │
│                                                                              │
│  SOURCE ALERTS:                                                              │
│  ⚠️  Business Registry: Response time >30s (degraded performance)           │
│  ⚠️  Dark Web Index: 2 hours since last update (stale data)                 │
│                                                                              │
│  HISTORICAL RELIABILITY (30 days):                                           │
│  [████████████████████████████████████████████████████████████░░░░░░] 94%   │
│  Uptime: 99.2% | Failed Requests: 12 | Rate Limited: 45                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Recent Activity Feed Format

### Standard Activity Feed
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          RECENT ACTIVITY FEED                                │
│                      Auto-refresh: Every 30 seconds                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  TODAY (2024-01-27)                                                          │
│  ═══════════════════════════════════════════════════════════════════════    │
│                                                                              │
│  14:32:18  ●  New entity discovered                                          │
│            └─ Entity: Acme Corporation (Organization)                        │
│            └─ Source: Business Registry                                      │
│            └─ Confidence: 85% | Auto-verified: Yes                          │
│                                                                              │
│  14:15:42  →  Automated scan completed                                       │
│            └─ Duration: 3m 12s                                               │
│            └─ New items: 3 entities, 2 sources, 5 connections               │
│            └─ Status: Success                                                │
│                                                                              │
│  13:58:03  ✓  Source verification completed                                  │
│            └─ Source: LinkedIn Profile (linkedin.com/in/johndoe)            │
│            └─ Entity: John Doe (Person)                                      │
│            └─ Verification: Cross-referenced with 3 other sources            │
│                                                                              │
│  13:42:27  ⚠  Risk score updated                                             │
│            └─ Entity: John Doe (Person)                                      │
│            └─ Previous: MEDIUM (45/100)                                      │
│            └─ Current: HIGH (73/100)                                         │
│            └─ Reason: Financial fraud indicators detected                    │
│                                                                              │
│  12:30:15  ●  New connection identified                                      │
│            └─ From: John Doe (Person)                                        │
│            └─ To: Acme Corporation (Organization)                            │
│            └─ Type: Employment (CEO)                                         │
│            └─ Confidence: 92%                                                │
│                                                                              │
│  11:47:33  ℹ️  System notification                                           │
│            └─ Daily backup completed successfully                            │
│            └─ Size: 2.3 MB | Duration: 4s                                    │
│                                                                              │
│  ──────────────────────────────────────────────────────────────────────────  │
│                                                                              │
│  YESTERDAY (2024-01-26)                                                      │
│  ═══════════════════════════════════════════════════════════════════════    │
│                                                                              │
│  17:23:45  ✓  Report generated                                               │
│            └─ Type: Executive Brief                                          │
│            └─ Pages: 3 | Size: 45 KB                                         │
│            └─ Recipients: 2                                                  │
│                                                                              │
│  15:12:08  ●  Entity updated                                                 │
│            └─ Entity: John Doe (Person)                                      │
│            └─ Changes: Email address added, Phone updated                    │
│            └─ Sources: +2                                                    │
│                                                                              │
│  [F] Filter  [M] More history  [E] Export log  [C] Clear filter             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Compact Activity Stream
```
┌─────────────────────────────────────────────────────────────────────────────┐
│ ACTIVITY STREAM                                    [F]ilter [M]ore [E]xport │
├─────────────────────────────────────────────────────────────────────────────┤
│ 14:32 ●Entity "Acme Corp" added (85%)                                       │
│ 14:15 →Scan complete: +3E +2S +5C                                           │
│ 13:58 ✓LinkedIn verified for John Doe                                        │
│ 13:42 ⚠Risk ↑ John Doe: MEDIUM→HIGH (73) - Financial fraud                 │
│ 12:30 ●Connection: John Doe → Acme Corp (Employment, 92%)                   │
│ 11:47 ℹ️ Backup complete (2.3MB)                                             │
│ 10:23 ●Entity "Jane Smith" added (78%)                                       │
│ 09:15 ✓Twitter verified for Jane Smith                                       │
│ 08:42 →Daily scan started                                                    │
│ ───────────────────────────────────────────────────────────────────────────  │
│ Showing: Last 10 of 247 activities | Filter: None                            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Quick Action Menu Design

### Main Action Menu
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           QUICK ACTIONS                                      │
│                    Press number or click to execute                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ENTITY OPERATIONS              ANALYSIS TOOLS                               │
│  ┌─────────────────────────┐    ┌─────────────────────────────────────┐     │
│  │ [1] Add New Entity      │    │ [5] Search & Query                  │     │
│  │ [2] View Entity List    │    │ [6] Run Automated Analysis          │     │
│  │ [3] Edit Entity         │    │ [7] Compare Entities                │     │
│  │ [4] Archive Entity      │    │ [8] Find Connections                │     │
│  └─────────────────────────┘    └─────────────────────────────────────┘     │
│                                                                              │
│  VISUALIZATION                  REPORTING                                    │
│  ┌─────────────────────────┐    ┌─────────────────────────────────────┐     │
│  │ [V] View Relationship   │    │ [R] Generate Report                 │     │
│  │     Graph               │    │ [T] View Timeline                   │     │
│  │ [M] Show Risk Matrix    │    │ [E] Export Data                     │     │
│  │ [N] Network Topology    │    │ [P] Print Dashboard                 │     │
│  └─────────────────────────┘    └─────────────────────────────────────┘     │
│                                                                              │
│  SYSTEM                         HELP                                         │
│  ┌─────────────────────────┐    ┌─────────────────────────────────────┐     │
│  │ [S] Settings            │    │ [?] Help & Documentation            │     │
│  │ [B] Backup Data         │    │ [H] Command Reference               │     │
│  │ [I] Import Data         │    │ [A] About OSINT Investigator        │     │
│  │ [U] Update Sources      │    │ [L] Keyboard Shortcuts              │     │
│  └─────────────────────────┘    └─────────────────────────────────────┘     │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  [Q] Quit to Main Menu  |  [X] Exit Application  |  [D] Dashboard  │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Contextual Action Bar
```
┌─────────────────────────────────────────────────────────────────────────────┐
│  CONTEXT: Entity "John Doe" (Person) selected                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  [E]dit  [V]iew details  [C]onnections  [S]ources  [R]eports  [D]elete      │
│                                                                              │
│  Quick Actions:                                                              │
│  [1] Verify identity    [3] Find related     [5] Add note                   │
│  [2] View timeline      [4] Check risk       [6] Export entity              │
│                                                                              │
│  [Esc] Back  [Space] Select  [Enter] Confirm  [?] Help                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Responsive Text-Based UI

### Width Adaptations

#### 120+ Characters (Wide)
```
┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│[OSINT] PHOENIX | Entities: 47 ↑5 | Sources: 23 ↑2 | Findings: 34 ↑3 | Risk: HIGH (73) | Status: 78% | 14:32:18 | [●LIVE]│
├────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│┌──────────┐┌──────────┐┌──────────┐┌──────────┐┌──────────┐┌──────────┐┌────────────────────────────────────────────────┐│
││[1] Add   ││[2] Graph ││[3] Time  ││[4] Report││[5] Query ││[6] Export││[7] Settings  [8] Help  [9] Refresh  [Q] Quit    ││
││  Entity  ││          ││  line   ││          ││          ││          ││                                                ││
│└──────────┘└──────────┘└──────────┘└──────────┘└──────────┘└──────────┘└────────────────────────────────────────────────┘│
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

#### 80 Characters (Standard)
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[OSINT] PHOENIX | 47E 23S 34F | RISK: HIGH | 78% | 14:32:18 | [●LIVE]       │
├─────────────────────────────────────────────────────────────────────────────┤
│┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌──────────────┐│
││[1]Add  ││[2]Graph││[3]Time ││[4]Rpt  ││[5]Query││[6]Exprt││[7]Set [?]Help││
│└────────┘└────────┘└────────┘└────────┘└────────┘└────────┘└──────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

#### 60 Characters (Narrow)
```
┌──────────────────────────────────────────────────────────┐
│ PHOENIX | 47E 23S 34F | HIGH | 14:32                    │
├──────────────────────────────────────────────────────────┤
│[1]Add [2]View [3]Time [4]Report [5]More...              │
└──────────────────────────────────────────────────────────┘
```

#### 40 Characters (Mobile/Compact)
```
┌────────────────────────────────────────┐
│ PHOENIX|47E|HIGH|14:32                │
├────────────────────────────────────────┤
│[1]Add [2]View [3]Menu                │
└──────────────────────────────────────────────────────────┘
```

### Height Adaptations

#### Tall Terminal (40+ lines)
- Full dashboard with all components
- Extended activity feed (20+ entries)
- Multiple metric panels visible

#### Standard Terminal (24 lines)
- Primary metrics only
- Recent activity (5-8 entries)
- Collapsed sections

#### Short Terminal (12 lines)
- Critical metrics only
- Last 3 activities
- Minimal action menu

---

## 6. Example Dashboard States

### State 1: Initial Setup
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[LOGO] OSINT INVESTIGATOR v2.0           [CASE: NEW]        [○ IDLE]  14:32 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│                         WELCOME TO OSINT INVESTIGATOR                        │
│                                                                              │
│  To begin your investigation:                                                │
│                                                                              │
│  1. Create a new case or open existing one                                  │
│  2. Add your first target entity                                            │
│  3. Run initial reconnaissance                                              │
│                                                                              │
│  ┌───────────────────────────┐  ┌────────────────────────────────────────┐  │
│  │   [C] Create New Case     │  │   [O] Open Existing Case               │  │
│  │                           │  │                                        │  │
│  │   Start a fresh           │  │   Load a previously saved              │  │
│  │   investigation           │  │   investigation                        │  │
│  └───────────────────────────┘  └────────────────────────────────────────┘  │
│                                                                              │
│  ┌───────────────────────────┐  ┌────────────────────────────────────────┐  │
│  │   [I] Import Data         │  │   [T] Tutorial                         │  │
│  │                           │  │                                        │  │
│  │   Load data from file     │  │   Learn the basics                     │  │
│  │   or external source      │  │   of OSINT investigation               │  │
│  └───────────────────────────┘  └────────────────────────────────────────┘  │
│                                                                              │
│  Recent Cases: None                                                          │
│                                                                              │
│  [?] Help  [S] Settings  [A] About  [Q] Quit                                │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### State 2: Active Investigation
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[LOGO] OSINT INVESTIGATOR v2.0           [CASE: PHOENIX]    [● LIVE]  14:32 │
├─────────────────────────────────────────────────────────────────────────────┤
│┌──────────────┐┌──────────────┐┌──────────────┐┌──────────────────────────┐│
││  ENTITIES    ││   SOURCES    ││   FINDINGS   ││      RISK LEVEL          ││
││     47       ││     23       ││     34       ││      HIGH (73/100)       ││
││   [████]     ││   [██░░]     ││   [████]     ││   [██████████████░░░░]   ││
││  +5 today    ││  +2 today    ││  +3 today    ││      ↑ +12 pts           ││
│└──────────────┘└──────────────┘└──────────────┘└──────────────────────────┘│
│┌────────────────────────────────────┐┌─────────────────────────────────────┐│
││      RECENT ACTIVITY FEED          ││        QUICK ACTIONS                ││
││  14:32 ● New entity discovered     ││                                     ││
││  13:15 ✓ Source verified          ││  [1] Add Entity    [5] Run Query    ││
││  11:47 ⚠ Risk score updated       ││  [2] View Graph    [6] Export Data  ││
││  09:23 → Daily scan completed     ││  [3] Timeline      [7] Settings     ││
││  08:45 ● New connection found     ││  [4] Reports       [8] Help         ││
││                                    ││                                     ││
││  [M]ore activity...               ││  [Q]uit  [R]efresh  [S]earch        ││
│└────────────────────────────────────┘└─────────────────────────────────────┘│
│┌───────────────────────────────────────────────────────────────────────────┐│
││  STATUS: ████████████████████████████████░░░░ 78% | Phase: DEEP DIVE     ││
││  Next Milestone: Network Mapping (ETA: 3 days)                           ││
│└───────────────────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

### State 3: Analysis Mode
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[LOGO] OSINT INVESTIGATOR v2.0           [CASE: PHOENIX]    [● ANALYZING]   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│                         ANALYSIS IN PROGRESS                                 │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                                                                     │    │
│  │  Current Task: Cross-referencing entity connections                 │    │
│  │                                                                     │    │
│  │  Progress: [████████████████████████████████████░░░░░░░░░░░░░░░░]  │    │
│  │            75% complete                                            │    │
│  │                                                                     │    │
│  │  Processed: 35/47 entities | Estimated time remaining: 4m 23s      │    │
│  │                                                                     │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ACTIVE PROCESSES:                                                           │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │ Process                      │ Status  │ Progress │ ETA             │    │
│  ├──────────────────────────────┼─────────┼──────────┼─────────────────┤    │
│  │ Entity cross-reference       │ Running │ 75%      │ 4m 23s          │    │
│  │ Source verification          │ Running │ 60%      │ 6m 12s          │    │
│  │ Risk assessment update       │ Queued  │ 0%       │ 10m 00s         │    │
│  │ Network mapping              │ Queued  │ 0%       │ 15m 00s         │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [P] Pause  [C] Cancel  [D] Details  [B] Background mode                    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### State 4: Alert State
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[LOGO] OSINT INVESTIGATOR v2.0           [CASE: PHOENIX]    [● LIVE]  14:32 │
│╔═══════════════════════════════════════════════════════════════════════════╗│
│║                          ⚠️  CRITICAL ALERT  ⚠️                           ║│
│╠═══════════════════════════════════════════════════════════════════════════╣│
│║  Risk level has escalated from MEDIUM to CRITICAL for entity:            ║│
│║  "John Doe" (Person) - Financial fraud indicators detected               ║│
│╚═══════════════════════════════════════════════════════════════════════════╝│
├─────────────────────────────────────────────────────────────────────────────┤
│┌──────────────┐┌──────────────┐┌──────────────┐┌──────────────────────────┐│
││  ENTITIES    ││   SOURCES    ││   FINDINGS   ││      RISK LEVEL          ││
││     47       ││     23       ││     34       ││   CRITICAL (95/100)      ││
││   [████]     ││   [██░░]     ││   [████]     ││   [████████████████████] ││
││  +5 today    ││  +2 today    ││  +3 today    ││      ↑ +22 pts ⚠️        ││
│└──────────────┘└──────────────┘└──────────────┘└──────────────────────────┘│
│┌────────────────────────────────────┐┌─────────────────────────────────────┐│
││      RECENT ALERTS                 ││        IMMEDIATE ACTIONS            ││
││                                    ││                                     ││
││  ⚠️  14:32 CRITICAL RISK ESCALATION││  [1] View full risk report         ││
││  ⚠️  14:15 High-value target found ││  [2] Notify stakeholders           ││
││  ℹ️  13:58 Source verified         ││  [3] Export evidence               ││
││  ⚠️  11:47 Risk score updated      ││  [4] Create incident report        ││
││                                    ││  [5] Acknowledge alert             ││
││  [V]iew all alerts                 ││                                     ││
││                                    ││  [S]ilence for 1h  [D]ismiss       ││
│└────────────────────────────────────┘└─────────────────────────────────────┘│
│┌───────────────────────────────────────────────────────────────────────────┐│
││  ALERTS: 3 critical, 5 warnings | Action required within 1 hour          ││
│└───────────────────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

### State 5: Completed Investigation
```
┌─────────────────────────────────────────────────────────────────────────────┐
│[LOGO] OSINT INVESTIGATOR v2.0           [CASE: PHOENIX]    [○ COMPLETE]    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│                    ┌─────────────────────────────┐                          │
│                    │   INVESTIGATION COMPLETE    │                          │
│                    │                             │                          │
│                    │  Duration: 30 days          │                          │
│                    │  Status: FINALIZED          │                          │
│                    │  Quality Score: 89/100      │                          │
│                    └─────────────────────────────┘                          │
│                                                                              │
│  FINAL METRICS:                                                              │
│  ┌──────────────┐┌──────────────┐┌──────────────┐┌──────────────────────────┐│
│  │  ENTITIES    ││   SOURCES    ││   FINDINGS   ││      FINAL RISK          ││
│  │     52       ││     28       ││     41       ││      HIGH (78/100)       ││
│  │   [████]     ││   [████]     ││   [████]     ││   [████████████████░░░░] ││
│  │   Target: 50 ││   Target: 25 ││   Target: 40 ││   Peak: 95 (Day 18)      ││
│  └──────────────┘└──────────────┘└──────────────┘└──────────────────────────┘│
│                                                                              │
│  NEXT STEPS:                                                                 │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │ [R] Generate Final Report  |  [E] Export All Data  |  [A] Archive  │    │
│  │ [C] Close Case             |  [N] New Investigation               │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  Case archived on: 2024-02-15 | Archived by: [ANALYST] | Size: 4.2 MB       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Interactive Elements

### Input Forms
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          ADD NEW ENTITY                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  Entity Type: [Person ▼]                                                    │
│                                                                              │
│  Name:        [John Doe______________________________________]             │
│                                                                              │
│  Aliases:     [JD, Johnny____________________________________]             │
│                                                                              │
│  Description: [______________________________________________]             │
│               [______________________________________________]             │
│               [______________________________________________]             │
│                                                                              │
│  Confidence:  [████████████░░░░░░░░░░] 60% [+][-]                          │
│                                                                              │
│  Source:      [LinkedIn Profile_______________________________]             │
│                                                                              │
│  Tags:        [target] [person] [high-priority] [+]                         │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  [S]ave Entity  |  [A]dd Another  |  [C]ancel  |  [?] Help         │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Confirmation Dialogs
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          CONFIRM ACTION                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ⚠️  You are about to DELETE the following entity:                          │
│                                                                              │
│     Name: John Doe                                                           │
│     Type: Person                                                             │
│     Entities connected: 12                                                   │
│     Sources attached: 5                                                      │
│     Findings linked: 8                                                       │
│                                                                              │
│  This action CANNOT be undone.                                               │
│                                                                              │
│  Type "DELETE" to confirm: [__________]                                     │
│                                                                              │
│  [C]ancel  |  [D]elete Anyway  |  [A]rchive Instead                         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Search Interface
```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            SEARCH                                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  Search: [acme___________________________________________] [Search]        │
│                                                                              │
│  Filters: [All Types ▼] [Any Confidence ▼] [All Sources ▼] [+More]         │
│                                                                              │
│  Results (3 found):                                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │ ▶ 1. Acme Corporation (Organization) - 92% confidence               │    │
│  │      Matched: Name contains "acme"                                  │    │
│  │                                                                     │    │
│  │   2. John Doe @ Acme (Person) - 88% confidence                     │    │
│  │      Matched: Associated with "acme"                                │    │
│  │                                                                     │    │
│  │   3. acme-corp.com (Domain) - 95% confidence                       │    │
│  │      Matched: Domain name contains "acme"                           │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [↑↓] Navigate  [Enter] Select  [Esc] Clear  [S]ave Search                  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

*Version: 2.0 | Dashboard States: 5 | Last Updated: 2026-02-27*

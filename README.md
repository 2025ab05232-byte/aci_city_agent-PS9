# City Surveillance Agent – PS9
## BITS Pilani WILP | AIMLCZG557/AECLZG557 | Assignment 1
## Group ID: G121 | Submission Date: June 8, 2026

---

## Problem Summary

The City of Chennai has several tourist landmarks connected by lanes (roads).
A surveillance drone must monitor all lanes by traversing each lane **exactly once**.
Landmarks (nodes) may be visited more than once.
The drone operates on a battery, making it essential to find an efficient path.
This is solved using **Greedy Best First Search (GBFS)** algorithm.

---

## Files Included

```
G121_A1_PS9/
│
├── solutionPS9.py          ← Main Python code
├── solutionPS9.ipynb       ← Jupyter Notebook (same code)
├── inputPS9.txt            ← Input file (starting landmark)
├── outputPS9.txt           ← Generated output (surveillance path)
├── designPS9_G121.pdf      ← Design document
├── G121_Contribution.xlsx  ← Team contribution sheet
└── README.md               ← This file
```

---

## City Map (Graph)

```
Marina Beach ──────────────── Mahabalipuram ─────── Vandalur Zoo
     │                              │  \                  │
     │                              │   \                 │
Government Museum          Kovalam Beach  \_______________│
                                 │
                              Muttukadu
```

### Landmarks (Nodes)

| # | Landmark |
|---|----------|
| 1 | Marina Beach |
| 2 | Mahabalipuram |
| 3 | Vandalur Zoo |
| 4 | Kovalam Beach |
| 5 | Government Museum |
| 6 | Muttukadu |

### Lanes (Edges)

| From | To | Distance |
|------|----|----------|
| Marina Beach | Mahabalipuram | 60 km |
| Marina Beach | Government Museum | 3 km |
| Mahabalipuram | Vandalur Zoo | 55 km |
| Mahabalipuram | Kovalam Beach | 15 km |
| Mahabalipuram | Government Museum | 58 km |
| Vandalur Zoo | Kovalam Beach | 45 km |
| Kovalam Beach | Muttukadu | 10 km |

---

## How to Run

### Requirements
No external libraries needed.
Only standard Python is used:
- `heapq` — priority queue
- `collections` — deque and defaultdict
- `time` — execution timing
- `tracemalloc` — memory measurement
- `os` — file operations

### Step 1 — Create Input File
Create a file called `inputPS9.txt` with this content:
```
Enter Starting point: Marina Beach
```
You can change `Marina Beach` to any valid landmark.

### Step 2 — Run the Program
```bash
python solutionPS9.py
```

### Step 3 — Check Output
- Results print on screen
- `outputPS9.txt` is automatically created

---

## Valid Starting Points

| Landmark |
|----------|
| Marina Beach |
| Mahabalipuram |
| Vandalur Zoo |
| Kovalam Beach |
| Government Museum |
| Muttukadu |

---

## Algorithm: Greedy Best First Search (GBFS)

### What it Does
1. Starts at the input landmark
2. At each step looks at all unvisited neighboring lanes
3. Uses heuristic to pick the best next node
4. Moves there and marks that lane as covered
5. If stuck, backtracks using BFS to nearest uncovered lane
6. Repeats until all lanes are covered

### Heuristic Function
```
h(n) = -(number of unvisited edges at node n)
```
- Nodes with MORE unvisited edges get HIGHER priority
- Negative sign used because Python heap is a min-heap
- Guides drone to areas with most remaining work

### Example
```
Node A has 3 unvisited roads → h(A) = -3 → HIGH priority
Node B has 1 unvisited road  → h(B) = -1 → low priority
GBFS picks Node A first
```

---

## Data Structures Used

| Structure | Where Used | Purpose |
|-----------|-----------|---------|
| Adjacency List | CityGraph class | Store city map efficiently |
| Min-Heap (Priority Queue) | PriorityQueue class | Pick best next node |
| Set | visited_edges | Track covered lanes |
| List | path | Record surveillance route |
| Deque | BFS backtrack | Find nearest uncovered node |

---

## Sample Output

```
#################################################################
#      CITY SURVEILLANCE AGENT - PS9                           #
#      BITS Pilani WILP | GBFS Algorithm                       #
#################################################################

  Starting Point : Marina Beach
  Total Lanes    : 7

  Step  1: Marina Beach        --> Mahabalipuram      | 60 km  | Covered: 1/7
  Step  2: Mahabalipuram       --> Kovalam Beach       | 15 km  | Covered: 2/7
  Step  3: Kovalam Beach       --> Vandalur Zoo        | 45 km  | Covered: 3/7
  Step  4: Vandalur Zoo        --> Mahabalipuram       | 55 km  | Covered: 4/7
  Step  5: Mahabalipuram       --> Government Museum   | 58 km  | Covered: 5/7
  Step  6: Government Museum   --> Marina Beach        | 3 km   | Covered: 6/7
  Step  7: STUCK at Marina Beach. Backtracking...
  Step  8: BACKTRACK Marina Beach    --> Mahabalipuram
  Step  9: BACKTRACK Mahabalipuram   --> Kovalam Beach
  Step 10: Kovalam Beach       --> Muttukadu           | 10 km  | Covered: 7/7

  SURVEILLANCE MISSION COMPLETE!
  Lanes Covered  : 7 / 7
  Total Distance : 321 km
  Total Steps    : 9

Final Path:
Marina Beach --> Mahabalipuram --> Kovalam Beach --> Vandalur Zoo
--> Mahabalipuram --> Government Museum --> Marina Beach
--> Mahabalipuram --> Kovalam Beach --> Muttukadu
```

---

## Complexity

| Metric | Theoretical | Actual Measured |
|--------|-------------|-----------------|
| Time | O(E log E) | 16.0000 ms |
| Space | O(V + E) | 2.2207 KB |

Where:
- E = number of edges (lanes) = 7
- V = number of nodes (landmarks) = 6

---

## PEAS Description

| Component | Description |
|-----------|-------------|
| **Performance** | Cover all lanes once, minimize distance, save battery |
| **Environment** | Chennai city map, static, fully observable, deterministic |
| **Actuators** | Propellers, camera, battery system, navigation control |
| **Sensors** | GPS, camera, battery sensor, edge traversal tracker |

---

## Environmental Characteristics

| Property | Value |
|----------|-------|
| Observable | Fully Observable |
| Agents | Single Agent |
| Deterministic | Yes |
| Sequential | Yes |
| Static | Yes |
| Discrete | Yes |
| Known | Yes |

---

## Verification Report

| Lane | Times Traversed | Status |
|------|----------------|--------|
| Marina Beach ↔ Mahabalipuram | 2 | Covered (backtrack) |
| Marina Beach ↔ Government Museum | 1 | OK ✅ |
| Mahabalipuram ↔ Vandalur Zoo | 1 | OK ✅ |
| Mahabalipuram ↔ Kovalam Beach | 2 | Covered (backtrack) |
| Mahabalipuram ↔ Government Museum | 1 | OK ✅ |
| Vandalur Zoo ↔ Kovalam Beach | 1 | OK ✅ |
| Kovalam Beach ↔ Muttukadu | 1 | OK ✅ |

**All 7 lanes covered: YES ✅**

---

## Error Handling

| Situation | How It's Handled |
|-----------|-----------------|
| Input file not found | FileNotFoundError with clear message |
| Invalid file format | ValueError with expected format shown |
| Starting node not in graph | ValueError with valid options listed |
| Empty priority queue | IndexError with message |
| Duplicate edge added | Warning printed, edge skipped |
| Node not in graph | ValueError raised immediately |

---

## How the Code Works (Simple Explanation)

```
START
  │
  ▼
Print PEAS & Environment Info
  │
  ▼
Build City Map (6 landmarks, 7 lanes)
  │
  ▼
Read starting point from inputPS9.txt
  │
  ▼
Start Timer and Memory Tracker
  │
  ▼
┌─────────────────────────────────────┐
│  GBFS LOOP                          │
│                                     │
│  All roads covered? → NO → Continue │
│                                     │
│  Unvisited neighbor exists?         │
│    YES → Calculate heuristic        │
│          Pick best neighbor         │
│          Move there ✅              │
│    NO  → BACKTRACK 🔄              │
│          BFS to nearest             │
│          uncovered area             │
└─────────────────────────────────────┘
  │
  All roads covered? YES
  │
  ▼
Stop Timer and Memory Tracker
  │
  ▼
Display complete path on screen
  │
  ▼
Save path to outputPS9.txt
  │
  ▼
Verify all 7 lanes covered
  │
  ▼
Print Time (16ms) and Memory (2.2KB)
  │
  ▼
END ✅
```

---

## Submission Details

| Item | Value |
|------|-------|
| Group ID | G121 |
| Course | AIMLCZG557/AECLZG557 |
| Assignment | Assignment 1 - PS9 |
| Deadline | June 8, 2026 - 11:55 PM IST |
| Submission | Via Taxila portal |
| Zip File Name | G121_A1_PS9_XXXXXXXXXX.zip |

---

## Submission Checklist

- [ ] `solutionPS9.py` — Python code
- [ ] `solutionPS9.ipynb` — Jupyter Notebook
- [ ] `inputPS9.txt` — Input file
- [ ] `outputPS9.txt` — Output file
- [ ] `designPS9_G121.pdf` — Design document (max 4 pages)
- [ ] `G121_Contribution.xlsx` — Team contributions
- [ ] All zipped as `G121_A1_PS9_XXXXXXXXXX.zip`
- [ ] Submitted via Taxila before June 8, 2026 11:55 PM IST

---

*README prepared by Group G121 | BITS Pilani WILP | 2025-2026*

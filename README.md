# Generalized Klotski Puzzle Solver: BFS vs. A* Search

An academic project for the **Computational Intelligence** course demonstrating the modeling, implementation, and empirical comparison of state-space graph search algorithms: **Breadth-First Search (BFS)** (uninformed) and **A\* Search** (informed / heuristic-guided).

---

## 📌 Project Overview

**Generalized Klotski** is a classic sliding-block puzzle played on an $R \times C$ grid. The board contains empty spaces (`.`) and rigid rectangular pieces of various dimensions.

The objective is to slide the pieces horizontally and vertically until the designated **target piece** (e.g., piece `A`) reaches the target exit at the bottom edge of the board.

### Problem Formulation:
* **State Representation:** Immutable, hashable 2D grid structure (`tuple` of `tuples`).
* **Transition Model (Unit-Step Metric):** Each valid action slides a single piece by exactly **1 grid cell** (`UP`, `DOWN`, `LEFT`, or `RIGHT`) into unoccupied space. Every move carries a uniform step cost of $c = 1$.
* **Goal Condition:** The target piece completely occupies the designated bottom exit columns.

---

## 🚀 Key Features & Architectural Highlights

1. **State Canonicalization (Symmetry Breaking):**
   * Puzzles contain identical pieces (e.g., multiple $1 \times 1$ or $2 \times 1$ blocks). Swapping their positions creates symmetric, redundant states.
   * A canonicalization function deterministically re-labels identical pieces based on board scan order. This reduces the reachable state space by over 80% without affecting path optimality, allowing hard benchmarks (`p04`, `p05`) to solve in seconds.
2. **Admissible Heuristic Function ($h(n)$ for A\*):**
   * **Manhattan Distance:** Horizontal and vertical grid distance between target piece `A` and the goal exit.
   * **Blocking Obstacle Penalty:** Counts unique obstacle pieces directly blocking the exit corridor below `A`.
   * **Admissibility ($h(n) \le h^*(n)$):** Guarantees optimal (shortest-path) solutions for A\*.
3. **Completeness & Unsolvability Detection:**
   * Fully detects unsolvable configurations (such as benchmark `p06`) by exhaustively traversing the reachable state component.

---

## 📊 Benchmark Results

Evaluated across the 6 standard benchmark instances (`p01`–`p06`):

| Problem | Board Size | Algorithm | Solved | Optimal Moves | Nodes Explored | Wall-Clock Time (s) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **p01** | $3 \times 3$ | **BFS** | `True` | **8** | 80 | 0.0047 |
| | | **A\*** | `True` | **8** | **53** | **0.0044** |
| **p02** | $4 \times 4$ | **BFS** | `True` | **18** | 247 | 0.0207 |
| | | **A\*** | `True` | **18** | **200** | **0.0189** |
| **p03** | $4 \times 4$ | **BFS** | `True` | **45** | 2103 | **0.1235** |
| | | **A\*** | `True` | **45** | **2082** | 0.1617 |
| **p04** | $5 \times 4$ | **BFS** | `True` | **116** | 23,954 | **1.5465** |
| | | **A\*** | `True` | **116** | **23,769** | 2.1258 |
| **p05** | $5 \times 4$ | **BFS** | `True` | **179** | 39,540 | 3.8068 |
| | | **A\*** | `True` | **179** | **37,570** | **3.6300** |
| **p06** | $4 \times 4$ | **BFS** | `False` | 0 | 833 | 0.0451 |
| | | **A\*** | `False` | 0 | 833 | 0.0591 |

### Key Experimental Insights:
* **Optimality:** Both BFS and A\* discover identical optimal move sequences across all solvable instances.
* **Exploration Efficiency:** A\* consistently expands fewer states than BFS by pruning unpromising branches.
* **Computational Trade-off:** While A\* explores fewer states, the per-node overhead of heuristic calculation and heap operations (`heapq`) can result in slightly higher wall-clock execution times when the search space reduction is moderate.
* **Completeness:** On `p06`, both algorithms traverse exactly 833 states before proving no solution exists.

---

## 🛠️ Repository Structure

```text
├── Klotski_Search_Project.ipynb   # Main Jupyter Notebook (code, tests & explanations)
├── klotski_problems.txt           # Benchmark problem instances (p01 - p06)
└── README.md                      # Academic project documentation
```

---

## 💻 Requirements & Getting Started

### Prerequisites
* Python 3.8+
* Required libraries:
  ```bash
  pip install pandas
  ```
  *(All graph search algorithms utilize Python standard libraries: `collections.deque`, `heapq`, and `time`)*.

### Running the Notebook
1. Clone or download this repository.
2. Ensure `klotski_problems.txt` is in the same directory as the notebook.
3. Open and run all cells in `Klotski_Search_Project.ipynb`:
   ```bash
   jupyter notebook Klotski_Search_Project.ipynb
   ```
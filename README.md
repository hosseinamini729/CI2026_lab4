# Generalized Klotski Puzzle Solver: BFS vs. A\* Search

An academic computational intelligence project comparing uninformed (**Breadth-First Search**) and informed (**A\* Search**) algorithms on the classic **Generalized Klotski** sliding-block puzzle benchmark.

---

## 1. Problem Overview

The Generalized Klotski puzzle consists of a rectangular grid populated by rectangular pieces of varying dimensions ($1 \times 1$, $1 \times 2$, $2 \times 1$, $2 \times 2$) and open cells (`.`). The objective is to guide a designated target piece (`A`) to the bottom exit corridor in the minimum number of valid moves.

- **Action Metric:** Unit-step metric (sliding a piece by exactly 1 grid cell horizontally or vertically equals 1 move).
- **Step Cost:** Uniform cost of $1$ per slide ($c = 1$).
- **Optimality Condition:** Achieving the goal state with the minimal sequence of 1-cell slides.

---

## 2. Methodology & Key Components

### 2.1 State Representation & Symmetry Breaking

- **Board Representation:** Immutable, hashable tuple of tuples.
- **Canonical Board Mapping (`canonical_board`):** Pieces sharing identical geometric dimensions are deterministically re-labeled upon board traversal. This breaks symmetry and collapses duplicate permutation branches, slashing memory requirements and search time by over 80%.

### 2.2 Breadth-First Search (BFS)

- **Strategy:** Uninformed, level-by-level exhaustive exploration.
- **Frontier:** FIFO queue using `collections.deque` ($O(1)$ push/pop).
- **Guarantees:** Complete and optimal under uniform step costs.

### 2.3 A\* Search & Heuristic Formulation

- **Evaluation Function:** $f(n) = g(n) + h(n)$
- **Admissible Heuristic ($h(n)$):**
  $$h(n) = h_{\text{Manhattan}}(\text{target}, \text{exit}) + \text{blocking\_obstacles}$$
  - _Manhattan Distance:_ Absolute horizontal and vertical steps required on an empty grid.
  - _Blocking Obstacles:_ Count of unique obstacle pieces occupying the vertical path between target piece `A` and the bottom boundary.
  - _Admissibility Proof:_ Since obstacles require at least one move to clear out of the corridor, $h(n) \le h^*(n)$ holds universally, ensuring solution optimality.
- **Frontier:** Priority Queue implemented as a Min-Heap using `heapq`.

---

## 3. Installation & Setup

1. **Clone the repository:**

```bash
git clone https://github.com/hosseinamini729/CI2026_lab4
cd CI2026_lab4
```

2. **Create and activate a virtual environment (optional but recommended):**

```bash
python -m venv venv

# On Windows:
.\venv\Scripts\activate

# On Linux/macOS:
source venv/bin/activate

```

3. **Install required dependencies:**

```bash
pip install -r requirements.txt

```

4. **Launch the Jupyter Notebook:**

```bash
jupyter notebook Klotski_Search_Project.ipynb

```

## 4. Performance Visualization & Analysis

### Key Analytical Takeaways:

1. **Empirical Optimality:** BFS and A\* found identical move counts across all solvable instances, verifying the admissibility of $h(n)$.

2. **Search Space Pruning:** A\* systematically explores fewer states than BFS across all solvable instances.

3. **Behavioral Convergence in Complex Instances:** In deep, constrained puzzles (`p04`, `p05`), piece `A` is blocked at the top while surrounding pieces undergo extensive cyclical maneuvers. Because $h(n) \ll h^*(n)$, $f(n) \approx g(n)$, causing A\* to explore states similarly to BFS.

4. **Algorithmic Overhead & Runtime:** In instances where state reduction is modest, the constant per-node overhead of Priority Queue heap operations ($O(\log N)$) makes A\* slightly slower in wall-clock time compared to the lightweight $O(1)$ deque used in BFS.

5. **Completeness on Unsolvable Problems:** For `p06`, both algorithms exhaustively traversed the exact same 833 reachable states and cleanly terminated with `Solved: False`, demonstrating complete state-space exploration.

---

## 5. Repository Structure

```text
├── klotski_problems.txt            # Benchmark problem instances
├── Klotski_Search_Project.ipynb    # Main notebook (code, explanations, experiments)
├── requirements.txt                # Required Python packages
└── README.md                       # Comprehensive documentation

```

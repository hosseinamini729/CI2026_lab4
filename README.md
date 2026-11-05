# Computational Intelligence Project: Solving Generalized Klotski using BFS and A\* Search

## 1. Project Introduction

Search algorithms form a foundational pillar of Artificial Intelligence and Computational Intelligence, enabling autonomous agents to solve complex puzzles, plan actions, and navigate large state spaces. In this project, we implement, analyze, and compare two classic graph search algorithms—**Breadth-First Search (BFS)** as an uninformed search method, and **A\* Search** as an informed (heuristic-guided) search method—to find optimal solutions for the **Generalized Klotski** sliding-block puzzle.

## 2. Problem Definition

**Generalized Klotski** is played on a rectangular grid of size $R \times C$ containing empty cells (`.`) and various rectangular pieces of different dimensions (e.g., $1 \times 1$, $1 \times 2$, $2 \times 1$, $2 \times 2$). Cells sharing the same alphanumeric label belong to a single rigid piece that must move as one unit.

- **Initial State:** The starting configuration of pieces on the $R \times C$ board as specified in the problem instance.
- **Valid Actions (Unit-Step Metric):** In our problem formulation, a single valid move consists of sliding any single piece by **exactly one grid cell** in one of four cardinal directions (`UP`, `DOWN`, `LEFT`, `RIGHT`), provided that all destination cells newly occupied by that piece are currently empty (`.`) and within board boundaries. Every 1-cell slide has a uniform step cost of $1$.
- **Goal State:** Any board configuration in which the designated `target` piece (e.g., piece `A`) sits at the bottom edge of the board and completely covers the specified `exit bottom` column indices (0-indexed from left to right).

## 3. Background and Theoretical Concepts

To solve Klotski systematically, we model the puzzle as a **State-Space Graph**:

- **Node (State):** Represents a unique arrangement of all pieces on the board at a given moment.
- **Edge (Action/Transition):** Connects one board state to another via a single valid 1-cell piece movement.
- **Search Space:** The complete set of all reachable board configurations starting from the initial board.
- **Path:** A sequence of valid moves transforming the initial state into another state. An **optimal path** achieves the goal state in the minimum possible number of moves.
- **Uninformed vs. Informed Search:**
  - _Uninformed (Blind) Search_ explores the search space systematically without any clue about which states are closer to the goal (e.g., **BFS**).
  - _Informed (Heuristic) Search_ leverages domain-specific knowledge via a heuristic function $h(n)$ to estimate how many moves remain to reach the goal, prioritizing more promising states (e.g., **A\***).

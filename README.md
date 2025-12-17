# SAT Solver & Sudoku Solver

This project was developed as part of the **AAD (Algorithms and Design)** course and implements two complementary approaches for solving combinatorial constraint problems:

1. A **dedicated Backtracking-based solver** with classic CSP heuristics  
2. A **general-purpose DPLL-based SAT solver** capable of solving multiple NP-complete problems via CNF encodings

The system solves **Sudoku puzzles of arbitrary N × N size** and demonstrates the expressive power of SAT by solving **N-Queens** and **Graph Coloring**. The project includes a comprehensive benchmarking and visualization pipeline to empirically evaluate the impact of modern heuristics such as **MRV**, **Forward Checking**, **Unit Propagation**, **Pure Literal Elimination**, and **VSIDS**.

---

## Core Capabilities

### Solvers and Algorithms

- **Parameterized Sudoku Solver**  
  Solves N × N Sudoku instances (where N is a perfect square: 4, 9, 16, …) loaded from external files.

- **Backtracking Solver**  
  A recursive CSP solver enhanced with:
  - Minimum Remaining Values (MRV)
  - Forward Checking (FC)

- **DPLL-based SAT Solver**  
  A configurable SAT solver operating on arbitrary CNF formulas, supporting:
  - Unit Propagation (BCP)
  - Pure Literal Elimination
  - VSIDS-based variable selection

- **Additional SAT Benchmarks**
  - **N-Queens**: CNF generation and solving for arbitrary board sizes
  - **Graph Coloring**: K-coloring of graphs loaded from edge lists

---

## Benchmarking and Analysis

- **Exhaustive Heuristic Evaluation**  
  Automatically evaluates **11+ solver configurations**, covering all combinations of heuristics for both solvers.

- **Timeout Safety**  
  Each solver configuration is capped at **30 seconds**, preventing pathological instances from stalling execution.

- **Structured Metrics Collection**  
  For every run, the system records:
  - Execution time
  - Number of decisions
  - Number of backtracks
  - Unit propagations (SAT)
  - Success / timeout status

- **CSV Export**  
  Results are saved to:
  - `results/backtracking_results.csv`
  - `results/sat_results.csv`

- **Solution Verification**  
  Sudoku solutions are rigorously validated (rows, columns, subgrids), with verification results logged explicitly.

- **Persistent Logging**  
  All terminal output is mirrored into structured log files under `results/`.

---

## Building the Project

The solver uses `std::async` for timeout handling and therefore requires pthread support.

Compile with optimizations enabled:

    g++ -o sat_solver sat_solver.cpp -std=c++17 -O2 -lpthread

(On Windows, the same command works in PowerShell with MinGW.)

---

## Running the Solvers

### Sudoku Benchmarks

Provide a puzzle file containing the grid size followed by the grid values (0 represents empty cells).

    ./sat_solver test_cases/easy.txt
    ./sat_solver test_cases/medium.txt
    ./sat_solver test_cases/hard.txt

Each run:
- Executes all solver configurations
- Enforces per-configuration timeouts
- Logs detailed statistics
- Exports CSV results
- Verifies correctness

---

### N-Queens

Run the solver with the `--nqueens` flag:

    ./sat_solver --nqueens 8
    ./sat_solver --nqueens 12

The solver generates the CNF encoding, solves it using DPLL, and reports the board configuration.

---

### Graph Coloring

Provide the number of colors and an edge list file:

    ./sat_solver --gcolor 3 graph_edges.txt

Each line of the edge file contains two integers representing an undirected edge.

---

## Visualization Pipeline

After generating CSV results, performance graphs can be created using the provided Python script.

Install dependencies:

    pip install pandas matplotlib seaborn

Run visualization:

    python visualize.py

This produces five analysis plots in the `analysis/` directory, including:
- Backtracking heuristic comparison
- SAT heuristic comparison
- Unit propagation impact
- Backtracking vs SAT performance
- Decision and backtrack behavior analysis

---

## Implementation Highlights

### Key Optimizations

- **Trail-based Assignment Tracking**  
  The SAT solver uses a trail structure to undo assignments efficiently, avoiding expensive state copying.

- **Efficient Domain Representation**  
  The backtracking solver uses compact domain structures for fast pruning.

- **Asynchronous Execution Model**  
  Each solver configuration executes independently with timeout protection.

---

## Supported Problem Classes

- **Sudoku** — General N × N CSP benchmark  
- **N-Queens** — Canonical SAT encoding problem  
- **Graph Coloring** — Constraint satisfaction on arbitrary graphs  

These problems serve as controlled benchmarks to evaluate solver behavior under different constraint structures.

---

## Project Structure

    ├── sat_solver.cpp
    ├── visualize.py
    ├── Makefile
    ├── include/
    ├── src/
    ├── test_cases/
    ├── results/
    └── analysis/

---

## Educational Value

This project emphasizes:
- Algorithmic trade-offs in CSP and SAT solving
- The practical impact of heuristics on exponential search
- Benchmarking methodology and empirical evaluation
- Translating theoretical algorithms into robust implementations

It is suitable for **systems, algorithms, and software engineering interviews**, as well as further experimentation with SAT-based problem solving.

---

## Limitations

- The SAT solver is based on DPLL rather than CDCL, and therefore does not include clause learning, non-chronological backtracking, or learned clause databases.

- Heuristic evaluation is single-threaded per configuration; solver configurations are benchmarked sequentially rather than in parallel.

- CNF encodings for N-Queens and Graph Coloring are naïve and correctness-focused, not optimized for minimal clause or variable counts.

- The system does not support incremental SAT solving; each problem instance is solved from scratch.

- Memory usage is optimized for clarity and traceability rather than large-scale industrial instances.

- The visualization pipeline analyzes post-execution CSV logs and does not provide real-time profiling.

- The solver is designed for research and educational benchmarking, not as a drop-in replacement for production SAT solvers.

  ---

  ## Conclusion

This project demonstrates a practical and empirical exploration of constraint satisfaction and SAT solving, bridging theoretical algorithms with real-world implementation concerns. By implementing both a heuristic-driven backtracking solver and a general-purpose DPLL-based SAT solver, the project highlights how different algorithmic choices and heuristics affect performance on exponential search problems.

Through systematic benchmarking and visualization, the project shows that techniques such as Unit Propagation, MRV, Forward Checking, and VSIDS are not merely optimizations but are often essential for tractable solving. Extending the SAT solver to problems like Sudoku, N-Queens, and Graph Coloring further demonstrates the expressiveness and generality of SAT as a unifying problem-solving framework.

Overall, this work emphasizes algorithmic trade-offs, empirical evaluation, and solver design principles, making it a strong foundation for further exploration in SAT, CSPs, and optimization-based problem solving, as well as a solid discussion piece for systems and algorithms interviews.

---

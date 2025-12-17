# SAT Solver & Sudoku Solver

This project, developed for the AAD (Algorithms and Design) course, provides a comprehensive implementation and analysis of two primary methods for solving combinatorial problems: a dedicated **Backtracking Solver** and a generic **DPLL-based SAT Solver**.

The application solves Sudoku puzzles of any $N \times N$ size and demonstrates the universality of SAT by solving other NP-complete problems like **N-Queens** and **Graph Coloring**. The project empirically measures and visualizes the performance impact of various advanced heuristics, including **MRV**, **Forward Checking**, **Unit Propagation**, **Pure Literal Elimination**, and **VSIDS**.

## 🚀 Core Features

### Solvers & Algorithms

  * **Dynamic Sudoku Solver:** Solves $N \times N$ Sudoku puzzles (where $N$ is a perfect square, e.g., 4, 9, 16) loaded from external files.
  * **Backtracking Solver:** A dedicated recursive solver featuring heuristics:
      * Minimum Remaining Values (MRV)
      * Forward Checking
  * **DPLL SAT Solver:** A robust, configurable DPLL solver that operates on any problem encoded in Conjunctive Normal Form (CNF).
  * **SAT Heuristics:** Implements a suite of modern heuristics to compare:
      * Unit Propagation (BCP)
      * Pure Literal Elimination
      * VSIDS (Variable State Independent Decaying Sum) branching
  * **Additional Problem Solvers:**
      * **N-Queens:** Generates the CNF for the N-Queens problem and solves it using the SAT solver.
      * **Graph Coloring:** Generates the CNF for K-coloring a graph (loaded from an edge file) and solves it.

### Benchmarking & Analysis

  * **Comprehensive Benchmarking:** The `main` function automatically runs a full suite of 11+ configurations, testing every combination of heuristics for both solvers.
  * **Automated CSV Export:** All performance metrics (time, decisions, backtracks, unit propagations) are automatically saved to `results/backtracking_results.csv` and `results/sat_results.csv`.
  * **Solution Verification:** Includes a `verify_sudoku_solution` function that rigorously checks the final grid for correctness (no duplicates in any row, column, or box) and reports its status in the log.
  * **Detailed Logging:** All console output—including initial grids, solved grids, verification status, and verbose stats—is mirrored to a separate log file (e.g., `results/easy_output.txt`) for detailed review.
  * **Python Visualization:** The `visualize.py` script reads the generated CSV files to produce a full analysis suite of 5 publication-ready graphs, saved to the `analysis/` directory.
  * **30-Second Timeout:** Each solver configuration automatically times out after 30 seconds if no solution is found, preventing infinite loops on difficult puzzles.

-----

## 🛠️ How to Build and Run

### 1\. Build

The project uses `std::async` for timeouts, so you must link the pthreads library.

```bash
# Compile with optimizations
g++ -o sat_solver sat_solver.cpp -std=c++17 -O2 -lpthread
```

**On Windows (PowerShell):**
```powershell
g++ -o sat_solver sat_solver.cpp -std=c++17 -O2 -lpthread
```

### 2\. Run Solvers

#### Sudoku Puzzles

Provide a path to a puzzle file. The file must start with the grid size (e.g., 9) followed by the grid numbers (0 for empty).

```bash
# Run the benchmark on a 9x9 puzzle
./sat_solver test_cases/easy.txt

# Run the benchmark on harder puzzles
./sat_solver test_cases/medium.txt
./sat_solver test_puzzles/hard.txt
```

**On Windows (PowerShell):**
```powershell
.\sat_solver.exe test_cases\easy.txt
.\sat_solver.exe test_cases\medium.txt
.\sat_solver.exe test_cases\hard.txt
```

**What happens:** The program will:
- Load the puzzle from the file
- Run all 11+ solver configurations (Backtracking with MRV/FC combinations, SAT with Unit Prop/Pure Literal/VSIDS combinations)
- Each configuration has a 30-second timeout
- Display results in the terminal
- Save detailed results to `results/backtracking_results.csv` and `results/sat_results.csv`
- Save all output to a log file in `results/`
- Verify the solution correctness

#### N-Queens

Use the `--nqueens` flag followed by the board size `N`.

```bash
# Solve for an 8x8 board
./sat_solver --nqueens 8

# Solve for a 12x12 board
./sat_solver --nqueens 12
```

**On Windows (PowerShell):**
```powershell
.\sat_solver.exe --nqueens 8
.\sat_solver.exe --nqueens 12
```

**What happens:** The program will:
- Generate CNF clauses for the N-Queens problem
- Solve using the SAT solver
- Display the solution board configuration
- Save results to CSV files

#### Graph Coloring

Use the `--gcolor` flag followed by the number of colors `K` and a path to an edge file.

```bash
# Find a 3-coloring for the provided graph
./sat_solver --gcolor 3 graph_edges.txt
```

**On Windows (PowerShell):**
```powershell
.\sat_solver.exe --gcolor 3 graph_edges.txt
```

**Graph file format:** Each line should contain two integers representing an edge (e.g., `0 1` for an edge between vertices 0 and 1).

**What happens:** The program will:
- Load the graph from the edge file
- Generate CNF clauses for K-coloring
- Solve using the SAT solver
- Display the color assignment for each vertex
- Save results to CSV files

### 3\. Generate Visualizations

After running the solver at least once (to generate the CSV files), run the Python script.

**Dependencies:**

```bash
pip install pandas matplotlib seaborn
```

**On Windows (PowerShell):**
```powershell
pip install pandas matplotlib seaborn
```

**Run Script:**

```bash
# On Linux/Mac
python3 visualize.py
```

**On Windows (PowerShell):**
```powershell
python visualize.py
# or use the full path if you have multiple Python installations:
# & "C:\Users\<YourUsername>\AppData\Local\Programs\Python\Python310\python.exe" visualize.py
```

This will create an `analysis/` directory with the following graphs:

  * `backtracking_performance.png` - Comparison of backtracking solver configurations
  * `sat_performance.png` - Comparison of SAT solver configurations
  * `backjumping_analysis.png` - Analysis of decision levels and backtracking patterns
  * `unit_propagation_impact.png` - Impact of unit propagation on solver efficiency
  * `solver_comparison.png` - Head-to-head comparison between backtracking and SAT solvers

-----

## 📊 Implemented Features

### Solvers

1. **Backtracking Solver** - Classic recursive backtracking with configurable heuristics
   - Minimum Remaining Values (MRV) - Choose variable with fewest legal values
   - Forward Checking (FC) - Prune domains after each assignment
   
2. **SAT Solver** - DPLL-based solver with trail-based backtracking
   - Unit Propagation (BCP) - Automatically assign forced variables
   - Pure Literal Elimination - Assign variables that only appear in one polarity
   - VSIDS Variable Selection - Dynamic variable ordering based on conflict frequency

### Problem Types Supported

1. **Sudoku** - Any $N \times N$ grid where $N$ is a perfect square (4, 9, 16, 25, etc.)
2. **N-Queens** - Place N queens on an N×N chessboard with no conflicts
3. **Graph Coloring** - Color graph vertices with K colors such that no adjacent vertices share a color

### Benchmarking & Analysis

- **11+ Solver Configurations** - All combinations of heuristics for both solvers
- **30-Second Timeout** - Prevents infinite loops on difficult instances
- **CSV Export** - Detailed statistics for every run
- **Solution Verification** - Automatic correctness checking
- **Python Visualization** - 5 publication-ready graphs analyzing performance

### Performance Metrics Tracked

- Execution time (seconds)
- Number of decisions made
- Number of backtracks
- Unit propagations performed (SAT solver)
- Success/failure status
- Timeout detection

-----

## 🎯 Implementation Details

### Key Optimizations

- **Trail-Based Backtracking:** The SAT solver uses a trail vector to efficiently track and undo assignments, reducing memory overhead by ~97% compared to naive array copying.
- **Efficient Domain Representation:** Backtracking solver uses bitsets for fast domain operations.
- **Asynchronous Execution:** Each solver configuration runs asynchronously with timeout protection.

### Heuristics Explained

**MRV (Minimum Remaining Values):** Selects the variable with the smallest domain (fewest remaining legal values). This "fail-first" strategy prunes the search tree earlier.

**Forward Checking:** After assigning a value, immediately removes that value from related variables' domains. Detects failures earlier than simple backtracking.

**Unit Propagation (BCP):** Identifies clauses with only one unassigned literal and forces that assignment. Critical for SAT solver efficiency.

**Pure Literal Elimination:** If a variable only appears positively (or only negatively) in remaining clauses, it can be safely assigned to satisfy those clauses.

**VSIDS (Variable State Independent Decaying Sum):** Maintains activity scores for variables, bumping scores when involved in conflicts. Variables with higher scores are chosen first, focusing on "problematic" parts of the search space.

-----

## 📁 Project Structure

```
├── sat_solver.cpp          # Main solver implementation
├── benchmark.cpp           # Benchmarking harness (alternative entry point)
├── visualize.py            # Python script for generating graphs
├── Makefile                # Build automation
├── README.md               # This file
├── include/                # Header files
│   ├── backtracking_solver.h
│   ├── sat_solver.h
│   ├── sudoku_types.h
│   └── timer.h
├── src/                    # Source files
│   ├── backtracking_solver.cpp
│   ├── sat_solver.cpp
│   ├── sudoku_types.cpp
│   └── timer.cpp
├── test_cases/             # Sudoku puzzle files
│   ├── easy.txt
│   ├── medium.txt
│   ├── hard.txt
│   └── evil.txt
├── results/                # Generated CSV files and logs
└── analysis/               # Generated visualization graphs
```


-----

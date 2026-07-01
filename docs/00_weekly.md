# Weekly Progress Log

> Update this file **every week**. Add a new entry at the top for each week.
> This is the first thing we check during review. Keep it honest and specific — it also feeds your attendance record (Rule 1).

**How to use:** copy the *Week template* block below for each new week. Newest week goes at the top.

---

## Week template — copy me

### Week N — YYYY-MM-DD

**Attended this week's meeting:** Yes / No (if No, did you email leave? Yes / No)

**Progress this week**
- _What did you actually do / finish?_

**Challenges & blockers**
- _What got in the way? What are you stuck on?_

**Next steps**
- _What will you do next week?_

**Hours spent (optional):** _e.g. 6h_

**Links (optional):** _commits, notebooks, docs, datasets..._

---

<!-- =================  YOUR ENTRIES BELOW  ================= -->

### Week 2 — 2026-06-29

**Attended this week's meeting:** Yes

**Progress this week**
- _What did you actually do / finish?_
  - Successfully recreated the baseline environments for POMO and GA.
  - Implemented dynamic instance scaling for GA to support comparative analysis across 50 and 100 client nodes.
  - Integrated a timing mechanism into the GA framework to capture precise execution runtimes.
  - Conducted performance comparison experiments; results are summarized below.

| Methodology | Problem Scale | Feasibility Status | Objective Value | Total Runtime (s) |
| :--- | :--- | :--- | :--- | :--- |
| POMO (Basic CVRP) | 50 | Fully Feasible | 10.92 | 19.8 |
| POMO (Basic CVRP) | 100 | Fully Feasible | 15.83 | 105.0 |
| GA (VRPTW) | 50 | Feasible (Satisfies E/TW) | 17392.41 | 0.44 |
| GA (VRPTW) | 100 | Feasible (Satisfies E/TW) | 34056.65 | 3.67 |
| OR (MILP ETRP exact model, with energy E constraint) | 50 | Optimal & Fully Feasible | 51.0 | 0.21 |
| OR (MILP ETRP exact model, with energy E constraint) | 100 | Optimal & Fully Feasible | 101.0 | 14.09 |

**Challenges & blockers**
- _What got in the way? What are you stuck on?_
  - **Constraint Implementation:** Implementing E (Capacity) and TW (Time Window) constraints in the baseline code required manual intervention in the evaluation functions (penalty mechanisms). 
  - **Data Scalability:** The standard Solomon datasets are primarily scaled for 100 nodes. I implemented a dynamic truncation method to allow the GA to evaluate 50-node instances, ensuring consistency in performance comparison.
  - **OR Modeling:** Currently transitioning to the MILP formulation for the UAV-Truck problem; setting up the objective function and decision variables is the immediate blocker.

**Next steps**
- _What will you do next week?_
  - Finalize the MILP (OR) model formulation for the UAV-Truck problem.
  - Draft the comparison reflection document, specifically analyzing the trade-offs between heuristic-based approaches (GA/POMO) and exact methods (OR).
  - Investigate the impact of E/TW constraints on the convergence rates of different algorithms.

**Hours spent:** 12h

**Links (optional):** _commits, notebooks, docs, datasets..._

---

### Week 1 — 2026-06-12

**Attended this week's meeting:** Yes 

**Progress this week**
- Set up a repository from the FURP template.
- Complete Project Info in repo root README.md
- Finished Python + OR-Tools environment deployment, full environment record as below:
  - Operating system: macOS Sequoia 15.1
  - Python version: Python 3.13.5
  - Package manager: pip
  - OR-Tools solver version: 9.15.6755
  - Exact install command: `python -m pip install ortools`
  - Runtime hardware: Apple M4, 16GB RAM
- Run tiny VRPTW instance smoke test, saved all required outputs
  - command: `time python vrptw_smoke_test.py`
  - instance name and size: Tiny VRPTW, 1 depot + 16 customer nodes, 4 vehicles
  - objective value: 71
  - feasibility status: Feasible
  - runtime: 0.113s (Total)
  - textual route output: <img width="762" height="282" alt="VRPTW route output screenshot" src="../src/results/vrptw_smoke_test_result.png" />
- Write a reflection paragraph: The time window constraint for each customer was the easiest constraint to understand, as it simply defines a fixed delivery time range that each vehicle must comply with and the printed solution window clearly shows the valid arrival interval for every node. The distinction between the objective value and the total route runtime in the terminal output was confusing at first, since I initially mixed up weighted cost counted by the objective function and the simple summation of physical travel and waiting times. For Week 2, the baseline target is complete VRPTW literature review, sort out standard VRP data formats, and prepare readable notes & problem diagram based on the OR-Tools classical VRPTW baseline.

**Challenges & blockers**
- Understanding the mapping between OR-Tools' internal routing indices (Index) and original node identifiers (NodeIndex) was initially challenging; it required explicit conversion using manager.IndexToNode.
- Confusion arose regarding the definition of the "Objective Value," but it was clarified after analyzing the solver’s cost evaluation and finalizer functions.

**Next steps**
- complete VRPTW literature review, sort out standard VRP data formats, and prepare readable notes & problem diagram based on the OR-Tools classical VRPTW baseline.

**Hours spent (optional):10h

**Links**
- OR-Tools VRPTW official reference: https://developers.google.com/optimization/routing/vrptw
- Repository VRPTW source file: [src/experiments/vrptw_smoke_test.py](src/experiments/vrptw_smoke_test.py)

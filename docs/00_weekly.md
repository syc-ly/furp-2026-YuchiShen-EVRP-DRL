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

### Week 2 — 2026-07-01
Lab Theme: Baseline Methods Recreation and Comparison
Lab Objectives Recap:
1. Recreate three types of routing baseline algorithms (POMO RL, GA heuristic, OR MILP exact solver) from reference papers.
2. Test each method under 2 problem scales: 50 clients and 100 clients.
3. Record objective cost, feasibility status and runtime for all experiments.
4. Write comparative analysis & reflection on algorithm performance and constraint extension challenges.

**Attended this week's meeting:** Yes

## Progress this week
- _What did you actually do / finish?_
1. Step 1 POMO Recreation: Reimplemented the original POMO reinforcement learning baseline for pure CVRP (no energy E / time window TW constraints). Completed inference tests on n=50 and n=100 customer instances, recorded augmented & non-augmented travel cost and CPU runtime.
2. Step 2 GA Recreation: Reproduced GA-based VRPTW heuristic framework. Built dynamic instance truncation pipeline to generate 50-node subsets from standard Solomon 100-node datasets. Added precise runtime timer module to log convergence time. Current GA supports vehicle capacity & time window rules, energy (E) constraint not yet integrated.
3. Step 3 OR MILP Recreation: Implemented two separate MILP exact models as required:
   - Model A: Electric truck delivery (ETRP) with nonlinear charging & energy consumption constraints
   - Model B: Real-time food dispatch with mixed UAV-courier fleet
   Both models successfully solved n=50 and n=100 instances to global optimal status.
4. Step 4 Experiment Recording: Standardized all test metrics and compiled unified comparative result table covering POMO, GA and two OR MILP variants.

### Full Experimental Result Table
| Methodology | Problem Scale | Feasibility Status | Objective Value | Total Runtime (s) |
| :--- | :--- | :--- | :--- | :--- |
| POMO (Basic CVRP, no E / TW constraints) | 50 | Fully Feasible | 10.92 | 19.8 |
| POMO (Basic CVRP, no E / TW constraints) | 100 | Fully Feasible | 15.83 | 105.0 |
| GA (VRPTW, capacity + time window, no energy E constraint) | 50 | Feasible (All capacity & time window satisfied) | 17392.41 | 0.44 |
| GA (VRPTW, capacity + time window, no energy E constraint) | 100 | Feasible (All capacity & time window satisfied) | 34056.65 | 3.67 |
| OR (MILP ETRP: Electric Truck, built-in energy constraint) | 50 | Optimal & Fully Feasible | 51.0 | 0.21 |
| OR (MILP ETRP: Electric Truck, built-in energy constraint) | 100 | Optimal & Fully Feasible | 101.0 | 14.09 |
| OR (MILP UAV-Courier mixed fleet dispatch) | 50 | Optimal & Fully Feasible | 105.0 | 0.0468 |
| OR (MILP UAV-Courier mixed fleet dispatch) | 100 | Optimal & Fully Feasible | 241.0 | 0.0229 |

## Challenges & blockers
- _What got in the way? What are you stuck on?_
1. **Constraint Concept Distinction & Extension Barriers**
Original POMO and GA code only support basic vehicle capacity constraints. Separating electric energy (E) consumption rules and time window (TW) logic requires custom action masking (RL) and penalty functions (heuristic GA). Confusion between "vehicle load capacity" and "electric battery energy E" slowed down initial constraint coding.
2. **Instance Scalability for GA VRPTW Benchmark**
Public Solomon VRPTW datasets only provide complete 100-customer instances. Custom truncation logic was developed to extract valid 50-node subproblems to guarantee fair cross-scale comparison.
3. **MILP Variable Coupling for UAV-Truck Hybrid System**
The hybrid drone-truck delivery MILP model relies on coupled decision variables for vehicle routing and drone launch/landing operations. Defining interconnected objective cost terms and constraint sets was the main modeling bottleneck during recreation.

## Step5 Reflection & Comparative Conclusion (Lab Required Overview)
### 1. Cross-method performance comparison (Objective Value & Runtime)
1. **Exact OR MILP Methods**
   - Solution Quality: Guaranteed global optimal results, the most reliable objective values as benchmark ground truth.
   - Runtime Characteristic: Two MILP models show opposite scaling trends. The UAV-courier dispatch model runs extremely fast even at n=100 (0.0229s), while electric truck ETRP MILP runtime surges from 0.21s (n=50) to 14.09s (n=100), due to complex nonlinear charging energy constraints expanding solution space.
2. **GA Heuristic (VRPTW)**
   - Solution Quality: Suboptimal feasible routes with much larger objective cost magnitude compared to OR MILP, as evolutionary heuristics cannot prove optimality.
   - Runtime: Ultra-fast convergence within seconds for both scales, far faster than POMO RL inference on CPU. Suitable for rapid preliminary route searching.
3. **POMO Reinforcement Learning (CVRP)**
   - Solution Quality: Near-optimal CVRP solutions improved by instance augmentation; only valid for simple capacity-only routing without E/TW support.
   - Runtime: CPU inference time rises significantly with problem size, 105 seconds for n=100, slower than GA and lightweight OR MILP.

### 2. Core challenges of adding E / TW constraints to baseline algorithms
- For POMO RL: Must expand environment state space to track real-time battery level and customer arrival time, plus hard action masking to block invalid routes that drain battery or miss delivery windows. Retraining is required after constraint modification.
- For GA heuristic: Need extra fitness penalty terms to penalize energy depletion and time window violation, which easily disturbs population convergence and reduces final solution quality.
- For OR MILP: Energy and time window constraints introduce massive continuous decision variables, exponentially increasing solver computation time for large instances.

### 3. Insights for target EVRP-TW model construction
1. Algorithm Combination Strategy: Use lightweight OR MILP as small-scale benchmark ground truth; adopt GA/POMO for fast large-scale approximate solving, hybridizing exact and heuristic strengths.
2. Constraint Modular Design: Decouple capacity, time window and electric energy constraints into independent modules for flexible extension on POMO and GA baselines.
3. Scale-Adaptive Selection: For small instances (n ≤ 50), exact MILP is preferred to get optimal references; for medium/large instances (n ≥ 100), RL or GA heuristic balances runtime and solution quality.
4. Energy Constraint Optimization: Nonlinear charging energy constraints drastically raise MILP computation cost, so RL/heuristic methods are more suitable for complex EVRP-TW with nonlinear power consumption rules.

## Next steps
- _What will you do next week?_
1. Extend POMO and GA baselines with electric energy (E) constraint modules to build complete EVRP-TW test environments.
2. Run unified EVRP-TW experiments on n=50 and n=100 instances, record updated feasibility, objective cost and runtime for full cross-method comparison.
3. Complete final lab deliverables: organize all baseline source code, raw experiment log files, and this comparative reflection document for submission.

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

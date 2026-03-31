[README.md](https://github.com/user-attachments/files/26378332/README.md)
# Solve–Verify Cost Asymmetry: Empirical Dataset

**Paper:** "Empirical Evidence for Solve–Verify Cost Asymmetry Across Five Independent Computational Domains: Towards a Conservation Law for Energy Obligation Propagation"

**Author:** M.R., Gap in the Matrix Limited, London, UK

**Date:** 31 March 2026

**arXiv:** [link to be added after submission]

---

## Summary

This repository contains all datasets, computation logs, and verification scripts for the paper above. The central empirical claim is that solver cost consistently exceeds verifier cost across five independent computational domains, with no counter-examples found across eleven independently receipted runs.

**Primary session:** `55bb8a5c-c9ac-4965-9a1a-33317a69c193`  
**Supplementary session (Goldbach):** `b3125eba-d8c0-46c6-9478-cbc426b0c0de`

---

## Repository Structure

```
data/
  primary_dataset_11runs.json     # Complete structured dataset, all 11 runs
  supplementary_goldbach.json     # Goldbach supplementary run (single run)

logs/
  log_2026-03-31_09-16-26.txt    # P vs NP Run 1
  log_2026-03-31_09-21-59.txt    # P vs NP Run 2
  log_2026-03-31_09-24-24.txt    # Graph Colouring Run 1
  log_2026-03-31_09-25-44.txt    # Graph Colouring Run 2
  log_2026-03-31_09-28-19.txt    # Integer Factorisation Run 1
  log_2026-03-31_09-31-05.txt    # Integer Factorisation Run 2
  log_2026-03-31_09-34-37.txt    # Collatz Run 1
  log_2026-03-31_09-37-02.txt    # Collatz Run 2
  log_2026-03-31_09-44-06.txt    # Riemann Run 1
  log_2026-03-31_09-48-21.txt    # Riemann Run 2
  log_2026-03-31_10-11-23.txt    # Goldbach (supplementary)

scripts/
  verify_receipts.py             # SHA-256 verification for all receipts
  extract_structural.py          # Extract and compare structural invariants
  plot_node_growth.py            # Reproduce Figure 1 (DPLL node growth)
  plot_gap_ratios.py             # Reproduce Figure 2 (gap ratios across domains)
```

---

## SHA-256 Receipt Chain

All receipts are generated via the Web Crypto API (`SubtleCrypto.digest`, algorithm `SHA-256`) at the moment of computation completion. The hash input is a canonical JSON serialisation of the computation record including timing data and a UTC timestamp. Because timing varies between runs, hashes differ between runs on the same algorithm — this is expected. Structural invariants (node counts, backtrack counts, zero counts, stopping times) are identical across runs and constitute the reproducibility evidence.

### Primary Session Receipts

| Domain | Run | SHA-256 |
|--------|-----|---------|
| P vs NP (3-SAT) | 1 | `0038385dc0922011b1a34e39d0d660bdc4a12c6d5b6fbcd0305c071e0b79b72e` |
| P vs NP (3-SAT) | 2 | `ffc04c32c0ab795d4f32f40ecd8f6ffd2e21858cd0c2b7c9943d34ff6fd69d58` |
| Graph Colouring | 1 | `28bf03c222f71cf79be8b4cff9aa7037db92fb3cdbde18d1fed2987694d3a595` |
| Graph Colouring | 2 | `bcfb99f78b1165567ba0b1c7a2d0cecb7033c01ed54a160be2ebbb7d6b25de87` |
| Int. Factorisation | 1 | `52a8de982f19035d551556cc5d874a56bab54297f53c118d2f5f3872dcdec90b` |
| Int. Factorisation | 2 | `403553eb3f24ae974c2c6d17ce4c1b4a350874b5bd561b5d2d8c64ccddec0be6` |
| Collatz | 1 | `52409da48387b14ec99587ef45bfdc0f6b447bf5d230c3c2e7910e7e0f650c5e` |
| Collatz | 2 | `6ba8544ad095f509d8137a4cf48cdbd57680460630dc91e51597afe1763fa50e` |
| Riemann | 1 | `29925d99a452fd5b9b3638c014225feb3d3349cecf94ec17ce4627ee3f002d0b` |
| Riemann | 2 | `017ba8f227d35a62e3208d8331695356a760ed57f765f5c36e05f489a3eca7e8` |
| Riemann | 3 | `062bb849cefcde3ebeb8c7b182dfd290009d2b7baeb62ce4bddaccc5dd0e102d` |

### Supplementary Session

| Domain | Run | SHA-256 |
|--------|-----|---------|
| Goldbach Conjecture | 1 | `e6a7fe29d27bceffd4eed82d1aea6237199bfc1f8c24801ffcd39a0f56868dc2` |

---

## Key Structural Invariants (Reproduced Across Independent Runs)

These measurements are deterministic properties of the algorithms on the given inputs and are identical across all runs. They constitute the core reproducibility evidence.

**P vs NP — DPLL node sequence** (both runs identical):
```
n=8:   11 nodes    n=44:  145 nodes (nodes=backtracks ★)
n=10:   9 nodes    n=52:  185 nodes (nodes=backtracks ★)
n=12:  14 nodes    n=62:  394 nodes
n=14:  15 nodes    n=74:  715 nodes (nodes=backtracks ★)
n=16:  19 nodes    n=88: 1411 nodes (nodes=backtracks ★)
n=19:  23 nodes    n=105:4005 nodes (nodes=backtracks ★)
n=22:  37 nodes
n=26:  32 nodes    Gap at n=105: 411ms solve / <0.01ms verify = 4,110×
n=31:  51 nodes
n=37:  71 nodes
```
★ = UNSAT instances where every node explored was a backtrack (zero productive forward progress)

**Graph Colouring** (both runs identical):
- 3-colouring total backtrack events: **95,599**
- 2-colouring total backtrack events: **0**
- Same 24 graphs, same hardware, different complexity class

**Collatz** (both runs identical):
- Maximum stopping time: **585 steps** at n = **1,723,519**
- Total operations across 2,000,000 integers: **10,448,783**
- Counterexamples: **0**

**Riemann** (all three runs identical):
- Zeros confirmed on Re(s) = ½: **265**
- Off-critical-line detections: **0**
- Average zero spacing: **1.8265**

---

## Reproducing the Measurements

### Requirements
- Python 3.8+
- Any modern web browser (Chrome, Firefox, Safari) for engine re-execution
- No proprietary dependencies

### Verifying the receipt chain
```bash
python3 scripts/verify_receipts.py data/primary_dataset_11runs.json
```

This script reads each run record, reconstructs the hash input from the stored fields, computes SHA-256, and checks it against the stored hash. Note: timing fields will differ in a re-run of the engine; the script verifies that the stored hash matches the stored data, not that a fresh execution produces the same hash.

### Extracting structural invariants
```bash
python3 scripts/extract_structural.py data/primary_dataset_11runs.json
```

Outputs a comparison table of structural results across all runs of each domain, confirming invariance.

### Re-running the algorithms

The computational engine is a browser-based JavaScript implementation. To re-run:

1. Open the engine HTML file in any modern browser
2. Navigate to the desired problem tab
3. Set parameters to match the paper (n=105 for P vs NP, 20-node graphs for colouring, etc.)
4. Run — the engine will produce the same structural results

Seeds used in this paper are recorded in `primary_dataset_11runs.json` under `params.seeds` for each run.

---

## Dataset Schema

`primary_dataset_11runs.json` top-level structure:
```json
{
  "session_id": "55bb8a5c-...",
  "system": "browser-based computational engine",
  "law": "Fog Conservation hypothesis statement",
  "runs": [ ... ],
  "total_runs": 11,
  "problems_covered": [ ... ],
  "fog_conservation_law": { ... },
  "variance_analysis": [ ... ]
}
```

Each run record:
```json
{
  "problem": "P vs NP",
  "run_number": 1,
  "timestamp": "2026-03-31T09:16:26Z",
  "params": { "maxN": 105, "clauseRatio": 4.267, ... },
  "measurements": { "node_growth": [...], "gap_factor": "4110×", ... },
  "sha256_full": "0038385d...",
  "fog_conservation": { "law": "ψ transfer confirmed", "solver_bears_fog": true, ... }
}
```

---

## Citation

If you use this dataset, please cite the paper:

```
M.R. (2026). Empirical Evidence for Solve–Verify Cost Asymmetry Across Five 
Independent Computational Domains: Towards a Conservation Law for Energy 
Obligation Propagation. arXiv:[number]. 31 March 2026.
```

---

## License

Data and logs: CC BY 4.0  
Code: MIT License

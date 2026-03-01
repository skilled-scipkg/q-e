---
name: q-e-examples-and-tutorials
description: This skill should be used when users ask for runnable q-e examples, tutorial selection, and adaptation of reference workflows.
---

# q-e: Examples and Tutorials

## Scope
- Route users to the shortest runnable example matching their goal, then show how to adapt it safely.
- Keep examples tied to reference outputs and script patterns used in the repo.

## Primary documentation references
- `PW/examples/README`
- `PHonon/examples/README`
- `PP/examples/README`
- `TDDFPT/examples/README`
- `QEHeat/examples/README.md`
- `KCW/examples/example05.1/README.md`
- `KCW/examples/example05.1/nspin1/README.md`
- `KCW/examples/example05.1/nspin1/0_dft/README.md`
- `KCW/examples/example05.1/nspin1/1_wannier/README.md`

## High-Signal Playbook
### Route conditions
- Route compile/runtime setup blockers to `q-e-build-and-install`.
- Route detailed input parameter choices to `q-e-inputs-and-modeling`.
- Route formal regression comparison to `q-e-test-suite`.

### Triage questions
- Which property/workflow is needed (bands, DOS, phonons, TDDFPT spectra, QEHeat, KCW)?
- Is a serial or MPI run environment available?
- Are required executables and pseudopotentials already present?
- Is the goal learning/reference reproduction or production adaptation?
- Which output artifact is required (band file, phonon dispersion, current trace, etc.)?

### Canonical workflow
1. Pick the closest module README entry (`PW/examples/README`, `PHonon/examples/README`, etc.).
2. Enter that example directory and source `environment_variables` assumptions used by scripts.
3. Run `run_example` (or module-specific run script) unchanged first.
4. Compare generated outputs with each example's reference files/directory to confirm baseline behavior.
5. Modify one control variable at a time and re-run to isolate effects.

### Minimal working example
```bash
# PW tutorial-style run
cd PW/examples/example12
sh run_example
```

```bash
# PH dispersion tutorial-style run
cd PHonon/examples/example17
sh run_example
```

### Pitfalls/fixes
- Missing or wrong pseudopotentials are the most common first failure in example scripts.
- Many scripts assume helper vars from top-level `environment_variables`.
- Some plotting/report steps require optional tools (`gnuplot`); simulation data may still be valid.
- Certain examples require parallel resources to finish in practical time (`QEHeat/examples/README.md`).
- In EPW/KCW-style workflows, stage ordering is strict (do not reorder SCF/NSCF/interface steps).

### Convergence/validation checks
- Match key scalar checkpoints against each example's reference outputs (energies, frequencies, spectra features).
- Ensure each stage output exists before advancing in multi-step scripts.
- Reproduce baseline first, then tune cutoffs/meshes/thresholds for system-specific convergence.
- Keep a small diff log of script/input edits so regressions are reversible.

## Source-code entry links for unresolved behavior
- `PW/examples/run_all_examples`
- `PHonon/examples/run_all_examples`
- `PP/examples/run_all_examples`
- `TDDFPT/examples/run_all_examples`
- `QEHeat/examples/example_H2O_trajectory/run_example.sh`
- `Modules/open_close_input_file.f90`
- `PP/src/open_grid.f90`
- `PP/src/initial_state.f90`
- `KCW/src/ks_hamiltonian.f90`
- `references/source_map.md`

---
name: q-e-simulation-workflows
description: This skill should be used when users ask for runnable q-e calculation chains, restart patterns, and output checkpoints.
---

# q-e: Simulation Workflows

## Scope
- Handle SCF/NSCF/post-processing chains, restart/recover flows, and example run-script patterns.
- Keep workflows executable and checkpointed, not purely conceptual.

## Primary documentation references
- `PW/examples/example12/run_example`
- `PP/examples/example01/run_example`
- `PHonon/examples/example17/run_example`
- `PHonon/examples/Recover_example/README`
- `PHonon/examples/GRID_recover_example/README`
- `KCW/examples/example05.1/nspin1/0_dft/README.md`
- `KCW/examples/example05.1/nspin1/1_wannier/README.md`
- `PW/Doc/INPUT_PW.def`
- `PHonon/Doc/INPUT_PH.txt`

## High-Signal Playbook
### Route conditions
- Route parameter-level input design to `q-e-inputs-and-modeling`.
- Route compile/runtime environment problems to `q-e-build-and-install`.
- Route pass/fail against references to `q-e-test-suite`.

### Triage questions
- Which target pipeline is needed (bands, DOS, phonons, Wannier/KCW, TDDFPT, QEHeat)?
- Is there an existing `prefix/outdir` dataset to reuse?
- Must the run be restart-safe due to queue walltime?
- Which executables and pseudos are available on this machine?
- What is the minimal correctness checkpoint for each stage?

### Canonical workflow
1. Start from a known example chain and keep its stage order.
2. Run SCF first; keep `prefix/outdir` fixed for all downstream steps.
3. Run NSCF/bands or response step (`ph.x`, `turbo_*`, etc.) against SCF data.
4. Run post-processing (`bands.x`, `pp.x`, `q2r.x`, `matdyn.x`, plotting tools).
5. Add restart controls where needed:
   - `pw.x`: `max_seconds` + `restart_mode='restart'` after clean stop.
   - `ph.x`: `recover=.true.` to continue interrupted jobs.
6. Compare key outputs against example reference files before scaling up.

### Minimal working example
```bash
# SCF -> bands -> bands postprocess (KCW/PW-style)
pw.x -in scf.pwi | tee scf.pwo
pw.x -in bands.pwi | tee bands.pwo
bands.x -in bands.in | tee bands.out
```

```bash
# Phonon dispersion chain (PH example17 pattern)
pw.x -in bn.scf.in > bn.scf.out
ph.x -in bn.ph.disp.in > bn.ph.disp.out
q2r.x -in q2r.in > q2r.out
matdyn.x -in matdyn.in > matdyn.out
```

### Pitfalls/fixes
- Changing `prefix/outdir` between stages breaks downstream reads.
- Using `restart_mode='restart'` for a new NSCF workflow is wrong; restart only interrupted runs.
- `ph.x` recover fails if previous run used incompatible I/O settings (`reduce_io` caveat).
- Example scripts assume `environment_variables` is sourced and pseudos are reachable.
- Some plotting steps require `gnuplot`; physics output is still valid without plotting.

### Convergence/validation checks
- Verify `JOB DONE` at each stage before launching next one.
- Confirm stage artifacts exist (`*.save`, `*.dyn*`, `*.fc`, `*.freq`, band files).
- Compare a few scalar checkpoints (energies/frequencies/gaps) against references.
- For restarts, verify continuation from prior step count instead of clean restart.

## Source-code entry links for unresolved behavior
- `PW/src/run_driver.f90`
- `PW/src/init_run.f90`
- `PW/src/run_pwscf.f90`
- `PW/src/stop_run.f90`
- `PHonon/PH/run_nscf.f90`
- `TDDFPT/src/lr_run_nscf.f90`
- `NEB/src/stop_run_path.f90`
- `CPV/src/stop_run.f90`
- `Modules/run_info.f90`
- `references/source_map.md`

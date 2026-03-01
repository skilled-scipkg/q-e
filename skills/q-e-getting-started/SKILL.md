---
name: q-e-getting-started
description: This skill should be used when users ask about getting started in q-e; it prioritizes documentation references and then source inspection only for unresolved details.
---

# q-e: Getting Started

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `QEHeat/examples/README.md`
- `CPV/Doc/user_guide.md`

## High-Signal Playbook
### Route conditions
- Route build and dependency blockers to `q-e-build-and-install`.
- Route input design and convergence tuning to `q-e-inputs-and-modeling`.
- Route multi-stage production chains to `q-e-simulation-workflows`.

### Quick triage
- Is the user trying to run an existing example or write a fresh input?
- Are `pw.x`/`ph.x` binaries already available?
- Is MPI required from the start, or is a serial smoke run acceptable?
- Which minimal checkpoint defines success (`JOB DONE`, reference match, produced artifact)?

### Minimal startup workflow
1. Build at least `pw.x` if binaries are missing.
2. Run one untouched example first (recommended: `PW/examples/example12`).
3. Confirm the run completed and produced expected artifacts before editing inputs.
4. Move to the specialized skill based on the user’s next question.

### Minimal working example
```bash
# If binaries are missing: build pw.x quickly
mkdir -p build && cd build
cmake -DCMAKE_Fortran_COMPILER=mpif90 -DCMAKE_C_COMPILER=mpicc ..
make -j8 pw
```

```bash
# First runnable simulation baseline
cd PW/examples/example12
sh run_example
```

### Validation checkpoints
- Confirm `JOB DONE` appears in the example output.
- Confirm the example produced data under its working/output directory.
- Confirm no missing pseudopotential errors before changing any physics parameters.

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `COUPLE/examples`
- `CPV/examples`
- `EPW/examples`
- `FFTXlib/examples`
- `GWW/examples`
- `HP/examples`
- `KCW/examples`
- `NEB/examples`
- `PHonon/examples`
- `PIOUD/examples`
- `PP/examples`
- `PW/examples`
- `PWCOND/examples`
- `QEHeat/examples`
- `TDDFPT/examples`
- `XSpectra/examples`
- `atomic/examples`
- `GUI/Guib/examples`
- `GUI/PWgui/examples`
- `PHonon/FD/example`
- `PP/simple_transport/examples`

## Test references
- `test-suite`
- `COUPLE/tests`
- `FFTXlib/tests`
- `LAXlib/tests`
- `UtilXlib/tests`
- `XClib/test_input_files`
- `GUI/PWgui/tests`

## Optional deeper inspection
- `COUPLE`
- `CPV`
- `EPW`
- `FFTXlib`
- `GWW`
- `HP`
- `KCW`
- `KS_Solvers`
- `LAXlib`
- `LR_Modules`
- `Modules`
- `NEB`
- `PHonon`
- `PIOUD`
- `PP`
- `PW`
- `PWCOND`
- `QEHeat`
- `TDDFPT`
- `UtilXlib`
- `XClib`
- `XSpectra`
- `atomic`
- `dft-d3`
- `include`
- `upflib`

## Source entry points for unresolved issues
- `QEHeat/src/cpv_traj.f90`
- `QEHeat/src/cpv_traj_test.f90`
- `QEHeat/Makefile`
- `QEHeat/CMakeLists.txt`
- `include/cpv_device_macros.h`
- `CPV/Makefile`
- `CPV/CMakeLists.txt`
- `CPV/src/mainvar.f90`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" COUPLE CPV EPW FFTXlib GWW HP KCW KS_Solvers LAXlib LR_Modules Modules NEB PHonon PIOUD PP PW PWCOND QEHeat TDDFPT UtilXlib XClib XSpectra atomic dft-d3 include upflib`).

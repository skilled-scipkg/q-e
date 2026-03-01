---
name: q-e-index
description: This skill should be used as the top-level router for q-e requests, enforcing docs-first lookup and routing to the best topic skill before source inspection.
---

# q-e Skills Index

## Route the request
- Start docs-first: route to the narrowest topic skill, answer from docs/examples/tests, then inspect source only if needed.
- Prefer workflow-level routing over file-by-file deep dives.

## Skill routing table
- `q-e-getting-started`: onboarding, first run, package orientation.
- `q-e-build-and-install`: configure/CMake/Make, dependencies, MPI/OpenMP/GPU toggles.
- `q-e-inputs-and-modeling`: input syntax and modeling choices (`pw/ph/neb` and related tools).
- `q-e-simulation-workflows`: runnable stage chains, restart/recover, checkpoints.
- `q-e-examples-and-tutorials`: choose/adapt examples and tutorial baselines.
- `q-e-api-and-scripting`: CLI helpers, converters, automation/testcode interfaces.
- `q-e-test-suite`: regression execution, compare/tolerance interpretation, failure triage.
- `q-e-advanced-topics`: consolidated low-volume topics (CPV/EPW/LAXlib/UtilXlib one-doc areas).

## Docs-first escalation policy
1. Use target skill primary references.
2. If missing detail, inspect `<target-skill>/references/doc_map.md` (for example `skills/q-e-inputs-and-modeling/references/doc_map.md`).
3. If behavior still unclear, inspect `<target-skill>/references/source_map.md` and then source entry files.
4. Use targeted search only after route is fixed.

## Simulation startup handoff
- If the user needs a first runnable calculation, hand off to `q-e-getting-started`.
- If the user already has an input and wants execution order/restarts, hand off to `q-e-simulation-workflows`.
- If the user is blocked on binaries/libraries first, hand off to `q-e-build-and-install`.

## Documentation roots
- `Doc`
- `CPV/Doc`
- `EPW/doc`
- `GWW/doc`
- `HP/Doc`
- `KCW/Doc`
- `NEB/Doc`
- `PHonon/Doc`
- `PP/Doc`
- `PW/Doc`
- `PWCOND/Doc`
- `QEHeat/Doc`
- `TDDFPT/Doc`
- `XSpectra/Doc`
- `atomic/Doc`
- `GUI/Guib/doc`
- `GUI/PWgui/doc`
- `GUI/QE-modes/Doc`
- `test-suite/testcode/docs`

## Validation roots (examples/tests)
- Examples: `PW/examples`, `PHonon/examples`, `PP/examples`, `TDDFPT/examples`, `QEHeat/examples`, `EPW/examples`, `NEB/examples`, `KCW/examples`, `CPV/examples`, `atomic/examples`
- Tests: `test-suite`, `LAXlib/tests`, `UtilXlib/tests`, `FFTXlib/tests`, `COUPLE/tests`, `GUI/PWgui/tests`

## Source roots (deep inspection only)
- `COUPLE CPV EPW FFTXlib GWW HP KCW KS_Solvers LAXlib LR_Modules Modules NEB PHonon PIOUD PP PW PWCOND QEHeat TDDFPT UtilXlib XClib XSpectra atomic dft-d3 include upflib`

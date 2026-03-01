---
name: q-e-advanced-topics
description: This skill should be used for consolidated low-volume q-e topics (CPV, EPW graveyard notes, LAXlib tests, UtilXlib tests) before deeper source inspection.
---

# q-e: Advanced Topics

## Scope
- Consolidated topic for previously one-doc skills: CPV, EPW graveyard notes, LAXlib tests, UtilXlib tests.
- Keep routing compact while preserving direct doc/source entry points.

## Primary documentation references
- `CPV/Doc/autopilot_guide.md`
- `EPW/doc/graveyard.txt`
- `LAXlib/tests/README.md`
- `UtilXlib/tests/README.md`

## Route map
- CPV autopilot/restart-course rules -> `CPV/Doc/autopilot_guide.md`.
- EPW deprecated/removed-path checks -> `EPW/doc/graveyard.txt`.
- LAXlib test harness/questions -> `LAXlib/tests/README.md`.
- UtilXlib test harness/questions -> `UtilXlib/tests/README.md`.

## High-Signal Playbook
### Triage questions
- Is the request about CPV autopilot/restart, EPW deprecated flow behavior, or library-test harness behavior?
- Is the goal to run a minimal reproducer or inspect implementation semantics?
- Is MPI/CUDA required for the requested test path?

### Practical startup workflow
1. Start from the matching doc in the route map.
2. Run the smallest available reproducer in that topic unchanged.
3. Validate expected checkpoints (`JOB DONE`, test pass markers, or expected generated files).
4. Escalate to `references/source_map.md` only when doc behavior and runtime behavior diverge.

### Minimal working examples
```bash
# CPV autopilot smoke path
cd CPV/examples/autopilot-example
sh run_example_water
```

```bash
# EPW HDF5 example path
cd EPW/examples/lif_hdf5
sh setup.sh
sh run.espresso.sh
```

```bash
# UtilXlib serial+MPI quick test harness
cd UtilXlib/tests
bash compile_and_run_tests.sh -sm
```

### Validation checkpoints
- CPV: confirm autopilot rule application appears in output and run reaches `JOB DONE`.
- EPW: confirm setup and run scripts complete and expected EPW outputs are generated.
- UtilXlib/LAXlib: confirm test executables run without runtime aborts and summary reports no failures.

## Workflow
- Start from the relevant doc above.
- If unresolved, use `references/doc_map.md` for merged inventory.
- Escalate to `references/source_map.md` only for implementation behavior.

## Source entry points for unresolved issues
- `CPV/src/cp_autopilot.f90`
- `Modules/autopilot.f90`
- `EPW/src/wfpt.f90`
- `EPW/src/wannierization.f90`
- `LAXlib/la_module.f90`
- `LAXlib/mp_diag.f90`
- `UtilXlib/mp.f90`
- `UtilXlib/error_handler.f90`
- `UtilXlib/thread_util.f90`

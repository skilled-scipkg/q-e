---
name: q-e-test-suite
description: This skill should be used when users ask how to run, compare, and debug q-e regression tests.
---

# q-e: Test Suite

## Scope
- Handle `test-suite` execution, benchmark comparison, tolerance interpretation, and failure triage to module workflows.
- Prefer reproducible commands over ad-hoc output grepping.

## Primary documentation references
- `test-suite/README`
- `test-suite/Makefile`
- `test-suite/README_CMake`
- `test-suite/testcode/README.rst`
- `test-suite/testcode/docs/testcode.py.rst`
- `test-suite/testcode/docs/jobconfig.rst`
- `test-suite/testcode/docs/userconfig.rst`
- `test-suite/testcode/docs/verification.rst`
- `test-suite/userconfig.tmp`

## High-Signal Playbook
### Route conditions
- Route compile/toolchain failures before testing to `q-e-build-and-install`.
- Route input physics issues exposed by tests to `q-e-inputs-and-modeling`.
- Route run-sequence design (SCF/NSCF/postprocess) to `q-e-simulation-workflows`.

### Triage questions
- Which module family is failing (`pw`, `ph`, `epw`, `tddfpt`, `hp`, `pp`, `kcw`, `image`)?
- Need CMake/CTest path or legacy `make` + `testcode.py` path?
- What MPI/OpenMP settings were used (`NPROCS`, `OMP_NUM_THREADS`)?
- Is this a run failure, compare failure, or tolerance drift?
- Are pseudos and generated `userconfig` present?

### Canonical workflow
1. Enter `test-suite/`; generate runtime config via make prolog path (`userconfig` from `userconfig.tmp`).
2. Ensure pseudos exist (`make pseudo` or `check_pseudo.sh`).
3. Run focused categories first (`make run-tests-pw NPROCS=...`).
4. Compare against current benchmark (`make compare-pw`, or explicit `testcode.py ... compare`).
5. If failures persist, inspect extractor/tolerance mapping (`extract-*.sh`, `userconfig`).
6. Map failing category back to module workflow and re-run a minimal reproducer.

### Minimal working example
```bash
cd test-suite
make run-tests-pw NPROCS=4
make compare-pw
```

```bash
# Direct testcode path
cd test-suite
env QE_USE_MPI=4 ./testcode/bin/testcode.py --verbose --category=pw_all run compare
```

### Pitfalls/fixes
- Missing pseudopotentials causes broad failures: run `make pseudo` first.
- CTest integration is still partial (`README_CMake`); use legacy testcode for full numeric checks.
- `run-pw.sh` has case-specific behaviors (e.g., restart test id `22` handling).
- Non-zero expected exits should use `.expected_exit_msg` patterns (CMake path).
- Wrong category selection can silently skip intended tests; verify chosen category names.

### Convergence/validation checks
- Validate both run status and compare status; both matter.
- Inspect extracted metrics (`e1`, `n1`, `f1`, etc.) via `extract-*.sh` outputs.
- Confirm tolerance context in `userconfig` before declaring physics regressions.
- Re-run a single failing category/test after fixes to confirm deterministic recovery.

## Source-code entry links for unresolved behavior
- `test-suite/Makefile`
- `test-suite/CMakeLists.txt`
- `test-suite/ctest_runner.sh`
- `test-suite/run-pw.sh`
- `test-suite/extract-pw.sh`
- `test-suite/userconfig.tmp`
- `test-suite/testcode/bin/testcode.py`
- `test-suite/testcode/lib/testcode2/config.py`
- `test-suite/testcode/lib/testcode2/validation.py`
- `references/source_map.md`

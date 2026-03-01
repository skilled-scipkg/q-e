---
name: q-e-api-and-scripting
description: This skill should be used when users ask for q-e CLI helpers, conversion scripts, and lightweight automation patterns.
---

# q-e: API and Scripting

## Scope
- Cover scriptable entry points (`PW/tools`, `test-suite/testcode`, selected `dev-tools`) and IO conversion helpers.
- Prefer stable helper scripts and documented CLI workflows before custom parsing.

## Primary documentation references
- `PW/tools/README`
- `PW/tools/cif2qe.sh`
- `PW/tools/castep2qe.sh`
- `PW/tools/pwi2xsf.sh`
- `PW/tools/pwo2xsf.sh`
- `PW/tools/xsf2pwi.sh`
- `test-suite/testcode/docs/testcode.py.rst`
- `test-suite/testcode/docs/configuration_files.rst`
- `test-suite/testcode/docs/userconfig.rst`
- `test-suite/ph_multipole/README.txt`
- `KCW/examples/example05.1/nspin1/1_wannier/README.md`

## High-Signal Playbook
### Route conditions
- Route build/toolchain setup to `q-e-build-and-install`.
- Route physics input design to `q-e-inputs-and-modeling`.
- Route full simulation stage sequencing to `q-e-simulation-workflows`.

### Triage questions
- Is the request file conversion, workflow automation, or regression orchestration?
- Which source/target formats are involved (`cif`, CASTEP, XSF, QE input/output)?
- Is Python dependency acceptable (`testcode.py`, optional scripts)?
- Is this one-off conversion or repeated CI-style automation?
- Are KCW/Wannier interface steps in scope?

### Canonical workflow
1. Select an existing helper from `PW/tools/README` instead of writing a new parser.
2. Run converter with the documented usage flags.
3. Sanity-check generated QE input cards before heavy runs.
4. For repeated checks, wrap with `testcode.py` actions (`run`, `compare`, `make-benchmarks`).
5. For Wannier/KCW scripting, follow the `pw.x -> pw2wannier90.x -> wannier90.x -> kcw.x` interface sequence.

### Minimal working example
```bash
# Structure conversion helpers
PW/tools/cif2qe.sh -i graphene.cif > graphene.pwi
PW/tools/pwi2xsf.sh graphene.pwi > graphene.xsf
```

```bash
# Regression automation
cd test-suite
./testcode/bin/testcode.py --verbose --category=pw_all run compare
```

### Pitfalls/fixes
- `cif2qe.sh` expects GNU awk behavior and warns about DOS line endings.
- Converter outputs still need manual review of pseudopotential names/paths.
- `testcode.py` needs valid `jobconfig` + `userconfig` in scope.
- `ph_multipole` flow depends on a Python script and `spglib` availability.
- KCW/Wannier chains fail if symbolic link step (`link_wann.sh`) is skipped.

### Convergence/validation checks
- Validate converted input with a cheap dry-run (`nstep=0`) before production.
- Confirm coordinate/unit consistency after XSF/CIF round-trips.
- For testcode, require both successful run and compare pass.
- For KCW interfaces, verify intermediary files exist before launching next stage.

## Source-code entry links for unresolved behavior
- `PW/tools/cif2qe.sh`
- `PW/tools/castep2qe.sh`
- `PW/tools/pwi2xsf.sh`
- `PW/tools/pwo2xsf.sh`
- `PW/tools/xsf2pwi.sh`
- `test-suite/testcode/bin/testcode.py`
- `Modules/wannier_new.f90`
- `PW/src/wannier_init.f90`
- `PP/src/wannier_proj.f90`
- `KCW/src/read_wannier.f90`
- `references/source_map.md`

# q-e source map: API and Scripting

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "cif|xsf|wannier|usage|awk" PW/tools`
- `rg -n "def (run_tests|compare_tests|parse_userconfig|compare_data)" test-suite/testcode`
- `rg -n "wannier" Modules PW/src PP/src KCW/src`

## Function-level entry points

### Conversion and CLI helper scripts
- `PW/tools/cif2qe.sh`: CIF to QE input conversion, lattice/card rendering, and awk-based parsing.
- `PW/tools/castep2qe.sh`: CASTEP to QE input conversion path.
- `PW/tools/pwi2xsf.sh`: QE input to XSF conversion for geometry inspection.
- `PW/tools/pwo2xsf.sh`: QE output to XSF extraction for post-run visualization.
- `PW/tools/xsf2pwi.sh`: XSF to QE input conversion for round-trip checks.

### Regression scripting engine
- `test-suite/testcode/bin/testcode.py`: top-level actions (`run_tests`, `compare_tests`, `make_benchmarks`).
- `test-suite/testcode/lib/testcode2/config.py`: `parse_userconfig` and `parse_jobconfig` behavior.
- `test-suite/testcode/lib/testcode2/validation.py`: numeric comparison logic (`compare_data`).

### Wannier/KCW interface behavior
- `Modules/wannier_new.f90`: shared Wannier data structures and flags.
- `PW/src/wannier_init.f90`: setup and file initialization for Wannier projections.
- `PP/src/wannier_proj.f90`: projection-generation implementation (`wannier_proj`).
- `KCW/src/read_wannier.f90`: KCW Wannier manifold readers (`read_wannier*`).

## Behavior checks
- `bash -n PW/tools/cif2qe.sh PW/tools/pwi2xsf.sh PW/tools/xsf2pwi.sh`
- `python3 test-suite/testcode/bin/testcode.py --help`
- `rg -n "def (run_tests|compare_tests|parse_userconfig|compare_data)" test-suite/testcode/bin/testcode.py test-suite/testcode/lib/testcode2/config.py test-suite/testcode/lib/testcode2/validation.py`
- `rg -n "subroutine wannier_proj|subroutine read_wannier" PP/src/wannier_proj.f90 KCW/src/read_wannier.f90`

## Practical simulation checkpoints
- Validate converted inputs with a cheap run (`nstep=0`) before production runs.
- For `testcode.py`, require both run success and compare success.
- In Wannier/KCW chains, verify intermediate files exist before launching the next executable.

# q-e source map: Test Suite

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "run-tests|compare|pseudo|NPROCS|category" test-suite/Makefile test-suite/userconfig.tmp`
- `rg -n "expected_exit|benchmark|ctest|python" test-suite/CMakeLists.txt test-suite/ctest_runner.sh`
- `rg -n "def (run_tests|compare_tests|parse_userconfig|compare_data)" test-suite/testcode`

## Function-level entry points

### Harness entry and category orchestration
- `test-suite/Makefile`: canonical test targets (`run-tests-*`, `compare-*`, pseudo preparation).
- `test-suite/run-pw.sh`: category-specific pw runner behavior.
- `test-suite/extract-pw.sh`: extracted scalar metrics used for comparisons.
- `test-suite/userconfig.tmp`: tolerance and executable defaults template.

### CMake/CTest integration path
- `test-suite/CMakeLists.txt`: test registration, expected-failure handling, and wrappers.
- `test-suite/ctest_runner.sh`: execution and compare adapter used by CTest jobs.

### testcode internals for run/compare semantics
- `test-suite/testcode/bin/testcode.py`: action dispatcher (`run_tests`, `compare_tests`, `main`).
- `test-suite/testcode/lib/testcode2/config.py`: config parsing (`parse_userconfig`, `parse_jobconfig`).
- `test-suite/testcode/lib/testcode2/validation.py`: tolerance/status logic (`compare_data`, `Tolerance`).

## Behavior checks
- `make -C test-suite -n run-tests-pw NPROCS=2`
- `rg -n "run-tests-|compare-|pseudo" test-suite/Makefile`
- `python3 test-suite/testcode/bin/testcode.py --help`
- `rg -n "def (run_tests|compare_tests|parse_userconfig|compare_data)" test-suite/testcode/bin/testcode.py test-suite/testcode/lib/testcode2/config.py test-suite/testcode/lib/testcode2/validation.py`

## Practical simulation checkpoints
- Require both run success and compare success before closing a regression issue.
- Re-run one minimal failing category after a fix to confirm deterministic recovery.
- Validate tolerances from `userconfig` before labeling a deviation as physics regression.

# q-e source map: Build and Install

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "QE_ENABLE_|CUDA|ELPA|SCALAPACK|HDF5|MPI|OPENMP" CMakeLists.txt test-suite/CMakeLists.txt`
- `rg -n "x_ac_qe_(cuda|hdf5|scalapack)" install/configure.ac install/m4`
- `rg -n "run-tests|compare|ctest|expected_exit" test-suite/ctest_runner.sh test-suite/CMakeLists.txt`

## Function-level entry points

### CMake and test integration gates
- `CMakeLists.txt`: top-level option gates and hard dependencies (MPI/CUDA/ELPA/ScaLAPACK).
- `test-suite/CMakeLists.txt`: CTest wiring, expected-exit handling, and config generation.
- `test-suite/ctest_runner.sh`: runtime wrapper used by CTest-driven checks.

### Autoconf dependency probes
- `install/configure.ac`: macro wiring for build feature detection.
- `install/m4/x_ac_qe_cuda.m4`: CUDA/NVHPC checks and related configure flags.
- `install/m4/x_ac_qe_hdf5.m4`: HDF5 probe logic.
- `install/m4/x_ac_qe_scalapack.m4`: ScaLAPACK probe logic.
- `install/make.inc.in`: generated make include template and variable plumbing.

## Behavior checks
- `rg -n "QE_ENABLE_CUDA|QE_ENABLE_ELPA|QE_ENABLE_SCALAPACK" CMakeLists.txt`
- `rg -n "AC_DEFUN|cuda|hdf5|scalapack" install/m4/x_ac_qe_cuda.m4 install/m4/x_ac_qe_hdf5.m4 install/m4/x_ac_qe_scalapack.m4`
- `./configure --help | rg -n "cuda|hdf5|scalapack|mpi|openmp"`

## Practical simulation checkpoints
- Confirm configure/cmake summaries match intended compilers and external libraries.
- Confirm at least `pw.x` builds before broad module targets.
- Run one smoke simulation and one focused test-suite slice before production workloads.

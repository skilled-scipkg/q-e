---
name: q-e-build-and-install
description: This skill should be used when users ask about building, configuring, or installing q-e across CPU/GPU and MPI/OpenMP setups.
---

# q-e: Build and Install

## Scope
- Handle configure/CMake/Make build flows, dependency toggles, install paths, and first-run validation.
- Use docs first; inspect CMake/autoconf sources only when a flag or behavior is ambiguous.

## Primary documentation references
- `README.md`
- `README_GPU.md`
- `install/configure.msg.in`
- `test-suite/buildbot/Udine_farm/README.txt`
- `test-suite/testcode/docs/installation.rst`
- `test-suite/CMakeLists.txt`
- `CMakeLists.txt`

## High-Signal Playbook
### Route conditions
- Route input syntax, material setup, and convergence tuning to `q-e-inputs-and-modeling`.
- Route runtime chains/restarts to `q-e-simulation-workflows`.
- Route regression failures to `q-e-test-suite`.

### Triage questions
- Which build system is required: `./configure`+`make` or `cmake`?
- Which compiler wrappers are intended (`mpif90` + `mpicc`, `nvfortran`, Intel, GCC)?
- CPU-only or GPU build? If GPU, is NVHPC available?
- Which optional libraries are required (`ScaLAPACK`, `ELPA`, `HDF5`, `libxc`)?
- Is this a developer build (tests/docs on) or a lean production build?

### Canonical workflow
1. Pick build path from `README.md`: autoconf+make or CMake.
2. Set explicit compilers/wrappers (recommended in `README.md`).
3. Configure features:
   - Autoconf: `./configure [flags]`
   - CMake: `-DQE_ENABLE_MPI`, `-DQE_ENABLE_OPENMP`, `-DQE_ENABLE_CUDA`, etc. (`CMakeLists.txt`).
4. Build with `make -jN`.
5. Optional install: `make install` with prefix.
6. For GPU, use CUDA flags from `README_GPU.md` (`--with-cuda`, `--with-cuda-runtime`, `--with-cuda-cc`, `--with-cuda-mpi`).
7. Validate with a small run and one focused test-suite category.

### Minimal working example
```bash
# CPU CMake build
mkdir -p build && cd build
cmake -DCMAKE_Fortran_COMPILER=mpif90 -DCMAKE_C_COMPILER=mpicc ..
make -j8

# Quick test slice
ctest -L system--pw --output-on-failure
```

```bash
# GPU configure build (NVHPC path from README_GPU.md)
./configure --with-cuda="$CUDA_HOME" --with-cuda-cc=70 --with-cuda-runtime=11.0 \
  --enable-openmp --with-cuda-mpi=yes
make -j8 pw
```

### Pitfalls/fixes
- `QE_ENABLE_CUDA=ON` with non-NVHPC compiler fails (`CMakeLists.txt` hard check): use NVHPC.
- `QE_ENABLE_ELPA=ON` without `QE_ENABLE_SCALAPACK=ON` fails: enable both or disable ELPA.
- `QE_ENABLE_SCALAPACK=ON` without MPI fails: enable MPI.
- HDF5 not detected: rerun `./configure LIBDIRS="..."` or set HDF5 CMake paths.
- CTest expectations: `test-suite/CMakeLists.txt` notes only pw/cp CTest reliability is mature.

### Convergence/validation checks
- Read configure summary (`install/configure.msg.in`) and confirm expected external libs.
- Confirm executables are present in the generated binary output directory.
- Run one SCF smoke test and verify `JOB DONE`.
- Run one module test category (`pw_all` or `cp_all`) before wider campaigns.

## Source-code entry links for unresolved behavior
- `CMakeLists.txt`
- `test-suite/CMakeLists.txt`
- `test-suite/ctest_runner.sh`
- `install/configure.ac`
- `install/m4/x_ac_qe_cuda.m4`
- `install/m4/x_ac_qe_hdf5.m4`
- `install/m4/x_ac_qe_scalapack.m4`
- `install/make.inc.in`
- `references/source_map.md`

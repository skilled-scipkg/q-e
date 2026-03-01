# q-e source map: Getting Started

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "program pwscf|read_input_file|run_driver|stop_run" PW/src`
- `rg -n "environment|version|parallel" Modules/environment.f90`
- `rg -n "cpv|traj|autopilot" QEHeat/src CPV/src include`

## Function-level entry points

### Core pw.x startup path
- `PW/src/pwscf.f90`: main executable entry point for `pw.x`.
- `PW/src/input.f90`: input transfer/validation and high-impact defaults.
- `PW/src/run_driver.f90`: top-level SCF/NSCF loop orchestration.
- `PW/src/stop_run.f90`: end-of-run cleanup and restart-file handling.
- `Modules/environment.f90`: runtime/environment reporting seen in startup logs.

### CPV and QEHeat orientation anchors
- `QEHeat/src/cpv_traj.f90`: trajectory analysis behavior linked from QEHeat examples.
- `QEHeat/src/cpv_traj_test.f90`: test harness path for trajectory routines.
- `CPV/src/mainvar.f90`: CPV global state foundation.
- `include/cpv_device_macros.h`: CPU/GPU macro layer used by CPV/QEHeat paths.

## Behavior checks
- `rg -n "program|subroutine" PW/src/pwscf.f90 PW/src/run_driver.f90 PW/src/stop_run.f90`
- `rg -n "restart|from_scratch|nstep" PW/src/input.f90 PW/src/run_driver.f90`
- `rg -n "cpv_traj|trajectory|autopilot" QEHeat/src/cpv_traj.f90 CPV/src/mainvar.f90`

## Practical simulation checkpoints
- For first-run smoke tests, require `JOB DONE` before any input tuning.
- Confirm run output includes expected environment and parallel layout headers.
- If restart is requested, confirm restart files are produced before stopping the job.

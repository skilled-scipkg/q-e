# q-e source map: Simulation Workflows

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "SUBROUTINE run_driver|stop_run|restart|max_seconds" PW/src`
- `rg -n "SUBROUTINE run_nscf|recover|ext_restart" PHonon/PH TDDFPT/src KCW/src`
- `rg -n "stop_run_path|restart" NEB/src CPV/src`

## Function-level entry points

### PW main stage orchestration
- `PW/src/init_run.f90`: setup/allocation and initial state setup.
- `PW/src/run_driver.f90`: stage orchestration and handoff to iteration loops.
- `PW/src/run_pwscf.f90`: SCF/NSCF iteration engine and restart save points.
- `PW/src/stop_run.f90`: cleanup/restart-file lifecycle at exit.

### Response and chained NSCF flows
- `PHonon/PH/run_nscf.f90`: PH-side NSCF orchestration and restart/recover interplay.
- `TDDFPT/src/lr_run_nscf.f90`: TDDFPT NSCF workflow wrapper.
- `KCW/src/kcw_run_nscf.f90`: KCW NSCF stage driver.

### Path/restart stop points
- `NEB/src/stop_run_path.f90`: NEB stop semantics and restart-file retention/removal.
- `CPV/src/stop_run.f90`: CP stop and cleanup behavior.
- `Modules/run_info.f90`: run metadata and diagnostics used in multi-stage logs.

## Behavior checks
- `rg -n "SUBROUTINE run_driver|CALL stop_run|restart_dir" PW/src/run_driver.f90`
- `rg -n "max_seconds|restart|save restart" PW/src/run_pwscf.f90`
- `rg -n "recover|ext_restart|seqopn\( 4, 'restart'" PHonon/PH/run_nscf.f90 TDDFPT/src/lr_run_nscf.f90`
- `rg -n "SUBROUTINE stop_run_path|restart" NEB/src/stop_run_path.f90 CPV/src/stop_run.f90`

## Practical simulation checkpoints
- Require stage-local completion markers before launching the next executable.
- Confirm expected intermediate artifacts (`*.save`, `*.dyn*`, `*.fc`, band files) exist.
- For restart scenarios, verify continuation from previous counters rather than fresh start.

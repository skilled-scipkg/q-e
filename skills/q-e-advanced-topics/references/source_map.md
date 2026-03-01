# q-e source map: Advanced Topics

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "AUTOPILOT|ON_STEP|pilot|parse_mailbox|restart" CPV/src Modules`
- `rg -n "wannier|wfpt|tdbe|restart" EPW/src`
- `rg -n "diag|mp_|thread|error" LAXlib UtilXlib`

## Function-level entry points

### CPV autopilot and restart behavior
- `CPV/src/cp_autopilot.f90`: runtime rule application (`pilot`) and autopilot variable updates.
- `Modules/autopilot.f90`: rule parsing, mailbox processing (`parse_mailbox`), and event scheduling.
- `CPV/src/restart_sub.f90`: restart state I/O boundaries and restart safety.

### EPW workflow and legacy-path behavior
- `EPW/src/wfpt.f90`: wavefunction transport pipeline state and physics control flow.
- `EPW/src/wannierization.f90`: Wannier projection/wrapper behavior and manifold handling.
- `EPW/src/tdbe_driver.f90`: time-dependent BTE driver sequence.

### LAXlib and UtilXlib test-critical primitives
- `LAXlib/la_module.f90`: linear-algebra interface used by test executables.
- `LAXlib/mp_diag.f90`: distributed diagonalization path used in parallel tests.
- `UtilXlib/mp.f90`: MPI helper layer used by most utility tests.
- `UtilXlib/error_handler.f90`: error propagation and trace path (`trace_back`).
- `UtilXlib/thread_util.f90`: threaded memory helper behavior.

## Behavior checks
- `rg -n "subroutine pilot|parse_mailbox|ON_STEP" CPV/src/cp_autopilot.f90 Modules/autopilot.f90`
- `rg -n "subroutine read_wannier|wannier|wfpt|tdbe" EPW/src/wannierization.f90 EPW/src/wfpt.f90 EPW/src/tdbe_driver.f90`
- `rg -n "subroutine|function" LAXlib/mp_diag.f90 UtilXlib/mp.f90 UtilXlib/error_handler.f90`

## Practical simulation checkpoints
- Run `CPV/examples/autopilot-example/run_example_water` and confirm autopilot rule transitions appear in output.
- Run `UtilXlib/tests/compile_and_run_tests.sh -sm` and confirm no runtime abort in the report.
- For EPW test paths, confirm setup/run scripts produce stage outputs before changing inputs.

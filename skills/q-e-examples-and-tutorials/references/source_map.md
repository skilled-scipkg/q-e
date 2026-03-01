# q-e source map: Examples and Tutorials

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "run_example|run_all_examples|reference" PW/examples PHonon/examples PP/examples TDDFPT/examples QEHeat/examples KCW/examples`
- `rg -n "open|initial|grid|hamiltonian" Modules PP/src KCW/src`

## Function-level entry points

### Example orchestration scripts
- `PW/examples/run_all_examples`: canonical multi-example runner for PW.
- `PHonon/examples/run_all_examples`: canonical multi-example runner for PH.
- `PP/examples/run_all_examples`: canonical PP example batch runner.
- `TDDFPT/examples/run_all_examples`: TDDFPT tutorial batch runner.
- `QEHeat/examples/example_H2O_trajectory/run_example.sh`: trajectory-style script chain.

### Source paths that explain example artifacts
- `Modules/open_close_input_file.f90`: input file open/close behavior shared across workflows.
- `PP/src/open_grid.f90`: grid loading behavior used in PP example paths.
- `PP/src/initial_state.f90`: initial-state generation path used by CLS-style examples.
- `KCW/src/ks_hamiltonian.f90`: Hamiltonian build behavior for KCW tutorial outputs.

## Behavior checks
- `rg -n "run_example|run_all_examples|reference" PW/examples PHonon/examples PP/examples TDDFPT/examples QEHeat/examples`
- `rg -n "subroutine|function" Modules/open_close_input_file.f90 PP/src/open_grid.f90 PP/src/initial_state.f90 KCW/src/ks_hamiltonian.f90`

## Practical simulation checkpoints
- Run one example unchanged and confirm script exits cleanly.
- Compare key output checkpoints against the example's bundled reference artifacts.
- Change one parameter at a time to keep regression attribution clear.

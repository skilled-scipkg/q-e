# q-e source map: Inputs and Modeling

Use this map only after exhausting topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "read_input_file|restart_mode|ecutwfc|ecutrho|mixing_beta" PW/src/input.f90`
- `rg -n "recover|tr2_ph|nmix_ph|prefix|outdir" PHonon/PH/phcom.f90`
- `rg -n "path_thr|restart_mode|num_of_images" NEB/src/path_input_parameters_module.f90`
- `rg -n "subroutine (ld1_readin|read_input_file)" atomic/src/ld1_readin.f90 PP/src/band_interpolation.f90`

## Function-level entry points

### pw.x and shared input handling
- `PW/src/input.f90`: main namelist/card ingestion and restart/convergence checks.
- `PW/src/oscdft_input.f90`: OScDFT-specific input extensions.

### ph.x, neb.x, and TDDFPT input layers
- `PHonon/PH/phcom.f90`: phonon input state (`recover`, restart control, shared variables).
- `NEB/src/path_input_parameters_module.f90`: NEB path-input parameters (`path_thr`, restart mode).
- `TDDFPT/src/bcast_lr_input.f90`: distributed TDDFPT input broadcast behavior.
- `TDDFPT/src/lr_lanczos.f90`: Lanczos driver configuration handling.
- `TDDFPT/src/lr_eels_main.f90`: EELS-mode control flow and input-dependent execution.

### Post-processing and atomic input readers
- `PP/src/band_interpolation.f90`: post-processing input read/default handling.
- `PWCOND/src/summary_band.f90`: transmission/band summary output coupling.
- `atomic/src/ld1_readin.f90`: `ld1.x` input parser and broadcast helpers.

## Behavior checks
- `rg -n "read_input_file|restart_mode|ecutwfc|ecutrho" PW/src/input.f90`
- `rg -n "recover|ext_recover|tmp_dir_phq" PHonon/PH/phcom.f90`
- `rg -n "path_thr|restart_mode_allowed" NEB/src/path_input_parameters_module.f90`
- `rg -n "subroutine ld1_readin|subroutine read_input_file" atomic/src/ld1_readin.f90 PP/src/band_interpolation.f90`

## Practical simulation checkpoints
- Validate prefix/outdir consistency across all chained stages.
- Confirm restart flags are compatible with I/O mode before queue restarts.
- Converge cutoffs/meshes on the target observable, then freeze them before workflow expansion.

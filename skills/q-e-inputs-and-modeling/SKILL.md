---
name: q-e-inputs-and-modeling
description: This skill should be used when users ask how to construct, tune, or debug q-e input files and modeling choices.
---

# q-e: Inputs and Modeling

## Scope
- Cover `pw.x` / `ph.x` / `neb.x` input structure, modeling choices, and high-impact convergence controls.
- Keep docs-first guidance anchored to official input reference files and runnable examples.

## Primary documentation references
- `PW/Doc/INPUT_PW.def`
- `PHonon/Doc/INPUT_PH.txt`
- `NEB/Doc/INPUT_NEB.txt`
- `PW/examples/example12/run_example`
- `PHonon/examples/example17/run_example`
- `NEB/examples/neb1.in`

## High-Signal Playbook
### Route conditions
- Route compile/toolchain flags to `q-e-build-and-install`.
- Route end-to-end execution/restart chains to `q-e-simulation-workflows`.
- Route regression tolerance disputes to `q-e-test-suite`.

### Triage questions
- Which engine is being configured (`pw.x`, `ph.x`, `neb.x`, PP/TDDFPT tools)?
- What is the target task (`scf`, `nscf`, `bands`, `relax`, phonons, reaction path)?
- Which pseudopotentials/XC/hubbard/spin-SOC settings are required?
- What are initial cutoff and k/q meshes?
- Is this a fresh run or a restart/recover continuation?

### Canonical workflow
1. Build a minimal `pw.x` input skeleton from `INPUT_PW.def`: `&CONTROL`, `&SYSTEM`, `&ELECTRONS`, then cards.
2. Set core physics knobs early: `ecutwfc`, `ecutrho`, `occupations`/`smearing`/`degauss`, `nspin`/SOC/Hubbard.
3. Validate structure cards: `ATOMIC_SPECIES`, `ATOMIC_POSITIONS`, `K_POINTS`, `CELL_PARAMETERS`.
4. Run SCF and stabilize convergence (`conv_thr`, `mixing_beta`, `electron_maxstep`).
5. For phonons, keep `prefix/outdir` consistent with SCF and tune `tr2_ph`, `nmix_ph`, `ldisp/nq1,nq2,nq3`.
6. For NEB, use supercards (`BEGIN_PATH_INPUT`, `BEGIN_ENGINE_INPUT`, `BEGIN_POSITIONS`) and run with `neb.x -inp file`.

### Minimal working example
```bash
# Minimal SCF (pw.x)
cat > si.scf.in <<'INP'
&control
  calculation='scf', prefix='si', outdir='./tmp'
/
&system
  ibrav=2, celldm(1)=10.2, nat=2, ntyp=1, ecutwfc=30, ecutrho=240
/
&electrons
  conv_thr=1.0d-8, mixing_beta=0.7
/
ATOMIC_SPECIES
Si 28.086 Si.pz-vbc.UPF
ATOMIC_POSITIONS alat
Si 0.00 0.00 0.00
Si 0.25 0.25 0.25
K_POINTS automatic
6 6 6 1 1 1
INP
pw.x -in si.scf.in
```

```bash
# Minimal NEB launch (note: neb.x does not read stdin)
cp NEB/examples/neb1.in ./neb.in
neb.x -inp neb.in
```

### Pitfalls/fixes
- `restart_mode='restart'` in `pw.x` is only for interrupted runs with same parallel layout (`INPUT_PW.def`).
- `ph.x` restart requires `recover=.true.`; `reduce_io=.true.` blocks restart (`INPUT_PH.txt`).
- `neb.x` fails if launched with stdin redirection; use `-inp` (`INPUT_NEB.txt`).
- Mismatched `prefix/outdir` between SCF and follow-on tools causes missing data-file errors.
- Overly aggressive `mixing_beta` can stall SCF; reduce it before changing many other knobs.

### Convergence/validation checks
- Converge `ecutwfc/ecutrho` and k-point mesh on target observable, not only total energy.
- For metals, verify smearing choice/width sensitivity.
- For phonons, converge `tr2_ph` and q-grid (`nq1,nq2,nq3`) and check mode stability.
- For NEB, verify `num_of_images > 3` and monitor `path_thr` reduction to threshold.

## Source-code entry links for unresolved behavior
- `PW/src/oscdft_input.f90`
- `TDDFPT/src/bcast_lr_input.f90`
- `TDDFPT/src/lr_lanczos.f90`
- `TDDFPT/src/lr_eels_main.f90`
- `PWCOND/src/summary_band.f90`
- `PP/src/band_interpolation.f90`
- `atomic/src/ld1_readin.f90`
- `NEB/src/path_input_parameters_module.f90`
- `PHonon/PH/phcom.f90`
- `references/source_map.md`

# nwchem source map: Inputs and Modeling

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "input|task|basis|geometry|md|qmmm|grid" src/task src/nwdft src/nwmd src/qmmm src/argos`
- `rg -n "subroutine .*input|read_" src`

## Function-level entry points
- `src/task/task_input.F` | role: top-level task and block parsing.
- `src/geom/geom_input.F` | role: geometry block parsing and validation.
- `src/basis/bas_input.F` | role: basis block parsing and atom-tag mapping.
- `src/nwdft/scf_dft/dft_main0d.F` | role: DFT task main control flow.
- `src/nwdft/scf_dft/dft_scf.F` | role: SCF iteration driver in DFT runs.
- `src/nwdft/grid/grid_setrad.F` | role: DFT radial grid setup used by `grid` controls.
- `src/NWints/rel/modelpotential_input.F` | role: model potential input handling.
- `src/nwmd/md_input.F` | role: MD keyword parsing.
- `src/qmmm/qmmm_input.F` | role: QM/MM block parsing.
- `src/argos/argos_input.F` | role: nwARGOS input parsing.
- `src/nwdft/rt_tddft/input/rt_tddft_input.F` | role: RT-TDDFT input parsing.
- `src/nwdft/rt_tddft/input/rt_tddft_input_field.F` | role: external field keyword parsing.

## Validation checkpoints
- Parsed decks should route to the intended task without fallback/default surprises.
- Grid, basis, and restart-related options should appear in runtime echo/output blocks.

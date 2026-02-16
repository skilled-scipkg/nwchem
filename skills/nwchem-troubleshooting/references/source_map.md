# nwchem source map: Troubleshooting

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "error|errquit|guess|conver|restart|input" src`
- `rg -n "task_input|dft_get_conv_info|md_input|qmmm_input|rdrest|wrtrst" src`

## Function-level entry points
- `src/task/task_input.F` | role: first-stop parser for many startup/input failures.
- `src/task/task.F` | role: dispatch path when task routing fails.
- `src/util/errquit.F` | role: common abort/error emission utility.
- `src/util/util_checkmpirun.c` | role: MPI launcher/runtime environment checks.
- `src/nwdft/scf_dft_cg/dft_get_conv_info.F` | role: SCF/DFT convergence diagnostics.
- `src/nwdft/scf_dft_cg/dft_cg_guess.F` | role: SCF initial guess path for hard cases.
- `src/nwdft/scf_dft/dft_quickguess.F` | role: quick-guess logic used in recovery workflows.
- `src/nwdft/grid/grid_atom_type_info.F` | role: DFT grid/atom-type reporting and checks.
- `src/nwmd/md_input.F` | role: MD keyword parsing failures.
- `src/qmmm/qmmm_input.F` | role: QM/MM input validation failures.
- `src/qmmm/qmmm_main.F` | role: QM/MM runtime failures after parsing.
- `src/argos/argos_rdrest.F` | role: restart read failures for nwARGOS flows.
- `src/argos/argos_wrtrst.F` | role: restart write/continuation failures.

## Validation checkpoints
- Reproducer should fail/succeed at a known stage after one targeted change.
- Restart, convergence, and input parsing fixes should be validated independently.

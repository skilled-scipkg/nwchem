# nwchem source map: Web

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "task|input|md|qmmm|main|checkmpirun" src`
- `rg -n "task_input|task_shell_input|md_main|qmmm_main|argos_main" src`

## Function-level entry points
- `src/nwchem.F` | role: executable startup path behind command-line examples.
- `src/task/task_input.F` | role: parses user manual input syntax into internal tasks.
- `src/task/task_shell_input.F` | role: shell/task command handling referenced by docs.
- `src/task/task.F` | role: central dispatcher for documented task forms.
- `src/task/task_dynamics.F` | role: dynamics task behavior behind MD command docs.
- `src/nwdft/scf_dft/dft_main0d.F` | role: DFT task execution path tied to manual sections.
- `src/nwmd/md_main.F` | role: classical MD runtime implementation.
- `src/qmmm/qmmm_main.F` | role: QM/MM runtime implementation.
- `src/argos/argos_main.F` | role: nwARGOS runtime implementation.
- `src/util/input_echo.F` | role: echoes parsed input for validation against docs.
- `src/util/util_checkmpirun.c` | role: MPI launcher/runtime checks for execution guidance.

## Validation checkpoints
- Runtime command guidance should map to the expected startup and task handlers.
- MPI/sequential launch paths should show expected input echo and module dispatch.

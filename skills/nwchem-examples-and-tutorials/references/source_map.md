# nwchem source map: Examples and Tutorials

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "task md|task qmmm|record rest|test 10" QA/tests examples`
- `rg -n "task_dynamics|md_input|qmmm_input|argos_prop" src`

## Function-level entry points
- `src/task/task_input.F` | role: parses task declarations from example decks.
- `src/task/task.F` | role: dispatches SCF/DFT/MD/QMMM tasks.
- `src/task/task_dynamics.F` | role: shared task-level dynamics execution path.
- `src/nwmd/md_input.F` | role: MD block keyword parsing.
- `src/nwmd/md_main.F` | role: classical MD run control.
- `src/argos/argos_prop_setup.F` | role: nwARGOS propagation setup.
- `src/argos/argos_prop_step.F` | role: nwARGOS per-step propagation kernel.
- `src/argos/argos_prop_record.F` | role: nwARGOS restart/statistics recording.
- `src/qmmm/qmmm_input.F` | role: QM/MM block parsing.
- `src/qmmm/task_qmmm_dynamics.F` | role: QM/MM dynamics task handler.
- `src/qmmm/task_qmmm_energy.F` | role: QM/MM single-point/task energy path.

## Validation checkpoints
- Minimal copied deck should hit the expected task handler.
- Restart cadence (`record rest`/equivalent) should produce updated `.rst` files.

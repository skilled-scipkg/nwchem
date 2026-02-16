# nwchem source map: Qa

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "test 10|task md dynamics|task qmmm|record rest" QA/tests`
- `rg -n "task_dynamics|task_freq|task_qmmm|md_main|qmmm_main" src`

## Function-level entry points
- `src/task/task_input.F` | role: parses QA input deck tasks.
- `src/task/task.F` | role: central task dispatcher used in QA runs.
- `src/task/task_dynamics.F` | role: dynamics task path hit by MD QA cases.
- `src/task/task_freq.F` | role: frequency task path used in QA frequency checks.
- `src/nwmd/md_input.F` | role: MD keyword parser for QA MD decks.
- `src/nwmd/md_main.F` | role: MD execution control flow.
- `src/qmmm/qmmm_main.F` | role: QM/MM runtime entrypoint.
- `src/qmmm/qmmm_input.F` | role: QM/MM input parser.
- `src/qmmm/task_qmmm_energy.F` | role: QM/MM energy task implementation.
- `src/qmmm/task_qmmm_gradient.F` | role: QM/MM gradient task implementation.
- `src/qmmm/task_qmmm_optimize.F` | role: QM/MM optimize task implementation.
- `src/qmmm/task_qmmm_dynamics.F` | role: QM/MM dynamics task implementation.
- `src/argos/argos_prop_step.F` | role: nwARGOS step kernel used by MD QA cases.
- `src/argos/argos_prop_record.F` | role: restart/statistics recording in QA MD runs.
- `src/util/corr_mk_ref.F` | role: reference/correlation utility useful in result checks.

## Validation checkpoints
- QA decks should trigger the expected task handlers (`md`, `qmmm`, `freq`).
- Output trend checks against `.tst` should match within expected tolerance bands.

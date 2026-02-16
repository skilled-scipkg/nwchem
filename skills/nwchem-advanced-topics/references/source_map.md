# nwchem source map: Advanced Topics

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" src contrib`
- `rg -n "subroutine|function|task" src/task src/argos src/peigs_comm`

## Function-level entry points
- `src/nwchem.F` | role: top-level NWChem driver and startup flow.
- `src/task/task_input.F` | role: parses global `task` and module directives.
- `src/task/task.F` | role: dispatches task execution to module handlers.
- `src/argos/argos_main.F` | role: nwARGOS runtime entrypoint.
- `src/argos/argos_input.F` | role: nwARGOS input keyword parsing.
- `src/peigs_comm/peigs_dgop.F` | role: PEIGS collective operations used in parallel paths.
- `src/peigs_comm/mxsubs.F` | role: PEIGS data movement and distribution helpers.
- `src/nwpw/nwpwlib/Parallel/Parallel-mpi.F` | role: NWPW MPI setup and communicator usage.
- `src/util/util_checkmpirun.c` | role: launch/runtime MPI environment checks.

## Validation checkpoints
- Confirm `task` parsing reaches the expected module entry.
- For HPC regressions, verify communicator setup and collective calls in traces/logs.

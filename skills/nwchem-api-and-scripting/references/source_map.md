# nwchem source map: API and Scripting

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "python|rtdb_put|rtdb_get|task python|input_parse" src contrib`
- `rg -n "task_python|nwchem_wrap|Xrtdb" src contrib`

## Function-level entry points
- `src/python/task_python.c` | role: Python task bridge invoked by `task python`.
- `src/python/python_input.F` | role: reads/parses Python blocks from input decks.
- `src/python/nwchem_wrap.c` | role: exports core wrappers to embedded Python.
- `src/python/nw_inp_from_string.c` | role: runtime input injection from Python strings.
- `src/task/task_shell_input.F` | role: shell/Python-style task input handoff.
- `src/task/task_input.F` | role: global task parser before Python dispatch.
- `src/rtdb/rtdb.c` | role: RTDB storage API used by Python wrappers.
- `src/rtdb/rtdb_seq.c` | role: sequential RTDB backend operations.
- `src/geninterface/NWChemWrap.F` | role: generated interface shim for external wrappers.
- `contrib/python/Xrtdb.py` | role: high-level RTDB helper used in script examples.

## Validation checkpoints
- `task python` decks should execute without parser errors.
- `rtdb_put`/`rtdb_get` calls should round-trip expected values.

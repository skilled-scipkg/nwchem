---
name: nwchem-api-and-scripting
description: This skill should be used when users ask about api and scripting in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: API and Scripting

## High-Signal Playbook

### Route Conditions
- Use this skill for Python-embedded workflows, RTDB access from scripts, and interface-layer behavior.
- Route to `nwchem-inputs-and-modeling` when the user needs method/input tuning rather than API behavior.
- Route to `nwchem-troubleshooting` for runtime crashes or convergence failures.

### Canonical Workflow
1. Start from documented Python/interface references (`doc/user/python.tex`, `doc/user/interface.tex`, `doc/prog/rtdb.tex`).
2. Pick a runnable smoke test (`src/python/tests/test.nw` or `contrib/python/testxrtdb.nw`).
3. Run with `task python` unchanged first.
4. Validate RTDB read/write and wrapper calls in output.
5. If behavior diverges, inspect `references/source_map.md` entry points for parser/wrapper/RTDB paths.

### Minimal Working Example
```bash
nwchem src/python/tests/test.nw > test_python.out
```
- Baseline Python wrapper smoke test with RTDB put/get operations.

```nwchem
python
rtdb_put("test_int2", 22)
print(rtdb_get("test_int2"))
end

task python
```
- Minimal pattern used by `src/python/tests/test.nw`.

### Validation checkpoints
- `task python` executes without parser/runtime abort.
- `rtdb_put` and `rtdb_get` values round-trip as expected.
- Wrapper-exposed constants/types are available in Python scope.

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses architectural first, then function-level only when needed.

## Primary documentation references
- `doc/user/python.tex`
- `doc/user/interface.tex`
- `doc/prog/rtdb.tex`
- `contrib/python/README`
- `src/python/tests/test.nw`
- `contrib/python/testxrtdb.nw`
- `web/support/faq/NWChem_FAQ.html`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use examples/tests as executable usage patterns.
- If ambiguity remains, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `examples`
- `web/training`
- `web/support/examples`

## Test references
- `QA`
- `contrib/mov2asc/test`
- `src/cca/tests`
- `src/chelp/test`
- `src/fft/test`
- `src/mm/test`
- `src/python/tests`
- `src/rism/QA`
- `src/rism/tests`
- `src/smd/test`
- `contrib/marat/rdf/test`
- `contrib/marat/swap/test`

## Optional deeper inspection
- `contrib`
- `src`

## Source entry points for unresolved issues
- `src/python/task_python.c`
- `src/python/python_input.F`
- `src/python/nwchem_wrap.c`
- `src/python/nw_inp_from_string.c`
- `src/task/task_shell_input.F`
- `src/task/task_input.F`
- `src/rtdb/rtdb.c`
- `src/rtdb/rtdb_seq.c`
- `src/geninterface/NWChemWrap.F`
- `contrib/python/Xrtdb.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

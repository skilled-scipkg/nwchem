---
name: nwchem-advanced-topics
description: This skill should be used for lower-frequency, single-doc topics consolidated from getting-started, readme, parallel-hpc, and simulation-workflows in nwchem.
---

# nwchem: Advanced Topics

## Scope
- Consolidated, lower-frequency topics covering legacy onboarding, PEIGS/HPC notes, and QA batch workflow entrypoints.
- Use this skill to quickly orient, then route to a specialized topic skill for module-level tuning.

## Route the request
- Use this skill when the request directly matches legacy getting-started, PEIGS performance, QA batch-run flow, or top-level repo context.
- Route to `nwchem-inputs-and-modeling` for concrete keyword and model tuning.
- Route to `nwchem-examples-and-tutorials` for runnable templates.
- Route to `nwchem-web` for broader doc-hub navigation.

## Consolidated tracks
- Getting-started context: `doc/nwahtml/nwargos_intro.html`
- Parallel/HPC PEIGS note: `web/doc/peigs/docs/performance.old.html`
- QA workflow entrypoint: `QA/runtest.md`
- Legacy repository notice: `doc/README.md`

## High-Signal Playbook

### Canonical Workflow
1. Pick the track above that matches the user request.
2. For run validation, start from `QA/runtest.md` and `QA/runtest.unix`.
3. Run one small QA case first, then expand to subsets.
4. If behavior differs from docs, inspect `references/source_map.md` and check task-dispatch/runtime entry points.

### Minimal Working Example
```bash
cd QA
./runtest.unix procs 2 \
  $NWCHEM_TEST/water/water_md
```
- Uses the documented QA runner path for a short regression smoke test.

### Validation checkpoints
- Runner starts and resolves the requested test path.
- Output includes expected task completion markers for selected cases.
- Any MPI/runtime launch issue is isolated before scaling test count.

## Primary documentation references
- `doc/nwahtml/nwargos_intro.html`
- `web/doc/peigs/docs/performance.old.html`
- `QA/runtest.md`
- `QA/runtest.unix`
- `doc/README.md`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for full inventory and `references/source_map.md` for function-level entry points.
- Keep responses compact and route to specialized skills once a concrete run/input/debug path is identified.

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
- `src/nwchem.F`
- `src/task/task_input.F`
- `src/task/task.F`
- `src/argos/argos_main.F`
- `src/argos/argos_input.F`
- `src/peigs_comm/peigs_dgop.F`
- `src/peigs_comm/mxsubs.F`
- `src/nwpw/nwpwlib/Parallel/Parallel-mpi.F`
- `src/util/util_checkmpirun.c`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

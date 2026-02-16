---
name: nwchem-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: Examples and Tutorials

## High-Signal Playbook

### Route Conditions
- Use this skill when the user asks for a runnable template, a worked starting point, or a "closest example" to adapt.
- Route to `nwchem-inputs-and-modeling` for detailed keyword tuning and physical parameter choices.
- Route to `nwchem-qa` when the goal is pass/fail regression validation.
- Route to `nwchem-nwahtml` when setup utilities (`nwTOP`, `nwRST`, sequence/topology files) are the blocker.

### Triage Questions
- Which task family is needed (`md dynamics`, `qmmm dft`, SCF/DFT single-point/optimize)?
- Do you need smallest smoke-test input or production-like settings?
- Which companion files are available (`.top`, `.rst`, `.pdb`)?
- Is PME/polarization/QM-MM coupling required?
- What is the acceptable run length for first validation (steps/iterations)?
- Which observable must match (energy, temperature/pressure trend, gradient/frequency)?

### Canonical Workflow
1. Select the closest seed deck from QA/tutorial assets (`QA/tests/water`, `QA/tests/trypsin`, `QA/tests/qmmm_grad0`, `QA/tests/qmmm_opt0`).
2. Copy the matching companion files (`.top`, `.rst`, and optional `.pdb`) together with the `.nw` file.
3. Rename `start` and `md system` identifiers consistently across files.
4. Keep the original task type unchanged for first run.
5. Run a short smoke test (`test 10` style or small data window).
6. Validate against `.tst`/reference output for the first steps.
7. Only then expand to production settings (remove `test 10`, increase `data`, set restart cadence).
8. If file-format details are unclear, use nwARGOS tutorial/file-structure docs.

### Minimal Working Example
```bash
cp QA/tests/water/water_pme.nw ./water_pme.nw
cp QA/tests/water/water.top ./water.top
cp QA/tests/water/water_pme.rst ./water_pme.rst
nwchem water_pme.nw > water_pme.out
```
- Uses the same files/layout as documented QA water regression assets (`QA/tests/water/water_pme.nw`).

```nwchem
start water_pme
md
  step 0.001 equil 0 data 10
  cutoff 1.0
  leapfrog
  pme grid 16 order 4 fft 1
  print step 1 stat 10 topol extra out6
  update pairs 1 center 0 rdf 0
  test 10
end
task md dynamics
```
- Compact MD+PME template extracted from `QA/tests/water/water_pme.nw`.

### Pitfalls and Fixes
- Example `.rst` files are restart snapshots, not standalone input decks.
  Fix: execute `.nw` decks and keep `.rst` only as state input (`examples/md/water/water_1.rst`, `QA/tests/water/water_pme.nw`).
- Missing companion topology/restart files causes immediate setup failure.
  Fix: copy `.top` and matching `.rst` alongside the deck (e.g., `QA/tests/water/`).
- QA-only controls (`test 10`, tiny `data`) accidentally used for production.
  Fix: remove `test`, expand data-gathering/restart intervals after validation (`QA/tests/water/water_md.nw`).
- Restart prefix drift (`start`, `system`, and filenames differ).
  Fix: keep `start <id>`, `md system <id>`, and `<id>.rst` consistent.
- QM/MM template incomplete.
  Fix: include `prepare`, QM basis/SCF/DFT block, `md system`, `qmmm` block, and task (`web/support/faq/qmmm.html`, `QA/tests/qmmm_grad0/qmmm_grad0.nw`).
- Restart continuity missing.
  Fix: set restart recording in MD blocks and verify new `.rst` file appears (`web/support/faq/md.html`).

### Convergence and Validation Checks
- Compare first-step thermodynamic trends against QA `.tst` where applicable (`QA/tests/water/water_md.tst`, `QA/tests/water/water_pme.tst`, `QA/tests/trypsin/trypsin_md.tst`).
- Confirm expected task completion line (`task md dynamics`, `task qmmm ...`) in output.
- Verify restart files are updated at requested cadence.
- For QM/MM gradient checks, confirm both `mm_charges exclude all` and `expand all` modes run when reproducing `qmmm_grad0` behavior (`QA/tests/qmmm_grad0/qmmm_grad0.nw`).
- If behavior diverges, escalate to source entry points for water/QM-MM kernels.

## Scope
- Handle questions about worked examples, tutorials, and cookbook usage.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `examples/md/ache/mache.nw`
- `examples/md/ache/mache_md.rst`
- `examples/md/water/water_1.rst`
- `examples/md/water/water_2.rst`
- `examples/md/water/water_3.rst`
- `examples/md/water/water_4.rst`
- `examples/md/water/water_5.rst`
- `QA/tests/water/water_md.nw`
- `QA/tests/water/water_pme.nw`
- `QA/tests/water/water_md.tst`
- `QA/tests/water/water_pme.tst`
- `QA/tests/trypsin/trypsin_md.nw`
- `QA/tests/trypsin/trypsin_md.tst`
- `QA/tests/qmmm_grad0/qmmm_grad0.nw`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
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
- `src/task/task_input.F`
- `src/task/task.F`
- `src/task/task_dynamics.F`
- `src/nwmd/md_input.F`
- `src/nwmd/md_main.F`
- `src/argos/argos_prop_setup.F`
- `src/argos/argos_prop_step.F`
- `src/argos/argos_prop_record.F`
- `src/qmmm/qmmm_input.F`
- `src/qmmm/task_qmmm_dynamics.F`
- `src/qmmm/task_qmmm_energy.F`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

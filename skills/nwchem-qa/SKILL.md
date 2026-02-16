---
name: nwchem-qa
description: This skill should be used when users ask about qa in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: Qa

## High-Signal Playbook

### Route Conditions
- Use this skill for regression reproduction, baseline verification, and "did this change behavior?" questions.
- Route to `nwchem-examples-and-tutorials` if the user wants a starter input but not strict regression validation.
- Route to `nwchem-troubleshooting` when failures need root-cause diagnosis beyond output comparison.
- Route to `nwchem-inputs-and-modeling` when designing new physics/method settings rather than validating existing ones.

### Triage Questions
- Which QA case or module should be validated (`water`, `trypsin`, `qmmm_grad0`, `qmmm_opt0`, etc.)?
- Is the goal smoke test, numerical drift check, or full reference reproduction?
- Which processor count/runtime mode is being used?
- Which files define expected behavior (`.tst`, `.out`, restart/topology files)?
- Do you need single-case repro or a batch subset (`runtest.unix`)?
- What tolerance is acceptable for the target observable?

### Canonical Workflow
1. Choose the closest existing QA directory and inspect `.nw` plus companion `.top/.rst/.pdb` files.
2. Identify expected observables from `.tst` or reference `.out`.
3. Run the case directly (`nwchem test.nw`) or as subset via `runtest.unix`.
4. Compare early-step trends and final task completion markers.
5. If mismatch occurs, reduce to minimal reproducer with one test case and one changed variable.
6. Escalate to source entry points (`qmmm_grad0`, `qmmm_opt0`, task handlers) only after repro is stable.

### Minimal Working Example
```bash
cd /tmp
runtest.unix procs 2 \
  $NWCHEM_TEST/na_k/nak \
  $NWCHEM_TEST/water/water_md
```
- Batch subset command follows `QA/runtest.md`.

```bash
cd QA/tests/water
nwchem water_md.nw > water_md.local.out
```
- Direct single-case execution for fast regression checks (`QA/tests/water/water_md.nw`).

### Pitfalls and Fixes
- Running only `.nw` without required `.top/.rst` companions.
  Fix: keep full case directory contents together (`QA/tests/water`, `QA/tests/qmmm_grad0`, `QA/tests/qmmm_opt0`).
- Missing `NWCHEM_TEST` path for `runtest.unix`.
  Fix: use direct path execution or export `NWCHEM_TEST` before running (`QA/runtest.md`).
- Treating QA `test 10` controls as production settings.
  Fix: use QA controls only for validation speed, then remove for real production decks.
- QM/MM gradient differences caused by MM charge treatment.
  Fix: compare like-for-like (`mm_charges exclude all` vs `expand all`) as in `qmmm_grad0` (`QA/tests/qmmm_grad0/qmmm_grad0.nw`).
- Comparing wrong reference artifact.
  Fix: use `.tst` for stepwise thermodynamic checks and `.out` for full textual behavior.
- Parallel-count changes affecting timing interpretation.
  Fix: compare physics outputs first (energy/temperature/pressure trend), not wall-clock lines.

### Convergence and Validation Checks
- Water MD/PME: first 10-step trend should match QA bands in `QA/tests/water/water_md.tst` and `QA/tests/water/water_pme.tst`.
- Polarizable water: confirm rising temperature/pressure trend consistent with `QA/tests/water/waterp_md.tst` for smoke runs.
- Trypsin MD: verify temperature stays near 298 K and energy plateau aligns with `QA/tests/trypsin/trypsin_md.tst`.
- QM/MM runs: ensure each declared `task qmmm ...` completes and writes expected sections.
- Regression sign-off: record exact input file, processor count, and comparison file used.

## Scope
- Handle questions about documentation grouped under the 'qa' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `QA/runtest.md`
- `QA/runtest.unix`
- `QA/tests/water/water_md.nw`
- `QA/tests/water/water_pme.nw`
- `QA/tests/water/waterp_md.nw`
- `QA/tests/water/water_md.tst`
- `QA/tests/water/water_pme.tst`
- `QA/tests/water/waterp_md.tst`
- `QA/tests/trypsin/trypsin_md.nw`
- `QA/tests/trypsin/trypsin_md.tst`
- `QA/tests/qmmm_grad0/qmmm_grad0.nw`
- `QA/tests/qmmm_opt0/qmmm_opt0.nw`
- `QA/tests/qmmm_esp0/qmmm_esp0.nw`
- `QA/tests/qmmm_freq/qmmm_freq.nw`
- `QA/tests/qmmm_rename/qmmm_rename.nw`

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
- `src/task/task_freq.F`
- `src/nwmd/md_input.F`
- `src/nwmd/md_main.F`
- `src/qmmm/qmmm_main.F`
- `src/qmmm/qmmm_input.F`
- `src/qmmm/task_qmmm_energy.F`
- `src/qmmm/task_qmmm_gradient.F`
- `src/qmmm/task_qmmm_optimize.F`
- `src/qmmm/task_qmmm_dynamics.F`
- `src/argos/argos_prop_step.F`
- `src/argos/argos_prop_record.F`
- `src/util/corr_mk_ref.F`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

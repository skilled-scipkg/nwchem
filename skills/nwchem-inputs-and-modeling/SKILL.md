---
name: nwchem-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: Inputs and Modeling

## High-Signal Playbook

### Route Conditions
- Use this skill when the user is defining or editing NWChem input blocks (SCF/DFT/QM/MM/MD), basis mapping, or model controls.
- Route to `nwchem-examples-and-tutorials` when the user asks for a ready-made deck to adapt.
- Route to `nwchem-qa` when the user asks for regression-style validation against known outputs.
- Route to `nwchem-troubleshooting` when there is an existing crash/non-convergence log to diagnose.

### Triage Questions
- Which task is required: `task scf|dft|md dynamics|qmmm ...`?
- Is this a fresh start or restart, and which files already exist (`.top`, `.rst`, `.pdb`, `.movecs`)?
- Which accuracy target is needed (routine vs weak interactions/high-accuracy)?
- Are symmetry and orbital adaptation expected to stay on (`SYM/ADAPT`) or be disabled intentionally?
- For DFT, is default `GRID` sufficient, or is a denser grid/tighter tolerances needed?
- For MD/QM-MM, what timestep, cutoff, thermostat/barostat, and restart cadence are required?

### Canonical Workflow
1. Start from the smallest runnable deck with explicit `start` and final `task` line (docs: `web/support/faq/input.html`).
2. Define geometry and basis with unambiguous atom tags; verify basis-tag mapping rules before execution (docs: `web/support/faq/basis.html`).
3. Set SCF controls from required precision: `thresh`, optional `tol2e`, and `maxiter` (docs: `web/doc/user.4.6/node12.html`).
4. For DFT, pick grid/tolerance defaults first; tighten only if needed (docs: `web/doc/user.4.6/node13.html`).
5. For MD/QM-MM, set `step`, `equil`, `data`, cutoffs, and explicit restart recording (docs: `doc/nwahtml/nwargos_input.html`, `web/support/faq/md.html`).
6. Run a short smoke test (10-100 steps or a single-point) before longer jobs.
7. If behavior differs from docs, inspect `references/source_map.md` entry points with targeted symbol search.

### Minimal Working Example
```nwchem
start h2o
geometry
  O  0.000000  0.000000  0.221431
  H  1.430429  0.000000 -0.885726
  H -1.430429  0.000000 -0.885726
end
basis
  O library 6-31g*
  H library 6-31g*
end
scf
  thresh 1e-6
  tol2e 1e-9
  maxiter 50
end
task scf
```
- SCF precision controls and defaults come from `web/doc/user.4.6/node12.html`.

```nwchem
start water_md
md
  step 0.001 equil 0 data 100
  cutoff 0.9
  leapfrog
  isotherm
  isobar
  record rest 500
end
task md dynamics
```
- MD step/cutoff/restart pattern matches documented MD usage and QA decks (`web/support/faq/md.html`, `QA/tests/water/water_md.nw`).

### Pitfalls and Fixes
- `! warning: processed input with no task`.
  Fix: add a blank line after the final `task` line so parser reads it (docs: `web/support/faq/input.html`).
- Basis unexpectedly applied to wrong atoms.
  Fix: use exact tag matches, then element-level fallback; avoid ambiguous tags (docs: `web/support/faq/basis.html`).
- `ADAPT OFF` with `SYM ON` causes unstable/incorrect convergence.
  Fix: pair `ADAPT OFF` with `SYM OFF` (docs: `web/doc/user.4.6/node12.html`).
- Large diffuse basis stalls near the end of SCF.
  Fix: use realistic `thresh` (`~1e-7`) and tighter `tol2e` (`~1e-10`) when needed (docs: `web/support/faq/guess.html`, `web/doc/user.4.6/node12.html`).
- DFT BSSE setup uses plain `bq` and misses grid terms.
  Fix: label ghost atoms as `bqX` (e.g., `bqO`, `bqH`) and matching basis tags (docs: `web/doc/user.4.6/node13.html`, `web/support/faq/dft.html`).
- MD restart file not advancing.
  Fix: set nonzero restart recording frequency (`record rest` / restart frequency keywords) (docs: `web/support/faq/md.html`, `doc/nwahtml/nwargos_input.html`).

### Convergence and Validation Checks
- SCF/DFT: confirm orbital-gradient threshold reached and not just max-iteration exit (docs: `web/doc/user.4.6/node12.html`).
- Tightening check: rerun with one-step tighter `thresh` to ensure energy/gradient stability for target use.
- DFT numerical stability: compare default `GRID medium` vs one finer grid for sensitive properties (docs: `web/doc/user.4.6/node13.html`).
- MD sanity: verify early-step temperature/pressure/energy trends against QA reference bands when using QA-like systems (`QA/tests/water/water_md.tst`, `QA/tests/water/water_pme.tst`).
- Restart integrity: ensure `.rst` is produced and can be reused for continuation.

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `web/doc/user.4.6/node44.html`
- `web/doc/user.4.6/node12.html`
- `doc/nwahtml/nwargos_nwinput.html`
- `doc/nwahtml/nwargos_input.html`
- `doc/nwahtml/nwargos_forcefields.html`
- `web/support/faq/input.html`
- `web/support/faq/basis.html`

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
- `src/geom/geom_input.F`
- `src/basis/bas_input.F`
- `src/nwdft/scf_dft/dft_main0d.F`
- `src/nwdft/scf_dft/dft_scf.F`
- `src/nwdft/grid/grid_setrad.F`
- `src/NWints/rel/modelpotential_input.F`
- `src/nwmd/md_input.F`
- `src/qmmm/qmmm_input.F`
- `src/argos/argos_input.F`
- `src/nwdft/rt_tddft/input/rt_tddft_input.F`
- `src/nwdft/rt_tddft/input/rt_tddft_input_field.F`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

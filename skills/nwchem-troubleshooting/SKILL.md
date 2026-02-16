---
name: nwchem-troubleshooting
description: This skill should be used when users ask about troubleshooting in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: Troubleshooting

## High-Signal Playbook

### Route Conditions
- Use this skill when the user provides an error message, non-convergence behavior, restart failure, or platform/runtime issue.
- Route to `nwchem-qa` when a reproducible regression comparison is required.
- Route to `nwchem-inputs-and-modeling` when the issue is resolved and the user now needs a clean production input.
- Route to `nwchem-web`/`nwchem-advanced-topics` for legacy platform/build-policy questions.

### Triage Questions
- What is the exact error/warning text?
- Which task/module was running (`scf`, `dft`, `md`, `qmmm`, optimization)?
- Is this a startup run or restart run?
- What changed since the last successful run (input, basis, machine, process count)?
- Is the issue deterministic on a minimal case?
- Which convergence/settings are currently used (`thresh`, grids, memory, semidirect/direct)?
- Which platform/runtime path is used (MPI, legacy IBM SP settings, etc.)?

### Canonical Workflow
1. Classify the failure into input parse, convergence, memory/runtime, restart, or platform class.
2. Apply one documented fix at a time (avoid multi-change debugging).
3. Re-run a short reproducer (single-point or short MD steps).
4. If fixed, reintroduce original settings incrementally.
5. If unresolved, map symptom to module source entry points and inspect targeted routines.
6. Capture final known-good diff and validation checks.

### Minimal Working Example
```nwchem
# DFT memory pressure workaround
set fock:replicated logical .false.

dft
  grid ssf euler lebedev 75 11
end
```
- Fix and grid alignment are from `web/support/faq/dft.html`.

```nwchem
# MD restart-safe pattern
md
  system QJDa_md1
  pme grid 64 order 4
  record rest 500
end
task md dynamics

task shell "cp QJDa_md1.rst QJDa_md2.rst"
```
- Restart flow is documented in `web/support/faq/md.html`.

### Pitfalls and Fixes
- `! warning: processed input with no task`.
  Fix: append a blank line after final `task` line (docs: `web/support/faq/input.html`).
- Open-shell or ionic SCF fails to converge.
  Fix: use staged guesses (minimal basis/project/fragment/swap strategies) before full model (docs: `web/support/faq/guess.html`).
- Large-basis final-iteration stagnation.
  Fix: use realistic `thresh` (often around `1e-7`) plus tighter `tol2e` and avoid over-tight unreachable targets (docs: `web/support/faq/guess.html`, `web/doc/user.4.6/node12.html`).
- DFT error `ao_replicated: insufficient memory`.
  Fix: switch to distributed-data Fock build via `set fock:replicated logical .false.` (docs: `web/support/faq/dft.html`).
- DFT virtual orbital energies appear shifted.
  Fix: account for level-shift contribution before interpreting virtual levels (docs: `web/support/faq/dft.html`).
- AUTOZ/internal coordinate failures in optimizations.
  Fix: verify units/geometry sanity, use `noautoz` where needed, or regenerate internals (`REDOAUTOZ`) (docs: `web/support/faq/optimization.html`).
- Geometry restart not picked up.
  Fix: keep same `permanent_dir`, use `restart <name>`, and preserve restart files (docs: `web/support/faq/optimization.html`).
- MD continuation not advancing.
  Fix: ensure nonzero restart recording frequency and consistent system/restart naming (docs: `web/support/faq/md.html`).

### Convergence and Validation Checks
- Error-class check: original error text is absent in rerun output.
- Iteration check: run reaches requested threshold rather than max-iteration fallback (`THRESH`/`MAXITER` guidance in `web/doc/user.4.6/node12.html`).
- DFT stability check: one-step tighter grid/tolerances does not materially change target observable (`web/doc/user.4.6/node13.html`).
- Restart check: new `.rst` file is produced and successfully reused.
- Regression check: short QA case remains within expected trends after the fix.

## Scope
- Handle questions about known issues, diagnostics, and debugging patterns.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `web/support/faq/md.html`
- `web/support/faq/windowso.html`
- `web/support/faq/windows.html`
- `web/support/faq/test.html`
- `web/support/faq/sgi.html`
- `web/support/faq/qmmm.html`
- `web/support/faq/optimization.html`
- `web/support/faq/mp2.html`
- `web/support/faq/linux.html`
- `web/support/faq/itanium.html`
- `web/support/faq/info.html`
- `web/support/faq/ibm.html`

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
- `src/util/errquit.F`
- `src/util/util_checkmpirun.c`
- `src/nwdft/scf_dft_cg/dft_get_conv_info.F`
- `src/nwdft/scf_dft_cg/dft_cg_guess.F`
- `src/nwdft/scf_dft/dft_quickguess.F`
- `src/nwdft/grid/grid_atom_type_info.F`
- `src/nwmd/md_input.F`
- `src/qmmm/qmmm_input.F`
- `src/qmmm/qmmm_main.F`
- `src/argos/argos_rdrest.F`
- `src/argos/argos_wrtrst.F`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

---
name: nwchem-nwahtml
description: This skill should be used when users ask about nwahtml in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: Nwahtml

## High-Signal Playbook

### Route Conditions
- Use this skill for nwARGOS-oriented docs: topology/sequence/restart file model, setup utilities, legacy MD workflow, and benchmark families.
- Route to `nwchem-inputs-and-modeling` for detailed SCF/DFT/QM-MM keyword tuning in modern input blocks.
- Route to `nwchem-examples-and-tutorials` for quick runnable `.nw` seeds.
- Route to `nwchem-troubleshooting` for runtime failures and convergence problems.

### Triage Questions
- Are `project.top`, `project_id.rst`, `project_id.inp`, and `project_id` already available?
- Is this a new setup or a restart/continuation run?
- Which force field is intended, and is it supported in current docs?
- Is the target task SP, MD, EM, or MCTI?
- Do you need sequence/segment database generation (`nwTOP`, `nwSGM`) or only run-time input edits?
- What system size/benchmark class is targeted (water boxes, protein, crown-ether free-energy)?

### Canonical Workflow
1. Validate naming and file-role conventions (`project.top`, `project_id.*`) (docs: `doc/nwahtml/nwargos_files.html`).
2. Prepare topology data (segment definitions if needed) and run `nwTOP` to generate `project.top` (docs: `doc/nwahtml/nwargos_nwTOP.html`, `doc/nwahtml/nwargos_topology.html`).
3. Generate initial restart state with `nwRST` flow (docs: `doc/nwahtml/nwargos_restart.html`).
4. Write `project_id.inp` with nwARGOS simulation keywords and task selection (docs: `doc/nwahtml/nwargos_input.html`).
5. Write `project_id` NWChem control file (`memory`, `start`, `task nwargos`) (docs: `doc/nwahtml/nwargos_nwinput.html`).
6. Run `nwchem project_id` and verify output/restart artifacts (docs: `doc/nwahtml/nwargos_tutor.html`).
7. For scale/benchmark expectations, compare against published nwARGOS benchmark families (docs: `doc/nwahtml/nwargos_bmrks.html`).

### Minimal Working Example
```text
# project_id (NWChem control file)
title; nwARGOS
memory noverify heap 1 mb stack 32 mb global 8 mb
start nwarg
task nwargos
```
- Minimal control file from `doc/nwahtml/nwargos_nwinput.html`.

```text
# project_id.inp (nwARGOS keyword file, minimal skeleton)
Title Example run
Task MD
Time step 0.001
Equilibration steps 0
Data gathering steps 100
Frequency recording restart 500
```
- Task and defaulted keyword family from `doc/nwahtml/nwargos_input.html`.

### Pitfalls and Fixes
- Missing one of the required four core files.
  Fix: provide all files listed in tutorial before running (docs: `doc/nwahtml/nwargos_tutor.html`).
- Misnamed topology file.
  Fix: use `project.top` (not `project_id.top`) per file-structure convention (docs: `doc/nwahtml/nwargos_files.html`).
- Confusing NWChem control file with nwARGOS keyword file.
  Fix: keep `project_id` for `task nwargos` and `project_id.inp` for simulation keywords.
- Unsupported force field assumed available.
  Fix: verify supported list; only AMBER is marked available in this doc set (docs: `doc/nwahtml/nwargos_forcefields.html`).
- Sequence/segment connectivity issues.
  Fix: validate sequence card format and segment terminators before `nwTOP` (docs: `doc/nwahtml/nwargos_sequence.html`).
- No restart continuity across runs.
  Fix: ensure restart write frequency is nonzero and preserve generated `.rst` files.

### Convergence and Validation Checks
- Pre-run: confirm existence of `project.top`, `project_id.rst`, `project_id.inp`, and `project_id`.
- Startup: check that NWChem hands off to nwARGOS and begins requested task class.
- Dynamics: verify step/time and recorded statistics evolve as expected for chosen benchmark class.
- Restart: rerun from produced `.rst` and confirm continuation behavior.
- Scaling sanity: for replicated-box benchmarks, ensure physically comparable trajectories across system-size variants (docs: `doc/nwahtml/nwargos_bmrks.html`).

## Scope
- Handle questions about documentation grouped under the 'nwahtml' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/nwahtml/nwargos_xmpls.html`
- `doc/nwahtml/nwargos_tutor.html`
- `doc/nwahtml/nwargos_topology.html`
- `doc/nwahtml/nwargos_sequence.html`
- `doc/nwahtml/nwargos_restart.html`
- `doc/nwahtml/nwargos_nwTOP.html`
- `doc/nwahtml/nwargos_nwSGM.html`
- `doc/nwahtml/nwargos_nwRST.html`
- `doc/nwahtml/nwargos_imple.html`
- `doc/nwahtml/nwargos_funct.html`
- `doc/nwahtml/nwargos_files.html`
- `doc/nwahtml/nwargos_extensions.html`

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
- `src/argos/argos_main.F`
- `src/argos/argos_input.F`
- `src/argos/argos_prepare_input.F`
- `src/argos/argos_prepare_mktop.F`
- `src/argos/argos_prepare_wrtrst.F`
- `src/argos/argos_rdrest.F`
- `src/argos/argos_wrtrst.F`
- `src/argos/argos_prop_setup.F`
- `src/argos/argos_prop_step.F`
- `src/argos/argos_prop_record.F`
- `src/argos/argos_cafe_print_top.F`
- `src/util/seq_output.F`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

---
name: nwchem-src
description: This skill should be used when users ask about src in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: Src

## High-Signal Playbook

### Route Conditions
- Use this skill for source-level questions tied to module behavior (RI-MP2 gradients, RISM, RTDB, CCA interfaces).
- Route to `nwchem-inputs-and-modeling` for user-level input construction.
- Route to `nwchem-qa` for strict regression workflows.

### Canonical Workflow
1. Start from module docs in `references/doc_map.md`.
2. Select one runnable module case (for example `src/rism/QA/h2o.nw`).
3. Run unchanged once to establish baseline output.
4. Map observed behavior to the module entry points in `references/source_map.md`.
5. Inspect only the smallest set of routines needed for the reported symptom.

### Minimal Working Example
```bash
cd src/rism/QA
nwchem h2o.nw > h2o.local.out
```
- Direct source-tree RISM smoke run with companion data files in the same folder.

### Validation checkpoints
- Task reaches the expected module handler (`task rism`, RI-MP2 gradient path, etc.).
- Output sections tied to the target module appear and are internally consistent.
- Any source-level hypothesis is confirmed on a minimal reproducer before broader claims.

## Scope
- Handle source-oriented documentation and low-level developer references.
- Keep responses targeted and evidence-based; avoid broad code tours.

## Primary documentation references
- `src/rimp2_grad/doc/manifest.txt`
- `src/basis/doc/api`
- `src/cca/tests/reference.txt`
- `src/rtdb/rtdb.doc`
- `src/rism/tests/CH3NH3/Readme_CH3NH3.txt`
- `src/rism/tests/CH3CO2H/readme_CH3CO2H.txt`
- `src/rism/tests/CH3CO2/readme_CH3CO2.txt`
- `src/rism/QA/h2o.nw`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use runnable module tests as behavior anchors.
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
- `src/rimp2_grad/rimp2g.F`
- `src/rimp2_grad/rimp2g_parm.F`
- `src/rimp2_grad/driver_g.F`
- `src/rimp2_grad/e_final.F`
- `src/rism/rism_input.F`
- `src/rism/task_rism.F`
- `src/rism/rism_output.F`
- `src/rtdb/rtdb.c`
- `src/rtdb/rtdb_seq.c`
- `src/cca/Chemistry/server/cxx/NWChem_Chemistry_QC_ModelFactory_Impl.cc`
- `src/cca/Chemistry/server/cxx/NWChem_Chemistry_QC_Model_Impl.cc`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

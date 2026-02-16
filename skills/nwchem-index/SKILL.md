---
name: nwchem-index
description: This skill should be used when users ask how to use nwchem and the correct generated documentation skill must be selected before going deeper into source code.
---

# nwchem Skills Index

## Route the request
- Classify the request into one of the generated topic skills listed below.
- Prefer abstract, workflow-level guidance for large scientific packages; do not attempt full function-by-function coverage unless explicitly requested.

## Generated topic skills
- `nwchem-inputs-and-modeling`: Inputs and Modeling (input decks, basis/tag mapping, SCF/DFT/QM-MM/MD model controls)
- `nwchem-examples-and-tutorials`: Examples and Tutorials (runnable templates, adaptation patterns, smoke-test flows)
- `nwchem-troubleshooting`: Troubleshooting (known issues, diagnostics, and doc-backed fixes)
- `nwchem-qa`: Qa (regression/test workflows and expected behavior baselines)
- `nwchem-nwahtml`: Nwahtml (nwARGOS file model, setup utilities, restart/topology conventions)
- `nwchem-web`: Web (documentation hubs, release/support routing, runtime command references)
- `nwchem-api-and-scripting`: API and Scripting (language bindings, APIs, and programmatic interfaces)
- `nwchem-src`: Src (source-oriented docs and low-level developer references)
- `nwchem-advanced-topics`: Advanced Topics (collapsed one-doc topics: getting started, readme, parallel-hpc, simulation-workflows)

## Documentation-first inputs
- `doc`
- `web`
- `src/basis/doc`
- `src/peigs/doc`
- `src/rimp2_grad/doc`
- `src/mrpt/fci/doc`

## Tutorials and examples roots
- `examples`
- `web/training`
- `web/support/examples`

## Test roots for behavior checks
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

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, search the topic skill `references/doc_map.md`.
- If documentation still leaves ambiguity, open `references/source_map.md` inside the same topic skill and inspect the suggested source entry points.
- Use targeted symbol search while inspecting source (e.g., `rg -n "<symbol_or_keyword>" contrib src`).

## Source directories for deeper inspection
- `contrib`
- `src`

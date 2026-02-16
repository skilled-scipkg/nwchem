---
name: nwchem-web
description: This skill should be used when users ask about web in nwchem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# nwchem: Web

## High-Signal Playbook

### Route Conditions
- Use this skill to navigate official NWChem web docs: manual entry points, release notes, FAQs, known bugs, support, training, and benchmark pages.
- Route to `nwchem-inputs-and-modeling` for detailed input keyword construction.
- Route to `nwchem-troubleshooting` for concrete error diagnosis and fix workflow.
- Route to `nwchem-qa` for test-backed validation once guidance is selected.

### Triage Questions
- Is the user asking about capabilities, runtime commands, troubleshooting docs, or historical release behavior?
- Which version context matters for the question?
- Does the user need authoritative syntax (manual) or change summary (release notes)?
- Is the run mode sequential, MPI, or legacy batch scheduler?
- Is the request about support/reporting channels?

### Canonical Workflow
1. Start from the site hubs (`web/index.html`, `web/nwchem_index.html`, `web/nwchem_main.html`).
2. Route to the correct documentation family:
   - syntax/behavior: `web/doc/user.4.6/index.html` and `web/doc/user.4.6/node44.html`
   - known issues: `web/support/known_bugs.html` and FAQ pages
   - release deltas: `web/release-notes/index.html`
   - tutorials/benchmarks: `web/training/training.html`, `web/benchmarks/index.html`
3. Apply precedence: user manual over release-note summaries on conflicts (docs: `web/release-notes/index.html`).
4. For execution commands, use the runtime section in `web/doc/user.4.6/node44.html`.
5. If docs are insufficient, escalate to source entry points tied to module mains.

### Minimal Working Example
```bash
# sequential
nwchem input.nw >& input.out &

# MPI
mpirun -np 8 $NWCHEM_TOP/bin/$NWCHEM_TARGET/nwchem input.nw
```
- Command forms come from `web/doc/user.4.6/node44.html`.

```csh
#!/bin/csh -x
# @ job_type         =    parallel
# @ class            =    small
# @ network.lapi     = css0,not_shared,US
# @ min_processors   =    7
# @ max_processors   =    7
# @ queue
cd /scratch
nwchem <INPUT_FILE_NAME>
```
- IBM SP LoadLeveler starter template from `web/doc/user.4.6/node44.html`.

### Pitfalls and Fixes
- Treating release notes as full reference docs.
  Fix: use release notes for deltas, then validate syntax in user manual (docs: `web/release-notes/index.html`).
- Using stale homepage announcements as current capability inventory.
  Fix: check capability and manual pages (`web/capabilities/nwchem_capab.html`, `web/doc/user.4.6/index.html`).
- Running MPI jobs through `parallel` command.
  Fix: use `mpirun`/MPI launcher for MPI builds (docs: `web/doc/user.4.6/node44.html`).
- TCGMSG process-zero cannot access input/database paths.
  Fix: ensure procgroup process 0 has file visibility (docs: `web/doc/user.4.6/node44.html`).
- IBM SP shared-vs-not-shared directives misconfigured.
  Fix: use `shared` with `node/tasks_per_node` on SMP nodes; otherwise `not_shared` with processor counts (docs: `web/doc/user.4.6/node44.html`).
- Skipping known-bugs/support routing before deep debugging.
  Fix: check `web/support/known_bugs.html` and `web/support/support.html` first.

### Convergence and Validation Checks
- Confirm selected doc family matches question intent (manual, FAQ, release, support, benchmark).
- For runtime guidance, verify command path aligns with intended backend (sequential/MPI/LoadLeveler).
- Ensure version-sensitive advice cites the specific release-note or manual page used.
- If a behavior claim remains ambiguous, link to source entry points and inspect module code.

## Scope
- Handle questions about documentation grouped under the 'web' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `web/index.html`
- `web/release-notes/index.html`
- `web/nwchem-support/index.html`
- `web/benchmarks/index.html`
- `web/applications/index.html`
- `web/doc/user.4.6/index.html`
- `web/doc/user.4.6/node37.html`
- `web/doc/user.4.6/node29.html`
- `web/search_nwchem.html`
- `web/return_users.html`
- `web/nwchem_main.html`
- `web/nwchem_index.html`

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
- `src/nwchem.F`
- `src/task/task_input.F`
- `src/task/task_shell_input.F`
- `src/task/task.F`
- `src/task/task_dynamics.F`
- `src/nwdft/scf_dft/dft_main0d.F`
- `src/nwmd/md_main.F`
- `src/qmmm/qmmm_main.F`
- `src/argos/argos_main.F`
- `src/util/input_echo.F`
- `src/util/util_checkmpirun.c`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" contrib src`).

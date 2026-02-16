# nwchem source map: Src

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "rimp2g|rism|rtdb|ModelFactory|task_rism" src`
- `rg -n "subroutine .*input|task_" src/rimp2_grad src/rism src/task`

## Function-level entry points
- `src/rimp2_grad/rimp2g.F` | role: RI-MP2 gradient main driver.
- `src/rimp2_grad/rimp2g_parm.F` | role: RI-MP2 gradient parameter handling.
- `src/rimp2_grad/driver_g.F` | role: gradient evaluation driver routine.
- `src/rimp2_grad/e_final.F` | role: final energy/gradient accumulation path.
- `src/rism/rism_input.F` | role: RISM block parsing.
- `src/rism/task_rism.F` | role: RISM task entrypoint.
- `src/rism/rism_output.F` | role: RISM output/report path.
- `src/rtdb/rtdb.c` | role: RTDB core storage implementation.
- `src/rtdb/rtdb_seq.c` | role: sequential RTDB access path.
- `src/cca/Chemistry/server/cxx/NWChem_Chemistry_QC_ModelFactory_Impl.cc` | role: CCA model factory implementation.
- `src/cca/Chemistry/server/cxx/NWChem_Chemistry_QC_Model_Impl.cc` | role: CCA QC model behavior implementation.

## Validation checkpoints
- Confirm task handlers (`task rism`, RI-MP2 gradient paths) are reached from input decks.
- Verify RTDB read/write behavior when reproducing API or workflow regressions.

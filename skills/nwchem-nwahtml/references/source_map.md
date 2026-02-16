# nwchem source map: Nwahtml

Use this map only after reviewing `doc_map.md`.

## Fast source navigation
- `rg -n "nwargos|prepare|restart|top|sequence|record" src/argos src/util`
- `rg -n "subroutine .*prep|subroutine .*prepare|rdrest|wrtrst" src/argos`

## Function-level entry points
- `src/argos/argos_main.F` | role: nwARGOS task entrypoint.
- `src/argos/argos_input.F` | role: nwARGOS keyword parsing.
- `src/argos/argos_prepare_input.F` | role: setup/prepare input processing.
- `src/argos/argos_prepare_mktop.F` | role: topology generation pipeline.
- `src/argos/argos_prepare_wrtrst.F` | role: initial restart writing during setup.
- `src/argos/argos_rdrest.F` | role: restart state read path.
- `src/argos/argos_wrtrst.F` | role: restart state write path.
- `src/argos/argos_prop_setup.F` | role: propagator/state setup before MD/EM.
- `src/argos/argos_prop_step.F` | role: core propagation step implementation.
- `src/argos/argos_prop_record.F` | role: periodic trajectory/restart/stat recording.
- `src/argos/argos_cafe_print_top.F` | role: topology printing and consistency checks.
- `src/util/seq_output.F` | role: sequence-style output helpers used in workflows.

## Validation checkpoints
- Required nwARGOS files should be consumed without setup-phase aborts.
- Restart read/write paths should be exercised and produce consistent continuation.

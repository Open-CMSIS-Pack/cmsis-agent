# Device debug-sequence contract

Reuse `.agent-artifacts/<pdsc-stem>.debug-topology.md`; cross-check selected scope, processors, DP/AP/APID mappings, inherited definitions, and required device behavior. If it lacks connection facts, return to `$cmsis-pack-debug-description`.

Create `.agent-artifacts/<pdsc-stem>.debug-sequences.md` with the topology reference, scope, processors, inherited sequences, proposed names, placement/applicability, evidence, standard-default provenance, and open questions.

Override a predefined sequence only for verified device behavior. Obtain the current specification default and retain its complete structure, comments, variables, checks, waits, cleanup, and control blocks except for documented device-specific changes. For a custom sequence, evidence every register address, value, delay, condition, and call.

Analyze each affected CPU's `Pname`, DP/AP/APID mapping, and initial `__ap`/`__apid`. Use connection-based dispatch only when the remaining behavior is proven identical; otherwise define it per `Pname`. Preserve all trace configuration. Report changed sequences, CPU applicability, sources, placement, and validation results; for overrides, record every difference from the specification default.

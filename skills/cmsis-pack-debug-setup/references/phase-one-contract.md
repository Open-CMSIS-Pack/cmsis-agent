# Debug setup phase-one contract

Run `$cmsis-pack-debug-description` first. Establish or update `.agent-artifacts/<pdsc-stem>.debug-topology.md` with processors, DP/AP/APID mappings, connection models, `debug`/`debugconfig`, `dbg_datapatch`, `debugvars`, and prerequisites.

Run `$cmsis-pack-debug-sequences` only for verified non-trace behavior such as unlock, debug clocks, reset, bootloader, flash, debug authentication, or multi-core startup. It is optional for trace unless required before trace components can be discovered or accessed.

Keep ownership strict: `$cmsis-pack-debug-description` owns non-sequence Debug Description elements and topology evidence; `$cmsis-pack-debug-sequences` owns device-specific non-trace sequences; `$cmsis-pack-trace-sequences` owns trace `debugvars` and sequences. Do not duplicate, overwrite, or move another skill's content merely to consolidate XML.

When trace is requested and topology is `READY FOR TRACE`, create `.agent-artifacts/<pdsc-stem>.trace-input.md`. Include only the PDSC path; selected scope and descendants; topology-record path and status; processor `Pname` and DP/AP/APID paths; requested or evidenced trace paths; required CoreSight instances with type, address, AP, role, and mode; relevant `debug`/`debugconfig` and non-trace prerequisites; concise evidence citations; and unresolved items. Do not copy raw documents, PDSC XML, component templates, or the full non-trace sequence record.

Create/update agent artifacts without confirmation. Present only Phase 1 PDSC changes: Debug Description elements, data patches, and selected non-trace sequences. After confirmation, immediately apply and validate them in the same turn with the applicable specialist gates. Do not run trace assembly in this turn. End by naming `trace-input.md` and directing the user to a fresh `$cmsis-pack-trace-sequences` turn.

Return to the owning specialist when its evidence gate fails. Do not invent topology, data patches, default behavior, device register settings, trace paths, or component order to make the workflow appear complete.

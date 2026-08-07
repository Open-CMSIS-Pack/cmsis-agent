---
name: cmsis-pack-debug-setup
description: Coordinate evidence-backed CMSIS-Pack debug setup in an existing Device Family Pack PDSC, then prepare a compact handoff for a separate CoreSight trace-sequence turn. Use for end-to-end debug setup across connection topology, data patches, non-trace device behavior, and CoreSight trace.
---

# CMSIS-Pack debug setup

Coordinate the debug-description and optional non-trace sequence phases without duplicating specialist rules or output. For trace work, produce a compact handoff and stop; `$cmsis-pack-trace-sequences` owns the later trace turn and its PDSC confirmation.

## Phase 1: topology, non-trace behavior, and trace handoff

1. Find the target PDSC and selected family/subFamily/device/variant scope. Confirm that the PDSC already contains device descriptions; do not create a new DFP.
2. Run `$cmsis-pack-debug-description` first. Establish or update the shared `.agent-artifacts/<pdsc-stem>.debug-topology.md` record, including processors, DP/AP/APID mappings, connection models, `debug`/`debugconfig`, `dbg_datapatch`, `debugvars`, and any device prerequisites.
3. Decide whether `$cmsis-pack-debug-sequences` is needed. Run it for verified non-trace behavior such as unlock, debug clocks, reset, bootloader, flash, debug authentication, or multi-core startup. It reuses the shared topology record and maintains its own `.agent-artifacts/<pdsc-stem>.debug-sequences.md` record. It is optional for trace, except when its verified device setup is needed before trace components can be discovered or accessed.
4. When trace is requested and the topology is `READY FOR TRACE`, create or update `.agent-artifacts/<pdsc-stem>.trace-input.md`. Keep it compact: include the PDSC path; selected scope and applicable descendants; the referenced topology-record path and status; processor `Pname` and DP/AP/APID paths; only the requested or evidence-backed trace paths; required CoreSight instances with type, address, AP, role, and path/mode; relevant `debug`/`debugconfig` and non-trace prerequisites; concise evidence citations; and unresolved items. Do not copy raw document excerpts, PDSC XML, component templates, or the full non-trace sequence record into this handoff.
5. Keep ownership boundaries strict:
   - `$cmsis-pack-debug-description` owns non-sequence Debug Description elements and topology evidence.
   - `$cmsis-pack-debug-sequences` owns device-specific non-trace sequences.
   - `$cmsis-pack-trace-sequences` owns trace `debugvars` and trace sequences.
   Do not duplicate, overwrite, or move another skill's content merely to consolidate XML.
6. Create or update agent-artifact records as evidence is gathered; no confirmation is needed for those files. Present only the Phase 1 PDSC changes: Debug Description elements, data patches, and selected non-trace sequences. Do not combine trace XML with this proposal.
7. After confirmation, immediately apply and validate the complete Phase 1 PDSC change in the same turn. Run the applicable Debug Description and non-trace sequence validation gates. Report sources, confidence, records updated, PDSC placement, and validation results.
8. Do not run `$cmsis-pack-trace-sequences` in this turn. End Phase 1 by naming `.agent-artifacts/<pdsc-stem>.trace-input.md` and direct the user to start a fresh trace-sequence turn. The trace skill uses that manifest first and performs its own proposal, confirmation, generation, and validation.

## Escalation

Return to the owning specialist skill whenever its evidence gate fails. Do not invent topology, data patches, default sequence behavior, device-specific register settings, trace paths, or component ordering to make the combined workflow appear complete.

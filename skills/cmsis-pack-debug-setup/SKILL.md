---
name: cmsis-pack-debug-setup
description: Coordinate a complete, evidence-backed CMSIS-Pack debug setup in an existing Device Family Pack PDSC by combining Debug Description, device debug-sequence, and trace-sequence work. Use for end-to-end debug setup across connection topology, data patches, non-trace device behavior, and CoreSight trace.
---

# CMSIS-Pack debug setup

Coordinate the specialist skills without duplicating their rules or output. This skill owns workflow order, shared evidence, and the consolidated PDSC confirmation; the specialist skills own their respective XML content.

## Workflow

1. Find the target PDSC and selected family/subFamily/device/variant scope. Confirm that the PDSC already contains device descriptions; do not create a new DFP.
2. Run `$cmsis-pack-debug-description` first. Establish or update the shared `.agent-artifacts/<pdsc-stem>.debug-topology.md` record, including processors, DP/AP/APID mappings, connection models, `debug`/`debugconfig`, `dbg_datapatch`, `debugvars`, and any device prerequisites.
3. Decide whether `$cmsis-pack-debug-sequences` is needed. Run it for verified non-trace behavior such as unlock, debug clocks, reset, bootloader, flash, debug authentication, or multi-core startup. It reuses the shared topology record and maintains its own `.agent-artifacts/<pdsc-stem>.debug-sequences.md` record. It is optional for trace, except when its verified device setup is needed before trace components can be discovered or accessed.
4. Run `$cmsis-pack-trace-sequences` for requested trace setup. It reuses the shared topology record and maintains `.agent-artifacts/<pdsc-stem>.trace-sequences.md`. Unless the user narrows scope, assemble device-level trace support for all processors and all evidence-backed trace paths.
5. Keep ownership boundaries strict:
   - `$cmsis-pack-debug-description` owns non-sequence Debug Description elements and topology evidence.
   - `$cmsis-pack-debug-sequences` owns device-specific non-trace sequences.
   - `$cmsis-pack-trace-sequences` owns trace `debugvars` and trace sequences.
   Do not duplicate, overwrite, or move another skill's content merely to consolidate XML.
6. Create or update agent-artifact records as evidence is gathered; no confirmation is needed for those files. When all required evidence and user choices are available, present one combined proposed change for the existing PDSC, including placement, applicable descendants, sequence ownership, and unresolved items.
7. After the user confirms the combined proposal, immediately apply all complete changes in the same turn, in this order: Debug Description elements and data patches; non-trace debug sequences when selected; trace `debugvars` and sequences. Do not stop after updating records unless missing evidence or a validation failure blocks completion.
8. Validate the PDSC/XML once after the combined update, then run each applicable specialist skill's required validation gates. Report the source and confidence of each change, records updated, PDSC placement, and validation results.

## Escalation

Return to the owning specialist skill whenever its evidence gate fails. Do not invent topology, data patches, default sequence behavior, device-specific register settings, trace paths, or component ordering to make the combined workflow appear complete.

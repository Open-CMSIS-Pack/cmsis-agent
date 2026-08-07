---
name: cmsis-pack-debug-description
description: Inspect and create evidence-backed non-trace CMSIS-Pack Debug Description content in an existing Device Family Pack (DFP). Use when a PDSC needs debug, debugconfig, dbg_datapatch, debugvars, debug-port, access-port, or device debug-sequence definitions; when DP/AP/CoreSight topology is incomplete; or before generating CMSIS-Pack trace sequences.
---

# CMSIS-Pack debug description

Create a verified debug-description foundation for an existing DFP. Device-specific non-trace sequences belong to `$cmsis-pack-debug-sequences`; trace sequence generation belongs to `$cmsis-pack-trace-sequences`.

## Entry and progressive workflow

1. Find the target `.pdsc`, selected family/subFamily/device/variant scope, and affected `<processor>` descriptions. Do not create a new DFP and do not continue if no device description exists.
2. After identifying the target and before gathering evidence or editing, read `references/evidence-contract.md`. It owns the topology inventory, evidence process, review record, PDSC placement, confirmation, validation, and trace handoff gate.
3. Load only the target PDSC, selected device subtree, and evidence relevant to the requested connection or trace path. Do not load non-trace sequence records or trace component assets in this skill.

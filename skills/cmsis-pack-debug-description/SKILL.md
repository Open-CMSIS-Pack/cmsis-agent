---
name: cmsis-pack-debug-description
description: Build or review evidence-backed non-trace debug topology in an existing CMSIS Device Family Pack PDSC. Use for debug connection, DP/AP, CoreSight, debugconfig, debugvars, debug-port, access-port, or datapatch definitions, especially when trace setup lacks validated topology. Do not use for device debug sequences or trace sequences.
---

# CMSIS-Pack debug description

Establish the non-trace debug-description foundation for an existing DFP. Device-specific non-trace sequences belong to `$cmsis-pack-debug-sequences`; trace sequence generation belongs to `$cmsis-pack-trace-sequences`.

## Entry and progressive workflow

1. Find the target `.pdsc`, selected family/subFamily/device/variant scope, and affected `<processor>` descriptions. Do not create a DFP or proceed without a device description.
2. Before investigating or editing, read `../../common/cmsis-pack-workflow.md`, `../../common/cmsis-pack-spec.md`, and `references/evidence-contract.md`.
3. Load only the target PDSC, selected subtree, and evidence for the requested connection or trace path. Do not load device-sequence records or trace component assets.

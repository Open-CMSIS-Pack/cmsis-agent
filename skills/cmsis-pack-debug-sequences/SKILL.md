---
name: cmsis-pack-debug-sequences
description: Add evidence-backed, device-specific non-trace CMSIS-Pack Debug Description sequences to an existing Device Family Pack PDSC. Use for device unlock, debug-port setup, reset, bootloader, flash, debug-authentication, multi-core startup, or other predefined/custom debug access sequences; do not use for CoreSight trace setup.
---

# CMSIS-Pack device debug sequences

Add device-specific debug access sequences to an existing PDSC without generating trace setup. This skill is independent of `$cmsis-pack-trace-sequences`; neither skill is a prerequisite for the other.

## Entry and progressive workflow

1. Find the target PDSC, selected family/subFamily/device/variant scope, affected processors, existing non-trace `<sequences>`, and `.agent-artifacts/<pdsc-stem>.debug-topology.md` from `$cmsis-pack-debug-description`. Do not create a new DFP. Do not alter `TraceStart`, `TraceCapture`, `TraceFlush`, `TraceStop`, or trace-specific helpers; direct that work to `$cmsis-pack-trace-sequences`.
2. After confirming the target and topology record, read `references/sequence-contract.md` before gathering sequence evidence or editing. It owns topology reuse, specification evidence, CPU dispatch, XML formatting, `.dbgconf` proposals, confirmation, and validation.
3. Load only the topology facts and device documentation needed for the requested non-trace sequence. Do not load trace component assets, trace scaffolds, or trace sequence records.

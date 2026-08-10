---
name: cmsis-pack-debug-sequences
description: Add or update evidence-backed device-specific non-trace debug sequences in an existing CMSIS Device Family Pack PDSC. Use for unlock, reset, debug authentication, bootloader, flash, debug-port setup, or multi-core startup. Requires valid debug topology; do not use for topology definition or CoreSight trace setup.
---

# CMSIS-Pack device debug sequences

Add device-specific debug access sequences without generating trace setup. `$cmsis-pack-trace-sequences` remains independent of this skill unless the requested trace path needs a non-trace prerequisite.

## Entry and progressive workflow

1. Find the target PDSC, selected scope, affected processors, existing non-trace `<sequences>`, and `.agent-artifacts/<pdsc-stem>.debug-topology.md`. Do not create a DFP or alter trace sequences/helpers.
2. Before investigating or editing, read `../../common/cmsis-pack-workflow.md`, `../../common/cmsis-pack-spec.md`, and `references/sequence-contract.md`.
3. Load only the topology facts and device documentation needed for the requested non-trace sequence.

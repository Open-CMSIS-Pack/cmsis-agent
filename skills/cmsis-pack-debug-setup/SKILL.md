---
name: cmsis-pack-debug-setup
description: "Coordinate complete non-trace CMSIS-Pack debug setup in an existing Device Family Pack PDSC: debug topology and, when needed, device-specific debug sequences. Use when a change spans both areas. Produce a trace-input handoff when trace is requested, but do not implement CoreSight trace sequences."
---

# CMSIS-Pack debug setup

Coordinate the debug-description and optional non-trace sequence phases without duplicating specialist rules or output. For trace work, produce a compact handoff and stop; `$cmsis-pack-trace-sequences` owns the later trace turn and its PDSC confirmation.

## Entry and progressive workflow

1. Find the target PDSC and selected scope. Confirm it already contains device descriptions; do not create a DFP.
2. Before running a specialist or editing, read `../../common/cmsis-pack-workflow.md`, `../../common/cmsis-pack-spec.md`, and `references/phase-one-contract.md`.
3. Do not load trace component assets, trace scaffolds, or the trace assembly contract. End with the compact trace-input artifact and a fresh trace turn.

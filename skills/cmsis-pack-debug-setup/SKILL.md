---
name: cmsis-pack-debug-setup
description: Coordinate evidence-backed CMSIS-Pack debug setup in an existing Device Family Pack PDSC, then prepare a compact handoff for a separate CoreSight trace-sequence turn. Use for end-to-end debug setup across connection topology, data patches, non-trace device behavior, and CoreSight trace.
---

# CMSIS-Pack debug setup

Coordinate the debug-description and optional non-trace sequence phases without duplicating specialist rules or output. For trace work, produce a compact handoff and stop; `$cmsis-pack-trace-sequences` owns the later trace turn and its PDSC confirmation.

## Entry and progressive workflow

1. Find the target PDSC and selected family/subFamily/device/variant scope. Confirm that the PDSC already contains device descriptions; do not create a new DFP.
2. Read `references/phase-one-contract.md` before running a specialist or editing the PDSC. It owns phase order, ownership boundaries, compact trace handoff, confirmation, validation, and escalation.
3. Do not load trace component assets, trace scaffolds, or the trace assembly contract in this skill. End the phase with the compact trace-input artifact and a fresh trace turn.

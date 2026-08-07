# CMSIS-Pack Debug Setup

Coordinates the topology and non-trace phases of a CMSIS-Pack debug setup workflow for an existing DFP PDSC.

Use it when a task spans debug connection description, device-specific non-trace sequences, and CoreSight trace setup. It runs Debug Description first, adds Debug Sequences only when required, and creates a compact `trace-input.md` handoff when trace is in scope.

The orchestration skill maintains ownership boundaries and confirms only the Phase 1 PDSC update. Trace Sequences runs in a fresh turn from the handoff and has its own confirmation.

---
name: cmsis-pack-trace-sequences
description: Add or maintain evidence-backed CoreSight trace sequences in an existing CMSIS Device Family Pack PDSC. Use when a trace-input manifest or READY FOR TRACE topology exists and the request involves SWO, synchronous trace, trace buffers, CoreSight routing, or TraceStart/TraceCapture/TraceFlush/TraceStop. Do not use to establish topology or non-trace device sequences.
---

# CMSIS-Pack trace sequences

Assemble modular CoreSight trace snippets into an existing PDSC. Before selecting assets or editing, read `../../common/cmsis-pack-workflow.md`, `../../common/cmsis-pack-spec.md`, and `references/assembly-contract.md`.

## Preconditions

1. Find the target PDSC, selected scope, and trace-input manifest when it exists. Require its referenced topology record to be `READY FOR TRACE`; cross-check only the selected components, paths, placement, and relevant `debugconfig`/`debug` elements. Treat it as complete unless it is absent, stale, contradictory, or insufficient.
2. Otherwise require the matching topology record to be `READY FOR TRACE`; if it is absent, blocked, stale, or insufficient, direct the user to `$cmsis-pack-debug-description`.
3. Maintain `<sequences traceSetup="full">`; convert `legacy` when encountered. Treat trace setup as device-level unless the user narrows the scope.

## Progressive assembly

1. After selecting paths, read `references/component-index.md` and load only required component assets.
2. Use the selected scaffold after reading the assembly contract. Read `references/legacy-to-full-migration.md` only for legacy conversion.

---
name: cmsis-pack-trace-sequences
description: Add modular, evidence-backed CoreSight trace sequences to an existing CMSIS Device Family Pack PDSC. Use after CMSIS-Pack debug topology is defined, when assembling per-component trace snippets into TraceStart, TraceCapture, TraceFlush, and TraceStop sequences, or when maintaining trace setup with device-specific extension placeholders.
---

# CMSIS-Pack trace sequences

Assemble modular CoreSight trace snippets into an existing PDSC. When `.agent-artifacts/<pdsc-stem>.trace-input.md` exists, read it first, then read `references/component-index.md` and only the selected component assets at the skill-root-relative paths listed there (for example, `assets/components/etf.xml`). Use `assets/scaffolds/full-from-scratch.xml` for new full-mode sequences; read `references/legacy-to-full-migration.md` and use `assets/scaffolds/legacy-to-full.xml` when converting legacy mode.

## Preconditions

1. Find the target PDSC, selected family/subFamily/device/variant scope, and `.agent-artifacts/<pdsc-stem>.trace-input.md` when it exists. Require its referenced topology record to be `READY FOR TRACE`, and cross-check only its selected component instances, addresses, DP/AP paths, recorded placement scope, and explicit `debugconfig`/`debug` elements against the PDSC. Treat this manifest as the complete topology input; do not load the full topology or non-trace sequence record unless the manifest is absent, stale, contradictory, or insufficient for the requested trace path.
2. If no trace-input manifest exists, use the matching `.agent-artifacts/<pdsc-stem>.debug-topology.md` review record and require `Status: READY FOR TRACE`. If it is absent, blocked, stale, or insufficient for the requested path, stop and direct the user to `$cmsis-pack-debug-description`.
3. Generate and maintain the target `<sequences traceSetup="full">` configuration. Do not generate `legacy` trace setup; if the existing PDSC declares `legacy`, report that it will be converted to `full` as part of the trace change.
4. Treat trace setup as device-level by default, not CPU-level. Unless the user explicitly narrows the scope, consider all processors in the selected device subtree and every evidence-backed trace path the device supports, including SWO, synchronous trace, and trace-buffer variants.

## Progressive assembly

1. After the preconditions establish the selected paths, read `references/component-index.md` and load only the component assets required by those paths.
2. Before generating or editing PDSC XML, read `references/assembly-contract.md`. It owns component selection and placement, routing, scaffold structure, XML formatting, evidence limits, asset fidelity, confirmation, and validation. Follow it completely; do not restate or load unrelated component assets.
3. Use the selected scaffold and component assets only after reading the assembly contract. Load `references/legacy-to-full-migration.md` only when converting a legacy configuration.

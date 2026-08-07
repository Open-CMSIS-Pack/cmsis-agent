---
name: cmsis-pack-trace-sequences
description: Add modular, evidence-backed CoreSight trace sequences to an existing CMSIS Device Family Pack PDSC. Use after CMSIS-Pack debug topology is defined, when assembling per-component trace snippets into TraceStart, TraceCapture, TraceFlush, and TraceStop sequences, or when maintaining trace setup with device-specific extension placeholders.
---

# CMSIS-Pack trace sequences

Assemble modular CoreSight trace snippets into an existing PDSC. Read `references/component-index.md` first, then only the selected component assets. Use `assets/scaffolds/full-from-scratch.xml` for new full-mode sequences; read `references/legacy-to-full-migration.md` and use `assets/scaffolds/legacy-to-full.xml` when converting legacy mode.

## Preconditions

1. Find the target PDSC, selected family/subFamily/device/variant scope, and its matching `.agent-artifacts/<pdsc-stem>.debug-topology.md` review record.
2. Require `Status: READY FOR TRACE`. Cross-check its component instances, addresses, DP/AP paths, recorded placement scope, and explicit `debugconfig`/`debug` elements against the PDSC. If the record is absent, blocked, stale, or insufficient for the requested path, stop and direct the user to `$cmsis-pack-debug-topology`.
3. Generate and maintain the target `<sequences traceSetup="full">` configuration. Do not generate `legacy` trace setup; if the existing PDSC declares `legacy`, report that it will be converted to `full` as part of the trace change.

## Assembly rules

1. Map the selected device subtree and group descendants with an identical verified trace configuration. Emit each shared configuration at the topmost selected tree level common to that group. Emit only differing snippets, variables, scaffold calls, or extension placeholders on outer leaf variants; do not copy an identical full configuration to each variant.
2. Select one component asset per physical CoreSight instance that is required by the verified trace path. Keep the result minimal: omit unused assets, snippets, variables, and calls; copy each required asset's `<debugvars>` contributions into the target's single `<debugvars>` element; deduplicate names and reject conflicting definitions.
3. Rename every copied non-predefined sequence with a numeric component-instance suffix, for example `CS_TPIU_0_Enable`, `CS_TPIU_1_Enable`. Number instances of each component type from `0` in ascending verified base-address order, and preserve those numbers on later updates. Update every `Sequence("...")` call accordingly.
4. Copy sequence bodies into the PDSC and use the scaffold to call them from the relevant predefined sequence. Never emit an unresolved generic sequence name shared by two instances.
5. When verified device behavior uses a CoreSight component in a non-standard role or order, its selected snippet sequences may be called from an `ADD-DEVICE-SPECIFIC-HERE` region instead of a standard mode-specific CoreSight helper. Do not also call those snippet sequences from the standard CoreSight path: each component operation has exactly one owning path.
6. Route each component operation according to `references/assembly-contract.md`: `Configure` to the active mode's start helper, `Capture` to its capture helper, `Flush` to its flush helper, and `ReadBuffer` only to the trace-buffer-read helper. In each applicable start or capture path, order calls as funnels, then trace glue such as an ETF in hardware-FIFO mode, then the trace sink. Do not add an operation to a mode whose verified topology does not contain that component.
7. Preserve each `ADD-DEVICE-SPECIFIC-HERE` marker pair defined in `references/assembly-contract.md`; it delimits user-maintained device extensions. Retain one `INSERT-CORESIGHT-SNIPPET-CALLS-HERE` marker in every populated mode-specific CoreSight helper.
8. Enforce order exactly:
   - `TraceStart` and `TraceCapture`: device extension calls, then CoreSight calls.
   - `TraceFlush` and `TraceStop`: CoreSight calls, then device extension calls.
9. Assume the debugger explicitly calls `TraceFlush` before `TraceStop` to drain remaining trace data. Do not call `DoTraceFlush` or its mode-specific variants again from `TraceStop` or a `DoTraceStop_<mode>` helper.
10. Preserve scaffold sequence boundaries, including helpers such as `DoTraceFlush`; do not flatten or inline them merely for presentation. Consolidate sequences only when it demonstrably reduces the context loaded for the task while preserving their call boundaries and behavior.
11. Format generated XML for review: align matching XML tags, put C-like content on lines inside each `<block>`, indent it one level deeper than the tag, and use one semicolon-terminated statement per line. Keep XML entities intact.
12. Put each high-level routing sequence's `__var` declarations in its opening `<block>`, before executable statements. These routing sequences are `TraceStart`, `TraceCapture`, `DoTraceFlush`, `TraceFlush`, `TraceStop`, and their `DoTrace*_<mode>` helpers; each begins with `swoTrace`, `syncTrace`, and `bufferTrace` definitions derived from `__traceout`. Component snippet sequences declare only the variables they use.
13. Do not invent register addresses, register values, funnel routes, timestamping, formatter settings, component ordering, or a dormant-state requirement. Honor the topology record's evidence-backed `debugconfig dormant` decision; return to `$cmsis-pack-debug-topology` if it is not recorded. Ask for documentation when a selected snippet needs information not supplied by the verified topology. If an identified document cannot be downloaded, list its URL and retrieval issue in the trace assembly record, then ask the user to download it manually and copy it into the target workspace before using it as evidence.

## Deliverable and validation

Create and update `.agent-artifacts/<pdsc-stem>.trace-sequences.md` without requesting confirmation; it is agent-owned working output. Before modifying the existing user-owned PDSC, present the proposed `<debugvars>` and `<sequences>` change and obtain user confirmation. After the user confirms all trace-path choices, immediately generate, apply, and validate the complete XML in the same turn. Do not stop after creating or updating the assembly record unless missing evidence or a validation failure blocks completion. Keep the output recognizable through its `CMSIS-PACK-TRACE` markers. Report each definition's PDSC placement and applicable descendants, selected component instances, emitted sequence names, trace setup mode, extension placeholders, and validation result. Validate PDSC/XML syntax with the project's available toolchain.

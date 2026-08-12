---
name: generate-trace-sequences
description: Add modular, evidence-backed CoreSight trace sequences to an existing CMSIS Device Family Pack PDSC. Use after `$trace-knowledge` has produced a READY trace knowledge record that evidences available trace paths, when assembling per-component trace snippets into TraceStart, TraceCapture, TraceFlush, and TraceStop sequences, or when maintaining trace setup with device-specific extension placeholders. Consult `$debug-knowledge` only when a required debug connection or dormant-state fact is not self-contained in the trace record.
---

# Generate Trace Sequences

## Target & Persona

- **Role:** CMSIS-Pack Tool Integrator and CoreSight Trace Verification Engineer.
- **Objective:** Generate complete, evidence-backed device-level CoreSight trace setup for every selected, available trace path.

## Prerequisites & Context

- **Expected input:** An existing target `.pdsc`, selected family/subFamily/device/variant scope, a `READY` `.agent-artifacts/<pdsc-stem>.trace-knowledge.md` record, and documentation for the selected CoreSight topology. Use the current `.agent-artifacts/<pdsc-stem>.debug-knowledge.md` record only when the trace record needs its debug connection or dormant-state facts.
- **Dependencies:** `$trace-knowledge`; `$debug-knowledge` when required; the current [Open-CMSIS-Pack *Debug Description* specification](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/debug_description.html); `references/trace-sequence-contract.md`, `references/component-index.md`, selected component assets, and the applicable scaffold or legacy-migration reference.
- **Portability:** Support any existing CMSIS Device Family Pack with documented CoreSight paths. Do not assume a device family, debug probe, toolchain, active target, or trace transport.

## Execution Steps (Strict Workflow)

1. **Analysis:** Locate the target PDSC, selected scope, and matching READY trace knowledge record. Verify every selected trace path, component instance, address, DP/AP path, placement scope, and explicit debug definition. Return missing or contradictory trace facts to `$trace-knowledge`; return to `$debug-knowledge` only for a required debug connection or dormant-state fact that the trace record does not contain.
2. **Processing:** Read `references/trace-sequence-contract.md` before editing. Select only required component assets after reading `references/component-index.md`, choose the applicable scaffold, and assemble complete full-mode device-level trace setup. Maintain the agent-owned trace-sequences record, present the proposal, and obtain confirmation before changing user-owned files.
3. **Validation:** Apply the confirmed complete XML, validate XML/PDSC syntax and all contract structural, routing, asset-fidelity, marker, and block-formatting gates. Inspect every reported match and resolve each failure.
4. **Formatting:** Report the selected scope, trace paths, component instances, emitted sequences, placements, extension placeholders, evidence, `.dbgconf` proposals, unresolved items, and validation results.

## Guardrails & Constraints (Strict Rules)

- **No fabrication:** Do not invent addresses, values, routes, component order, trace-clock behavior, trace mode, timestamps, formatter settings, or dormant-state requirements. Return missing or contradictory trace facts to `$trace-knowledge`; return a missing debug connection or dormant-state fact to `$debug-knowledge` only when needed.
- **Portability:** Do not introduce a device-family, debugger, toolchain, runtime-target, or trace-transport dependency unless authoritative inputs require it. Identify each required dependency in the output.
- **Critical blockers:** Stop when the trace knowledge record is not READY or insufficient, a required debug knowledge record is absent or insufficient, required assets or documentation are missing, user confirmation is absent for user-owned files, or validation fails.
- **Scope:** Generate only full-mode CoreSight trace configuration. Preserve unrelated PDSC content; do not create legacy setup, omit verified paths, or modify `.dbgconf` files without explicit confirmation.
- **Tone and style:** Respond factually and directly. Omit conversational filler.

## Expected Output

Create or update `.agent-artifacts/<pdsc-stem>.trace-sequences.md`; apply the confirmed full-mode trace XML; and report trace paths, component instances, placement, evidence, `.dbgconf` proposals, and validation results.

## Validation Resources

Read [the trace-sequence contract](references/trace-sequence-contract.md) in full before selecting assets or editing. Read [the component index](references/component-index.md) before loading component assets. The contract defines scaffolds, routing, asset fidelity, confirmation, and repeatable structural and formatting checks.

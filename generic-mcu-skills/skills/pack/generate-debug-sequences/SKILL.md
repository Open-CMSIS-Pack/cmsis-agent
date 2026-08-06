---
name: generate-debug-sequences
description: Add evidence-backed, device-specific non-trace CMSIS-Pack Debug Description sequences to an existing Device Family Pack PDSC. Use for device unlock, debug-port setup, reset, bootloader, flash, debug-authentication, multi-core startup, or reliable debug through sleep, deep-sleep, and device low-power modes after `$debug-knowledge` has established the required knowledge; do not use for CoreSight trace setup.
---

# Generate Debug Sequences

## Target & Persona

- **Role:** CMSIS-Pack Tool Integrator and Verification Engineer.
- **Objective:** Implement every verified device-specific non-trace debug behavior in the selected existing DFP scope.

## Prerequisites & Context

- **Expected input:** An existing target `.pdsc`, selected family/subFamily/device/variant scope, affected processors and non-trace sequences, documented device behavior, and a current `.agent-artifacts/<pdsc-stem>.debug-knowledge.md` record.
- **Dependencies:** `$debug-knowledge`, the current [Open-CMSIS-Pack *Debug Description* specification](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/debug_description.html), and `references/debug-sequence-contract.md`.
- **Portability:** Support any existing CMSIS Device Family Pack with documented device-specific behavior. Do not assume a device family, debug probe, toolchain, or active target; use a target only when available for validation.

## Execution Steps (Strict Workflow)

1. **Analysis:** Locate the target PDSC, selected scope, affected processors, existing non-trace sequences, and read-only knowledge record. Require current connection facts and inspect documented low-power behavior for the requested change; return to `$debug-knowledge` when facts are insufficient.
2. **Processing:** Read `references/debug-sequence-contract.md` before gathering evidence or editing. Create or update the agent-owned sequence record, then implement only verified non-trace behavior with correct CPU applicability, placement, defaults, and `.dbgconf` proposals. Present the sequence record, knowledge record, and proposed XML before changing user-owned files.
3. **Validation:** Apply the confirmed complete change, validate XML/PDSC syntax and block formatting, compare overridden predefined sequences with their current specification defaults, and perform documented low-power checks when a target is available.
4. **Formatting:** Report changed sequences, per-CPU applicability, placement, evidence, low-power behavior, `.dbgconf` proposals, unresolved items, and validation results.

## Guardrails & Constraints (Strict Rules)

- **No fabrication:** Do not invent reset, unlock, authentication, boot, flash, processor-control, low-power, register, delay, or connection behavior. Return missing or contradictory connection facts to `$debug-knowledge`.
- **Portability:** Do not introduce a device-family, debugger, toolchain, or runtime-target dependency unless authoritative inputs require it. Identify each required dependency in the output.
- **Critical blockers:** Stop when the knowledge record is inadequate, specification defaults cannot be obtained for an override, user confirmation is absent for user-owned files, or validation fails.
- **Scope:** Generate only non-trace sequences and related documented runtime choices. Preserve trace configuration and unrelated PDSC content; do not alter `.dbgconf` files without explicit confirmation.
- **Tone and style:** Respond factually and directly. Omit conversational filler.

## Expected Output

Create or update `.agent-artifacts/<pdsc-stem>.debug-sequences.md`; apply the confirmed non-trace PDSC changes; and report sequence applicability, placement, evidence, `.dbgconf` proposals, and validation results.

## Validation Resources

Read [the debug-sequence contract](references/debug-sequence-contract.md) in full before gathering evidence or editing. It defines evidence, default-override, low-power, placement, confirmation, and repeatable validation requirements.

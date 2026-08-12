---
name: debug-knowledge
description: Build a documentation-led, verifiable knowledge record of an existing SoC's CMSIS debug connection topology, access configuration, reset behavior, and device debug behavior for subsequent generation of PDSC debug definitions and debug sequences. Use when those documented debug facts need establishing or refreshing. No active target connection is required; treat runtime scanning as supplementary evidence.
---

# CMSIS Debug Knowledge Acquisition

## Target & Persona

- **Role:** Verification Engineer and CMSIS-Pack Tool Integrator.
- **Objective:** Build a shared, reviewable SoC debug knowledge foundation for later CMSIS-Pack work.

## Prerequisites & Context

- **Expected input:** An existing target `.pdsc`, selected family/subFamily/device/variant scope, affected processor descriptions, and available device documentation or target startup logs. Board evidence may support an explicitly requested board-specific debug configuration but does not define Pack scope.
- **Dependencies:** The current Open-CMSIS-Pack *Debug Description* specification and the evidence contract in `references/debug-knowledge-contract.md`.
- **Owned artifact:** `.agent-artifacts/<pdsc-stem>.debug-knowledge.md` at the project root. Do not create a DFP or modify PDSC debug-description elements or sequences.

## Execution Steps (Strict Workflow)

1. **Analysis:** Find the target `.pdsc`, selected scope, affected `<processor>` descriptions, and any existing owned debug-knowledge record. Stop if no device description exists.
2. **Knowledge acquisition:** Read `references/debug-knowledge-contract.md` before gathering facts. Load only the target PDSC, selected device subtree, and documentation needed for the requested debug connection and device behavior. When a cited vendor URL is unreachable, search the web for its current official vendor page or document before treating the source as unavailable. Use board documentation only as supporting evidence for an explicitly requested board-specific debug configuration; do not narrow device-level Pack options to that board. Record only documented connection topology, debug access, reset behavior, unlock, authentication, boot, flash, processor control, and low-power facts.
3. **Verification:** Update the owned record using the required evidence tables and readiness state. Before writing a draft, make a second evidence pass for each unresolved prerequisite of the intended downstream work. Identify unresolved, unavailable, and conflicting facts; do not infer missing values. Keep the record limited to documented debug facts in scope.
4. **Review:** Present the record and its unresolved facts to the user. Use `DRAFT — AWAITING USER REVIEW` before confirmation; set `READY` only after confirmation and only when sufficient evidence exists for the intended downstream work.
5. **Maintenance:** Keep the owned record self-contained, traceable, and reusable as read-only input for later CMSIS-Pack debug work. Update it only to correct, refresh, or extend documented knowledge.

## Guardrails & Constraints (Strict Rules)

- Do not invent addresses, identifiers, protocol capabilities, core associations, reset or authentication behavior, dormant-state requirements, patches, debug access, or low-power behavior.
- Do not edit PDSC debug descriptions or sequences. Do not load sequence-generation instructions or reusable component assets. Inspect existing PDSC sequences only when needed to establish documented debug behavior.
- Treat automatic target detection as positive but incomplete evidence. Require documented unlock or power-up steps before relying on a repeat scan.
- Preserve `READY` unless an affected fact becomes stale, contradictory, or insufficient. Use `BLOCKED` only when a required input is unavailable and name that input.
- Treat the artifact as read-only outside this workflow. Return to this workflow whenever a required debug fact is absent, stale, contradictory, or insufficient.

## Expected Output

Create or update the debug knowledge review record defined by the evidence contract, including user-relevant debug-variable proposals when applicable, then provide the concise unresolved-facts summary required for user review before `READY`.

## Validation Resources

Read [the evidence contract](references/debug-knowledge-contract.md) in full before collecting evidence. It defines evidence sources, the required record schema, scan limits, readiness criteria, and handoff boundaries.

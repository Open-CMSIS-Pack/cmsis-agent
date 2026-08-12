---
name: debug-knowledge
description: Build a documentation-led, verifiable knowledge record of an existing SoC's CMSIS debug and trace capabilities, topology, and constraints for subsequent generation of PDSC debug definitions and debug or trace sequences. Use when documented SoC debug and trace facts need establishing or refreshing before generating PDSC debug definitions or debug and trace sequences. No active target connection is required; treat runtime scanning as supplementary evidence.
---

# CMSIS Debug Knowledge Acquisition

## Target & Persona

- **Role:** Verification Engineer and CMSIS-Pack Tool Integrator.
- **Objective:** Build a shared, reviewable SoC debug and trace knowledge foundation for later CMSIS-Pack work.

## Prerequisites & Context

- **Expected input:** An existing target `.pdsc`, selected family/subFamily/device/variant scope, affected processor descriptions, and available device documentation or target startup logs. A named board's manual or design guide, connector pin maps, and jumper settings are optional supporting evidence for board-specific configurations; they are not required input and do not define Pack scope.
- **Dependencies:** The current Open-CMSIS-Pack *Debug Description* specification and the topology evidence contract in `references/debug-knowledge-contract.md`.
- **Owned artifact:** `.agent-artifacts/<pdsc-stem>.debug-knowledge.md` at the project root. Do not create a DFP or modify PDSC debug-description elements or sequences.

## Execution Steps (Strict Workflow)

1. **Analysis:** Find the target `.pdsc`, selected scope, affected `<processor>` descriptions, and any existing owned topology record. Stop if no device description exists.
2. **Knowledge acquisition:** Read `references/debug-knowledge-contract.md` before gathering facts. Load only the target PDSC, selected device subtree, and documentation for the requested connection and every device-level supported trace path. When a cited vendor URL is unreachable, search the web for its current official vendor page or document before treating the source as unavailable. When available, use a named board's manual or design guide, connector pins, and jumpers only as supporting evidence for optional board-specific configurations. Do not narrow device-level Pack options to that board. For each trace path, establish its clock source and enablement, pin-function and electrical configuration, path-selection controls, and sink operating mode and lifecycle requirements. Consider mux or function registers, pad control, drive strength, slew rate, pull configuration, and voltage-domain settings when documented as relevant. Record only documented hardware and topology facts.
3. **Verification:** Update the owned topology record using the required evidence tables and readiness state. Before writing a draft, make a second evidence pass for every unresolved trace prerequisite: clock control, pin-function or electrical configuration, source selection, and sink lifecycle. For an identified CoreSight component whose lifecycle remains unresolved, search its authoritative Arm technical reference manual or architecture specification before recording a blocker. Use that evidence only for component-local initialization, capture, flush, stop, and status-polling behavior; require device-level evidence for integration, address, clock, reset, pin, and routing facts. Board evidence may supplement an optional configuration but cannot limit device-level Pack options. Identify unresolved, unavailable, and conflicting facts; do not infer missing values or treat topology alone as trace readiness.
   Do not present the draft until its completion checklist shows that each trace path has all required facts or a concrete retrieval attempt and precise blocker.
4. **Review:** Present the record and every available, unavailable, or unresolved trace path to the user. Use `DRAFT — AWAITING USER REVIEW` before confirmation; set `READY` only after confirmation and only when sufficient evidence exists for the intended downstream work.
5. **Handoff:** Keep the owned record self-contained, traceable, and reusable as read-only input for later CMSIS-Pack work. Update it only to correct, refresh, or extend documented knowledge.

## Guardrails & Constraints (Strict Rules)

- Do not invent addresses, identifiers, protocol capabilities, core associations, reset or authentication behavior, dormant-state requirements, patches, trace routing, clock control, pin-function or electrical configuration, path-selection controls, or sink lifecycle behavior.
- Do not edit PDSC debug descriptions or sequences. Do not load sequence-generation instructions or reusable trace-component assets. Inspect existing PDSC sequences only when needed to establish documented topology.
- Treat automatic target detection as positive but incomplete evidence. Require documented unlock or power-up steps before relying on a repeat scan.
- Preserve `READY` unless an affected fact becomes stale, contradictory, or insufficient. Use `BLOCKED` only when a required input is unavailable and name that input.
- Treat the artifact as read-only outside this workflow. Return to this workflow whenever a required topology fact is absent, stale, contradictory, or insufficient.

## Expected Output

Create or update the topology review record defined by the evidence contract, including board-specific configurable-routing-variable proposals when applicable, then provide the concise trace-path summary required for user review before `READY`.

## Validation Resources

Read [the evidence contract](references/debug-knowledge-contract.md) in full before collecting evidence. It defines evidence sources, the required record schema, scan limits, readiness criteria, and handoff boundaries.

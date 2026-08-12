# Debug knowledge evidence contract

Use the current [Open-CMSIS-Pack *Debug Description* specification](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/debug_description.html) as the grammar authority. Collect debug hardware and device-behavior evidence only; do not edit PDSC debug descriptions or sequences.

## Debug inventory and evidence

The Pack scope is device-level: device/SoC documentation establishes supported options, while board documentation can support an explicitly requested board-specific debug configuration but must not remove or limit device-level options.

Map the selected device subtree, including inheritance and leaf variants. Inspect relevant PDSC debug definitions—ports, DP/APs, variables, patches, and sequences—and record inherited and local facts without prescribing PDSC XML. Establish every affected processor's connection model, DP/AP/APID mapping, supported protocol, and evidenced dormant-state decision.

For the requested debug work, establish documented reset behavior and any required debug unlock, authentication, boot, flash, processor-control, and low-power behavior. Record reset source and type, reset scope, effects on the processor and debug connection, availability during reset, required recovery or reconnect action, and any required sequencing. Record an authoritative `not applicable` decision for a requested behavior that the device does not support.

Search linked and local documentation first:

- Datasheets, reference manuals, implementation guides, SDFs, SVDs, and vendor packs.
- Debug scripts and relevant source code.
- A named board's manual or design guide, connector pin maps, and jumper settings only for an explicitly requested board-specific debug configuration.

When a cited vendor documentation URL is unreachable, search the web for the current official vendor page or document before recording it as unavailable. Search by vendor, document title or identifier, product family, and revision when known. Prefer the vendor's canonical page or download; do not replace an unavailable official source with an unofficial mirror. Retain the failed URL, retrieval result, search terms, and replacement official URL or the precise reason no official replacement was found.

For each item, cite the document edition/revision and section/page when known; for source, cite repository/version, path, and symbol or line. Do not infer from a part name or similar device: addresses, identifiers, protocol capabilities, core associations, reset/debug-authentication behavior, dormant-state requirements, patches, or low-power behavior.

If documentation does not establish the debug connection topology, offer `pyocd gdbserver -vv` for an accessible target or request its complete startup log. Detection is positive but incomplete evidence: it can omit powered-down/reset-held, debug-locked, or disabled subsystems. Rely on a repeat scan only with documented unlock or power-up steps; an incomplete scan remains incomplete.

## Required review record

Create or update `.agent-artifacts/<pdsc-stem>.debug-knowledge.md` at the project root:

```markdown
# CMSIS-Pack debug knowledge review

PDSC: `<path>`
Device / processor: `<name>`
Selected scope: `<family | subFamily | device | variants>`
Last updated: `<ISO 8601 date and time>` (Model: `<name> <version> <reasoning-level>`)
Status: `DRAFT — AWAITING USER REVIEW` | `READY` | `BLOCKED`

## Evidence
| Item | Value | Evidence type | Source | Location | Confidence |
|---|---|---|---|---|---|

## Debug connection topology
| Connection model | Processor | DP / AP / APID path | Protocol | Dormant-state decision | Evidence |
|---|---|---|---|---|---|

## Debug-definition requirements
| Definition / property | Applicability | Verified value or decision | Evidence | Status |
|---|---|---|---|---|

## Reset and device debug behavior
| Behavior / mode | Processors | Reset or access impact | Required action / recovery | Evidence | Status |
|---|---|---|---|---|---|

## Optional board debug configuration and variables
| Board / connector / jumper | Debug choice | Proposed variable | Valid values and default | Evidence | Status |
|---|---|---|---|---|---|

## Completion checklist
| Intended downstream work | Connection topology | Reset and required device behavior | Retrieval attempt and precise blocker | Complete |
|---|---|---|---|---|

## Open questions
- `<question or none>`

## Documents requiring user download
| Title | Failed URL | Retrieval issue and search terms | Replacement official URL | Requested workspace path |
|---|---|---|---|---|
```

- Update `Last updated` on every change; copy model details exactly from the execution environment. Use `not reported` for unavailable values; do not infer them.
- Only this skill may update the artifact. Treat it as read-only elsewhere; return here if a required debug fact is absent, stale, contradictory, or insufficient.
- Before writing or presenting a draft, make a second evidence pass for each unresolved prerequisite of the intended downstream work.

## Readiness state

- The user's first review confirmation, or a follow-up that clearly authorizes completing the requested debug work, is sufficient to set `READY` when the evidence requirements are met; do not require a separate conversational confirmation.
- Create and update the artifact without confirmation. Before confirmation, use exactly `DRAFT — AWAITING USER REVIEW` and present it for corrections.
- Set `READY` only after confirmation and sufficient evidence for the intended downstream work. Debug definitions require evidenced connection topology and every emitted property. Debug sequences require evidenced connection topology, reset behavior, and every requested device behavior, including any relevant low-power access and recovery behavior. Leave the record non-READY when a required fact is unknown, unavailable, or contradictory.
- Preserve `READY` unless an affected fact becomes stale, contradictory, or insufficient. Use `BLOCKED` only for an unavailable required input, and name it.

Keep verified debug knowledge as reusable CMSIS-Pack input.

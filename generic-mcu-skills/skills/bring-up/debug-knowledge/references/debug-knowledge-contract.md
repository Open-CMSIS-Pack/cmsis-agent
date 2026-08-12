# Debug and trace topology evidence contract

Use the current [Open-CMSIS-Pack *Debug Description* specification](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/debug_description.html) as the grammar authority. Collect hardware and topology evidence only; do not edit PDSC debug descriptions or sequences.

## Topology inventory and evidence

Map the selected device subtree, including inheritance and leaf variants. Inspect relevant PDSC debug and trace definitions—ports, DP/APs, variables, patches, and sequences—and record inherited and local facts without prescribing PDSC XML. Identify every documented SWO, synchronous, and trace-buffer path.

For each trace path, record the configuration that makes it operable: trace-clock source and enablement, pin-function and electrical configuration, path-selection controls, and the sink operating mode and lifecycle (initialization, capture, flush, and stop). Consider mux or function registers (including PFS where that is the vendor term), pad control, drive strength, slew rate, pull configuration, and voltage-domain settings when documented as relevant; record an authoritative `not applicable` decision when they are not required.

Search linked and local documentation first:

- Datasheets, reference manuals, implementation guides, SDFs, SVDs, and vendor packs.
- Debug scripts and relevant source code.
- When a board is named, its board manual or design guide, connector pin maps, and jumper settings.
- When device or board evidence leaves the lifecycle of an identified CoreSight component unresolved, the authoritative Arm CoreSight component technical reference manual and applicable architecture specification.

When a cited vendor documentation URL is unreachable, search the web for the current official vendor page or document before recording it as unavailable. Search by vendor, document title or identifier, product family, and revision when known. Prefer the vendor's canonical page or download; do not replace an unavailable official source with an unofficial mirror. In the record, retain the failed URL, retrieval result, search terms, and replacement official URL or the precise reason no official replacement was found.

For each item, cite the document edition/revision and section/page when known; for source, cite repository/version, path, and symbol or line. An authoritative vendor SDK header or source file may evidence a generated pin-function-register encoding when the hardware manual omits that encoding; cite the matching device, SDK version, path, and symbol. Do not infer from a part name or similar device: addresses, identifiers, protocol capabilities, core associations, reset/debug-authentication behavior, dormant-state requirements, patches, or trace routing.

If documentation does not establish DP/AP or CoreSight topology, offer `pyocd gdbserver -vv` for an accessible target or request its complete startup log. Detection is positive but incomplete evidence: it can omit powered-down/reset-held, debug-locked, or disabled subsystems. Rely on a repeat scan only with documented unlock or power-up steps; an incomplete scan remains incomplete.

Use CoreSight documentation to establish only component-local lifecycle programming: initialization, enable/capture, flush, stop, and status polling for the identified component and version. Cite the component/version and exact section. Do not use it to infer SoC or board integration, component address, clock or reset control, pin configuration, or routing; require device or board evidence for those facts. Before recording a sink-lifecycle blocker, complete this CoreSight evidence pass.

## Required review record

Create or update `.agent-artifacts/<pdsc-stem>.debug-knowledge.md` at the project root:

```markdown
# CMSIS-Pack debug topology review

PDSC: `<path>`
Device / processor: `<name>`
Selected scope: `<family | subFamily | device | variants>`
Last updated: `<ISO 8601 date and time>` (Model: `<name> <version> <reasoning-level>`)
Status: `DRAFT — AWAITING USER REVIEW` | `READY` | `BLOCKED`

## Evidence
| Item | Value | Evidence type | Source | Location | Confidence |
|---|---|---|---|---|---|

## Debug connection topology
| Connection model | Processor | DP / AP / APID path | Dormant-state decision | Evidence |
|---|---|---|---|---|

## Trace components
| CoreSight component instance | Product / variant / version (optional) | Base address | DP / AP path | Evidence | Status |
|---|---|---|---|---|---|

## Available trace paths
| Path / mode | Processor | Route / components | DP / AP path | Availability | Evidence |
|---|---|---|---|---|---|

## Trace operating requirements
| Path / mode | Trace-clock source and enablement | Pin-function and electrical configuration | Path-selection controls | Sink mode and lifecycle | Evidence | Status |
|---|---|---|---|---|---|---|

## Board configuration and configurable routing variables
| Board / connector / jumper | Routing choice | Proposed variable | Valid values and default | Evidence | Status |
|---|---|---|---|---|---|

## Completion checklist
| Path / mode | Clock control | Pin-function and electrical configuration | Source selection | Sink lifecycle | CoreSight lifecycle evidence pass | Retrieval attempt and precise blocker | Complete |
|---|---|---|---|---|---|---|---|

## Open questions
- `<question or none>`

## Documents requiring user download
| Title | Failed URL | Retrieval issue and search terms | Replacement official URL | Requested workspace path |
|---|---|---|---|---|
```

- Update `Last updated` on every change; copy model details exactly from the execution environment. Use `not reported` for unavailable values; do not infer them.
- Only this skill may update the artifact. Treat it as read-only elsewhere; return here if a required fact is absent, stale, contradictory, or insufficient.
- For user review, outline every available, unavailable, or unresolved trace path and its reason, even when trace was not requested. After confirmation, configure every evidenced path by default; do not require per-path selection.
- For a board-specific request, record every documented connector or jumper routing choice and the evidenced configurable-routing-variable proposal; do not defer this analysis to trace-sequence generation. This skill proposes variables only and does not edit PDSC `debugvars`.
- Before writing or presenting a draft, make a second evidence pass for every unresolved trace prerequisite: clock control, pin-function or electrical configuration, source selection, and sink lifecycle. For every unresolved lifecycle, complete the CoreSight lifecycle evidence pass before recording a blocker. Mark the completion checklist complete only when every path has all required facts or records a concrete retrieval attempt and precise blocker.

## Readiness state

- The user's first review confirmation, or a follow-up that clearly authorizes completing trace work, is sufficient to set `READY` when the evidence requirements are met; do not require a separate conversational confirmation.
- Create and update the artifact without confirmation. Before confirmation, use exactly `DRAFT — AWAITING USER REVIEW` and present it for corrections.
- Set `READY` only after confirmation and sufficient evidence for the intended downstream work. Trace assembly additionally requires an evidenced processor, debug connection, DP/AP selection, CoreSight addresses, and every available trace path; for each path, it also requires evidenced trace-clock source and enablement, pin-function and electrical configuration, path-selection controls, and sink mode plus initialization, capture, flush, and stop lifecycle requirements. Record an authoritative `not applicable` decision where a control does not apply. Leave the record non-READY for trace assembly when any required control is unknown, unavailable, or contradictory.
- Preserve `READY` unless an affected fact becomes stale, contradictory, or insufficient. Use `BLOCKED` only for an unavailable required input, and name it.

Keep verified knowledge as reusable CMSIS-Pack input.

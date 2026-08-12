# Debug and trace topology evidence contract

Use the current [Open-CMSIS-Pack *Debug Description* specification](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/debug_description.html) as the grammar authority. Collect hardware and topology evidence only; do not edit PDSC debug descriptions or sequences.

## Topology inventory and evidence

Map the selected device subtree, including inheritance and leaf variants. Inspect relevant PDSC debug and trace definitions—ports, DP/APs, variables, patches, and sequences—and record inherited and local facts without prescribing PDSC XML. Identify every documented SWO, synchronous, and trace-buffer path.

For each trace path, record the configuration that makes it operable: trace-clock source and enablement, pin mux/PFS programming, path-selection controls, and the sink operating mode and lifecycle (initialization, capture, flush, and stop).

Search linked and local documentation first:

- Datasheets, reference manuals, implementation guides, SDFs, SVDs, and vendor packs.
- Debug scripts and relevant source code.

For each item, cite the document edition/revision and section/page when known; for source, cite repository/version, path, and symbol or line. Do not infer from a part name or similar device: addresses, identifiers, protocol capabilities, core associations, reset/debug-authentication behavior, dormant-state requirements, patches, or trace routing.

If documentation does not establish DP/AP or CoreSight topology, offer `pyocd gdbserver -vv` for an accessible target or request its complete startup log. Detection is positive but incomplete evidence: it can omit powered-down/reset-held, debug-locked, or disabled subsystems. Rely on a repeat scan only with documented unlock or power-up steps; an incomplete scan remains incomplete.

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
| Path / mode | Trace-clock source and enablement | Pin mux / PFS programming | Path-selection controls | Sink mode and lifecycle | Evidence | Status |
|---|---|---|---|---|---|---|

## Open questions
- `<question or none>`

## Documents requiring user download
| Title | URL | Retrieval issue | Requested workspace path |
|---|---|---|---|
```

- Update `Last updated` on every change; copy model details exactly from the execution environment. Use `not reported` for unavailable values; do not infer them.
- Only this skill may update the artifact. Treat it as read-only elsewhere; return here if a required fact is absent, stale, contradictory, or insufficient.
- For user review, outline every available, unavailable, or unresolved trace path and its reason, even when trace was not requested. After confirmation, configure every evidenced path by default; do not require per-path selection.

## Readiness state

- Create and update the artifact without confirmation. Before confirmation, use exactly `DRAFT — AWAITING USER REVIEW` and present it for corrections.
- Set `READY` only after confirmation and sufficient evidence for the intended downstream work. Trace assembly additionally requires an evidenced processor, debug connection, DP/AP selection, CoreSight addresses, and every available trace path; for each path, it also requires evidenced trace-clock source and enablement, pin mux/PFS programming, path-selection controls, and sink mode plus initialization, capture, flush, and stop lifecycle requirements. Record an authoritative `not applicable` decision where a control does not apply. Leave the record non-READY for trace assembly when any required control is unknown, unavailable, or contradictory.
- Preserve `READY` unless an affected fact becomes stale, contradictory, or insufficient. Use `BLOCKED` only for an unavailable required input, and name it.

Keep verified knowledge as reusable CMSIS-Pack input.

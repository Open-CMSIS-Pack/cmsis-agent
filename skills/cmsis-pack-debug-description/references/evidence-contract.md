# Debug topology evidence contract

Use the current Open-CMSIS-Pack *Debug Description* specification as the grammar authority. It defines `<debug>`, `<debugconfig>`, `<debugvars>`, debug-port, access-port, sequence, and trace elements.

## Contents

- [Topology inventory and evidence](#topology-inventory-and-evidence)
- [Required review record](#required-review-record)
- [PDSC update, validation, and trace handoff](#pdsc-update-validation-and-trace-handoff)

## Topology inventory and evidence

Map the selected device subtree, including inheritance and outer leaf variants. Inspect existing `<debug>`, `<dbg_datapatch>`, `<debugconfig>`, `<debugport>`, `dp_*`, `accessportV1`, `accessportV2`, `<debugvars>`, and non-trace `<sequences>` definitions. Record inherited and local definitions. Always provide explicit `debugconfig` and `debug` elements for each verified connection model; omit optional debug-port/access-port definitions when the standard default model is sufficient, and do not add `accessportV1` merely to restate a default `__dp`/`__ap` path.

Collect evidence for every value not supported by the PDSC. For each `dbg_datapatch`, record the affected location, value or payload, applicability condition, intended workaround, and authoritative source. If evidence does not establish the DP/AP or CoreSight topology, offer the pyOCD workflow described below; do not infer addresses, identifiers, protocol capabilities, core associations, reset/debug-authentication behavior, dormant-state requirements, or a patch from a part name.

## Required review record

Create `.agent-artifacts/<pdsc-stem>.debug-topology.md` at the project root in this form:

```markdown
# CMSIS-Pack debug topology review

PDSC: `<path>`
Device / processor: `<name>`
Selected scope: `<family | subFamily | device | variants>`
Status: `DRAFT — AWAITING USER REVIEW` | `READY FOR TRACE` | `BLOCKED`

## Evidence
| Item | Value | Evidence type | Source | Location | Confidence |
|---|---|---|---|---|

## CMSIS-Pack definitions
| Element | Identifier | PDSC placement | Applies to | Purpose | Evidence |
|---|---|---|---|---|---|

## Trace prerequisites
| CoreSight component instance | Base address | DP / AP path | Evidence | Status |
|---|---|---|---|---|

## Open questions
- `<question or none>`

## Documents requiring user download
| Title | URL | Retrieval issue | Requested workspace path |
|---|---|---|---|
```

Search for linked and local documentation first, including datasheets, reference manuals, implementation guides, debug manuals, SVDs, existing vendor packs, and references to those documents. Also inspect relevant source code and scripts, such as debug initialization, board-support packages, register definitions, debugger configuration, or trace setup code.

Treat a datasheet, reference manual, implementation guide, vendor SDF/pack, relevant source code, or explicit user-supplied topology as evidence. Record edition/revision and section/page when known; for source code record repository/version, path, and symbol or line. A snippet or prior PDSC is evidence only after checking that it applies to this device and revision. Mark source-code-derived facts as such: code can substantiate an implementation but does not replace missing hardware documentation. For every `dbg_datapatch`, record the affected location, value or payload, applicability condition, intended workaround, and source; do not infer a patch from a similar device or a generic erratum title.

Ask for documentation or the following concrete inputs when necessary: debug protocol(s), DP type and identifier, AP version/type/index or ID, processor-to-AP association, debug authentication/lock behavior, reset behavior, CoreSight component instances and base addresses, and trace-path connectivity. If no other evidence establishes DP/AP or CoreSight topology, offer to run `pyocd gdbserver -vv` against an accessible target and extract the detected facts from its terminal output. Otherwise, ask the user to run it and provide the complete automatic-detection startup log. Tell the user that automatic detection can omit subsystems that are powered down, held in reset, debug-locked, or otherwise disabled; device-specific unlock or power-up steps may be needed before repeating the scan. For a newer TPIU, explicitly ask whether the trace clock must be enabled before automatic detection. Because TPIU registers can span two clock domains, accessing an unavailable trace-clock domain can cause an AHB lockup. This enable is usually device-specific: unless evidence shows it is unnecessary, derive it from the target's documented debug or clock-control behavior and put it in `DebugDeviceUnlock` before discovery. If no better evidence can be found, treat the log as sufficient only for the topology it positively reports, and record that the scan may be incomplete. Do not infer unreported subsystems or undocumented behavior from its absence.

Apply a minimum-description rule. Always emit explicit `debugconfig` and `debug` elements for each verified connection model. A single debug port may still omit `debugport`; without access-port elements, the standard `__dp="0"` and `__ap="0"` defaults apply. Add `accessportV1` only when it conveys required topology, notably when APv1 elements are needed alongside APv2 in a mixed ADIv5/ADIv6 topology. Avoid adding other elements that only restate defaults.

Evaluate the `debugconfig dormant` attribute explicitly. It is `false` by default and should be `true` only for a CoreSight DP that requires the dormant state for protocol switching. Treat Arm-provided ADIv6 DPs as a priority research case, but record the supporting document or source-code evidence rather than assuming the result from ADI version alone.

Do not use `__` as a prefix for DFP-defined local sequence variables. The CMSIS-Pack specification reserves that convention for predefined variables and debugger-provided implementation locals.

Create and update the `.agent-artifacts/` review record without requesting confirmation; it is agent-owned working output. Before editing an existing user-owned PDSC or another existing user file, show the completed review record and obtain the user's confirmation. Apply only the confirmed facts; record corrections and unanswered questions before setting `READY FOR TRACE`.

When a source document cannot be downloaded because of access control, licensing, network failure, an unsupported format, or another retrieval failure, add it to `Documents requiring user download`. Show the user the URL and ask them to download it manually and copy it into the target workspace. After the user supplies the local path, inspect that copy and update the evidence record; do not treat the URL alone as verified technical evidence.

For a selected family, subFamily, device with variants, or another subtree, identify the highest selected ancestor whose descendants share a verified definition. Put the shared definition there. Put a differing definition only on the outer leaf variant that requires it. Record both `PDSC placement` and `Applies to` in the review table.

## PDSC update, validation, and trace handoff

Present the completed review record for user corrections. If the topology is insufficient, ask focused questions; do not write speculative debug descriptions. Update the PDSC only after user confirmation and only for verified data. Preserve unrelated descriptions and keep each configuration and patch associated with its correct device/processor.

Validate XML/PDSC syntax with the project toolchain when available. Update the review record with validation results and report source, confidence, and unresolved items for every topology entry.

Mark the topology `READY FOR TRACE` only after confirmation and when the target processor, debug connection, DP/AP selection, and CoreSight addresses required by the requested trace path are documented and evidenced. Otherwise mark it `AWAITING USER REVIEW` or `BLOCKED` and name the missing input. Do not generate trace sequences; direct that work to `$cmsis-pack-trace-sequences` after this gate passes.

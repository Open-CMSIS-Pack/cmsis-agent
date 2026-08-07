# Debug topology evidence contract

Use the current Open-CMSIS-Pack *Debug Description* specification as the grammar authority. It defines `<debug>`, `<debugconfig>`, `<debugvars>`, debug-port, access-port, sequence, and trace elements.

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

Treat a datasheet, reference manual, implementation guide, vendor SDF/pack, relevant source code, or explicit user-supplied topology as evidence. Record edition/revision and section/page when known; for source code record repository/version, path, and symbol or line. A snippet or prior PDSC is evidence only after checking that it applies to this device and revision. Mark source-code-derived facts as such: code can substantiate an implementation but does not replace missing hardware documentation.

Ask for documentation or the following concrete inputs when necessary: debug protocol(s), DP type and identifier, AP version/type/index or ID, processor-to-AP association, debug authentication/lock behavior, reset behavior, CoreSight component instances and base addresses, and trace-path connectivity. When DP/AP or CoreSight automatic discovery would help fill these gaps, ask the user to connect the target with pyOCD in verbose, non-quiet mode (for example, `pyocd commander -vv`) and provide the complete console log. Record the log as user-supplied runtime evidence and use it to guide documentation review; it does not replace required authoritative evidence for undocumented behavior.

Apply a minimum-description rule. Always emit explicit `debugconfig` and `debug` elements for each verified connection model. A single debug port may still omit `debugport`; without access-port elements, the standard `__dp="0"` and `__ap="0"` defaults apply. Add `accessportV1` only when it conveys required topology, notably when APv1 elements are needed alongside APv2 in a mixed ADIv5/ADIv6 topology. Avoid adding other elements that only restate defaults.

Evaluate the `debugconfig dormant` attribute explicitly. It is `false` by default and should be `true` only for a CoreSight DP that requires the dormant state for protocol switching. Treat Arm-provided ADIv6 DPs as a priority research case, but record the supporting document or source-code evidence rather than assuming the result from ADI version alone.

Do not use `__` as a prefix for DFP-defined local sequence variables. The CMSIS-Pack specification reserves that convention for predefined variables and debugger-provided implementation locals.

Before editing the PDSC, show the completed review record to the user. Apply only the facts the user confirms; record corrections and unanswered questions before setting `READY FOR TRACE`.

When a source document cannot be downloaded because of access control, licensing, network failure, an unsupported format, or another retrieval failure, add it to `Documents requiring user download`. Show the user the URL and ask them to download it manually and copy it into the target workspace. After the user supplies the local path, inspect that copy and update the evidence record; do not treat the URL alone as verified technical evidence.

For a selected family, subFamily, device with variants, or another subtree, identify the highest selected ancestor whose descendants share a verified definition. Put the shared definition there. Put a differing definition only on the outer leaf variant that requires it. Record both `PDSC placement` and `Applies to` in the review table.

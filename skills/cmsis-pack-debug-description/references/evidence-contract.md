# Debug topology evidence contract

## Topology inventory

Map the selected device subtree and its inherited/local `<debug>`, `<dbg_datapatch>`, `<debugconfig>`, `<debugport>`, `dp_*`, `accessportV1`, `accessportV2`, `<debugvars>`, and non-trace `<sequences>` definitions. Emit explicit `debugconfig` and `debug` elements for every verified connection model. Omit debug-port and access-port elements that only restate a standard default; add `accessportV1` only when required topology, including mixed ADIv5/ADIv6, needs it.

Record every unsupported PDSC value, especially each `dbg_datapatch` location, value/payload, applicability, workaround, and source. Do not infer a DP/AP/CoreSight topology, reset or authentication behavior, dormant-state requirement, or patch from a part name.

## Required review record

Create `.agent-artifacts/<pdsc-stem>.debug-topology.md`:

```markdown
# CMSIS-Pack debug topology review

PDSC: `<path>`
Device / processor: `<name>`
Selected scope: `<family | subFamily | device | variants>`
Status: `DRAFT — AWAITING USER REVIEW` | `READY FOR TRACE` | `BLOCKED`

## Evidence
| Item | Value | Evidence type | Source | Location | Confidence |
|---|---|---|---|---|---|

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

If documentation does not establish topology, offer to inspect an accessible target with `pyocd gdbserver -vv`, or request its complete startup log. Treat automatic detection as positive evidence only: it can omit powered-down, reset-held, locked, or otherwise disabled components. Require documented unlock or power-up steps before relying on a repeat scan. For newer TPIU designs, confirm whether trace-clock enablement is needed before discovery; accessing an unavailable clock domain can lock the AHB. Record an incomplete scan as such.

Set `debugconfig dormant` only with device-specific evidence. For Arm-provided ADIv6 DPs, research the requirement rather than inferring it from the ADI version.

## Trace gate

Set `READY FOR TRACE` only after confirmation when the requested trace path has evidenced processor, connection, DP/AP selection, and CoreSight addresses. Otherwise retain `AWAITING USER REVIEW` or set `BLOCKED` and name the missing input. Do not generate trace sequences; hand that work to `$cmsis-pack-trace-sequences`.

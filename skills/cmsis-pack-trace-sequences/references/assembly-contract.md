# Trace assembly contract

Use the current Open-CMSIS-Pack *Debug Description* specification as the grammar authority. Its `Sequence("name")` function calls another debug access sequence; predefined sequence implementations in a PDSC override the debugger-provided behavior.

## Required recognizable output

Maintain exactly one target `<debugvars>` element and one target `<sequences traceSetup="full">` element for each affected debug description. This skill always generates full trace setup: the PDSC sequences perform the trace sink and trace data-path setup rather than relying on debugger a-priori CoreSight configuration. Use the following marker comments verbatim:

Trace setup is device-level by default. Unless the user explicitly narrows scope, assemble it for all processors and all evidence-backed trace paths available in the selected device subtree; do not create separate default configurations merely because a device has multiple CPUs.

```xml
<!-- CMSIS-PACK-TRACE: ADD-DEVICE-SPECIFIC-HERE BEGIN -->
<!-- CMSIS-PACK-TRACE: ADD-DEVICE-SPECIFIC-HERE END -->
<!-- CMSIS-PACK-TRACE: INSERT-CORESIGHT-SNIPPET-CALLS-HERE -->
```

The `ADD-DEVICE-SPECIFIC-HERE` pair delimits user-maintained device work. The single CoreSight insertion marker belongs in a mode-specific `DoTrace*` helper, whose name identifies the phase and trace mode; do not add redundant CoreSight begin/end wrappers.

The markers define the required order:

```text
TraceStart:   ADD-DEVICE-SPECIFIC-HERE -> mode-specific CoreSight start helpers
TraceCapture: ADD-DEVICE-SPECIFIC-HERE -> mode-specific CoreSight capture helpers
TraceFlush:   mode-specific CoreSight flush/read helpers -> ADD-DEVICE-SPECIFIC-HERE
TraceStop:    mode-specific CoreSight stop helpers -> ADD-DEVICE-SPECIFIC-HERE
```

The debugger is expected to call `TraceFlush` before `TraceStop` so trace data is drained before stopping. Therefore `TraceStop` and `DoTraceStop_<mode>` helpers must not call `DoTraceFlush` or any `DoTraceFlush_<mode>` helper again.

## Standard component-operation routing

Call a component operation only from the helper for its active trace mode:

| Operation | Helper destination | Applies to |
|---|---|---|
| `Configure` | `DoTraceStart_<mode>` | Active path only |
| `Capture` | `DoTraceCapture_<mode>` | Active path only |
| `Flush` | `DoTraceFlush_<mode>` | Active path only; never call `TraceFlush` from a capture snippet |
| `ReadBuffer` | `DoTraceReadBuffer` after `DoTraceFlush` | Trace-buffer path only |

For every applicable start or capture path, call operations in trace-route order: first funnels, then other glue logic such as an ETF in hardware-FIFO mode, then the trace sink or output component.

| Trace mode | Applicable component route |
|---|---|
| SWO | Funnels on the SWO route, then either a standalone SWO component or a Cortex-M TPIU used in SWO mode. |
| Sync | Funnels on the synchronous route, then applicable glue such as an ETF in hardware-FIFO mode, then the synchronous TPIU trace port. |
| Trace buffer | Funnels on the buffer route, then applicable glue, then the selected buffer sink: ETB, ETF in circular-buffer mode, ETR, or another verified buffer variant. |

Do not emit a component call merely because its snippet exists. The verified topology determines its trace mode and presence. For shutdown or other operations not listed above, use only the component-specific order supported by evidence.

Every component snippet must use a numeric `instance_suffix`, an optional `<debugvars>` contribution, and one or more sequence bodies. Number instances of each component type from `0` in ascending verified base-address order: `CS_<COMPONENT>_0_*`, `CS_<COMPONENT>_1_*`, and so on. Preserve prior numbering when updating an existing PDSC. Rename its sequence names and `Sequence("...")` calls before integration using that suffix. Do not merge duplicate variables silently: same name requires identical documented meaning and value, otherwise stop for user direction.

When a verified device-specific trace design uses a CoreSight component in a non-standard role or order, its snippet sequences may be called from the applicable `ADD-DEVICE-SPECIFIC-HERE` region. Exclude those operations from the standard mode-specific CoreSight helper path so the component is not configured, captured, flushed, or stopped twice.

For a selected device subtree, place a configuration shared by all selected descendants on the topmost selected common level. Put only specializations/deviations on outer leaf variants. The same placement rule applies to `<debugvars>`, component sequence bodies, scaffold calls, and device-extension placeholders.

The XML assets use a wrapper element only to remain well-formed standalone files. Copy their child `debugvars`/`sequence` elements into the PDSC; do not copy the wrapper. They are skeletons, not validated device configurations. Fill every `TODO` only from the topology record and authoritative device/CoreSight documentation.

Format generated sequence fragments for review: matching XML tags align; C-like text starts on the line after `<block>`, is indented one level inside it, and uses one statement per semicolon-terminated line. Do not split XML entities such as `&amp;`.

## Structural and formatting gates

Before editing, select the applicable scaffold and confirm that every required top-level sequence and applicable `DoTrace*_<mode>` helper remains present, with its opening trace-mode variable block. Insert calls only through the defined helper and extension markers. Do not flatten or inline helpers to shorten the result.

Treat one-line `<block>` bodies and multiple C-like semicolon-terminated statements on one line as validation failures. Before handoff, validate XML/PDSC syntax and run:

```text
rg -n '<block>.*;</block>' <target.pdsc>
rg -n '^[[:space:]]*[^<].*;.*;' <target.pdsc>
```

Inspect and resolve every match that is C-like sequence content. If context or time is tight, stop and continue later rather than producing compressed or partial sequences. Never trade required scaffold structure for schema validity or a shorter response.

Write `.agent-artifacts/<pdsc-stem>.trace-sequences.md` at the project root. Record the selected device-tree scope, each PDSC placement and its applicable descendants, trace component instances, emitted sequence names, extension placeholders, sources, and validation result. Include a `Documents requiring user download` table with `title | URL | retrieval issue | requested workspace path` whenever an identified technical document cannot be downloaded. Ask the user to download and copy it into the target workspace, then inspect that local copy before relying on it.

`.agent-artifacts/` files are agent-owned and may be created or updated without user confirmation. Before changing an existing user-owned PDSC or another existing user file, present the proposed change and obtain user confirmation.

After the user confirms all trace-path choices, generate, apply, and validate the complete XML in the same turn. Do not stop after updating the assembly record unless missing evidence or a validation failure blocks completion.

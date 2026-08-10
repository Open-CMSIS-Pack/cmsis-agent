# Trace assembly contract

Maintain one target `<debugvars>` and one `<sequences traceSetup="full">` for each affected debug description. Use full trace setup; PDSC sequences must configure the sink and trace data path. Retain these user-maintained extension markers verbatim:

```xml
<!-- CMSIS-PACK-TRACE: ADD-DEVICE-SPECIFIC-HERE BEGIN -->
<!-- CMSIS-PACK-TRACE: ADD-DEVICE-SPECIFIC-HERE END -->
<!-- CMSIS-PACK-TRACE: INSERT-CORESIGHT-SNIPPET-CALLS-HERE -->
```

The `ADD-DEVICE-SPECIFIC-HERE` pair remains in generated XML. Put the single scaffold-only insertion marker in the relevant mode-specific `DoTrace*` helper and remove it after adding selected calls. Do not add other CoreSight wrappers. Keep this order:

```text
TraceStart:   device extension -> mode-specific CoreSight start helpers
TraceCapture: device extension -> mode-specific CoreSight capture helpers
TraceFlush:   mode-specific CoreSight flush/read helpers -> device extension
TraceStop:    mode-specific CoreSight stop helpers -> device extension
```

The debugger flushes before stopping. `TraceStop` and `DoTraceStop_<mode>` must not call `DoTraceFlush` or `DoTraceFlush_<mode>`.

## Component selection and routing

Select one asset per required physical CoreSight instance. Merge each selected asset's `debugvars` contribution into the target's single `<debugvars>`; reject a conflicting duplicate variable. Number component sequences by type from `0` in ascending evidenced base-address order (for example, `CS_ETF_0_*`) and preserve existing numbering. Rename every copied sequence and `Sequence("...")` call consistently.

Use a component operation only from the helper for its active mode:

| Operation | Helper destination | Applies to |
|---|---|---|
| `Configure` | `DoTraceStart_<mode>` | Active path only |
| `Capture` | `DoTraceCapture_<mode>` | Active path only |
| `Flush` | `DoTraceFlush_<mode>` | Active path only; never call `TraceFlush` from generated component/helper code |
| `ReadBuffer` | `DoTraceReadBuffer` after `DoTraceFlush` | Trace-buffer path only |

For each start/capture route, call funnels first, then route glue (for example an ETF in hardware-FIFO mode), then the sink/output component. Use only the evidenced path:

| Trace mode | Route |
|---|---|
| SWO | Route funnels, then SWO or a Cortex-M TPIU in SWO mode. |
| Sync | Route funnels, applicable glue, then the synchronous TPIU port. |
| Trace buffer | Route funnels, applicable glue, then ETB, circular ETF, ETR, or another verified sink. |

`TraceFlush` is debugger-facing and is never a helper dependency. A `DoTraceFlush_<mode>` helper may be reused only when the evidenced operation requires it, never from the stop path. For a non-standard component role/order, call it from the device-extension region and exclude it from standard helpers so it is not operated twice.

## Asset fidelity and structural gates

Copy a selected asset's comments, declarations, checks, control blocks, waits, cleanup, and sequence bodies; it is a source template, not an example. Copy only its child `debugvars`/`sequence` elements, never its standalone wrapper. Fill `TODO`s solely from the topology record and authoritative device/CoreSight documentation.

Write `.agent-artifacts/<pdsc-stem>.trace-sequences.md` listing scope, PDSC placement/applicability, selected instances, emitted sequences, extension placeholders, sources, omissions/changes, and validation. For each selected asset, record every retained, changed, or omitted line/block and its device-specific evidence reason. After normalizing approved suffixes and values, compare the generated fragment with the asset; an undocumented missing comment, declaration, control block, wait, or cleanup is a validation failure.

Before editing, select the scaffold and retain every required top-level sequence and applicable `DoTrace*_<mode>` helper with its opening trace-mode variable block. Do not flatten helpers. When a retained helper has only standard trace-mode declarations, put `<!-- No trace operation is required for this mode. -->` after its opening `<block>`.

Also inspect `rg -n 'INSERT-CORESIGHT-SNIPPET-CALLS-HERE' <target.pdsc>` and resolve every remaining scaffold marker. If the topology record has no evidenced dormant-state decision or a selected snippet needs unavailable information, return to `$cmsis-pack-debug-description` or request documentation.

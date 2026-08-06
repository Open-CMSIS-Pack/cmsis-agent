# Trace assembly contract

Use the current Open-CMSIS-Pack *Debug Description* specification as the grammar authority. Its `Sequence("name")` function calls another debug access sequence; predefined sequence implementations in a PDSC override the debugger-provided behavior.

## Required recognizable output

Maintain exactly one target `<debugvars>` element and one target `<sequences traceSetup="full">` element for each affected debug description. This skill always generates full trace setup: the PDSC sequences perform the trace sink and trace data-path setup rather than relying on debugger a-priori CoreSight configuration. Use the following marker comments verbatim:

```xml
<!-- CMSIS-PACK-TRACE: DEVICE-EXTENSION-START BEGIN -->
<!-- CMSIS-PACK-TRACE: DEVICE-EXTENSION-START END -->
<!-- CMSIS-PACK-TRACE: CORESIGHT-START BEGIN -->
<!-- CMSIS-PACK-TRACE: CORESIGHT-START END -->
```

Use the analogous marker names `DEVICE-EXTENSION-CAPTURE`, `CORESIGHT-CAPTURE`, `CORESIGHT-FLUSH`, `DEVICE-EXTENSION-FLUSH`, `CORESIGHT-STOP`, and `DEVICE-EXTENSION-STOP` for the other three predefined sequences.

The markers define the required order:

```text
TraceStart:   DEVICE-EXTENSION-START  -> CORESIGHT-START
TraceCapture: DEVICE-EXTENSION-CAPTURE -> CORESIGHT-CAPTURE
TraceFlush:   CORESIGHT-FLUSH -> DEVICE-EXTENSION-FLUSH
TraceStop:    CORESIGHT-STOP -> DEVICE-EXTENSION-STOP
```

Every component snippet must use a numeric `instance_suffix`, an optional `<debugvars>` contribution, and one or more sequence bodies. Number instances of each component type from `0` in ascending verified base-address order: `CS_<COMPONENT>_0_*`, `CS_<COMPONENT>_1_*`, and so on. Preserve prior numbering when updating an existing PDSC. Rename its sequence names and `Sequence("...")` calls before integration using that suffix. Do not merge duplicate variables silently: same name requires identical documented meaning and value, otherwise stop for user direction.

For a selected device subtree, place a configuration shared by all selected descendants on the topmost selected common level. Put only specializations/deviations on outer leaf variants. The same placement rule applies to `<debugvars>`, component sequence bodies, scaffold calls, and device-extension placeholders.

The XML assets use a wrapper element only to remain well-formed standalone files. Copy their child `debugvars`/`sequence` elements into the PDSC; do not copy the wrapper. They are skeletons, not validated device configurations. Fill every `TODO` only from the topology record and authoritative device/CoreSight documentation.

Write `.agent-artifacts/<pdsc-stem>.trace-sequences.md` at the project root. Record the selected device-tree scope, each PDSC placement and its applicable descendants, trace component instances, emitted sequence names, extension placeholders, sources, and validation result. Include a `Documents requiring user download` table with `title | URL | retrieval issue | requested workspace path` whenever an identified technical document cannot be downloaded. Ask the user to download and copy it into the target workspace, then inspect that local copy before relying on it.

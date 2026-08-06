# Trace assembly contract

Use the current Open-CMSIS-Pack *Debug Description* specification as the grammar authority. Its `Sequence("name")` function calls another debug access sequence; predefined sequence implementations in a PDSC override the debugger-provided behavior.

## Required recognizable output

Maintain exactly one target `<debugvars>` element and one target `<sequences traceSetup="full">` element for each affected debug description. This skill always generates full trace setup: the PDSC sequences perform the trace sink and trace data-path setup rather than relying on debugger a-priori CoreSight configuration. Use the following marker comments verbatim:

```xml
<!-- CMSIS-PACK-TRACE: ADD-DEVICE-SPECIFIC-HERE BEGIN -->
<!-- CMSIS-PACK-TRACE: ADD-DEVICE-SPECIFIC-HERE END -->
<!-- CMSIS-PACK-TRACE: CORESIGHT-START BEGIN -->
<!-- CMSIS-PACK-TRACE: CORESIGHT-START END -->
```

Use the analogous `ADD-DEVICE-SPECIFIC-<PHASE>-HERE` markers for Capture, Flush, and Stop. These markers explicitly identify where device-specific work belongs.

The markers define the required order:

```text
TraceStart:   ADD-DEVICE-SPECIFIC-HERE -> CORESIGHT-START
TraceCapture: ADD-DEVICE-SPECIFIC-HERE -> CORESIGHT-CAPTURE
TraceFlush:   CORESIGHT-FLUSH -> ADD-DEVICE-SPECIFIC-HERE
TraceStop:    CORESIGHT-STOP -> ADD-DEVICE-SPECIFIC-HERE
```

Every component snippet must use a numeric `instance_suffix`, an optional `<debugvars>` contribution, and one or more sequence bodies. Number instances of each component type from `0` in ascending verified base-address order: `CS_<COMPONENT>_0_*`, `CS_<COMPONENT>_1_*`, and so on. Preserve prior numbering when updating an existing PDSC. Rename its sequence names and `Sequence("...")` calls before integration using that suffix. Do not merge duplicate variables silently: same name requires identical documented meaning and value, otherwise stop for user direction.

For a selected device subtree, place a configuration shared by all selected descendants on the topmost selected common level. Put only specializations/deviations on outer leaf variants. The same placement rule applies to `<debugvars>`, component sequence bodies, scaffold calls, and device-extension placeholders.

The XML assets use a wrapper element only to remain well-formed standalone files. Copy their child `debugvars`/`sequence` elements into the PDSC; do not copy the wrapper. They are skeletons, not validated device configurations. Fill every `TODO` only from the topology record and authoritative device/CoreSight documentation.

Format generated sequence fragments for review: matching XML tags align; C-like text starts on the line after `<block>`, is indented one level inside it, and uses one statement per semicolon-terminated line. Do not split XML entities such as `&amp;`.

Write `.agent-artifacts/<pdsc-stem>.trace-sequences.md` at the project root. Record the selected device-tree scope, each PDSC placement and its applicable descendants, trace component instances, emitted sequence names, extension placeholders, sources, and validation result. Include a `Documents requiring user download` table with `title | URL | retrieval issue | requested workspace path` whenever an identified technical document cannot be downloaded. Ask the user to download and copy it into the target workspace, then inspect that local copy before relying on it.



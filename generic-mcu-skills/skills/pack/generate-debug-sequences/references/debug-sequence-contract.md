# Device debug-sequence contract

## Scope and evidence

Reuse the read-only `.agent-artifacts/<pdsc-stem>.debug-knowledge.md` from `$debug-knowledge`: cross-check selected scope, processors, DP/AP/APID mappings, inherited definitions, and device behavior. Create `.agent-artifacts/<pdsc-stem>.debug-sequences.md` without confirmation. Record the knowledge reference, scope, processors, inherited sequences, proposed names, placement and descendants, evidence, default-implementation provenance, low-power debug behavior, and open questions. If the knowledge record lacks required connection facts, return to `$debug-knowledge`.

Browse the current official [Open-CMSIS-Pack *Debug Description* specification](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/debug_description.html) and relevant linked pages before implementation. Use it as the authority for XML grammar, predefined names, execution context, and default implementations. Search device documentation, vendor packs, SVDs, debug scripts, and source code for device-specific facts.

For verified device unlock behavior, prefer a `DebugDeviceUnlock` implementation that supplements the debugger-provided defaults over overriding another predefined sequence. Override a predefined sequence only when `DebugDeviceUnlock` cannot provide the required documented behavior. Obtain the current specification default, then copy its complete structure, comments, variables, checks, waits, cleanup, and control blocks before applying evidence-backed changes. Record the specification URL, version/revision, and section. For custom sequences, document every register address, value, delay, condition, and call with evidence; never invent reset, unlock, authentication, boot, flash, or processor-control behavior.

Implement every evidenced device-specific non-trace behavior in the selected scope by default. When a documented runtime choice belongs to the end user, generate or update it as a `debugvars` `__var` with a supported default and propose matching Configuration Wizard annotations in `.dbgconf`; do not use it to defer missing evidence or omit a verified behavior.

## Low-power and sleep-mode debug

For each relevant documented mode, determine whether the processor, DP/AP, debug clocks/power, pins, reset, and wake path remain accessible. Use only documented pre-entry, wake, reset, or reconnect actions to retain or restore debug access; never prevent sleep, force a higher-power mode, or write undocumented registers merely to keep a probe connected.

For every mode, record applicability, expected access, required action, affected sequence, evidence, and validation in `.agent-artifacts/<pdsc-stem>.debug-sequences.md`. When a target is available, enter the mode, check access where supported, then wake or recover it and confirm the processor and DP/AP path remain usable; otherwise record the required test. State the documented recovery path for modes that cannot retain debugging, and return to `$debug-knowledge` if low-power behavior changes a connection, DP/AP, or power-domain fact.

## CPU applicability and placement

Analyze every affected CPU connection: compare `Pname`, processor-to-DP/AP/APID mapping, and initial `__ap`/`__apid`. Prefer a shared implementation without `Pname` when the only difference is the initial `__ap` or `__apid` selection; rely on that connection context rather than duplicating the sequence. Use connection-based dispatch only when the remaining implementation is demonstrably identical; use `Pname`-specific definitions only for a verified behavioral difference that cannot be expressed through the connection context. Place shared definitions at the topmost common device-tree level and deviations only on outer leaf variants. Preserve unrelated PDSC content and all trace configuration.

When the change adds a `<debugvars>` `__var`, suggest adding or updating the matching `.dbgconf` file with Configuration Wizard annotations. Include the variable name, default, and applicable scope; do not modify the `.dbgconf` file unless the user includes it in their confirmation.

## Formatting, confirmation, and validation

Align matching XML tags. Put C-like content on lines inside each `<block>`, indented one level deeper than the tag, with one semicolon-terminated statement per line; keep XML entities intact. A one-line `<block>` body or multiple C-like statements on one line is a validation failure. Do not compress, partially generate, or omit required structure when context or time is tight.

Before changing a user-owned PDSC or other file, present the sequence record, referenced debug knowledge record, and proposed XML for confirmation. After confirmation, immediately apply and validate the complete non-trace change in the same turn.

Validate XML/PDSC syntax with the available toolchain. Run `rg -n '<block>.*;</block>' <target.pdsc>` and `rg -n '^[[:space:]]*[^<].*;.*;' <target.pdsc>`, then inspect and resolve every C-like match. For overridden predefined sequences, compare generated output with the current specification default and record every evidence-backed difference. Report changed sequences, per-CPU applicability, sources, placement, and validation results.

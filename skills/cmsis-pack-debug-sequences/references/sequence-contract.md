# Device debug-sequence contract

## Scope and evidence

Reuse `.agent-artifacts/<pdsc-stem>.debug-topology.md`: cross-check selected scope, processors, DP/AP/APID mappings, inherited definitions, and device behavior. Create `.agent-artifacts/<pdsc-stem>.debug-sequences.md` without confirmation. Record the topology reference, scope, processors, inherited sequences, proposed names, placement and descendants, evidence, default-implementation provenance, and open questions. If the topology lacks required connection facts, return to `$cmsis-pack-debug-description`.

Browse the current official Open-CMSIS-Pack *Debug Description* specification and relevant linked pages before implementation. Use it as the authority for XML grammar, predefined names, execution context, and default implementations. Search device documentation, vendor packs, SVDs, debug scripts, and source code for device-specific facts.

Override a predefined sequence only for verified device behavior. Obtain the current specification default, then copy its complete structure, comments, variables, checks, waits, cleanup, and control blocks before applying evidence-backed changes. Record the specification URL, version/revision, and section. For custom sequences, document every register address, value, delay, condition, and call with evidence; never invent reset, unlock, authentication, boot, flash, or processor-control behavior.

## CPU applicability and placement

Analyze every affected CPU connection: compare `Pname`, processor-to-DP/AP/APID mapping, and initial `__ap`/`__apid`. Use connection-based dispatch only when the remaining implementation is demonstrably identical; otherwise use `Pname`-specific definitions. Place shared definitions at the topmost common device-tree level and deviations only on outer leaf variants. Preserve unrelated PDSC content and all trace configuration.

When the change adds a `<debugvars>` `__var`, suggest adding or updating the matching `.dbgconf` file with Configuration Wizard annotations. Include the variable name, default, and applicable scope; do not modify the `.dbgconf` file unless the user includes it in their confirmation.

## Formatting, confirmation, and validation

Align matching XML tags. Put C-like content on lines inside each `<block>`, indented one level deeper than the tag, with one semicolon-terminated statement per line; keep XML entities intact. A one-line `<block>` body or multiple C-like statements on one line is a validation failure. Do not compress, partially generate, or omit required structure when context or time is tight.

Before changing a user-owned PDSC or other file, present the sequence record, referenced topology record, and proposed XML for confirmation. After confirmation, immediately apply and validate the complete non-trace change in the same turn.

Validate XML/PDSC syntax with the available toolchain. Run `rg -n '<block>.*;</block>' <target.pdsc>` and `rg -n '^[[:space:]]*[^<].*;.*;' <target.pdsc>`, then inspect and resolve every C-like match. For overridden predefined sequences, compare generated output with the current specification default and record every evidence-backed difference. Report changed sequences, per-CPU applicability, sources, placement, and validation results.

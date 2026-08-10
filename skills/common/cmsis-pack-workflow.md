# Shared CMSIS-Pack workflow

Read this file together with the specialist contract.

## Evidence and scope

Use the specification locator for standard CMSIS-Pack behavior and device documentation, vendor packs, SVDs, relevant source code, or explicit user input for device facts. Record each device-specific value with source and location; do not infer hardware topology, register values, paths, ordering, or workarounds from similar devices. If a required document cannot be retrieved, record its URL and request a local copy before relying on it.

Work only in the selected device subtree. Put a shared verified definition at its highest common selected ancestor; put a deviation only at the outer leaf that needs it. Preserve unrelated PDSC content.

## Artifacts, confirmation, and validation

`.agent-artifacts/` is agent-owned and may be updated without confirmation. Before editing an existing user-owned PDSC or other user file, show the relevant artifact and proposed XML, then obtain confirmation. Apply and validate the complete confirmed change in the same turn.

Format XML with aligned matching tags. Put C-like sequence content on lines inside `<block>`, one statement per line, and preserve XML entities. Validate with the project toolchain when available, then inspect:

```text
rg -n '<block>.*;</block>' <target.pdsc>
rg -n '^[[:space:]]*[^<].*;.*;' <target.pdsc>
```

When adding a `debugvars` `__var`, propose matching Configuration Wizard annotations in the relevant `.dbgconf`; modify that file only when it is included in the confirmation.

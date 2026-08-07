# CMSIS-Pack Debug Sequences

Adds evidence-backed, device-specific non-trace debug access sequences to an existing DFP PDSC.

Use it for device unlock, debug-clock enablement, reset, bootloader, flash, debug authentication, and multi-core startup behavior. It reuses the debug-description topology record and writes a separate sequence evidence record.

When overriding a predefined sequence, it copies the current official CMSIS-Pack default implementation before applying verified device-specific changes. It does not configure CoreSight trace; use the trace-sequences skill for that.


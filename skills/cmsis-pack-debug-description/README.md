# CMSIS-Pack Debug Description

Creates or updates evidence-backed, non-trace CMSIS-Pack Debug Description content in an existing DFP PDSC.

Use it to establish debug connection topology and related elements such as `debug`, `debugconfig`, `debugvars`, debug ports, access ports, and `dbg_datapatch`. It also produces the shared debug-topology evidence record consumed by the sequence skills.

Invoke it when device debug connectivity is missing, incomplete, or needs documentation-backed corrections. It does not generate device debug sequences or CoreSight trace sequences; use the specialized skills for those tasks.


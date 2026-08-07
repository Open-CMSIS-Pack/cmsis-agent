# CMSIS-Pack Trace Sequences

Builds evidence-backed `traceSetup="full"` CMSIS-Pack trace configuration in an existing DFP PDSC.

Use it after the device debug description has established the required connection and CoreSight topology. It assembles selected component assets into device-level SWO, synchronous TPIU, and trace-buffer paths, while preserving device-specific extension regions.

The skill keeps component assets intact, records merge evidence, validates XML formatting and sequence structure, and removes scaffold-only CoreSight insertion markers from generated PDSC XML.


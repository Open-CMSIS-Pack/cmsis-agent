# Debug setup phase-one contract

Run `$cmsis-pack-debug-description` first to establish or update `.agent-artifacts/<pdsc-stem>.debug-topology.md`. Run `$cmsis-pack-debug-sequences` only for verified non-trace device behavior, such as unlock, debug-clock enablement, reset, bootloader, flash, debug authentication, or multi-core startup. It is optional unless trace discovery or access needs it.

Keep ownership strict: Debug Description owns non-sequence elements and topology evidence; Debug Sequences owns device-specific non-trace sequences; Trace Sequences owns trace `debugvars` and sequences. Do not move, overwrite, or duplicate another skill's content merely to consolidate XML.

When trace is requested and topology is `READY FOR TRACE`, create `.agent-artifacts/<pdsc-stem>.trace-input.md` containing only: PDSC path; selected scope/descendants; topology-record path/status; processor `Pname` and DP/AP/APID paths; requested/evidenced trace paths; required component type, address, AP, role, and mode; relevant non-trace prerequisites; concise evidence citations; and unresolved items. Do not copy raw documents, PDSC XML, templates, or the full sequence record.

Confirm and apply only phase-one PDSC changes. End by naming `trace-input.md` and directing the user to a fresh `$cmsis-pack-trace-sequences` turn. Return to the owning specialist when its evidence gate fails.

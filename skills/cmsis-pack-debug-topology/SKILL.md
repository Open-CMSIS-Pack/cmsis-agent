---
name: cmsis-pack-debug-topology
description: Inspect and create evidence-backed CMSIS-Pack debug topology in an existing Device Family Pack (DFP). Use when a PDSC already contains device descriptions and needs debug, debugconfig, debug-port, or access-port definitions; when DP/AP/CoreSight topology is incomplete; or before generating CMSIS-Pack trace sequences.
---

# CMSIS-Pack debug topology

Create a verified debug-description foundation for an existing DFP. Read `references/evidence-contract.md` before editing.

## Workflow

1. Find the target `.pdsc`, selected family/subFamily/device/variant scope, and affected `<processor>` descriptions. Do not create a new DFP and do not continue if no device description exists.
2. Map the selected device subtree, including inheritance and its outer leaf variants. Inspect existing `<debug>`, `<debugconfig>`, `<debugport>`, `dp_*`, `accessportV1`, `accessportV2`, `<debugvars>`, and `<sequences>` definitions. Record inherited definitions as well as local ones. Always provide explicit `debugconfig` and `debug` elements for each verified connection model. Prefer the smallest remaining description: omit optional debug-port and access-port definitions when the standard default connection model is sufficient, and do not add `accessportV1` merely to restate a default `__dp`/`__ap` path.
3. Collect evidence for every value that is not already supported by the PDSC. Search the workspace and supplied links for device documentation, document references, debug/ADI/CoreSight documentation, vendor packs, and relevant source code (for example debug initialization, register definitions, SVD, board support, or debugger scripts). If the DP/AP or CoreSight topology remains unknown, ask the user to connect pyOCD to the target in verbose, non-quiet mode (for example, `pyocd commander -vv`) and provide the complete automatic-detection console log. Record it as user-supplied runtime evidence and use it to focus further documentation review; do not treat it as proof of undocumented hardware behavior. Determine explicitly whether `debugconfig dormant="true"` is required. Give this check priority for Arm-provided ADIv6 debug ports, but set the attribute only when evidence shows the DP requires protocol switching through the dormant state; its specification default is false. If an identified document cannot be downloaded, list its URL and the retrieval issue in the review record, then ask the user to download it manually and copy it into the target workspace. Do not rely on unavailable content or infer addresses, DP/AP identifiers, protocol capabilities, core associations, reset/debug-authentication behavior, or dormant-state requirement from a part name.
4. Create `.agent-artifacts/` at the project root when needed. Write or update `.agent-artifacts/<pdsc-stem>.debug-topology.md`, using the required form in the reference. Include every collected value, its source, and whether it is documentation-verified, source-code-derived, or user-supplied.
5. Present that review record to the user and request confirmation or corrections. If the topology is insufficient, ask focused questions identifying the missing facts and acceptable sources. Do not write speculative debug descriptions.
6. Update the PDSC only after user confirmation and only for verified data. Place each shared definition at the topmost selected device-tree level where it is valid for every descendant. Write specializations or deviations only at outer leaf variants; do not duplicate shared definitions on descendants. Keep the result minimal: introduce `accessportV1` only when the topology requires explicit APv1 elements, such as a mixed ADIv5/ADIv6 topology; otherwise use the standard `__dp`/`__ap` model. Preserve unrelated device descriptions and keep each debug configuration associated with the correct device/processor.
7. Validate XML/PDSC syntax with the project toolchain when available. Update the review record with the result and report the source, confidence, and unresolved items for every topology entry.

## Handoff gate

Mark the topology `READY FOR TRACE` only after user confirmation and when the target processor, debug connection, DP/AP selection, and the CoreSight-component addresses needed by the requested trace path are documented and evidenced. Otherwise mark it `AWAITING USER REVIEW` or `BLOCKED` and name the missing inputs.

Do not generate trace setup sequences. Direct that work to `$cmsis-pack-trace-sequences` after this gate passes.

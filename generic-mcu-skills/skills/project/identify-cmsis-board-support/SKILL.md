---
name: identify-cmsis-board-support
description: Identify the CMSIS BSP for a physical board or fall back to the matching DFP for its exact fitted device. Use when a CMSIS solution needs verified board, device, and pack identifiers.
---

# Identify CMSIS Board Support

## Target & Persona

- **Role:** CMSIS Pack Integrator
- **Objective:** Resolve verified CMSIS board, device, and support-pack identifiers for one physical board and its fitted device.

## Prerequisites & Context

- **Expected input:** The physical board manufacturer, model, revision when applicable, and exact fitted MCU or SoC from authoritative board information.
- **Dependencies:** Access to the Keil Boards and Devices catalogs. An active CMSIS-Toolbox environment is optional for installed-pack cross-checks.
- **Portability:** Applies across MCU and SoC vendors when their board or device is represented in the referenced catalogs. It has no compiler, RTOS, debugger, or probe dependency.

Require:

- physical board manufacturer, model, and revision;
- exact fitted MCU or SoC from authoritative board information.

An `identify-zephyr-board` result provides suitable input for a Zephyr-supported
board, but this skill does not depend on Zephyr.

## Execution Steps (Strict Workflow)

1. **Analysis:** Verify the physical board identity and fitted device from authoritative information.
2. **Processing:** Search the board catalog first, then the device catalog for the exact fitted device whether or not a BSP is established.
3. **Validation:** Confirm every accepted catalog result against the board, device, processor/core, family, and optional installed-pack query.
4. **Formatting:** Return the defined `BSP`, `DFP`, or `NO MATCH` result with exact identifiers and supporting URLs.

### Detailed procedure

1. Search the [Keil Boards catalog](https://www.keil.arm.com/boards/) using the
   manufacturer, model, and device.
2. Accept a BSP match only when its detail page identifies the same physical board
   and fitted device. Record the exact CMSIS board identifier, CMSIS device
   identifier, and BSP pack identifier.
3. Search the [Keil Devices catalog](https://www.keil.arm.com/devices/) using the
   exact fitted device, including when a matching BSP was found. This establishes
   the DFP pack required for a BSP-plus-DFP project and whether it is the same
   pack as the BSP.
4. Accept a DFP match only when the vendor, device or supported device pattern,
   processor/core, and family are compatible. Record the exact CMSIS device
   identifier and DFP pack identifier.
5. When the CMSIS-Toolbox environment is active, cross-check installed packs
   with `csolution list boards --filter <terms>` or
   `csolution list devices --filter <terms>`. An empty local result does not prove
   that the online catalog lacks a BSP or DFP.

Prefer the matching BSP over a DFP-only result. Stop when neither match can be
established or when multiple matches remain plausible.

## Guardrails & Constraints (Strict Rules)

- **No fabrication:** Do not invent CMSIS board, device, BSP, or DFP identifiers, and do not infer support from a similar part name.
- **Portability:** Do not add vendor, toolchain, RTOS, debugger, or probe assumptions beyond the verified catalog result.
- **Critical blockers:** Stop when the board or fitted device is not authoritative, neither catalog match is established, or multiple plausible matches remain.
- **Scope:** Identify CMSIS support only; do not install packs, create a project, or configure hardware.
- **Tone and style:** Respond factually and directly. Omit conversational filler.

## Expected Output

Report:

- status: `BSP`, `DFP`, or `NO MATCH`;
- physical board and fitted device used for the lookup;
- exact CMSIS board identifier when status is `BSP`;
- exact CMSIS device identifier;
- exact BSP pack identifier when status is `BSP`;
- exact DFP pack identifier for the fitted device; when status is `BSP`, report
  whether it is the same pack as the BSP or a separate pack;
- supporting catalog URLs;
- ambiguity or mismatch when status is `NO MATCH`.

Do not invent CMSIS identifiers, infer support from similar part names, or treat an
empty installed-pack query as proof that no online pack exists.

## Validation Resources

- [Keil Boards catalog](https://www.keil.arm.com/boards/)
- [Keil Devices catalog](https://www.keil.arm.com/devices/)
- CMSIS-Toolbox `csolution list boards` and `csolution list devices` commands

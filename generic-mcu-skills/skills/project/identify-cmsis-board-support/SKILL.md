---
name: identify-cmsis-board-support
description: Identify the CMSIS BSP for a physical board or fall back to the matching DFP for its exact fitted device. Use when a CMSIS solution needs verified board, device, and pack identifiers.
---

# Identify CMSIS Board Support

## Input

Require:

- physical board manufacturer, model, and revision;
- exact fitted MCU or SoC from authoritative board information.

An `identify-zephyr-board` result provides suitable input for a Zephyr-supported
board, but this skill does not depend on Zephyr.

## Workflow

1. Search the [Keil Boards catalog](https://www.keil.arm.com/boards/) using the
   manufacturer, model, and device.
2. Accept a BSP match only when its detail page identifies the same physical board
   and fitted device. Record the exact CMSIS board identifier, CMSIS device
   identifier, and BSP pack identifier.
3. If no matching BSP exists, search the [Keil Devices catalog](https://www.keil.arm.com/devices/)
   using the exact fitted device.
4. Accept a DFP match only when the vendor, device or supported device pattern,
   processor/core, and family are compatible. Record the exact CMSIS device
   identifier and DFP pack identifier.
5. When the CMSIS-Toolbox environment is active, cross-check installed packs
   with `csolution list boards --filter <terms>` or
   `csolution list devices --filter <terms>`. An empty local result does not prove
   that the online catalog lacks a BSP or DFP.

Prefer the matching BSP over a DFP-only result. Stop when neither match can be
established or when multiple matches remain plausible.

## Output

Report:

- status: `BSP`, `DFP`, or `NO MATCH`;
- physical board and fitted device used for the lookup;
- exact CMSIS board identifier when status is `BSP`;
- exact CMSIS device identifier;
- exact BSP or DFP pack identifier;
- supporting catalog URLs;
- ambiguity or mismatch when status is `NO MATCH`.

Do not invent CMSIS identifiers, infer support from similar part names, or treat an
empty installed-pack query as proof that no online pack exists.

## Resources

- [Keil Boards catalog](https://www.keil.arm.com/boards/)
- [Keil Devices catalog](https://www.keil.arm.com/devices/)
- CMSIS-Toolbox `csolution list boards` and `csolution list devices` commands

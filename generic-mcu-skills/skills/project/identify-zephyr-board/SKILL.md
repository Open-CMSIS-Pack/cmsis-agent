---
name: identify-zephyr-board
description: Resolve a user-supplied physical board to its exact Zephyr board target and fitted MCU or SoC using the installed west board catalog. Use when a project, build, or support lookup needs a version-matched Zephyr board identifier.
---

# Identify Zephyr Board

## Input

Require:

- physical board manufacturer and model;
- board revision when it affects the device or Zephyr configuration;
- verified Zephyr workspace and venv-local west executable.

Do not select a board on the user's behalf or infer it solely from a connected
debug probe or directory name.

## Workflow

1. From the verified Zephyr workspace, run the venv-local `west boards` command.
   Treat this version-matched catalog, generated from the installed `board.yml`
   files, as authoritative for the installed Zephyr version.
2. Find candidates with `west boards --fuzzy-match "<candidate>"`. Normalize
   punctuation and capitalization only for searching; do not confirm a fuzzy
   result without checking its metadata.
3. For each candidate, run:

   ```text
   west boards --board <board-name> --cmakeformat "{NAME}|{VENDOR}|{SOCS}|{QUALIFIERS}|{REVISION_DEFAULT}|{REVISIONS}|{DIR}"
   ```

4. Read the referenced `board.yml` and board documentation. Confirm the marketing
   name, vendor, fitted SoC, revision, and qualifiers against the physical board.
5. Construct the exact target as `board-name[@revision][/qualifiers]`. Request the
   intended target when multiple SoCs, CPU clusters, or variants remain possible.
6. Identify the exact fitted MCU or SoC from the board metadata, devicetree files,
   and documentation. Do not derive it solely from a board-name substring.
7. Compare the result with the latest [Zephyr Supported Boards and Shields](https://docs.zephyrproject.org/latest/boards/index.html)
   page. Report differences; keep the installed board data authoritative for the
   installed Zephyr version.

Stop when the physical board, exact target, or fitted device remains ambiguous.

## Output

Report:

- status: `MATCH` or `NO MATCH`;
- physical board manufacturer, model, and revision;
- Zephyr board name and exact board target;
- vendor, default revision, revisions, and qualifiers from west;
- fitted MCU or SoC;
- installed board directory;
- supporting local files and documentation URLs;
- unresolved choice or mismatch when status is `NO MATCH`.

## Resources

- [Zephyr Supported Boards and Shields](https://docs.zephyrproject.org/latest/boards/index.html)
- Installed Zephyr `board.yml`, devicetree, and board documentation files

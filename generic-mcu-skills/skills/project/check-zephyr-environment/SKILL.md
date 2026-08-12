---
name: check-zephyr-environment
description: Verify an existing Zephyr workspace, its Python virtual environment, and the venv-local west installation. Use before running Zephyr commands or creating projects that depend on an installed Zephyr workspace.
---

# Check Zephyr Environment

## Input

Obtain the Zephyr workspace path. Use `$HOME/zephyrproject` when the user followed
the default CMSIS-Zephyr installation instructions and did not choose another
location.

## Workflow

1. Determine the host operating system. Confirm that the workspace and its
   `.venv` directory exist.
2. Select the virtual-environment executables for the host:
   - Windows: `.venv/Scripts/python.exe` and `.venv/Scripts/west.exe`
   - macOS/Linux: `.venv/bin/python` and `.venv/bin/west`
3. Confirm that both executables exist.
4. Run the selected Python executable with
   `-c "import sys; print(sys.prefix); print(sys.base_prefix)"`. Confirm that
   `sys.prefix` differs from `sys.base_prefix`.
5. Run the selected `west` executable with `--version`. Do not substitute a
   global `west` executable.
6. From the Zephyr workspace, run the selected `west` executable with `topdir`.
   Confirm that it succeeds and resolves to the expected workspace.
7. From the same directory, run the selected `west` executable with
   `list zephyr -f "{abspath}"`. Confirm that it returns an existing Zephyr
   repository path.

Stop on the first failed check. Do not install, upgrade, repair, or reconfigure
Zephyr, Python, or west.

## Output

Report:

- status: `PASS` or `FAIL`;
- workspace path;
- virtual-environment path;
- selected Python and west executable paths;
- west version;
- Zephyr repository path;
- failed check and corrective reference when status is `FAIL`.

Return `PASS` only when every check succeeds.

## Resources

- [CMSIS-Zephyr setup](https://github.com/Arm-Examples/CMSIS-Zephyr)

---
name: start-zephyr-project
description: Create an initial CMSIS zephyr.csolution.yml for a board already supported by Zephyr. Use when the Zephyr and CMSIS environments, exact Zephyr board target, and matching CMSIS BSP or DFP must be verified and combined into a west-integrated CMSIS solution.
---

# Start Zephyr Project

## Workflow

Apply these sibling skills in order:

1. [check-zephyr-environment](../check-zephyr-environment/SKILL.md)
2. [check-cmsis-environment](../check-cmsis-environment/SKILL.md)
3. [identify-zephyr-board](../identify-zephyr-board/SKILL.md)
4. [identify-cmsis-board-support](../identify-cmsis-board-support/SKILL.md)
5. Create `zephyr.csolution.yml` as described below.

Accept an existing result instead of repeating a prerequisite only when its paths,
tool versions, physical board, and other relevant inputs still describe the current
request. Stop before writing project files unless both environment checks pass, the
Zephyr board matches, and CMSIS support resolves to a BSP or DFP.

## Create `zephyr.csolution.yml`

Collect:

- solution directory;
- existing Zephyr application directory;
- CMSIS compiler identifier available in the verified CMSIS-Toolbox environment.

Use `GCC` when it is available and the user has not requested another supported
compiler, matching the CMSIS-Zephyr reference project. Keep the application in its
existing location and express `app-path` relative to the solution directory.

1. Create the solution directory when it does not exist. If
   `zephyr.csolution.yml` already exists, inspect and preserve it; do not overwrite
   it without the user's approval.
2. Copy [assets/zephyr.csolution.yml](assets/zephyr.csolution.yml) to
   `<solution-directory>/zephyr.csolution.yml`.
3. Replace every template token:

   | Token | Source |
   |---|---|
   | `__CMSIS_TOOLBOX_VERSION__` | `check-cmsis-environment` result |
   | `__COMPILER__` | Selected installed CMSIS compiler identifier |
   | `__CMSIS_PACK__` | BSP pack, or DFP pack when no BSP exists |
   | `__TARGET_TYPE__` | Short, unique name derived from the CMSIS board or device |
   | `__CMSIS_BOARD__` | CMSIS board identifier from the BSP result |
   | `__CMSIS_DEVICE__` | CMSIS device identifier from the BSP or DFP result |
   | `__WEST_BOARD__` | Exact target from `identify-zephyr-board` |
   | `__APP_NAME__` | Zephyr application directory name |
   | `__APP_PATH__` | Relative path from the solution to the application |

4. For a DFP-only result, remove the complete `board: __CMSIS_BOARD__` line. Do
   not invent a CMSIS board identifier; keep the explicit `device:` line.
5. Add another `- pack:` entry only for a verified pack dependency that must be in
   the solution's pack scope. Use its exact identifier.
6. Keep the `west:` project node and the exact Zephyr target in the `west-board`
   variable. Do not create a `.cproject.yml` for the Zephyr application.
7. Confirm that no unresolved `__TOKEN__` strings remain.

The initial template provides `Debug` and `Release` build types. Add debugger,
trace, and board-specific validation only in their corresponding later workflow
steps.

## Guardrails

- Do not install, upgrade, repair, or reconfigure prerequisite tools.
- Do not require the Zephyr SDK; Keil Studio manages the development tools.
- Do not use Zephyr sysbuild; CMSIS-Toolbox west integration does not support it.
- Do not guess identifiers, paths, dependencies, or test results.
- Stop and report the producing prerequisite when a required value is absent or
  ambiguous.

## Output

Produce an initial `<solution-directory>/zephyr.csolution.yml` without unresolved
tokens. Summarize the environment versions, exact Zephyr target, fitted device,
CMSIS match type, CMSIS identifiers, and selected pack.

## Resources

- [CMSIS-Toolbox west build system integration](https://open-cmsis-pack.github.io/cmsis-toolbox/build-overview/#west-build-system-integration)
- [CMSIS-Zephyr solution example](https://github.com/Arm-Examples/CMSIS-Zephyr/blob/main/zephyr.csolution.yml)
- [CMSIS Solution extension template](https://github.com/Open-CMSIS-Pack/vscode-cmsis-solution/blob/main/templates/template.csolution.yml)

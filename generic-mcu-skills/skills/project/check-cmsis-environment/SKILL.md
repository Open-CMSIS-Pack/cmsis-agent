---
name: check-cmsis-environment
description: Verify CMSIS-Toolbox, its CMake and Ninja build environment, and the available compiler toolchains with cbuild. Use before creating or building a CMSIS solution project.
---

# Check CMSIS Environment

## Workflow

1. Run `cbuild --version`. Confirm that the command succeeds and record the
   reported CMSIS-Toolbox version. Stop when `cbuild` is unavailable.
2. Run `cbuild list environment`. Record the reported environment and confirm
   that CMake and Ninja are both found.
3. Run `cbuild list toolchains`. Record every detected compiler identifier and
   version. Use `cbuild list toolchains --verbose` when compiler path information
   is needed to understand a problem.

Return `PASS` only when `cbuild` is available, CMake and Ninja are found, and at
least one compiler toolchain is detected.

## Missing Tools

Do not install, upgrade, repair, or reconfigure development tools as part of this
skill.

- When CMake or Ninja is missing, return `FAIL` and recommend that the user set up
  the build environment outside this skill. In VS Code, use Arm Environment
  Manager; otherwise follow the manual CMSIS-Toolbox installation instructions.
- When no compiler is detected, return `FAIL` and recommend that the user set up a
  compiler outside this skill. In VS Code, use Arm Environment Manager; otherwise
  follow the CMSIS-Toolbox compiler toolchain instructions.

After the user completes the external setup, rerun all three checks. VS Code is an
optional setup method and is not required by this skill.

## Output

Report:

- status: `PASS` or `FAIL`;
- CMSIS-Toolbox version reported by `cbuild`;
- CMake and Ninja detection results from `cbuild list environment`;
- detected compiler identifiers and versions from `cbuild list toolchains`;
- each missing requirement and the applicable setup reference when status is
  `FAIL`.

## Resources

- [CMSIS-Toolbox build tools](https://open-cmsis-pack.github.io/cmsis-toolbox/build-tools/)
- [CMSIS-Toolbox installation](https://open-cmsis-pack.github.io/cmsis-toolbox/installation/)
- [CMSIS-Toolbox compiler toolchains](https://open-cmsis-pack.github.io/cmsis-toolbox/installation/#compiler-toolchains)

---
name: check-cmsis-environment
description: Verify CMSIS-Toolbox, its CMake and Ninja build environment, and the available compiler toolchains with cbuild. Use before creating or building a CMSIS solution project.
---

# Check CMSIS Environment

## Workflow

1. From the workspace root, check for `vcpkg-configuration.json` before running
   `cbuild`.
2. When the file exists, read its `requires` entries and treat their exact
   artifact versions as the intended workspace tools. Arm Tools Environment
   Manager activation is local to its VS Code instance and is not inherited by
   every shell or agent process.
3. Reproduce the already-installed workspace environment in the current process:
   - Locate each exact requested artifact in the local vcpkg artifact store. Do
     not acquire, download, install, update, or select a different version. Stop
     when an artifact is absent or when its location is ambiguous.
   - Prepend each selected tool's executable directory to `PATH`.
   - Export the CMSIS-Toolbox registration variable for each compiler using its
     family and normalized version, for example
     `GCC_TOOLCHAIN_15_3_1=<artifact>/bin` for GCC 15.3.1.
   - If the manifest selects CMSIS-Toolbox, use that exact artifact. Otherwise,
     when the workspace uses the VS Code CMSIS Solution extension, use its
     bundled `tools/cmsis-toolbox/bin` directory. Stop when multiple extension
     installations make the selection ambiguous.
   - Set `CMSIS_COMPILER_ROOT` to the `etc` directory belonging to the selected
     CMSIS-Toolbox. Do not combine a `cbuild` executable with another
     CMSIS-Toolbox installation's configuration directory.
   Keep all exported variables process-local; do not modify persistent user or
   system settings.
4. Resolve the effective `cbuild` executable and run `cbuild --version`. Confirm
   that the command succeeds and record both its path and reported
   CMSIS-Toolbox version. Stop when `cbuild` is unavailable.
5. Run `cbuild list environment`. Record the reported environment and confirm
   that CMake and Ninja are both found.
6. Run `cbuild list toolchains`. Record every detected compiler identifier and
   version. Use `cbuild list toolchains --verbose` to verify compiler paths and
   registration variables.

Return `PASS` only when `cbuild` is available, CMake and Ninja are found, and at
least one compiler toolchain is detected. When the manifest requests a compiler,
also require `cbuild list toolchains` to report that compiler and requested
version.

## Missing Tools

Do not install, upgrade, repair, or reconfigure development tools as part of this
skill.

- When CMake or Ninja is missing, return `FAIL` and recommend that the user set up
  the build environment outside this skill. In VS Code, use Arm Environment
  Manager; otherwise follow the manual CMSIS-Toolbox installation instructions.
- When no compiler is detected, return `FAIL` and recommend that the user set up a
  compiler outside this skill. In VS Code, use Arm Environment Manager; otherwise
  follow the CMSIS-Toolbox compiler toolchain instructions.
- When a requested vcpkg artifact is not already installed, return `FAIL` and ask
  the user to activate or repair the workspace environment outside this skill.

After the user completes the external setup, restart from the
`vcpkg-configuration.json` check and repeat the complete workflow. VS Code is an
optional setup method and is not required by this skill.

## Output

Report:

- status: `PASS` or `FAIL`;
- workspace `vcpkg-configuration.json` path and requested artifacts, or report
  that no manifest exists;
- process-local environment entries imported from installed artifacts;
- effective `cbuild` path and CMSIS-Toolbox version;
- CMake and Ninja detection results from `cbuild list environment`;
- detected compiler identifiers and versions from `cbuild list toolchains`;
- each missing requirement and the applicable setup reference when status is
  `FAIL`.

## Resources

- [CMSIS-Toolbox build tools](https://open-cmsis-pack.github.io/cmsis-toolbox/build-tools/)
- [CMSIS-Toolbox installation](https://open-cmsis-pack.github.io/cmsis-toolbox/installation/)
- [CMSIS-Toolbox compiler toolchains](https://open-cmsis-pack.github.io/cmsis-toolbox/installation/#compiler-toolchains)
- [Arm Tools Environment Manager](https://github.com/ARM-software/vscode-environment-manager)

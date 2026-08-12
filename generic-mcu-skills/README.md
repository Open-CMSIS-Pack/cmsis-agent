# Generic MCU Skills

This CMSIS-Agent collection contains skills for AI agents that create, bring up, debug, package, and automate microcontroller software through CMSIS and Open-CMSIS-Pack workflows.

"Generic MCU" describes reuse across multiple microcontroller families. It does not mean ecosystem-neutral. The skills use CMSIS interfaces, Open-CMSIS-Pack formats and tools, and the associated Visual Studio Code extensions. They avoid unnecessary compiler-, RTOS-, and vendor-specific behavior. Each skill identifies any additional device family, compiler, debugger, probe, RTOS, or vendor tool that it uses.

## List of skills

### Project

| Skill | Purpose |
| --- | --- |
| [`check-cmsis-environment`](skills/project/check-cmsis-environment/SKILL.md) | Verify CMSIS-Toolbox, CMake, Ninja, and available compiler toolchains. |
| [`check-zephyr-environment`](skills/project/check-zephyr-environment/SKILL.md) | Verify a Zephyr workspace, its Python virtual environment, and venv-local west installation. |
| [`identify-cmsis-board-support`](skills/project/identify-cmsis-board-support/SKILL.md) | Identify a CMSIS BSP for a board or a DFP for its fitted device. |
| [`identify-zephyr-board`](skills/project/identify-zephyr-board/SKILL.md) | Resolve a physical board to its exact Zephyr board target and fitted MCU or SoC. |
| [`start-zephyr-project`](skills/project/start-zephyr-project/SKILL.md) | Create an initial west-integrated CMSIS solution for a Zephyr-supported board. |

### Bring-up

| Skill | Purpose |
| --- | --- |
| [`debug-knowledge`](skills/bring-up/debug-knowledge/SKILL.md) | Build a documented, reviewable record of an SoC's CMSIS debug and trace topology and operating requirements. |

### Pack

| Skill | Purpose |
| --- | --- |
| [`generate-debug-description`](skills/pack/generate-debug-description/SKILL.md) | Add or review verified non-sequence CMSIS-Pack Debug Description definitions in an existing DFP PDSC. |
| [`generate-debug-sequences`](skills/pack/generate-debug-sequences/SKILL.md) | Add evidence-backed device-specific non-trace debug sequences to an existing DFP PDSC. |
| [`generate-trace-sequences`](skills/pack/generate-trace-sequences/SKILL.md) | Add modular, evidence-backed CoreSight trace sequences to an existing DFP PDSC. |

## CMSIS foundation

The collection builds on these components and tools:

- [CMSIS 6](https://github.com/ARM-software/CMSIS_6) provides the common processor, driver, and RTOS interfaces used by MCU software, including CMSIS-Core, CMSIS-Driver, and CMSIS-RTOS2.
- [Open-CMSIS-Pack](https://github.com/Open-CMSIS-Pack) provides the software-pack and project specifications used to describe devices, boards, components, projects, build configurations, and execution settings.
- [CMSIS-Toolbox](https://github.com/Open-CMSIS-Pack/cmsis-toolbox) provides the command-line tools for CMSIS solution projects and software packs.
- [CMSIS Solution](https://github.com/Open-CMSIS-Pack/vscode-cmsis-solution) provides project creation, configuration, build, run, and debug workflows in Visual Studio Code.
- [CMSIS Debugger](https://github.com/Open-CMSIS-Pack/vscode-cmsis-debugger) provides Visual Studio Code debugging for Arm Cortex-M targets and supported debug adapters.

Individual skills use the subset required for their task and list the exact tools, extensions, packs, hardware, and reference material in their prerequisites.

### Zephyr

[Zephyr](https://github.com/zephyrproject-rtos/zephyr) is supported where a task uses CMSIS interfaces, CMSIS-Pack content, CMSIS tooling, or a compatible debug and trace workflow. Zephyr-native dependencies such as `west`, CMake, Kconfig, devicetree, and the Zephyr SDK are explicit prerequisites when a skill uses them. General Zephyr workflows unrelated to CMSIS are outside this collection.

## Scope

Each skill performs one small, composable step with an observable result. Complex workflows combine several skills, with the verified output of one skill serving as input to another.

For example, a trace enablement workflow for a new device can consist of separate skills that:

1. Create or identify a test project with a working debug connection.
2. Configure the trace connection in the project metadata.
3. Verify that trace data can be collected.
4. Integrate the verified trace configuration into a Device Family Pack (DFP).

This separation makes failures easier to diagnose and enables subsequent skills to reuse verified outputs.

## Top-level directory structure

```text
generic-mcu-skills/
|-- README.md
|-- templates/
|   `-- SKILL_TEMPLATE.md
`-- skills/
```

## Catalog

Skills are grouped by their primary purpose:

| Directory | Purpose |
| --- | --- |
| `skills/project/` | Create, inspect, convert, or manage MCU projects. |
| `skills/bring-up/` | Establish basic device and board operation, including an initial debug connection. |
| `skills/debug/` | Configure and verify debug, trace, and runtime analysis. |
| `skills/pack/` | Create or update reusable device and software pack content. |
| `skills/devops/` | Create build, test, release, and CI/CD automation. |

Each skill lives in `skills/<category>/<skill-name>/` and contains a `SKILL.md`. Optional resource directories hold only the files needed to execute the skill:

```text
skill-name/
|-- SKILL.md
|-- scripts/       # Deterministic helpers, when needed
|-- references/    # Specifications or detailed guidance, when needed
`-- assets/        # Templates or files copied into generated output, when needed
```

## Using a skill

Choose the smallest skill that matches the task and give the agent the inputs named in its prerequisites. Review its declared dependencies and guardrails before execution, then use the validation procedure in the skill to check the result. Agent products discover and install skills differently; consult the documentation for the agent you use.

Generic skills share CMSIS-based procedures across device families. Device-, toolchain-, debugger-, and RTOS-specific choices come from the user, the input project, or the dependencies declared by the skill.

## Contributing a skill

1. Copy [`templates/SKILL_TEMPLATE.md`](templates/SKILL_TEMPLATE.md) to `skills/<category>/<skill-name>/SKILL.md`.
2. Use a lowercase, hyphen-separated skill name that describes one action.
3. State both what the skill does and when the agent activates it in the frontmatter `description`.
4. Treat the CMSIS foundation above as the catalog baseline. Declare every additional input and dependency, including the compiler, build system, debugger, RTOS, device family, vendor tool, or cloud service.
5. Define a strict workflow, blockers, expected output, and a repeatable validation method.
6. Add scripts, references, or assets only when they are necessary for the skill.
7. Generate `agents/openai.yaml` with the available skill tooling. Set `interface.display_name` to `CMSIS: <skill display name>` to namespace the skill in selection interfaces.
8. Test the skill with representative inputs, including more than one MCU family, toolchain, or execution environment when portability is claimed.

See the repository [CONTRIBUTING.md](../CONTRIBUTING.md) for the contribution process. The structure and template follow the design discussed in [issue #2](https://github.com/Open-CMSIS-Pack/cmsis-agent/issues/2).

## Example projects

The [Arm Examples](https://github.com/Arm-Examples) organization contains embedded example projects that provide realistic inputs and validation targets for skills. These projects are not skills: an example can intentionally demonstrate a specific device, toolchain, RTOS, or technology. Such project-specific details remain test data or explicit skill dependencies rather than catalog-wide requirements.

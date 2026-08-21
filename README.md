# CMSIS Skills

CMSIS Skills collects resources for AI agents that help developers build embedded applications using CMSIS APIs, software components, and CMSIS Solution project-based tools for build, programming, debugging, and analysis.

The repository is organized into independent top-level collections so it can grow beyond skills without mixing their documentation, templates, and resources.

## Repository contents

| Collection | Description |
| --- | --- |
| [Generic MCU Skills](generic-mcu-skills/README.md) | Reusable AI-agent skills for MCU project creation, device bring-up, debug and trace, software packs, and DevOps. The skills are applicable across MCU families and are mostly toolchain- and RTOS-agnostic. |
| [Workspace agent skills](.agents/skills/) | Repository-local skills that help AI agents maintain this workspace and keep its skills, templates, and documentation consistent. |

## Workspace agent skills

The [`.agents/skills/`](.agents/skills/) directory contains contributor tooling specific to this repository. These skills maintain CMSIS Skills content; they are not reusable MCU workflow skills from the Generic MCU Skills collection.

- [`maintain-workspace-skills`](.agents/skills/maintain-workspace-skills/SKILL.md) applies the repository and collection guidance when a skill, skill resource, template, or skill catalog documentation is created or changed. It also verifies structure, companion metadata, README entries, and available validation results.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for the contribution process and the README of a collection for its content-specific guidance.

## Related projects

The [Arm Examples](https://github.com/Arm-Examples) organization provides embedded example projects that may be used as realistic inputs and validation targets for resources in this repository.

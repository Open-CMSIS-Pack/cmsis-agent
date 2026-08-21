---
name: maintain-workspace-skills
description: Create, edit, review, or validate skills in this CMSIS Skills workspace. Use whenever a `SKILL.md`, its resources, a skill template, or skill catalog documentation changes, to apply the applicable README guidance and templates and keep README skill documentation accurate.
---

# Maintain Workspace Skills

## Target & Persona

- **Role:** Skill Maintainer and Verification Engineer
- **Objective:** Complement the task-specific authoring skill by leaving every changed workspace skill structurally compliant with its applicable README guidance and template, with accurate README documentation.

## Prerequisites & Context

- **Expected input:** A changed or proposed `SKILL.md`, its containing directory and resources, and the applicable repository and collection `README.md` files.
- **Dependencies:** `CONTRIBUTING.md` when contribution-wide rules apply; templates declared by the owning collection README; and every available authoring skill that matches the requested skill's domain or creation workflow. Use `$skill-creator` for general skill design and structural validation. Generic MCU Skills use `generic-mcu-skills/templates/SKILL_TEMPLATE.md`.
- **Portability:** Applies only to skills in this CMSIS skills workspace. Do not introduce MCU-family, device, architecture, toolchain, debugger, build-system, or RTOS assumptions unless the skill's own inputs require them.

## Execution Steps (Strict Workflow)

1. **Analysis:** Identify and use any available or explicitly invoked authoring skill that matches the requested skill's domain or creation workflow; let that skill govern the skill's substantive design. Then read every applicable `README.md` from the repository root to the changed skill's collection, plus `CONTRIBUTING.md` when relevant. Identify the owning collection, category, README-required layout, referenced templates, documented dependencies, artifact rules, and corresponding README catalog entry.
2. **Processing:** For every new Generic MCU Skill, begin from `generic-mcu-skills/templates/SKILL_TEMPLATE.md`. For an existing Generic MCU Skill, use that template as a structural check without replacing valid, task-specific content. Keep the skill focused on one composable action; use a lowercase hyphen-separated directory and frontmatter `name`; describe activation conditions in the frontmatter; declare non-CMSIS dependencies and portability; and retain strict workflow, blockers, expected output, and repeatable validation. Add resources only when necessary. Generate `agents/openai.yaml` for every new workspace skill using the available skill tooling, and set its `interface.display_name` to `CMSIS: <skill display name>` so skill-selection GUIs namespace it consistently. Inspect sibling skills only for local conventions; do not copy unsupported device, toolchain, RTOS, hardware, command, or artifact details.
3. **Validation:** Check that frontmatter contains only a lowercase hyphenated `name` and a specific `description`; no template placeholders or fabricated facts remain; all referenced relative files exist; optional resource directories are necessary; and the workflow yields observable, repeatable validation. For every new skill, check that `agents/openai.yaml` exists and its `interface.display_name` begins with `CMSIS: `. Verify the skill's path, scope, dependencies, portability, and artifacts against its owning README. Run the available skill structural validator when present.
4. **Formatting:** Check the owning collection README entry for every changed skill. Update its link, concise purpose, category, and documented artifacts when changed; add or remove entries for added or removed skills. Preserve the README's headings, table style, ordering, and terminology. Update the repository-root README only for a top-level collection addition, removal, rename, or material description change. Report the template consulted, README files updated or intentionally unchanged, and validation evidence.

## Guardrails & Constraints (Strict Rules)

- **No fabrication:** Do not invent device data, register values, configuration fields, paths, commands, templates, README requirements, or test results. Obtain them from the workspace or report that they are missing.
- **Portability:** Do not introduce a device-family, toolchain, debugger, build-system, or RTOS dependency unless the inputs require it. Identify every such dependency in the output.
- **Complementary operation:** Do not replace, duplicate, or override another applicable skill-authoring workflow. Apply its domain-specific instructions first, then add only this workspace's README, template, catalog, and validation requirements.
- **Critical blockers:** Stop and report the blocker if the changed skill has no identifiable owning README, a required template is missing, a README and template conflict, a referenced resource is unavailable, or required validation cannot run.
- **Scope:** Maintain only the changed skill and its required documentation. Preserve unrelated workspace changes and do not redesign the skill's domain behavior.
- **Tone and style:** Respond factually and directly. Omit conversational filler.

## Expected Output

Provide the compliant skill and any required README updates. Report the changed paths, template used, applicable README guidance, validation commands or checks and their results, and unresolved blockers. Do not add generic documentation files inside an individual skill folder unless its owning README requires them.

## Validation Resources

Use the applicable repository and collection `README.md` files as the rules source and each collection-declared template as the structural baseline. For Generic MCU Skills, compare the result with `generic-mcu-skills/templates/SKILL_TEMPLATE.md`. Inspect referenced paths and README links after edits. When the `$skill-creator` validator is available, run its `quick_validate.py` against the changed skill folder; otherwise, report that the structural checks were performed manually.

---
name: skill-name-id
description: Short, precise description of what this skill does and when an AI agent should activate it.
---

# Skill Name

## Target & Persona

- **Role:** [Tool Integrator, Software Developer, Verification Engineer, Security Auditor, or another appropriate role]
- **Objective:** [Describe the single verifiable outcome achieved by this skill]

## Prerequisites & Context

- **Expected input:** [Device documentation, project files, source code, logs, requirements, or other required input]
- **Dependencies:** [External specifications, tools, services, environment requirements, or related skills; write "None" when there are none]
- **Portability:** [State supported MCU families and any device-, architecture-, toolchain-, debugger-, build-system-, or RTOS-specific assumptions]

## Execution Steps (Strict Workflow)

1. **Analysis:** [State what to inspect first and which facts to extract]
2. **Processing:** [Describe the transformation or reasoning procedure]
3. **Validation:** [Describe the repeatable checks that prove the result]
4. **Formatting:** [Define how to present or store the final output]

## Guardrails & Constraints (Strict Rules)

- **No fabrication:** Do not invent device data, register values, configuration fields, paths, commands, or test results. Obtain them from authoritative inputs or report that they are missing.
- **Portability:** Do not introduce a device-family, toolchain, debugger, build-system, or RTOS dependency unless the inputs require it. Identify every such dependency in the output.
- **Critical blockers:** [List conditions that require stopping, such as missing authoritative data, an invalid project, a failed prerequisite, or unavailable hardware]
- **Scope:** Perform only the small step described by this skill. Report follow-on work separately.
- **Tone and style:** Respond factually and directly. Omit conversational filler.

## Expected Output

[Describe the exact files, changes, report, or structured response. Include a concise example or links to examples when an exact format is required.]

## Validation Resources

[List schemas, commands, tools, hardware checks, or reference results and explain how to use them. Validation must be observable and repeatable.]

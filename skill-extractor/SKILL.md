---
name: skill-extractor
description: Convert raw instructions, notes, prompts, standards, or process text into a well-structured agent skill. Use when the user wants to extract reusable instructions from any text, transform informal guidance into a portable SKILL.md, create a new skill folder from source material, or improve existing skill instructions for clarity, trigger quality, and progressive disclosure.
---

# Skill Extractor

## Purpose

Turn arbitrary instruction text into a reusable skill that another agent can follow without extra context. Preserve the source material's intent, remove noise, and format the result as a focused `SKILL.md` with valid frontmatter and actionable body instructions.

## Workflow

1. Identify the target skill name, audience, and task domain from the source text.
2. Extract durable instructions, constraints, workflows, examples, trigger phrases, and validation steps.
3. Discard temporary context, conversation filler, duplicate statements, personal commentary, and implementation history unless it affects future skill behavior.
4. Resolve contradictions by preferring explicit user instructions, then repository conventions, then the clearest repeated pattern. If contradictions materially change behavior, call them out instead of silently choosing.
5. Organize the extracted material into concise sections that an agent can execute.
6. If creating files, create a skill folder whose name exactly matches the frontmatter `name`.
7. Validate the completed skill when a validator is available.

## Output Requirements

Every generated skill must include a `SKILL.md` with only this required YAML frontmatter:

```yaml
---
name: lower-hyphen-case-name
description: Clear explanation of what the skill does and when to use it.
---
```

Write the `description` as the trigger surface. Include the task, source material type, and specific use cases that should activate the skill. Do not rely on a "when to use" section in the body because many skill systems use the description as the primary trigger surface.

Keep the body focused on reusable operating instructions. Prefer imperative, concrete guidance over generic advice.

## Extraction Rules

- Preserve domain-specific requirements, names, commands, file paths, API conventions, style rules, and validation steps.
- Convert prose into direct instructions such as "Inspect...", "Use...", "Avoid...", and "Validate...".
- Split long or dense material into sections by workflow stage, task category, or rule family.
- Keep examples only when they clarify a non-obvious pattern or prevent misinterpretation.
- Avoid restating obvious agent behavior unless the source text requires a specific policy or tradeoff.
- Do not add scripts, references, or assets unless the source text contains reusable material that is too long, deterministic, or template-like for the main `SKILL.md`.
- Do not create auxiliary documentation such as `README.md`, changelogs, install guides, or quick references inside the skill folder.

## Structure Patterns

Choose the smallest structure that makes the instructions executable:

- Use a workflow structure for step-by-step processes: `Purpose`, `Workflow`, task-specific sections, `Validation`.
- Use a standards structure for coding, writing, or design rules: `Role`, `Workflow`, rule groups, `Implementation Standard`.
- Use a task structure for broad utilities: `Purpose`, `Quick Start`, task categories, edge cases.
- Use references only when detailed material should be loaded conditionally. Link every reference directly from `SKILL.md`.

## Quality Bar

The resulting skill should be:

- Discoverable: the `name` is short and the `description` clearly explains when to use it.
- Portable: another agent can use it without the original conversation.
- Portable: it does not depend on a local machine path, a private repository layout, or one specific agent runtime unless the source text explicitly requires that dependency.
- Concise: every section changes future agent behavior.
- Faithful: extracted rules match the source text and do not invent unsupported requirements.
- Actionable: instructions are specific enough to guide real work.
- Valid: frontmatter parses as YAML and the folder name matches the skill name.

## Creating Or Updating Skill Files

When the user asks to create a repository skill, follow the repository's existing skill layout and naming conventions. Create only the files needed by the skill.

If the target repository or toolchain provides a scaffold or validator, use it. Do not hardcode personal filesystem paths or assume one specific local installation layout inside the generated skill.

If validation is unavailable, manually check:

- The directory name equals the frontmatter `name`.
- The skill name uses only lowercase letters, digits, and hyphens.
- `description` is specific and trigger-oriented.
- `SKILL.md` contains no placeholder text.
- Optional resource folders are included only when they are actually used.
- Any runtime dependency, tool dependency, or repository assumption is stated explicitly and comes from the source text rather than being inferred from the author's local environment.

## Final Response

When returning a generated skill as text, provide the complete `SKILL.md` content unless the user requested a summary. When editing files, summarize the skill created or updated, list validation performed, and mention any source ambiguities that required judgment.

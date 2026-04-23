# Agent Skills

This repository contains distributable custom agent skills for Codex and Claude. Each skill is a self-contained folder that gives an AI coding agent specialized instructions, workflows, and optional resources for a specific type of task.

## Structure

Each canonical skill lives in its own top-level directory:

```text
skill-name/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── scripts/
├── references/
└── assets/
```

Only `SKILL.md` is required. The other folders are optional and should be added only when the skill needs them.

Do not store installed copies in this repository. Claude Code installs skills into `.claude/skills/` for a project or `~/.claude/skills/` for a user, but this repository should contain the source skill folders that other people can copy, symlink, or package.

## Included Skills

- `senior-nest-js-dev`: Senior NestJS backend development standards for controllers, services, DTOs, TypeORM repositories, API documentation, response mapping, validation, and TypeScript style.

## Codex Usage

Use the top-level skill folder as the Codex skill source:

```text
senior-nest-js-dev/
└── SKILL.md
```

Codex reads the `name` and `description` frontmatter to decide when to use the skill, then loads the `SKILL.md` body when the skill is relevant.

## Claude Usage

For Claude Code, users can install a skill from this repository by copying or symlinking the top-level skill folder into their Claude skills directory.

Personal install:

```bash
mkdir -p ~/.claude/skills
cp -R senior-nest-js-dev ~/.claude/skills/senior-nest-js-dev
```

Project install:

```bash
mkdir -p .claude/skills
cp -R senior-nest-js-dev .claude/skills/senior-nest-js-dev
```

After installation, Claude Code can use the skill automatically when relevant or directly through the slash command:

```text
/senior-nest-js-dev
```

For Claude.ai, upload a ZIP that contains the skill directory, not just the files inside it:

```text
senior-nest-js-dev.zip
└── senior-nest-js-dev/
    └── SKILL.md
```

Claude.ai requires the directory name to match the `name` field in `SKILL.md`. Keep the skill description short and specific; Claude.ai has a shorter description limit than the general Agent Skills specification.

Create the ZIP from the repository root:

```bash
zip -r senior-nest-js-dev.zip senior-nest-js-dev
```

## Validation

Validate a skill after creating or editing it:

```bash
python3 /Users/marinluic/.codex/skills/.system/skill-creator/scripts/quick_validate.py /path/to/skill-folder
```

For Claude compatibility, also check:

- The skill directory name matches the `name` field.
- The `name` uses only lowercase letters, numbers, and hyphens.
- The `description` explains what the skill does and when to use it.
- `SKILL.md` stays focused; move large details into `references/`.

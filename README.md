# Java Style

`java-style` is a team-oriented Java backend skill for JDK 21 and Spring Boot 3.5.x projects.

It is designed for:

- code generation
- code review
- lightweight refactoring
- enforcing internal engineering conventions

## What It Covers

- naming and layered design
- logging and exception handling
- Lombok usage
- validation and controller boundaries
- modern JDK 21 and Spring Boot 3.5.x practices
- readable structure and maintainable backend code
- complete Chinese Javadoc comments for delivered code

## Directory Structure

```text
.
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    ├── default-style.md
    ├── review-checklist.md
    └── customization-notes.md
```

## Structure

- [SKILL.md](SKILL.md): main trigger rules, workflow, and output expectations
- [agents/openai.yaml](agents/openai.yaml): UI metadata
- [references/](references/): detailed style, review, and customization rules

## Core References

- `references/default-style.md`
- `references/review-checklist.md`

## Install And Verify

Use these commands to install or verify the skill from the repository:

```bash
npx skills add yeluod/java-style --list
npx skills add yeluod/java-style --skill java-style
npx skills add https://github.com/yeluod/java-style --skill java-style
```

If you want to target Codex explicitly:

```bash
npx skills add yeluod/java-style --skill java-style -a codex -g
```

Verify discovery again after installation:

```bash
npx skills add yeluod/java-style --list
```

## Publish To skills.sh

1. Push the repository to a public GitHub repository.
2. Validate discovery with the CLI:

```bash
npx skills add yeluod/java-style --list
npx skills add yeluod/java-style --skill java-style
npx skills add https://github.com/yeluod/java-style --skill java-style

npx skills add yeluod/java-style --skill java-style -a codex -g

npx skills add yeluod/java-style --list
```

3. Confirm the repository or skill appears on [skills.sh](https://skills.sh/).

## Maven Note

When running Maven commands in internal projects, prefer:

```bash
mvn -s ~/.m2/settings.xml -q test
```

If `~/.m2/settings.xml` is unavailable and private repositories are required, provide the correct settings file path before build or test execution.

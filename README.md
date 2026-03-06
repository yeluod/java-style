# Java Style

`java-style` is a team-oriented Java backend skill for JDK 21 and Spring Boot 3.5.x projects.

It is designed for:

- code generation
- code review
- lightweight refactoring
- enforcing internal engineering conventions
- prioritizing reuse of internal base components

## What It Covers

- naming and layered design
- logging and exception handling
- Lombok usage
- validation and controller boundaries
- modern JDK 21 and Spring Boot 3.5.x practices
- reuse of shared foundation, toolkit, web, validation, framework, and business components

## Structure

- [SKILL.md](SKILL.md): main trigger rules, workflow, and output expectations
- [agents/openai.yaml](agents/openai.yaml): UI metadata
- [references/](references/): detailed component and style rules

## Core References

- `references/default-style.md`
- `references/review-checklist.md`
- `references/internal-components.md`
- `references/foundation-components.md`
- `references/toolkit-components.md`
- `references/web-components.md`
- `references/validation-components.md`
- `references/framework-components.md`
- `references/business-components.md`

## Maven Note

For internal projects with private dependencies, Maven commands should prefer:

```bash
~/.m2/settings.xml
```

If that file is unavailable and private repositories are required, provide the correct Maven settings path before running build or test commands.

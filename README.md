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
    ├── internal-components.md
    ├── foundation-components.md
    ├── toolkit-components.md
    ├── web-components.md
    ├── validation-components.md
    ├── framework-components.md
    ├── business-components.md
    └── customization-notes.md
```

## Structure

- [SKILL.md](SKILL.md): main trigger rules, workflow, and output expectations
- [agents/openai.yaml](agents/openai.yaml): UI metadata
- [references/](references/): detailed component and style rules

## Quick Use

### 1) Local structure validation

```bash
test -f SKILL.md
test -f agents/openai.yaml
test -f references/default-style.md
test -f references/review-checklist.md
test -f references/internal-components.md
```

### 2) Local rule discovery check

```bash
rg -n "JDK 21|Spring Boot 3.5.x|internal base components|BusinessException|PageReq|PageRes|Result" SKILL.md references
```

### 3) Maven note for internal repositories

When running Maven commands in internal projects, prefer:

```bash
mvn -s ~/.m2/settings.xml -q test
```

If `~/.m2/settings.xml` is unavailable and private repositories are required, provide the correct settings file path before build or test execution.

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

## Current Validation Scope

This repository currently provides:

- skill metadata
- UI metadata
- detailed reference rules

It does not yet include:

- `evals/evals.json`
- automated review assertions
- helper scripts comparable to `java-doc/scripts/run_eval.sh`

If needed, those can be added later as the next hardening step.

---
name: java-style
description: Apply this skill whenever the user asks to generate Java backend code, review Java code, refactor Java code, clean up Spring Boot services, improve API design, fix logging or exception handling, align layered architecture, or enforce team engineering conventions in a JDK 21 and Spring Boot 3.5.x project. Use it for business-facing Spring Boot backend work even when the user only asks for "optimize", "rewrite", "review", "adjust style", or "make it more maintainable". Focus on readable design, clean layering, maintainability, modern JDK 21 plus Spring Boot 3.5.x practices, and fully compliant Chinese Javadoc comments in delivered code.
metadata:
  short-description: JDK 21 Spring Boot backend engineering style
---

# Java Backend Style

Use this skill for Spring Boot backend business projects that standardize on JDK 21 and Spring Boot 3.5.x.

This skill is not a formatter-only skill. It is for:

- generating code that matches the team's engineering conventions
- reviewing code for correctness, maintainability, and framework alignment
- performing lightweight refactors without changing business behavior
- improving structure, readability, and consistency in Java backend code

## Technology Baseline

Treat these constraints as the default unless the user explicitly says otherwise:

- JDK 21
- Spring Boot 3.5.x
- Modern Java and modern Spring Boot practices
- Lombok is preferred when it improves readability and keeps object construction explicit
- Slf4j logging with placeholder-based output

For Maven-based projects with private internal dependencies:

- prefer the user's Maven settings file at `~/.m2/settings.xml`
- if Maven execution depends on private repositories and `~/.m2/settings.xml` is not available, ask the user for the correct settings file path before proceeding

Do not default to outdated Java 8 era boilerplate when a clearer JDK 21 or modern Spring Boot style is available.
Do not force new syntax when it makes the code harder to read.

## When This Skill Must Influence the Result

Use this skill across three task types:

1. Code generation
2. Code review
3. Code refactoring

In all three cases, check both general Java quality and team-specific engineering consistency:

- naming clarity
- class and method responsibilities
- Spring layering and dependency injection style
- logging format
- exception boundaries
- Lombok usage
- modern JDK 21 / Spring Boot 3.5.x practices
- readability and maintainability
- complete Chinese Javadoc compliance in delivered code

## Required Workflow

Follow this sequence unless the user asks for a narrower task.

### 1. Classify the task

Decide whether the request is primarily:

- code generation
- code review
- code refactoring

If the task mixes these, keep review first, then propose or apply changes.

### 2. Read local code and conventions first

Inspect nearby classes, package structure, naming, DTO models, exception hierarchy, API response style, and logging style before editing.

Preserve established project conventions when they are clear and not harmful.

### 3. Check for local convention alignment before designing code

Before creating new DTOs, response models, helper classes, enum helpers, or generic utility abstractions:

- search nearby packages for existing project patterns
- prefer consistency with the local module when a convention is already established
- avoid introducing abstractions that are broader than the problem requires

If the user provides project-specific conventions, shared model definitions, or nearby examples, treat them as higher priority than generic public best practices.

### 4. Apply the backend style rules

Use [references/default-style.md](references/default-style.md) for generation and refactoring.

The baseline rules cover:

- naming and layering
- abstraction quality
- Lombok usage
- logging
- Spring practices
- exception and null handling
- modern JDK 21 style
- method and class size discipline
- Chinese Javadoc and comment discipline

### 5. Use the review checklist when reviewing or auditing code

For review tasks, use [references/review-checklist.md](references/review-checklist.md).

Always review in this order:

1. correctness and regression risk
2. layering, API design, and engineering consistency
3. abstraction quality
4. logging, exception handling, and maintainability
5. style and readability

Do not inflate style nits into major defects when behavior is correct.
Do call out engineering inconsistency when code introduces unnecessary abstractions or breaks local conventions without justification.

### 6. Escalate refactoring signals when present

Actively point out when you see:

- long methods
- oversized classes
- deep nesting
- duplicated logic
- mixed responsibilities
- confusing names
- repeated null checks or defensive boilerplate

For lightweight refactors, preserve behavior and improve naming, structure, and consistency.
If a refactor would change behavior, state that clearly.

## Output Rules

Tailor the response to the task type, but default to this structure when reviewing or refactoring user-provided code:

1. Problem list
2. Cause analysis
3. Concrete modification suggestions
4. Updated code when useful or requested
5. Rule summary

When reporting issues:

- group findings by category such as naming, abstraction, logging, Spring layering, exception handling, or maintainability
- explain why each issue matters
- state the expected fix, not just that the code "can be optimized"

When generating code:

- produce code that is ready to drop into a JDK 21 Spring Boot 3.5.x project
- match existing project conventions when they are clear
- keep naming explicit and business-oriented
- keep structure maintainable instead of merely concise
- provide complete Chinese Javadoc comments for public types and methods, with `@param`, `@return`, `@throws`, and other tags matching the signature

When refactoring code:

- do not change business semantics unless the user asked for that
- improve naming, extraction, structure, and consistency
- avoid style-only churn unrelated to the task
- if you return updated code, keep the Chinese Javadoc comments complete and consistent with the current signatures

## Non-Negotiable Rules

- Prefer constructor injection. Do not use field injection.
- Use Slf4j placeholders. Do not use string concatenation in logs.
- Do not use `System.out.println` or `printStackTrace()`.
- Keep Controller, Service, Repository, DTO, VO, and Entity responsibilities distinct.
- Provided code must include complete Chinese Javadoc comments that conform to signature and tag rules.
- Use modern JDK 21 and Spring Boot 3.5.x idioms when they improve clarity.
- Do not use modern syntax as a gimmick.

## Reference Map

- Detailed backend conventions: [references/default-style.md](references/default-style.md)
- Review and audit workflow: [references/review-checklist.md](references/review-checklist.md)
- Project-specific override notes: [references/customization-notes.md](references/customization-notes.md)

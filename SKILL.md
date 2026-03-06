---
name: java-style
description: Apply this skill whenever the user asks to generate Java backend code, review Java code, refactor Java code, clean up Spring Boot services, improve API design, fix logging or exception handling, align layered architecture, or enforce team engineering conventions in a JDK 21 and Spring Boot 3.5.x project. Use it for business-facing Spring Boot backend work even when the user only asks for "optimize", "rewrite", "review", "adjust style", or "make it more maintainable". Prioritize reuse of internal base components before proposing new DTOs, paging models, result wrappers, exceptions, enums, or utility abstractions.
metadata:
  short-description: JDK 21 Spring Boot backend engineering style
---

# Java Backend Style

Use this skill for Spring Boot backend business projects that standardize on JDK 21 and Spring Boot 3.5.x.

This skill is not a formatter-only skill. It is for:

- generating code that matches the team's engineering conventions
- reviewing code for correctness, maintainability, and framework alignment
- performing lightweight refactors without changing business behavior
- enforcing reuse of internal base components before inventing new abstractions

## Technology Baseline

Treat these constraints as the default unless the user explicitly says otherwise:

- JDK 21
- Spring Boot 3.5.x
- Modern Java and modern Spring Boot practices
- Lombok is preferred when it improves readability and keeps object construction explicit
- Slf4j logging with placeholder-based output

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
- reuse of internal base components

## Required Workflow

Follow this sequence unless the user asks for a narrower task.

### 1. Classify the task

Decide whether the request is primarily:

- code generation
- code review
- code refactoring

If the task mixes these, keep review first, then propose or apply changes.

### 2. Read local code and conventions first

Inspect nearby classes, package structure, naming, DTO models, result wrappers, paging models, exception hierarchy, and logging style before editing.

Preserve established project conventions when they are clear and not harmful.

### 3. Check for internal base component reuse before designing code

Before creating new DTOs, paging objects, result wrappers, range objects, enum helpers, tuple objects, or generic utility abstractions:

- search for existing internal base components
- prefer reuse or extension of those components
- avoid redefining models that already exist with the same semantics

Read [references/internal-components.md](references/internal-components.md) whenever the task touches shared models, pagination, result envelopes, exceptions, enums, tuple-like models, SPI, or common DTO containers.
Read [references/toolkit-components.md](references/toolkit-components.md) whenever the task touches null-safe value handling, stream helpers, page interval calculation, enum parsing, tree building, JSON serialization, HTTP invocation, Excel import/export, dictionary assembly, or exception utility methods.
Read [references/foundation-components.md](references/foundation-components.md) whenever the task touches `Result`, `PageReq`, `PageRes`, `BaseDTO`, shared range objects, key-value models, enums, tuples, SPI contracts, or exception base classes.
Read [references/web-components.md](references/web-components.md) whenever the task touches controller return style, global exception handling, Jackson/Web binding, application startup, WebFlux/Servlet configuration, lifecycle hooks, or XSS filtering.
Read [references/validation-components.md](references/validation-components.md) whenever the task touches request validation, enum field validation, collection element count validation, phone/证件/统一社会信用代码校验, or validation exception responses.
Read [references/framework-components.md](references/framework-components.md) whenever the task touches asynchronous execution, thread factory construction, MDC/request-context propagation, request-body repeatable reading, or environment-based conditions.
Read [references/business-components.md](references/business-components.md) whenever the task touches business exception boundaries, shared business properties, project/application/client metadata, or business auto-configuration.

If the user provides source code, jar documentation, shared model definitions, or examples from the internal component library, treat them as higher priority than generic public best practices.

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

### 5. Use the review checklist when reviewing or auditing code

For review tasks, use [references/review-checklist.md](references/review-checklist.md).

Always review in this order:

1. correctness and regression risk
2. internal component reuse and engineering consistency
3. layering, API design, and abstraction quality
4. logging, exception handling, and maintainability
5. style and readability

Do not inflate style nits into major defects when behavior is correct.
Do call out engineering inconsistency when code bypasses internal base components without justification.

### 6. Escalate refactoring signals when present

Actively point out when you see:

- long methods
- oversized classes
- deep nesting
- duplicated logic
- mixed responsibilities
- confusing names
- repeated null checks or defensive boilerplate

For lightweight refactors, preserve behavior and improve naming, structure, and reuse.
If a refactor would change behavior, state that clearly.

## Output Rules

Tailor the response to the task type, but default to this structure when reviewing or refactoring user-provided code:

1. Problem list
2. Cause analysis
3. Concrete modification suggestions
4. Updated code when useful or requested
5. Rule summary

When reporting issues:

- group findings by category such as naming, abstraction, logging, Spring layering, exception handling, or internal component reuse
- explain why each issue matters
- state the expected fix, not just that the code "can be optimized"
- explicitly call out when internal base components should replace custom implementations

When generating code:

- produce code that is ready to drop into a JDK 21 Spring Boot 3.5.x project
- prefer existing project abstractions and internal components
- keep naming explicit and business-oriented
- keep structure maintainable instead of merely concise

When refactoring code:

- do not change business semantics unless the user asked for that
- improve naming, extraction, structure, reuse, and consistency
- avoid style-only churn unrelated to the task

## Non-Negotiable Rules

- Prefer constructor injection. Do not use field injection.
- Use Slf4j placeholders. Do not use string concatenation in logs.
- Do not use `System.out.println` or `printStackTrace()`.
- Keep Controller, Service, Repository, DTO, VO, and Entity responsibilities distinct.
- Prefer existing internal base components over new business-side duplicates.
- Prefer existing toolkit utilities over ad hoc JSON, HTTP, Excel, enum, tree, pagination-range, or null-handling helpers.
- Prefer the shared web and validation exception handling flow over controller-local try/catch result wrapping.
- Prefer shared validation annotations and shared exception types over ad hoc manual parameter checking.
- Use modern JDK 21 and Spring Boot 3.5.x idioms when they improve clarity.
- Do not use modern syntax as a gimmick.

## Reference Map

- Detailed backend conventions: [references/default-style.md](references/default-style.md)
- Review and audit workflow: [references/review-checklist.md](references/review-checklist.md)
- Internal component reuse rules: [references/internal-components.md](references/internal-components.md)
- Foundation component rules: [references/foundation-components.md](references/foundation-components.md)
- Toolkit capability reuse rules: [references/toolkit-components.md](references/toolkit-components.md)
- Web component rules: [references/web-components.md](references/web-components.md)
- Validation component rules: [references/validation-components.md](references/validation-components.md)
- Framework component rules: [references/framework-components.md](references/framework-components.md)
- Business component rules: [references/business-components.md](references/business-components.md)
- Project-specific override notes: [references/customization-notes.md](references/customization-notes.md)

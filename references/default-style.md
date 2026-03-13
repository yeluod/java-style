# Backend Coding Rules

Use these rules for code generation and behavior-preserving refactoring in JDK 21 and Spring Boot 3.5.x backend projects.

## 1. Core Principles

- Optimize for readability, maintainability, and engineering consistency.
- Match local project conventions first when they are clear and healthy.
- Prefer straightforward code over clever code.
- Improve structure, not just formatting.
- For Maven execution in internal projects, prefer `~/.m2/settings.xml` before falling back to any other repository configuration.

## 2. Naming

- Use `PascalCase` for classes and `camelCase` for methods and variables.
- Use `UPPER_SNAKE_CASE` for constants.
- Use domain-specific names with business meaning.
- Do not use meaningless names such as `a`, `b`, `tmp`, `data`, `obj`, or `resultDto1`.
- Make layered roles visible in names: `OrderController`, `OrderService`, `OrderRepository`, `OrderQueryDTO`, `OrderDetailVO`.
- Use predicate-style names for booleans such as `enabled`, `deleted`, `matched`, `shouldRetry`.
- Names should reflect responsibility and abstraction boundaries, not just syntax conventions.

## 3. Layering And Class Design

- Keep Controller, Service, Repository, DTO, VO, and Entity boundaries clear.
- Controllers should coordinate request handling, validation entry, and response conversion. Do not put business orchestration there.
- Services should hold business logic and transaction boundaries where appropriate.
- Repositories or data-access components should focus on persistence concerns only.
- Do not mix DTOs, VOs, and Entities casually.
- Keep classes focused on one reason to change.
- Extract shared business logic when duplication is real, but do not create meaningless abstraction layers.

## 4. Method Structure

- Keep methods focused on one responsibility.
- Prefer guard clauses to reduce nesting.
- Split methods when they become long, branch-heavy, or mix validation, orchestration, persistence, and mapping concerns.
- Keep parameter lists readable. If several parameters always travel together, prefer a dedicated request object.
- Make mutations and side effects obvious in method names.

## 5. Formatting

- Use 4-space indentation.
- Separate logical blocks with blank lines where it improves scanning.
- Keep long statements under control; do not let chained calls become visually noisy.
- Keep braces, wrapping, and chaining style consistent inside the file.
- Output code in a format that can be dropped into the project with minimal cleanup.

## 6. Lombok

- Prefer Lombok when it removes boilerplate without hiding object semantics.
- Commonly preferred annotations include `@Getter`, `@Setter`, `@Builder`, `@NoArgsConstructor`, `@AllArgsConstructor`, `@RequiredArgsConstructor`, and `@Slf4j`.
- Prefer `@RequiredArgsConstructor` with `final` dependencies for Spring beans.
- Use `@Builder` when object construction is otherwise noisy or error-prone.
- Do not use Lombok in ways that make mutable state or lifecycle constraints unclear.
- For DTOs or command objects, `@Data` is often too broad; prefer targeted annotations when equality or mutability needs tighter control.
- For entities, be careful with generated `equals`, `hashCode`, and `toString`.
- For configuration or immutable carrier types, use the simplest annotation set that keeps intent obvious.

## 7. Logging

- Use Slf4j consistently.
- Keep log messages concise and action-oriented.
- Use placeholder formatting only. Do not concatenate strings.
- Follow the team style: `log.info("action description -> [{}]", value)`.
- For multiple values, preserve the same shape, for example `log.info("create order -> [tenantId={}, orderId={}]", tenantId, orderId)`.
- Use `info`, `warn`, and `error` consistently with the same formatting discipline.
- Include business identifiers and key context, not redundant prose.
- Do not use `System.out.println`.
- Do not use `printStackTrace()`.

## 8. Spring Boot Practices

- Target Spring Boot 3.5.x style by default.
- Prefer constructor injection. Do not use field injection.
- Use Spring stereotypes intentionally and keep layering explicit.
- Keep request validation at the application boundary.
- Align exception translation, response wrapping, and configuration style with current project conventions.
- Prefer modern Spring Boot configuration patterns over legacy verbose setup.

## 9. Exceptions And Null Handling

- Do not swallow exceptions.
- If an exception is caught, either recover intentionally or rethrow with added business context.
- Avoid duplicate log-and-throw patterns unless the log adds context unavailable upstream.
- Use specific exception types when the project defines them.
- Keep null handling simple and explicit.
- Do not return `null` collections; return empty collections instead.
- Use `Optional` mainly for return values and boundary semantics, not as a default field or parameter type.
- Use streams when they improve clarity, but do not force `Stream` pipelines when a loop is easier to read.

## 10. Modern JDK 21 Style

- Prefer modern Java constructs when they clearly improve readability.
- Consider `record` for simple immutable carriers when it fits the project and does not conflict with framework or serialization needs.
- Use switch expressions, pattern matching, `Stream#toList()`, text blocks, and other modern APIs when they simplify the code.
- Do not use advanced language features as a display of cleverness.
- If an older pattern is clearly more verbose and less expressive, prefer the modern replacement.
- Prefer standard library and well-established project utilities before adding local helper logic with the same responsibility.

## 11. Comments And Documentation

- Comment intent, invariants, and non-obvious tradeoffs.
- Do not comment what the code already says plainly.
- All code delivered by this skill must use Chinese comments.
- Add complete Chinese Javadoc to public APIs, extension points, and behavior with non-obvious contracts.
- Keep Javadoc tags fully aligned with the declaration signature, including `@param`, `@return`, `@throws`, and other necessary tags.
- If a method or type is returned as part of the answer, do not omit Javadoc just because the snippet is short.
- Keep documentation aligned with actual parameters, returns, and thrown exceptions.

## 12. Refactoring Signals

Treat these as prompts to refactor or at least call out:

- methods that are too long
- classes with multiple responsibilities
- repeated mapping or validation logic
- deeply nested conditionals
- duplicated null checks
- vague naming
- business logic leaking into controllers or DTOs

## 13. Testing Expectations

- Add or adjust tests when behavior, branch coverage, or edge handling changes materially.
- Prefer test names that describe scenario and expected result.
- Cover empty input, invalid input, boundary values, and failure paths when relevant.
- If a refactor is behavior-preserving but structurally meaningful, keep or expand tests around the affected branch points.

## 14. Override Order

- User-provided project conventions override this file.
- Clear local project conventions override baseline defaults when they remain readable and correct.

# Backend Review Checklist

Use this checklist for code review tasks in JDK 21 and Spring Boot 3.5.x backend projects.

Review in this order.

## 1. Correctness And Regression Risk

Check for:

- incorrect branching or state transitions
- missed null handling or invalid assumptions
- broken invariants
- transaction boundary mistakes
- request or response contract changes
- persistence mapping changes
- serialization or deserialization impact
- behavior drift hidden inside refactors

## 2. Layering And API Design

Check for:

- controller logic that should live in services
- repository concerns leaking into services or controllers
- DTO, VO, and Entity mixing
- weak naming
- confusing public method contracts
- missing validation at boundaries
- classes or methods with multiple responsibilities

## 3. Spring And Dependency Injection Style

Check for:

- field injection
- inconsistent component annotations
- legacy Spring patterns where modern Spring Boot 3.5.x style is expected
- configuration classes or bean wiring that are more complex than necessary
- controller-local exception translation or validation handling that breaks existing project conventions

## 4. Logging And Exceptions

Check for:

- string concatenation in logs
- log messages that do not follow the `action -> [value]` pattern
- missing business identifiers in important logs
- swallowed exceptions
- duplicated log-and-throw patterns without added value
- weak exception types or low-signal messages
- raw runtime exceptions used where the project already defines a clearer domain exception type
- `System.out.println` or `printStackTrace()`

## 5. Readability, Abstraction, And Maintainability

Check for:

- long methods
- oversized classes
- deep nesting
- duplicated logic
- poor extraction boundaries
- modern syntax used in a way that hurts readability
- old verbose patterns that should reasonably be updated for JDK 21
- missing, incomplete, or signature-mismatched Chinese Javadoc on delivered public types and methods

## 6. Performance And Concurrency

Check for:

- repeated database or remote calls
- unnecessary allocations
- avoidable repeated traversals
- accidental quadratic behavior
- unsafe shared mutable state
- incomplete synchronization or visibility assumptions
- asynchronous execution that loses MDC or request-context propagation

Raise these only when there is a concrete risk.

## 7. Tests

Check for:

- missing tests for new branches or failure paths
- missing compatibility coverage for refactors
- poor scenario naming
- untested boundary or empty-input behavior

## Reporting Rules

When reporting findings:

- lead with the concrete engineering risk
- mention the violated rule category
- point to the smallest relevant code region
- distinguish must-fix items from optional cleanup
- if there are no meaningful defects, say so plainly

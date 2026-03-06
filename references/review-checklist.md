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

## 2. Internal Base Component Reuse

Check whether the implementation is bypassing shared foundations.

Look for:

- custom paging models where `PageReq` or `PageRes` should be used
- custom result wrappers or response envelopes where a shared wrapper exists
- duplicate `BaseDTO`-like containers
- custom `start/end` carriers instead of shared range objects
- project-local `IdName`, `KeyValue`, tuple, enum, exception, or SPI abstractions that duplicate existing internal types

Flag these as engineering consistency issues, not optional style cleanup.
Also check whether the code duplicates existing toolkit utilities for JSON, HTTP, Excel, enum conversion, page interval calculation, stream entry, or exception helper logic.

## 3. Layering And API Design

Check for:

- controller logic that should live in services
- repository concerns leaking into services or controllers
- DTO, VO, and Entity mixing
- weak naming
- confusing public method contracts
- missing validation at boundaries
- classes or methods with multiple responsibilities

## 4. Spring And Dependency Injection Style

Check for:

- field injection
- inconsistent component annotations
- legacy Spring patterns where modern Spring Boot 3.5.x style is expected
- configuration classes or bean wiring that are more complex than necessary
- controller-local exception translation or validation handling that duplicates shared web/validation infrastructure

## 5. Logging And Exceptions

Check for:

- string concatenation in logs
- log messages that do not follow the `action -> [value]` pattern
- missing business identifiers in important logs
- swallowed exceptions
- duplicated log-and-throw patterns without added value
- weak exception types or low-signal messages
- raw runtime exceptions used where `BusinessException` or another shared exception type should be preferred
- `System.out.println` or `printStackTrace()`

## 6. Readability, Abstraction, And Maintainability

Check for:

- long methods
- oversized classes
- deep nesting
- duplicated logic
- poor extraction boundaries
- modern syntax used in a way that hurts readability
- old verbose patterns that should reasonably be updated for JDK 21

## 7. Performance And Concurrency

Check for:

- repeated database or remote calls
- unnecessary allocations
- avoidable repeated traversals
- accidental quadratic behavior
- unsafe shared mutable state
- incomplete synchronization or visibility assumptions
- asynchronous execution that loses MDC or request-context propagation despite shared framework wrappers

Raise these only when there is a concrete risk.

## 8. Tests

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
- explain the reuse path when the issue is failure to adopt an internal base component
- if there are no meaningful defects, say so plainly

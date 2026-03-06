# Validation Component Reuse

This file summarizes reusable capabilities extracted from `backend-components-validation`.

Use it whenever the task touches request parameter validation, custom constraints, enum validation, collection validation, identifier validation, or validation exception formatting.

## Positioning

`backend-components-validation` is the shared validation layer for Spring web entry points.

It provides reusable annotations and a centralized validation exception handler. Business modules should prefer these capabilities over ad hoc controller checks.

## Shared Validation Capabilities

Examples from the module:

- `EnumNameValid`
- `DistinctElements`
- `MinElements`
- `MaxElements`
- `PhoneValid`
- `IdCardValid`
- `CreditCodeValid`
- `DecimalValid`

Guidance:

- Prefer shared validation annotations over manual `if` checks in controllers and services when the constraint already exists.
- Use `@Valid` and `@Validated` with these shared constraints to keep request validation declarative.
- Prefer `EnumNameValid` for request fields that should map to shared enum names instead of custom parsing or manual whitelist checks.

## Validation Exception Handling

Key types:

- `ValidationExceptionHandler`
- `ValidationAutoConfiguration`

Guidance:

- Let shared validation exception handling convert binding and constraint failures into the standard `Result` response.
- Do not duplicate controller advice for common validation failures unless the project explicitly overrides the shared response format.

## Review Checklist Additions

When reviewing code, explicitly check whether the implementation:

- uses manual parameter checking where a shared validation annotation already exists
- parses enum-like request strings manually instead of using shared enum validation
- duplicates validation exception handling already provided by the shared module

## Generation Guidance

When generating request models:

- put validation rules on request objects and entry-point parameters
- prefer shared validation annotations before inventing new ones
- keep validation at the boundary and avoid scattering the same checks through service methods

# Business Component Reuse

This file summarizes reusable capabilities extracted from `backend-components-business`.

Use it whenever the task touches business exception boundaries, shared business metadata, project/application/client configuration, or business auto-configuration.

## Positioning

`backend-components-business` is the shared business support layer.

It currently provides standardized business exception handling and access to common business configuration properties.

## Shared Business Capabilities

### `BusinessException`

Source capability: `com.ygzy.components.business.exception.BusinessException`

Guidance:

- Prefer `BusinessException` for business-rule failures in application and domain services.
- Prefer the `BaseResultEnum` constructor when a standardized code-message pair exists.
- Do not throw `ToolkitException` or unrelated raw `RuntimeException` for ordinary business rule violations.

### `BusinessProperties` and `BusinessHelper`

Source capabilities:

- `BusinessProperties`
- `BusinessHelper`

These provide shared access to project, application, client, API domain, and API version metadata.

Guidance:

- Prefer shared business properties over scattering equivalent config keys across modules.
- If code needs project/application/client metadata, check `BusinessProperties` first before inventing duplicate configuration carriers.

### Auto-configuration

Key types:

- `BusinessAutoConfiguration`
- `BusinessConfiguration`

Guidance:

- Prefer the shared business auto-configuration path instead of custom bootstrap wiring for `BusinessProperties`.

## Review Checklist Additions

When reviewing code, explicitly check whether the implementation:

- throws raw runtime exceptions for business-rule failures that should be `BusinessException`
- duplicates project/application/client config holders already covered by `BusinessProperties`

## Generation Guidance

When generating business-service code:

- use `BusinessException` for business failures
- use shared business properties if the logic depends on project/application/client metadata

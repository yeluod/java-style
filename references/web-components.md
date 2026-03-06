# Web Component Reuse

This file summarizes reusable capabilities extracted from `backend-components-web`.

Use it whenever the task touches controllers, global exception handling, request/response binding, Jackson configuration, startup entry points, lifecycle hooks, or XSS-related web behavior.

## Positioning

`backend-components-web` is the shared web integration layer.

It establishes the project's controller return style, exception-to-result conversion, JSON binding defaults, startup conventions, and common servlet/web configuration.

## Core Conventions

### Unified exception handling

Key types:

- `WebExceptionHandler`
- `BaseExceptionHandler`

Guidance:

- Prefer global exception handlers over controller-local try/catch that only converts exceptions into `Result.fail(...)`.
- Business exceptions that extend `BaseException` will already be translated into the shared `Result` structure.
- In review, flag controller methods that manually catch broad exceptions just to build the same standard error response.

### Jackson and request binding

Key types:

- `JacksonConfiguration`
- custom editors for `LocalDate`, `LocalDateTime`, `LocalTime`, numeric types, and `BigDecimal`

Guidance:

- Prefer the shared Jackson and binder configuration over creating local controller-level binder customization unless the need is highly specific.
- Avoid local `ObjectMapper` tuning in web modules when the shared web configuration already defines the serialization baseline.

### Startup entry point

Key type:

- `RunApplication`

Guidance:

- Prefer `RunApplication` for Spring Boot startup when the project expects its default properties, startup logging, locale handling, PID integration, and graceful shutdown defaults.
- Do not hand-roll a custom `SpringApplication` bootstrap flow unless project startup requirements truly differ.

## Other Shared Web Capabilities

From module structure and configuration:

- Servlet and WebFlux auto-configuration
- lifecycle management integration
- XSS filtering support

Guidance:

- When the requirement is already handled by shared web auto-configuration, do not re-register duplicate filters, exception handlers, or lifecycle runners in each business module.

## Review Checklist Additions

When reviewing code, explicitly check whether the implementation:

- returns non-standard response envelopes instead of `Result`
- manually handles exceptions in controllers that should flow to `WebExceptionHandler`
- duplicates JSON converter or binder configuration
- introduces a custom startup pattern instead of `RunApplication`

## Generation Guidance

When generating controller code:

- return `Result<T>` consistently
- keep controller methods thin and let shared exception handling deal with failures
- rely on shared Jackson and web configuration unless the user provides a stronger local convention

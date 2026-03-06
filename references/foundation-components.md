# Foundation Component Reuse

This file summarizes reusable capabilities extracted from `backend-components-foundation`.

Use it whenever the task touches shared DTO containers, pagination, result wrappers, key-value models, range objects, tuples, enums, constants, SPI contracts, or base exception types.

## Positioning

`backend-components-foundation` is the core shared model layer.

Business modules should build on these contracts instead of redefining equivalent request wrappers, response wrappers, enums, tuple objects, or generic carrier models.

## Core Shared Models

### `Result<T>`

Source capability: `com.ygzy.components.foundation.result.Result`

Use `Result` as the default controller-facing response envelope.

Guidance:

- Prefer `Result.success(...)` and `Result.fail(...)` instead of project-local `Response`, `CommonResponse`, or `ApiResult` wrappers.
- Keep controller signatures aligned with `Result<T>` when the project already uses this contract.
- Prefer centralized exception handlers to convert exceptions into `Result.fail(...)` rather than wrapping every controller action in local try/catch blocks.

### `PageReq<T>` and `PageRes<T>`

Source capabilities:

- `com.ygzy.components.foundation.domain.page.PageReq`
- `com.ygzy.components.foundation.domain.page.PageRes`

Use them as the default paging request and response contracts.

Guidance:

- Do not define business-local paging wrapper classes if `PageReq` and `PageRes` already satisfy the contract.
- `PageReq` already carries validation rules for `current`, `size`, and `condition`.
- `PageRes` already supports converting record types through `converter(...)`.

### `BaseDTO`

Source capability: `com.ygzy.components.foundation.domain.BaseDTO`

`BaseDTO` is a lightweight map-backed DTO container.

Use it only when the requirement is genuinely dynamic or generic.

Guidance:

- Prefer explicit typed VO/DTO classes for stable business contracts.
- Prefer `BaseDTO` when you need dynamic aggregation or generic extension fields and the team already accepts that shape.
- Do not create another map-backed generic DTO container in business code.

## Shared Domain Shapes

### Range objects

Examples:

- `LocalDateTimeRange`
- `LocalDateRange`
- `LocalTimeRange`
- `HourMinuteRange`
- `YearMonthRange`
- `IntRange`
- `LongRange`

Guidance:

- Prefer these range types over manually modeling `start/end` pairs when the semantics already match.
- Do not create project-local `beginTime/endTime` carrier classes if a shared range object already fits.

### Key-value and id-name models

Examples:

- `IdName`
- `KeyValue`
- `CodeName`
- `IdValue`
- `NameValue`
- `IdAmount`
- `NameAmount`
- `IdCount`
- `NameCount`

Guidance:

- Reuse these shared carriers for dropdowns, option lists, lightweight lookup outputs, and statistics when the semantics match.
- Do not rebuild `id-name`, `key-value`, or similar tiny models in every module.

### Tree and tuple contracts

Examples:

- `Tree`
- `Pair`
- `Triple`
- `Solo`
- `Tuple`
- `HourMinute`
- `YearMonth`

Guidance:

- Prefer shared tuple contracts when returning lightweight paired or grouped values in internal layers.
- Do not introduce parallel tuple classes with overlapping semantics.

## Shared Enums And Result Codes

Examples:

- `BaseEnum`
- `EnableEnum`
- `NormalEnum`
- `YesNoEnum`
- `BaseResultEnum`
- `ResultEnum`

Guidance:

- Prefer the shared enum contracts for status expression and result-code expression.
- Use project-standard result codes through `ResultEnum` or your own `BaseResultEnum` implementation instead of scattered literal codes.

## Shared Exception Hierarchy

Examples:

- `BaseException`
- `FoundationException`

Guidance:

- Custom business or toolkit exceptions should sit on top of the shared base exception hierarchy.
- Do not create unrelated runtime exception hierarchies that bypass `BaseException` unless there is a strong external integration reason.

## SPI, Constants, Databind Modules

Examples:

- `Converter`, `Parser`, `Resolver`, `Validator`, `Merger`, `Aggregator`, `Normalizer`, `Formatter`, `Codec`
- `StringConstant`, `DatabaseConstant`, `TimeFormatConstant`
- databind module providers such as `JavaTimeModuleProvider`, `NumberAsStringModuleProvider`

Guidance:

- Reuse shared SPI contracts before creating one-off functional interfaces with the same responsibility.
- Prefer shared constants and shared Jackson module providers over duplicated literals or duplicated serialization configuration.

## Review Checklist Additions

When reviewing code, explicitly check whether the implementation:

- created a custom result wrapper instead of `Result`
- created a custom page request/response instead of `PageReq` and `PageRes`
- created dynamic key-value or range types that duplicate foundation contracts
- bypassed `BaseException` and the shared result-code model
- hardcoded result codes instead of using shared result enums

## Generation Guidance

When generating code:

- default to `Result`, `PageReq`, and `PageRes`
- use explicit typed VO/DTO classes first, and `BaseDTO` only when the use case is genuinely dynamic
- prefer shared ranges, key-value models, and enums whenever they match the business shape

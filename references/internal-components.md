# Internal Base Component Reuse

Read this file whenever the task involves shared DTOs, paging, result envelopes, exceptions, enums, tuples, range objects, SPI, constants, web return conventions, validation rules, infrastructure wrappers, or cross-cutting model design.

## Reuse Priority

Internal base components take priority over generic greenfield design.

Before creating a new type, ask:

1. Does the project already expose an equivalent base model or helper?
2. Does the internal component library already define the object shape or abstraction?
3. Can the requirement be satisfied by extension, composition, or parameterization instead of a new duplicate type?

If the answer is yes, reuse the internal component.

## Component Map

Use the detailed reference file that matches the task:

- Shared core models and result contracts: [foundation-components.md](foundation-components.md)
- Shared utility layer: [toolkit-components.md](toolkit-components.md)
- Shared web conventions: [web-components.md](web-components.md)
- Shared validation layer: [validation-components.md](validation-components.md)
- Shared infrastructure wrappers: [framework-components.md](framework-components.md)
- Shared business support: [business-components.md](business-components.md)

## Known Reuse Targets

Prefer existing internal types in these categories:

- Base DTO containers such as `BaseDTO`
- Pagination request and response models such as `PageReq` and `PageRes`
- Range models instead of manually defining `start` and `end` pairs
- Common key-value models such as `IdName` and `KeyValue`
- Existing enum contracts and enum helper interfaces
- Shared constants, tuple abstractions, and SPI contracts
- Unified result wrappers, exception base classes, and shared response protocols
- Shared web exception handling and response conversion
- Shared validation annotations and validation exception handling
- Shared async context propagation, thread factory, and servlet wrappers
- Shared business exception and business property access

Do not recreate these shapes in business modules unless there is a clear documented gap.

## What Counts As A Violation

Treat these as engineering consistency problems:

- defining a new paging request object when `PageReq` already exists
- defining a new paging response wrapper when `PageRes` already exists
- creating a project-local `Result`, `Response`, or `CommonResponse` type when a shared wrapper already exists
- adding `startTime` and `endTime` carrier classes when a shared range type already exists
- redefining `id-name` or `key-value` models in a business package
- bypassing shared enum protocols and inventing incompatible enum structures
- creating parallel exception hierarchies without a good reason
- building controller-local exception translation that bypasses shared web advice without good reason
- manually validating request fields when shared validation annotations already exist
- rebuilding async MDC/request-context propagation or request-body caching helpers already provided by framework components

Utility duplication is also a consistency problem when toolkit already provides the same capability. See [toolkit-components.md](toolkit-components.md).

## Generation Guidance

When generating code:

- search for shared models first
- build business DTOs on top of existing base abstractions
- keep naming, field semantics, and response structure aligned with the internal component library
- implement only the business-specific delta when the shared model is close but not complete

## Review Guidance

When reviewing code:

- explicitly check whether shared models or wrappers already exist
- call out duplicate model creation as a defect, not a style nit
- explain the reuse path clearly
- propose a concrete rewrite using the shared abstraction

## Refactoring Guidance

When refactoring:

- replace duplicate business-side models with internal shared models when behavior is compatible
- preserve public API compatibility when needed, but highlight transitional debt if a full replacement is not safe
- keep adapter code small and local if a temporary bridge is required

## If User Materials Are Provided

If the user provides any of the following, treat them as authoritative inputs:

- source code for the internal component library
- jar usage notes
- shared model definitions
- example controller/service/repository code
- internal framework starter code

Extract reusable capabilities from those materials before proposing new abstractions.

If internal conventions conflict with generic external best practices, prefer the team convention unless it is clearly unsafe or incorrect.

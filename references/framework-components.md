# Framework Component Reuse

This file summarizes reusable capabilities extracted from `backend-components-framework`.

Use it whenever the task touches asynchronous execution, thread pools, MDC propagation, request-context propagation, servlet request-body wrappers, or Spring condition helpers.

## Positioning

`backend-components-framework` is shared infrastructure support.

It provides reusable context propagation, thread factory naming, servlet wrappers, and conditional registration helpers. Business modules should reuse these primitives instead of re-implementing infrastructure behavior locally.

## Shared Infrastructure Capabilities

### Async context propagation

Key types:

- `ContextBridge`
- `TaskRunnable`
- `TaskCallable`

Guidance:

- Prefer these shared wrappers when executing tasks asynchronously and you need MDC or request-context propagation.
- Do not build local MDC-copy wrappers or ad hoc request-context transfer logic if these shared types already satisfy the need.

### Thread factory

Key type:

- `NamedThreadFactory`

Guidance:

- Prefer `NamedThreadFactory` for project-managed thread pools so thread names remain consistent and diagnosable.
- Do not create anonymous or poorly named thread factories when a standard naming policy already exists.

### Servlet wrappers

Key types:

- `CachedBodyHttpServletRequestWrapper`
- `CachedBodyHttpServletResponseWrapper`

Guidance:

- Prefer these wrappers when request or response bodies must be read multiple times in filters or interceptors.
- Do not duplicate request-body caching wrappers in business modules.

### Spring conditions

Key types:

- `BaseCondition`
- profile/application-name condition implementations

Guidance:

- Prefer the shared condition abstractions when bean registration depends on profile or application identity.

## Review Checklist Additions

When reviewing code, explicitly check whether the implementation:

- manually propagates MDC or request attributes instead of using `ContextBridge`, `TaskRunnable`, or `TaskCallable`
- creates project-local thread factories instead of `NamedThreadFactory`
- creates request-body caching wrappers instead of using the shared servlet wrappers

## Generation Guidance

When generating infrastructure code:

- use shared framework wrappers for cross-thread context propagation
- use shared servlet wrappers in filters if repeated body access is required
- keep framework concerns out of controllers and services where possible
